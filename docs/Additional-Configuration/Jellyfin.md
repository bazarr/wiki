# Jellyfin Integration

!!! info
    Jellyfin integration requires Bazarr 1.5.7-beta.26 or newer.

Bazarr integrates with Jellyfin Media Server to refresh items and libraries automatically after subtitle changes (downloads, uploads, deletions). Authentication is API key based and the integration still requires Sonarr and Radarr to function.

## Features

- **API Key Authentication** - Simple token-based auth using a key generated in the Jellyfin dashboard
- **Library Refresh** - Re-scan specific items (or fall back to the full library) after subtitle changes
- **Multi-Library Selection** - Pick one or more movie and TV show libraries
- **Provider-ID Item Resolution** - Locates items by IMDB, TMDB, or TVDB IDs with a title + year fallback
- **Immediate or Async Refresh** - Choose between instant metadata re-read or batched filesystem notification

## Setup

### Generate a Jellyfin API Key

1. Open the Jellyfin web UI as an administrator
1. Go to `Dashboard` → `API Keys`
1. Click `+` and give the key an app name (e.g. `Bazarr`)
1. Copy the generated key

### Enable Jellyfin in Bazarr

1. Navigate to `Settings` → `Jellyfin`
1. Toggle `Enabled`
1. Enter your Jellyfin server URL (e.g. `http://192.168.1.100:8096`)
1. Paste the API key
1. Click `Test` - on success the button shows the server name and version
1. Save settings

### Configure Libraries

1. Under `Movie Library`, select one or more movie libraries from the dropdown
1. Under `Series Library`, select one or more TV show libraries
1. Enable `Refresh movie metadata after downloading subtitles` and/or `Refresh series metadata after downloading subtitles` as desired

Only libraries with the `movies` or `tvshows` collection type appear in the dropdowns - libraries of other types (music, books, mixed content, etc.) are filtered out.

### Choose a Refresh Method

The `How to notify Jellyfin after subtitle changes` dropdown controls how Bazarr asks Jellyfin to pick up changes:

| Method        | Behavior                                                                                                  |
| ------------- | --------------------------------------------------------------------------------------------------------- |
| Immediate     | Re-reads item metadata right away without contacting external providers. Recommended for most setups.     |
| Async         | Notifies Jellyfin of a filesystem change. Jellyfin picks it up in the background after ~30-60 seconds.    |

Immediate is the default and is sufficient for refreshing newly downloaded subtitles. Use Async if you prefer Jellyfin's batched scanning behavior or want to avoid synchronous refresh calls.

## Settings Reference

| UI Field                                                  | Config Key                          |
| --------------------------------------------------------- | ----------------------------------- |
| Enabled                                                   | `general.use_jellyfin`              |
| Server URL                                                | `jellyfin.url`                      |
| API Key                                                   | `jellyfin.apikey`                   |
| How to notify Jellyfin after subtitle changes             | `jellyfin.refresh_method`           |
| Movie library selection                                   | `jellyfin.movie_library` (names)    |
|                                                           | `jellyfin.movie_library_ids` (IDs)  |
| Series library selection                                  | `jellyfin.series_library` (names)   |
|                                                           | `jellyfin.series_library_ids` (IDs) |
| Refresh movie metadata after downloading subtitles        | `jellyfin.update_movie_library`     |
| Refresh series metadata after downloading subtitles       | `jellyfin.update_series_library`    |

## How Item Resolution Works

When Bazarr refreshes a single item, it locates the matching Jellyfin entry using:

1. `Imdb`, `Tmdb`, then `Tvdb` provider IDs from Jellyfin's metadata
1. Case-insensitive title match (narrowed by year if available) as fallback
1. Full library refresh if no match is found

For series, Bazarr resolves the series first, then looks up the specific episode by season and episode number when running in Immediate mode.

## Troubleshooting

### Connection Errors

- Verify the URL includes the scheme and port (e.g. `http://host:8096`, `https://jellyfin.example.com`)
- Check that the Jellyfin server is reachable from Bazarr's host
- Confirm the API key has not been revoked in `Dashboard` → `API Keys`

### Library Not Listed

- Only `movies` and `tvshows` collection types are returned - mixed-content libraries are excluded by design
- Re-open `Settings` → `Jellyfin` to refetch libraries after creating one in Jellyfin
- Check the connection still tests successfully with the saved credentials

### Items Not Refreshing

- Ensure `Refresh movie metadata after downloading subtitles` / `Refresh series metadata after downloading subtitles` is enabled for the relevant content type
- Confirm the item has at least one of an IMDB, TMDB, or TVDB ID populated in Jellyfin (or matches by title + year)
- If Bazarr cannot find the item it falls back to a full library refresh - check Bazarr's logs for `Item not found in Jellyfin` warnings
- For Async mode, allow ~30-60 seconds for Jellyfin to process the filesystem update notification

### Debug Logging

1. `Settings` → `General` → `Logging`
1. Set log level to `DEBUG`
1. Reproduce the issue
1. Check `Settings` → `System` → `Logs` for entries from the `jellyfin` module

## API Reference

- `POST /api/jellyfin/test-connection` - Test server connectivity, returns server name and version
- `GET /api/jellyfin/libraries` - List movie and TV show libraries from the configured server

---

## Related Documentation

- [Webhooks Configuration](Webhooks.md)
- [Performance Tuning](Performance-Tuning.md)
- [Settings Reference](Settings.md)
