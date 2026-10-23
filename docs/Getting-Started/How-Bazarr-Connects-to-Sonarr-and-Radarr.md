# How Bazarr Connects to Sonarr and Radarr

Bazarr is a companion application: it gets its library from Sonarr and Radarr, then indexes the matching files on disk to know which subtitles are present. It also needs to know when that library changes.

This page explains how that communication works, what Bazarr is allowed to do, and what happens when the live connection is unavailable.

## The short version

- Bazarr talks to Sonarr and Radarr over their **REST API**, using the API key you enter in `Settings` => `Library` => `Sonarr` / `Radarr`.
- For instant updates, Bazarr opens **one outbound connection** to each app's **SignalR event feed** and listens for events.
- That feed is **read-only from Bazarr's side**. Bazarr never sends commands through it.
- If the feed can't be reached, Bazarr keeps working and catches up with its **scheduled REST sync**, as long as Sonarr/Radarr are reachable over the API and periodic synchronization is enabled.
- The API key is the real access boundary, not SignalR.

!!! note
    This page describes the default behaviour. If you start Bazarr with `--no-signalr` (or set `NO_SIGNALR=true`), the live feed is disabled and the sidebar shows an `OFF` badge. See [Disabling real-time updates](#disabling-real-time-updates).

## What Bazarr uses the API for

Bazarr connects **from Bazarr to Sonarr/Radarr**. It doesn't wait for them to call it, except for optional webhooks.

With the API key, Bazarr:

- reads your series, episodes, movies, files, tags and profiles;
- asks Sonarr or Radarr to rescan a series or movie after a subtitle is added, uploaded, deleted or extracted (`RescanSeries` / `RescanMovie`).

Bazarr compares the subtitles it indexed on disk with your language profiles to decide which subtitles are missing. That calculation happens inside Bazarr, not in Sonarr/Radarr.

Bazarr's code does not call the endpoints that delete, unmonitor or edit media in Sonarr/Radarr. The API key you provide is still a full API key, so if you want to limit what *any* client can do, use a scoped key or a proxy that allows only the routes Bazarr needs (see [Security and access](#security-and-access)).

## The live event feed (SignalR)

Sonarr and Radarr expose the same real-time event feed that powers their own web UI. Bazarr opens a persistent connection to it:

- Sonarr v4 and Radarr: `.../signalr/messages?access_token=<api-key>`
- Sonarr v3 (legacy): `.../signalr?apikey=<api-key>`

The connection is **outbound**: Bazarr reaches Sonarr/Radarr, not the other way around. Sonarr/Radarr push events such as "series updated", "episode updated" or "movie updated", including the resource data. Bazarr uses the event to update only the affected series, episode or movie instead of rescanning the whole library.

Bazarr registers receive handlers only. It does not invoke hub methods, and the feed cannot be used by Bazarr to change your library.

!!! info
    SignalR is a separate connection from the REST API. A proxy or firewall can block the live feed while the REST API still works; in that case the sidebar shows `DOWN`, and scheduled sync keeps working if the REST API is reachable and periodic sync is enabled.

### What the events trigger

| Event | What Bazarr does |
| --- | --- |
| Series changed | Updates that series in Bazarr, or resyncs its episodes if Sonarr says episodes changed |
| Episode changed | Syncs that episode; searches for missing subtitles unless deferred or the file isn't available yet |
| Movie changed | Updates that movie; searches for missing subtitles unless deferred or the file isn't available yet |

Whether a search actually runs depends on your defer-search setting and whether Bazarr sees a new or changed file. The table describes the common path, not a guarantee.

After a subtitle changes, Bazarr asks Sonarr/Radarr to rescan the affected series or movie so their UI picks up the new subtitle.

## If the live connection is unavailable

The live feed is an optimisation, not a requirement. If it drops, Bazarr:

1. retries the connection automatically;
2. keeps synchronizing on the schedule configured in `Settings` => `Application` => `Scheduler`, if periodic synchronization is enabled;
3. keeps running the disk scan on its Daily or Weekly schedule; a scan set to **Manually** only runs when you trigger it.

You can see the state in the Series and Movies entries of the sidebar:

| Badge | Meaning |
| --- | --- |
| `LIVE` | Connected to the live feed. |
| `DOWN` | The feed is unreachable. Bazarr retries; scheduled sync still runs if the REST API is reachable and periodic sync is enabled. |
| `OFF` | Real-time updates are disabled with `--no-signalr` or `NO_SIGNALR=true`. Scheduled sync still runs, if enabled. |

The `LIVE` badge can be hidden in `Settings` => `Application` => `UI` => `Show Live Badge`. `DOWN` and `OFF` are always shown so you can tell "connection lost" from "disabled on purpose".

### Scheduled sync is still there

Even with a healthy live connection, Bazarr runs a periodic sync, unless you set the interval to **Manually**. The default is every 60 minutes for Sonarr and Radarr; you can set it to 15 minutes, 3/6/12/24 hours, weekly or **Manually** in `Settings` => `Application` => `Scheduler`. The disk scan (also under Scheduler) runs daily by default and only re-indexes media already in Bazarr's database; set it to **Manually** to stop automatic scans.

If you disable real-time updates, lower the sync interval if you want new imports picked up sooner. Setting the sync interval to **Manually** disables periodic synchronization, so new imports are only picked up when you run a sync yourself. With real-time updates enabled, a longer interval is usually fine because the live events handle new imports.

## Do I need webhooks?

No. Webhooks are optional and serve a different purpose.

Bazarr already accepts webhooks from Sonarr, Radarr and Plex. The Sonarr/Radarr webhooks trigger a subtitle search when a file is imported. They are useful when you want an immediate search even if the live feed is unavailable, or when you deliberately want event-driven behaviour from the Arr side.

They are not a drop-in replacement for the live feed:

- they need Bazarr to be reachable **from** Sonarr/Radarr, which adds an inbound endpoint;
- they only cover the events you configure;
- Bazarr matches the file IDs in the payload against its own database and may skip an event for media it has not synced yet;
- they need their own authentication and replay protection (the current endpoints authenticate with Bazarr's API key, not a signed request).

The endpoints accept Bazarr's API key as a query parameter or in the `X-API-KEY` header. Use the header when your webhook client supports custom headers; otherwise the key has to be in the URL, so treat that URL as a secret.

Bazarr looks up the file IDs in its database. If it hasn't synced the file yet, the event can only be resolved when the payload also includes the episode IDs (`episodes[].id` in Sonarr) or the movie ID (`movie.id` in Radarr); otherwise Bazarr skips the event and still answers HTTP 200 so Sonarr/Radarr don't mark the webhook as unhealthy. See [Library settings](/Additional-Configuration/Settings/Library/) for the Sonarr/Radarr commands and [Webhooks](/Additional-Configuration/Webhooks/) for Plex.

## Security and access

SignalR does not give Bazarr extra permissions. It is an event feed, and it uses the same API key Bazarr already has for normal API calls. The questions to ask are:

- **What can the API key do?** That depends on Sonarr/Radarr. A full key can do everything their API allows. Scoped or read-only keys are not a Bazarr setting: they have to be provided by Sonarr/Radarr, or by a proxy in front of them (for example StarrProxy). Bazarr will use whichever URL and key you configure.
- **Who can reach Sonarr/Radarr?** Keep them on your local network. If you need remote access, put them behind a reverse proxy with authentication and 2FA. Don't expose them directly to the internet.
- **Who can reach Bazarr?** The webhooks and the UI are inbound services. Use Bazarr's authentication and keep it behind your proxy as well.

!!! note
    If you put a proxy in front of Sonarr/Radarr, allow the routes Bazarr uses:

    - the read endpoints for series, episodes, movies, files, tags and profiles;
    - `POST .../signalr/messages/negotiate` and the WebSocket upgrade, if real-time updates are enabled;
    - `POST .../command` if you want Sonarr/Radarr to rescan after a subtitle changes.

    A GET-only proxy blocks live updates because of the negotiate POST, and rescan requests fail (Bazarr logs the error and keeps working).

!!! warning
    Sonarr/Radarr's API key appears in the SignalR URL as a query parameter (`access_token` / `apikey`). This is normal for the protocol, but it means the key can appear in Sonarr/Radarr access logs if you log full URLs. Protect those logs and don't publish them.

!!! note
    Bazarr's SignalR clients currently connect with TLS certificate verification disabled, so an HTTPS URL does not by itself prove you are talking to the expected server. Prefer a trusted local network or a proxy you control.

Bazarr's own code only calls read endpoints and the rescan commands described above. If you don't want to give Bazarr a key with full API access, the right fix is a scoped key or proxy, not disabling SignalR: disabling SignalR leaves the same API key in place.

## Disabling real-time updates

If you still want Bazarr to avoid opening the live connection, start it with `--no-signalr`:

```bash
python bazarr.py --no-signalr
```

For Docker, set `NO_SIGNALR=true` in the container environment instead.

This is a startup setting: restart Bazarr after changing it. When disabled:

- Bazarr doesn't open a connection to the Sonarr/Radarr event feed;
- the sidebar shows `OFF`;
- library changes are picked up by the scheduled sync (unless scheduled tasks are disabled with `--no-tasks`), and the disk scan re-indexes media already known to Bazarr on its Daily or Weekly schedule (if not set to **Manually**);
- "Sync with Sonarr/Radarr on live connection establishment" has no effect, because there is no live connection.

!!! note
    `NO_SIGNALR` and the `OFF` badge are upcoming features and are not in released builds yet. On older builds, use `--no-signalr` and rely on the scheduled sync. Note that on those builds, saving the Sonarr/Radarr connection settings can restart SignalR even when `--no-signalr` was set.

!!! note
    Disabling SignalR does not reduce the API key's permissions. If your concern is access, use a scoped key or proxy.

## Technical details

??? technical "Endpoints and behaviour"

    - Sonarr v4 / Radarr: persistent connection to `/signalr/messages` with the API key as `access_token`. The client first sends `POST /signalr/messages/negotiate`, then upgrades to a WebSocket.
    - Sonarr v3 (legacy): connection to `/signalr` with the API key as `apikey`.
    - The modern client receives `receiveMessage` events and filters for `series`, `episode` and `movie`. The legacy client subscribes to the `series` and `episode` hub events.
    - Duplicate consecutive events are dropped, and the `statistics` block is removed to keep events small.
    - Reconnection is automatic; scheduled sync is independent of the connection.
