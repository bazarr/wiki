# Library Settings

Connect and manage your media libraries through Sonarr (for TV series) and Radarr (for movies). This section covers configuration of these essential integrations.

## Overview

Bazarr integrates with Sonarr and Radarr to automatically manage subtitles for your media. These two services act as your media library managers:

- **Sonarr**: Manages TV series and episodes
- **Radarr**: Manages movies

Once connected, Bazarr can automatically search for and download subtitles based on your configured language profiles and providers.

---

## Sonarr

Connect Bazarr to your Sonarr instance for automatic TV show subtitle management.

### Enable Sonarr

Toggle **Enabled** to activate Sonarr integration with Bazarr.

> [!NOTE]
> Once enabled, Bazarr will sync with Sonarr to get information about your TV series and episodes.

### Sonarr Host Configuration

**Address**

Hostname or IPv4 address where Sonarr is running (e.g., `sonarr.example.com` or `192.168.1.100`)

> [!WARNING]
> **Docker Users**: Cannot use loopback addresses (127.0.0.1, localhost) to reach other containers. Use the container name or internal IP address instead.

**Port**

- **Default**: `8989`

TCP port Sonarr is listening on.

**Base URL**

If Sonarr is behind a reverse proxy with a path prefix (e.g., `http://example.com/sonarr`), enter `/sonarr` here.

> [!WARNING]
> Don't forget the leading slash. Make sure this exactly matches your Sonarr configuration.

**HTTP Timeout**

How long Bazarr waits for responses from Sonarr before timing out. Useful if your Sonarr instance is slow to respond.

**API Key**

Your Sonarr API key (found in Sonarr Settings > General > Security).

**SSL**

Enable if Sonarr is accessed via HTTPS. Not needed for local IP addresses.

**Test Connection**

Click the test button after configuration. A successful test confirms Bazarr can reach Sonarr and authenticate properly.

> [!IMPORTANT]
> Always test the connection before proceeding with the rest of the setup.

### Sonarr Synchronization Options

**Sync with Sonarr on Live Connection Establishment**

When Bazarr connects or reconnects to Sonarr, immediately perform a full sync to ensure data is current.

> [!NOTE]
> Enable this to ensure Bazarr has the latest series and episode information immediately after startup.

**Excluded Tags**

Episodes from series with these tags (case-sensitive) in Sonarr will be excluded from subtitle searches and downloads. Tags are user-defined in Sonarr and help categorize series.

> [!EXAMPLE]
> If you tag anime series with "anime" in Sonarr, add "anime" here to exclude them from automatic downloads. Tags are case-sensitive.

<!-- -->
> [!TIP]
> Use multiple tags separated by commas. For example: `anime, no-subs, manual-only`

**Excluded Series Types**

Automatically exclude certain series types from subtitle searches:

- **Standard**: Regular TV series
- **Anime**: Anime series (often have different naming conventions)
- **Daily**: Daily news/sports programs (e.g., news broadcasts, sports events)

> [!EXAMPLE]
> If you don't want Bazarr to search subtitles for anime series, select "Anime" here.

**Download Only Monitored**

Only search for subtitles for episodes marked as monitored in Sonarr. Useful if you want to exclude archived series or specific seasons.

> [!NOTE]
> Monitored status is configured per series and per episode in Sonarr. Only episodes marked as monitored will have subtitles downloaded.

**Sync Only Monitored Series**

Only synchronize series marked as monitored in Sonarr. Allows you to keep unmonitored series excluded from Bazarr's consideration entirely.

> [!TIP]
> If you update an unmonitored series in Sonarr and want Bazarr to know about it, toggle its monitored status briefly in Sonarr and Bazarr will sync the changes.

**Sync Only Monitored Episodes**

Combined with "Sync Only Monitored Series", only syncs episodes that are individually monitored. Especially useful for long-running shows like Saturday Night Live where you only want current season episodes synced.

> [!NOTE]
> This prevents Bazarr from trying to download subtitles for archived episodes of long-running shows.

**Defer Searching of Subtitles Until Scheduled Task Execution**

When enabled, Bazarr won't search for subtitles immediately when episodes are imported into Sonarr. Instead, searches happen only during scheduled tasks. This prevents immediate API strain when importing many episodes at once.

> [!NOTE]
> Use this if you do bulk imports to Sonarr and want to avoid overwhelming subtitle providers with simultaneous requests.

**Exclude Season Zero (Extras)**

Season 0 contains specials, extras, behind-the-scenes content, and bonus episodes. Enable this to exclude these from subtitle searches and downloads.

> [!EXAMPLE]
> Most users enable this since extras rarely need subtitles and they can clutter search results.

### Sonarr Path Mappings

Use this section only if Sonarr and Bazarr access the same files using different paths. This is necessary when:

- Sonarr and Bazarr run on different devices or systems
- Running Synology with mixed package and Docker installations
- Different mount points between systems
- NAS accessed differently from Sonarr vs Bazarr
- Using Docker containers with different volume mounts

> [!CAUTION]
> **IF YOU HAVE THE SAME PATHS ON BOTH SIDES, YOU DON'T NEED PATH MAPPINGS!** Having identical paths on both sides when you don't need mapping will cause errors and prevent subtitle detection.

**Adding a Path Mapping**

Click "Add" to create a new mapping:

1. **Sonarr Path**: The path Sonarr uses to access your show files (e.g., `/media/tv_shows/seriesX/`)
2. **Bazarr Path**: The path Bazarr uses to access the same files (e.g., `\\nas\tv\seriesX\`)

Bazarr will translate between these paths when checking file properties and downloading subtitles.

> [!EXAMPLE]
>
> - If Sonarr uses `/media/tv_shows/` and Bazarr uses `\\nas\tv\`, create a mapping between these base paths
> - Bazarr will then translate `/media/tv_shows/Breaking_Bad/Season01/` to `\\nas\tv\Breaking_Bad\Season01\` automatically
> - This allows both systems to find and work with the same files

<!-- -->
> [!NOTE]
> For Docker setups with consistent volume mounts, path mappings are usually not needed. Please review TRaSH's [Hardlink Tutorial](https://trash-guides.info/hardlinks) for best practices.

### Sonarr Webhook Integration

To trigger subtitle searches immediately when episodes are imported (instead of waiting for scheduled tasks), use this webhook command in Sonarr:

> [!EXAMPLE]
>
> ```bash
> curl -H "Content-Type: application/json" \
>   -H "X-API-KEY: YOUR_BAZARR_API_KEY" \
>   -X POST \
>   -d '{ "eventType": "Download", "episodeFiles": [ { "id": SONARR_EPISODEFILE_ID } ] }' \
>   http://your-bazarr-address:6767/api/webhooks/sonarr
> ```

See [Webhooks](../Webhooks.md) for more details on setting up webhook notifications.

---

## Radarr

Configure Bazarr to manage subtitles for movies through Radarr.

### Enable Radarr

Toggle **Enabled** to activate Radarr integration with Bazarr.

> [!NOTE]
> Once enabled, Bazarr will sync with Radarr to get information about your movies.

### Radarr Host Configuration

**Address**

Hostname or IPv4 address where Radarr is running (e.g., `radarr.example.com` or `192.168.1.100`)

> [!WARNING]
> **Docker Users**: Cannot use loopback addresses (127.0.0.1, localhost) to reach other containers. Use the container name or internal IP address instead.

**Port**

- **Default**: `7878`

TCP port Radarr is listening on. Note this is different from Sonarr's default port (8989).

**Base URL**

If Radarr is behind a reverse proxy with a path prefix (e.g., `http://example.com/radarr`), enter `/radarr` here.

> [!WARNING]
> Don't forget the leading slash. Make sure this exactly matches your Radarr configuration.

**HTTP Timeout**

How long Bazarr waits for responses from Radarr before timing out. Useful if your Radarr instance is slow to respond.

**API Key**

Your Radarr API key (found in Radarr Settings > General > Security).

**SSL**

Enable if Radarr is accessed via HTTPS. Not needed for local IP addresses.

**Test Connection**

Click the test button after configuration. A successful test confirms Bazarr can reach Radarr and authenticate properly.

> [!IMPORTANT]
> Always test the connection before proceeding with the rest of the setup.

### Radarr Synchronization Options

**Sync with Radarr on Live Connection Establishment**

When Bazarr connects or reconnects to Radarr, immediately perform a full sync to ensure data is current.

> [!NOTE]
> Enable this to ensure Bazarr has the latest movie information immediately after startup.

**Excluded Tags**

Movies with these tags (case-sensitive) in Radarr will be excluded from subtitle searches and downloads. Tags are user-defined in Radarr and help categorize movies.

> [!EXAMPLE]
> If you tag foreign language films with "original-lang" in Radarr, add "original-lang" here to exclude them from automatic downloads if you only want subtitles for English movies.

<!-- -->
> [!TIP]
> Use multiple tags separated by commas. For example: `no-subs, manual-only, dubbed-only`

**Download Only Monitored**

Only search for subtitles for movies marked as monitored in Radarr. Useful if you want to exclude certain movies from automatic subtitle downloads.

> [!NOTE]
> Monitored status is configured per movie in Radarr. Only movies marked as monitored will have subtitles downloaded automatically.

**Sync Only Monitored Movies**

Only synchronize movies marked as monitored in Radarr. Allows you to keep unmonitored movies excluded from Bazarr's consideration entirely.

> [!TIP]
> If you add or update a movie in Radarr and want Bazarr to immediately sync it, ensure the movie is marked as monitored in Radarr.

**Defer Searching of Subtitles Until Scheduled Task Execution**

When enabled, Bazarr won't search for subtitles immediately when movies are imported into Radarr. Instead, searches happen only during scheduled tasks. This prevents immediate API strain when importing many movies at once.

> [!NOTE]
> Use this if you do bulk imports to Radarr and want to avoid overwhelming subtitle providers with simultaneous requests.

### Radarr Path Mappings

Use the same approach as Sonarr for path mappings when Radarr and Bazarr access files differently.

> [!CAUTION]
> **IF YOU HAVE THE SAME PATHS ON BOTH SIDES, YOU DON'T NEED PATH MAPPINGS!** Having identical paths on both sides when you don't need mapping will cause errors.

**Adding a Path Mapping**

Click "Add" to create a new mapping:

1. **Radarr Path**: The path Radarr uses to access your movie files (e.g., `/media/movies/`)
2. **Bazarr Path**: The path Bazarr uses to access the same files (e.g., `\\nas\films\`)

Bazarr will translate between these paths when checking file properties and downloading subtitles.

> [!EXAMPLE]
>
> - If Radarr uses `/media/movies/` and Bazarr uses `\\nas\films\`, create a mapping between these paths
> - Bazarr will then translate `/media/movies/Inception.mkv` to `\\nas\films\Inception.mkv` automatically

<!-- -->
> [!NOTE]
> For Docker setups with consistent volume mounts, path mappings are usually not needed.

### Radarr Webhook Integration

To trigger subtitle searches immediately when movies are imported (instead of waiting for scheduled tasks), use this webhook command in Radarr:

> [!EXAMPLE]
>
> ```bash
> curl -H "Content-Type: application/json" \
>   -H "X-API-KEY: YOUR_BAZARR_API_KEY" \
>   -X POST \
>   -d '{ "eventType": "Download", "movieFile": { "id": RADARR_MOVIEFILE_ID } }' \
>   http://your-bazarr-address:6767/api/webhooks/radarr
> ```

See [Webhooks](../Webhooks.md) for more details on setting up webhook notifications.

---

## Monitoring and Troubleshooting

### Connection Issues

If Bazarr cannot connect to Sonarr or Radarr:

1. **Test Connection**: Use the Test button in settings
2. **Verify Address**: Ensure hostname/IP and port are correct
3. **Check API Key**: Confirm the API key is valid and hasn't been regenerated
4. **Firewall**: Check that firewalls aren't blocking connections
5. **Docker Networking**: For Docker users, verify container networking is set up correctly

> [!TIP]
> Check the Bazarr logs (enable Debug mode in Application > Maintenance) for detailed connection error messages.

### Synchronization Issues

If series/movies aren't syncing:

1. **Check Monitored Status**: Verify series/movies are marked as monitored in Sonarr/Radarr
2. **Verify Sync Settings**: Confirm excluded tags and series types aren't hiding your content
3. **Check Path Mappings**: If applicable, ensure path mappings are configured correctly
4. **Resync**: Restart Bazarr to force a full resync with Sonarr/Radarr

### Missing Subtitles

If subtitles aren't being found:

1. **Verify Language Profiles**: Check that language profiles are configured
2. **Check Providers**: Ensure at least one subtitle provider is enabled
3. **Check Exclusions**: Verify excluded tags and series types aren't filtering your content
4. **Check Monitoring**: Confirm episodes/movies are marked as monitored
5. **Review Scores**: Check if minimum score settings are too high

### Performance Issues

If Bazarr is slow syncing with large libraries:

1. **Enable Adaptive Searching**: Reduces unnecessary searches
2. **Reduce Concurrent Jobs**: Lower the concurrent jobs limit in Application settings
3. **Use Sync Only Monitored**: Only sync monitored content
4. **Check Scheduler**: Adjust sync frequency if too frequent

---

## Best Practices

### Setup Order

1. Connect to Sonarr/Radarr first
2. Configure and test connections
3. Set up language profiles
4. Enable subtitle providers
5. Configure scheduler for optimal sync frequency

### Optimization

- **Large Libraries**: Use exclusion tags to reduce sync load
- **Remote Sonarr/Radarr**: Configure appropriate timeouts
- **Docker**: Use container names instead of IP addresses
- **Multiple Instances**: Use different tags/exclusions per instance

### Maintenance

- Regularly test connections
- Monitor logs for sync errors
- Review exclusion rules periodically
- Keep Sonarr, Radarr, and Bazarr updated

---

## Related Settings

For scheduler configuration, see [Application > Scheduler](Application.md#scheduler).

For subtitle provider configuration, see [Providers](Providers.md).

For language profile configuration, see [Languages](Languages.md).

For webhook setup, see [Webhooks](../Webhooks.md).
