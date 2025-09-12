# Plex Integration

Bazarr integrates with Plex Media Server to enable automatic subtitle management and workflow automation. The integration supports OAuth authentication (default) and legacy API key configuration.

**It should be noted that this functionality is available from Bazarr version 1.5.3 and onwards - and still needs Sonarr and Radarr to function.**

## Features

- **OAuth Authentication** - Secure token-based authentication with automatic server discovery
- **Library Updates** - Refresh specific items after subtitle changes
- **Added Date Updates** - Set movie and episode added dates automatically
- **Webhooks** - Real-time subtitle search on media playback (Plex Pass required)
- **Automatic Migration** - Seamless upgrade from API key to OAuth authentication

## Authentication Methods

### OAuth Authentication (Default)

OAuth is the default authentication method available through the web interface.

**Benefits:**

- Secure PIN-based authentication flow
- Automatic server discovery and testing
- Encrypted token storage with URLSafeSerializer
- No manual API key retrieval required

**Requirements:**

- Internet connection during setup
- Web browser with popup windows enabled
- Plex account with server access

### API Key Authentication (Legacy)

API key authentication is legacy and should be avoided. It's available only through manual `config.yaml` configuration for existing setups.

**Configuration:**

```yaml
plex:
  auth_method: apikey
  ip: 192.168.1.100
  port: 32400
  ssl: false
  apikey: your-plex-token-here
  movie_library: Movies
  series_library: TV Shows
```

**Requirements:**

- Manual config.yaml editing
- Manual API key retrieval

## Setup

### Enable Plex Integration

1. Navigate to `Settings` → `General`
1. Enable `Use Plex`
1. Save settings

### OAuth Configuration

1. Go to `Settings` → `Plex`
1. Click "Connect to Plex"
1. Complete authentication in popup window
1. Select server (if multiple servers available)
1. Configure movie and TV libraries

### Server Selection

- Single server: automatically selected
- Multiple servers: choose from dropdown and click "Select Server"
- Connection testing performed automatically

### Library Configuration

- Libraries automatically fetched from selected server
- Only movie and show libraries displayed
- Select one library per content type

## Settings Reference

| Setting | Description | OAuth | API Key |
|---------|-------------|-------|---------|
| Authentication Method | OAuth (web UI) / apikey (config) | Default | Manual |
| Server Discovery | Automatic with testing | Yes | Manual |
| Token Storage | Encrypted with URLSafeSerializer | Yes | Yes |
| Library Fetching | Automatic from server | Yes | Manual |

### Integration Options

| Feature | Description |
|---------|-------------|
| Set Movie Added Date | Update added date when subtitles found |
| Set Episode Added Date | Update added date when subtitles found |
| Update Movie Library | Refresh specific movies after changes |
| Update Series Library | Refresh specific episodes after changes |

## Webhooks

Plex webhooks enable real-time subtitle search when media is played. Webhook creation and management is automated through the Bazarr UI.

**Requirements:**

- Plex Pass subscription
- Configured Plex integration
- Bazarr API key

**Webhook URL Format:**

```
http(s)://your-bazarr-url/api/webhooks/plex?apikey=your-bazarr-api-key
```

**Webhook Management:**

- `POST /api/plex/webhook/create` - Create webhook automatically
- `GET /api/plex/webhook/list` - List existing webhooks
- `POST /api/plex/webhook/delete` - Delete webhook

## Migration

### Automatic Migration (Default)

Bazarr automatically migrates API key configurations to OAuth on startup.

**Process:**

1. Detects existing API key configuration
1. Creates backup of current settings
1. Validates OAuth configuration
1. Preserves library names and settings
1. Implements rollback on failure

**Safety Features:**

- Configuration backup before changes
- Validation before permanent changes
- Graceful rollback on failure
- Migration attempt tracking

### Manual Migration

If automatic migration fails:

1. Note current server IP, port, and library settings
1. Enable OAuth in settings
1. Complete OAuth authentication
1. Select matching server
1. Verify library configuration
1. Test functionality

## Troubleshooting

### OAuth Issues

**Authentication Fails:**

- Check internet connectivity
- Verify popup windows aren't blocked
- Clear browser cache
- Verify Plex account server access

**Server Not Found:**

- Verify Plex server is online
- Check server account association
- Ensure remote access if needed

**Token Errors:**

- Re-authenticate with OAuth
- Check if Plex password changed
- Clear Bazarr Plex settings

### API Key Issues

**Connection Timeouts:**

- Verify IP address and port
- Check network connectivity
- Ensure Plex server is running

**Invalid Token:**

- Regenerate Plex token
- Verify token permissions
- Check Plex account status

**Library Not Found:**

- Check exact library names
- Verify library permissions
- Check Plex server logs

### Debug Logging

1. `Settings` → `General` → `Logging`
1. Set log level to `DEBUG`
1. Reproduce issue
1. Check `Settings` → `System` → `Logs`

## API Reference

### OAuth Endpoints

- `POST /api/plex/oauth/pin` - Create authentication PIN
- `GET /api/plex/oauth/pin/{pinId}/check` - Check PIN status
- `GET /api/plex/oauth/validate` - Validate token
- `GET /api/plex/oauth/servers` - List available servers
- `GET /api/plex/oauth/libraries` - Fetch libraries
- `POST /api/plex/oauth/logout` - Clear authentication

### Server Management

- `POST /api/plex/test-connection` - Test server connectivity
- `GET /api/plex/select-server` - Get selected server
- `POST /api/plex/select-server` - Select server

### API Key Management

- `POST /api/plex/apikey` - Save and encrypt API key
- `POST /api/plex/encrypt-apikey` - Encrypt existing key

## Technical Implementation

### Encryption

- Uses `URLSafeSerializer` for token storage
- Secure random salt generation (`secrets.token_hex(16)`)
- 256-bit cryptographically secure keys

### OAuth Flow

- PIN-based authentication with CSRF protection
- Parallel server testing (max 5 threads)
- Automatic popup window management
- 600-second PIN TTL with cleanup

### Library Updates

- Individual item refresh via `PlexServer.refresh()`
- IMDB ID-based targeting
- Fallback to full library scan

### Webhook Processing

- Processes `media.play` events only
- IMDB ID extraction from Plex GUID arrays
- Series ID resolution for episodes
- API key authentication required

---

## Related Documentation

- [Webhooks Configuration](Webhooks.md)
- [Performance Tuning](Performance-Tuning.md)
- [Settings Reference](Settings.md)
