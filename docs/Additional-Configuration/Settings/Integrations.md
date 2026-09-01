# Integration Settings

Configure connections to external media services for subtitle management and automation.

## Plex

Integrate with Plex Media Server for subtitle management in Plex libraries.

### Use Plex

Enable Plex integration.

### Plex Connection

**Authentication**

Authenticate with your Plex account using OAuth (recommended). This grants Bazarr access to your Plex servers.

### Plex Server Configuration

Select which Plex server Bazarr should manage subtitles for (if you have multiple).

### Plex Movie Library

**Library Name**

Select your movie library from Plex.

**Mark movies as recently added after downloading subtitles**

Updates the movie's "added date" in Plex so newly subtitled movies appear in the "Recently Added" hub.

**Refresh movie metadata after downloading subtitles**

Refreshes the movie in Plex so the newly downloaded subtitle file is detected and made available. **Highly Recommended**.

### Plex Series Library

**Library Name**

Select your TV show library from Plex.

**Mark episodes as recently added after downloading subtitles**

Updates the episode's "added date" in Plex so newly subtitled episodes appear in "Recently Added".

**Refresh series metadata after downloading subtitles**

Refreshes the episode in Plex so newly downloaded subtitle files are detected. **Highly Recommended**.

### Plex Automation

**Webhooks**

Create a Bazarr webhook in Plex to automatically search for subtitles when content starts playing.

**Autopulse Configuration**

Generates a ready-to-use Autopulse configuration file with OAuth settings and path rewrites pre-configured for your Plex server.

---

## Jellyfin

Integrate with Jellyfin for subtitle management in Jellyfin libraries.

### Use Jellyfin

Enable Jellyfin integration.

### Jellyfin Connection

**Authentication**

Authenticate with your Jellyfin server. Configure the server address, username, and API key or access token.

### Jellyfin Server Configuration

Select which Jellyfin server Bazarr should manage subtitles for (if you have multiple).

### Jellyfin Movie Library

**Library Name**

Select your movie library from Jellyfin.

**Mark movies as recently added after downloading subtitles**

Updates the movie's "added date" in Jellyfin so newly subtitled movies appear in the "Recently Added" section.

**Refresh movie metadata after downloading subtitles**

Refreshes the movie in Jellyfin so the newly downloaded subtitle file is detected and made available. **Highly Recommended**.

### Jellyfin Series Library

**Library Name**

Select your TV show library from Jellyfin.

**Mark episodes as recently added after downloading subtitles**

Updates the episode's "added date" in Jellyfin so newly subtitled episodes appear in "Recently Added".

**Refresh series metadata after downloading subtitles**

Refreshes the episode in Jellyfin so newly downloaded subtitle files are detected. **Highly Recommended**.

### Jellyfin Automation

**Webhooks**

Configure Bazarr webhooks in Jellyfin to automatically search for subtitles on playback events.

---

## Related Settings

For comprehensive information about connecting to your media library management systems (Sonarr for TV series and Radarr for movies), see the [Library Settings](Library.md) section.

For notification setup about your integrations, see [Notifications](Notifications.md).
