# Application Settings

Manage core Bazarr configuration and system settings.

## General

Core configuration for Bazarr's network interface and basic operation.

### Host

**Address**

- **Default**: `*`
- **Valid Values**: Valid IP address or `*` for all interfaces

Specifies which network interface Bazarr should listen on.

> [!TIP]
> Use `*` to listen on every available IP address (recommended). If running inside a Docker container, `*` is the recommended value.

**Port**

- **Default**: `6767`

The TCP port on which Bazarr will listen. Ensure this port is available and not blocked by firewall rules.

**Base URL**

Allows you to serve Bazarr in a sub-directory, useful for reverse proxy setups. For example: `http://127.0.0.1:6767/bazarr/` instead of the default `http://127.0.0.1:6767/`

> [!WARNING]
> This is only necessary if you're using a reverse proxy. Otherwise, leave it empty.

**Instance Name**

Set a custom instance name that will appear in the browser's tab title. Useful when running multiple Bazarr instances.

**Hostname**

Hostname or IP address used to access Bazarr (e.g., `bazarr.mydomain.local` or `192.168.0.100`).

> [!WARNING]
> Required for webhook security. This must be the address where external services can reach your Bazarr instance.

---

## Security

Configure authentication and API access to Bazarr.

**Authentication Type**

Choose how users authenticate to Bazarr:

- **No Authentication**: Anyone with access to the URL can use Bazarr
- **Basic Authentication**: Browser popup-based login (less secure without SSL)
- **Forms Login**: Web form-based login (recommended)

> [!CAUTION]
> Basic authentication is not secure without SSL/TLS. If using basic auth, always pair it with a reverse proxy using SSL.

**Username**

Username required to access Bazarr when authentication is enabled.

**Password**

Password required to access Bazarr when authentication is enabled.

**API Key**

Unique API key for programmatic access to Bazarr's REST API.

**Actions**:

- **Copy API Key**: Copy the current API key to your clipboard (available in secure contexts only)
- **Regenerate**: Generate a new API key (replaces the old one immediately)

**Enable CORS Headers**

Allow third-party applications to make cross-origin requests to Bazarr.

> [!CAUTION]
> Requires a restart of Bazarr when changed. Only enable if you understand the security implications.

---

## Jobs Manager

**Concurrent Jobs**

- **Range**: 1 to your CPU core count
- **Default**: Based on system CPU cores

Number of concurrent jobs allowed in the jobs manager. Bazarr will process multiple operations simultaneously up to this limit.

> [!NOTE]
> **Tuning Guide**
> - **Too High**: Can cause performance issues, system slowdowns, and excessive resource usage
> - **Too Low**: Jobs remain queued longer than necessary
> - **Recommendation**: Start with your CPU core count and adjust based on system responsiveness

---

## Incoming Webhooks

Configure external webhook providers that can trigger events in Bazarr. This enables integration with Sonarr, Radarr, Plex, and Jellyfin webhook systems.

---

## Proxy

Configure proxy server settings for outbound connections.

**Type**

Select the proxy protocol:

- **No Proxy**: Direct connection (default)
- **HTTP(S)**: HTTP or HTTPS proxy
- **SOCKS4**: SOCKS4 proxy
- **SOCKS5**: SOCKS5 proxy

**Host**

Hostname or IP address of your proxy server.

**Port**

TCP port on which the proxy server listens.

**Username**

Username for proxy authentication (only required if your proxy needs authentication).

**Password**

Password for proxy authentication (only required if your proxy needs authentication).

**Ignored Addresses**

List of domains or IP addresses that should bypass the proxy. Separate multiple entries with commas.

> [!NOTE]
> - Supports: Domain names, IPv4 addresses, and subdomain wildcards
> - Not supported: Asterisk wildcards, regex, CIDR notation
> - **Example**: `.example.com` will exclude all subdomains of example.com
> - **Example**: `192.168.1.1` will exclude that specific IP

---

## Analytics

**Enable Analytics**

When enabled, Bazarr sends anonymous usage information to help the developers:

- Which subtitle providers are used
- What languages are searched
- Bazarr, Python, Sonarr, and Radarr versions
- Operating system information

> [!TIP]
> Keep this enabled. It's the primary way developers understand how Bazarr is used and helps prioritize features and bug fixes. No personally identifying information is collected.

---

## UI Settings

Configure the user interface appearance and behavior.

**Page Size**

Number of items displayed per page in list views (tables).

**Theme**

- **Auto**: Follows system theme preference
- **Light**: Force light theme
- **Dark**: Force dark theme

**Show Live Badge**

Controls the "LIVE" badge that appears when SignalR connection is active:

- **Auto**: Show when connected
- **Always**: Always show the badge
- **Never**: Never show the badge

> [!NOTE]
> The "DOWN" badge always appears when SignalR is disconnected, regardless of this setting.

---

## Scheduler

Manage automatic task execution schedules for Bazarr operations.

### Sonarr/Radarr Sync

**Sync with Sonarr**

How frequently Bazarr synchronizes with Sonarr to get updated series and episode information.

**Sync with Radarr**

How frequently Bazarr synchronizes with Radarr to get updated movie information.

### Disk Indexing

**Update All Episode Subtitles from Disk**

How often Bazarr scans the disk to detect subtitle files for episodes. This refreshes the subtitle status in Bazarr.

- **Daily**: Run at the same time every day
- **Weekly**: Run on a specific day and time each week
- **Manually**: Only run when triggered manually

**Use Cached Embedded Subtitles Parser Results**

When enabled, Bazarr caches embedded subtitle detection results to reduce disk I/O. When disabled, Bazarr re-analyzes all files each run.

> [!WARNING]
> Disabling this increases disk I/O significantly, which can impact system performance and wake sleeping hard drives.

**Update All Movie Subtitles from Disk**

Same as episodes but for movies.

### Search and Upgrade Subtitles

**Search for Missing Series Subtitles**

How often Bazarr automatically searches for missing subtitles for TV episodes.

**Search for Missing Movies Subtitles**

How often Bazarr automatically searches for missing subtitles for movies.

**Upgrade Previously Downloaded Subtitles**

How often Bazarr searches for better-quality versions of already-downloaded subtitles.

### Backups

**Folder**

Absolute path to the directory where Bazarr should store backup files. Must be writable by the Bazarr process.

> [!EXAMPLE]
> `/backup/bazarr` or `C:\Backups\Bazarr`

**Retention**

Number of days to keep backup files. Backups older than this are automatically deleted.

**Backup Database and Configuration File**

How often Bazarr automatically backs up the database and configuration:

- **Disabled**: No automatic backups
- **Daily**: Back up every day
- **Weekly**: Back up once per week

---

## Maintenance

Monitor and configure system maintenance features.

### Updates

**Automatic Updates**

Enable automatic downloading and installation of Bazarr updates. Bazarr will restart automatically when updates are installed.

**Branch**

- **Master**: Latest stable releases
- **Develop**: Latest development builds (may contain bugs)

### Logging

**Debug**

Enable debug-level logging for detailed troubleshooting. This significantly increases log file size.

> [!TIP]
> Only enable temporarily for troubleshooting. Disable after collecting logs.

**Include Filter**

Log only entries matching this pattern (text or regex, depending on settings below).

**Exclude Filter**

Exclude entries matching this pattern from logs (text or regex, depending on settings below).

**Use Regular Expressions (Regex)**

Treat the Include and Exclude filters as regular expressions instead of plain text.

**Ignore Case**

Make filter matching case-insensitive.
