# Plex Integration

Bazarr offers comprehensive integration with Plex Media Server, enabling automatic subtitle management and enhanced workflow automation. This integration supports both modern OAuth authentication and legacy API key methods.

## Overview

Plex integration allows Bazarr to:

- **Authenticate with Plex** - Secure OAuth-based authentication or legacy API key authentication
- **Update Added Dates** - Automatically set movie and episode added dates in Plex
- **Trigger Library Updates** - Refresh Plex libraries when subtitles are added
- **Receive Webhooks** - Real-time subtitle search when media is played (Plex Pass required)

## Authentication Methods

Bazarr supports two authentication methods for Plex integration:

### OAuth Authentication (Recommended)

OAuth provides a secure, user-friendly authentication flow without requiring you to manually obtain API keys.

**Benefits:**
- Secure token-based authentication
- No need to manually find API keys
- Automatic server discovery
- Encrypted token storage
- Seamless user experience

**Requirements:**
- Active internet connection during setup
- Modern web browser
- Plex account with access to the servers you want to use

### Legacy API Key Authentication

Traditional authentication method using Plex API keys.

**Benefits:**
- Works offline after initial setup
- Compatible with older setups
- Direct server connection

**Requirements:**
- Manual API key retrieval from Plex
- Manual server configuration

## Configuration

### Initial Setup

1. **Enable Plex Integration**
   - Navigate to `Settings` → `General`
   - Enable the `Use Plex` option
   - Save your settings

2. **Choose Authentication Method**
   - Go to `Settings` → `Plex`
   - Select your preferred authentication method

### OAuth Setup (Recommended)

1. **Start OAuth Flow**
   - In Plex settings, select "OAuth" as the authentication method
   - Click the "Authenticate with Plex" button
   - A new browser window will open

2. **Authenticate with Plex**
   - Sign in to your Plex account in the popup window
   - Grant permission to Bazarr
   - The window will close automatically upon success

3. **Select Server**
   - Bazarr will automatically discover your available Plex servers
   - Select the server you want to use from the dropdown
   - Test the connection to ensure it's working

4. **Configure Library Settings**
   - Set your Movie Library name
   - Set your TV Shows Library name
   - Configure additional options as needed

### API Key Setup (Legacy)

1. **Obtain Your Plex Token**
   - Follow [this guide](https://support.plex.tv/articles/204059436-finding-an-authentication-token-x-plex-token/) to find your Plex token
   - Or use browser developer tools on plex.tv to find the X-Plex-Token header

2. **Configure Server Details**
   - Enter your Plex server IP address
   - Set the port (default: 32400)
   - Enable SSL if your Plex server uses HTTPS
   - Enter your Plex token

3. **Test Connection**
   - Use the "Test" button to verify connectivity
   - Ensure Bazarr can communicate with your Plex server

## Settings Reference

### Authentication Settings

| Setting | Description | OAuth | API Key |
|---------|-------------|-------|---------|
| **Authentication Method** | Choose between OAuth and API key authentication | ✓ | ✓ |
| **Server URL** | Automatically configured during OAuth | Auto | Manual |
| **Token/API Key** | Encrypted token storage | Auto | Manual |
| **Server Name** | Display name for the selected server | Auto | Manual |

### Connection Settings

| Setting | Description | Default | Notes |
|---------|-------------|---------|-------|
| **IP Address** | Plex server IP address | 127.0.0.1 | Use local IP for best performance |
| **Port** | Plex server port | 32400 | Standard Plex port |
| **Use SSL** | Enable HTTPS connection | False | Enable if Plex uses SSL |

### Library Settings

| Setting | Description | Required |
|---------|-------------|----------|
| **Movie Library** | Name of your Plex movie library | Yes |
| **TV Shows Library** | Name of your Plex TV shows library | Yes |

### Integration Features

| Setting | Description | Impact |
|---------|-------------|--------|
| **Set Movie Added Date** | Update movie added date when subtitles are found | Enables tracking in Plex |
| **Set Episode Added Date** | Update episode added date when subtitles are found | Enables tracking in Plex |
| **Update Movie Library** | Refresh movie library after subtitle changes | Forces Plex to detect new subtitles |
| **Update Series Library** | Refresh TV library after subtitle changes | Forces Plex to detect new subtitles |

## Advanced Configuration

### Security Considerations

**OAuth Security Features:**
- Tokens are encrypted at rest using Fernet symmetric encryption
- Automatic token refresh every 24 hours
- Secure PIN-based authentication flow
- Rate limiting on authentication endpoints

**API Key Security:**
- Store API keys securely
- Use HTTPS when possible
- Limit API key scope if supported by Plex

### Connection Optimization

**Local Network:**
- Use local IP addresses for better performance
- Ensure Bazarr and Plex are on the same network
- Configure firewall rules if necessary

**Remote Access:**
- OAuth works seamlessly with remote servers
- API key method may require additional configuration
- Consider VPN for secure remote access

### Library Configuration

**Library Names:**
- Use exact library names as they appear in Plex
- Case-sensitive matching
- Verify library names in Plex settings

**Multiple Libraries:**
- Each library type (Movies/TV) supports one library
- Use library with the most content if you have multiple
- Consider organizing content into single libraries

## Troubleshooting

### Common OAuth Issues

**Authentication Fails:**
1. Ensure internet connectivity
2. Check if popup windows are blocked
3. Try clearing browser cache and cookies
4. Verify Plex account has access to servers

**Server Not Found:**
1. Check if Plex server is online
2. Verify server is associated with your Plex account
3. Ensure server allows remote access if needed
4. Check Plex server logs for connection issues

**Token Errors:**
1. Try re-authenticating with OAuth
2. Check if Plex account password was changed
3. Verify server permissions haven't changed
4. Clear Bazarr's Plex settings and start over

### Common API Key Issues

**Connection Timeouts:**
1. Verify IP address and port
2. Check network connectivity
3. Ensure Plex server is running
4. Test with direct server access

**Invalid Token Errors:**
1. Regenerate Plex token
2. Check token hasn't expired
3. Verify token has necessary permissions
4. Ensure Plex account is active

**Library Not Found:**
1. Check exact library names
2. Verify library permissions
3. Ensure libraries are shared properly
4. Check Plex server logs

### Migration Issues

**Upgrading from API Key to OAuth:**
1. Existing settings are preserved during migration
2. OAuth can be set up alongside existing API key
3. Switch authentication method in settings
4. Test thoroughly before removing old settings

### General Troubleshooting

**Enable Debug Logging:**
1. Go to `Settings` → `General` → `Logging`
2. Set log level to `DEBUG`
3. Reproduce the issue
4. Check logs in `Settings` → `System` → `Logs`

**Test Connection:**
1. Use the built-in connection test
2. Check network connectivity to Plex server
3. Verify Plex server is accessible from Bazarr
4. Test with minimal configuration first

## Integration with Other Features

### Webhooks Integration

Plex webhooks provide real-time subtitle search when media is played:

1. **Requirements:**
   - Plex Pass subscription
   - Properly configured Plex integration
   - Bazarr API key

2. **Setup:**
   - Follow the [Webhooks guide](Webhooks.md#plex) for detailed setup
   - Use OAuth or API key authentication as configured

3. **Benefits:**
   - Instant subtitle search on media playback
   - Reduced manual intervention
   - Better user experience

### Subtitle Management

**Automatic Updates:**
- Plex libraries are updated when subtitles are added
- Added dates are updated for tracking purposes
- Subtitle changes are reflected immediately

**Manual Management:**
- Force library updates from Bazarr
- Update specific movies or episodes
- Refresh metadata as needed

## Migration Guide

### From API Key to OAuth

1. **Backup Current Settings:**
   - Note current server configuration
   - Document library names and settings
   - Export configuration if needed

2. **Set Up OAuth:**
   - Change authentication method to OAuth
   - Complete authentication flow
   - Verify server selection matches previous setup

3. **Verify Settings:**
   - Check library names are correctly set
   - Test connection and functionality
   - Ensure all features work as expected

4. **Cleanup:**
   - Old API key settings are preserved for rollback
   - Remove old settings once OAuth is confirmed working

### From Legacy Setup

If upgrading from very old Bazarr versions:

1. **Check Current Configuration:**
   - Verify Plex integration is working
   - Document current settings
   - Test basic functionality

2. **Update Authentication:**
   - Choose appropriate authentication method
   - Migrate settings using built-in migration tools
   - Test after migration

## Best Practices

### Security

- **Use OAuth when possible** for better security
- **Keep software updated** for latest security features
- **Use local network connections** when available
- **Monitor authentication logs** for suspicious activity

### Performance

- **Use local IP addresses** for Plex servers
- **Configure appropriate timeouts** for your network
- **Enable library updates selectively** to avoid excessive refreshes
- **Monitor system resources** during heavy operations

### Maintenance

- **Regularly test connections** to ensure reliability
- **Monitor logs** for errors or warnings
- **Keep backups** of working configurations
- **Update settings** when Plex server changes

## API Reference

For developers and advanced users, Bazarr provides REST API endpoints for Plex integration:

### OAuth Endpoints

- `POST /api/plex/oauth/pin` - Create authentication PIN
- `GET /api/plex/oauth/pin/{pinId}/check` - Check PIN status
- `GET /api/plex/oauth/validate` - Validate stored token
- `GET /api/plex/oauth/servers` - List available servers
- `POST /api/plex/oauth/logout` - Clear authentication

### Server Management

- `POST /api/plex/test-connection` - Test server connectivity
- `POST /api/plex/select-server` - Select active server

### Legacy Operations

- Plex operations available through existing Bazarr API
- Library updates via standard API endpoints
- Webhook integration through `/api/webhooks/plex`

---

## Related Documentation

- [Webhooks Configuration](Webhooks.md) - Set up Plex webhooks for real-time integration
- [Performance Tuning](Performance-Tuning.md) - Optimize Bazarr performance with Plex
- [Settings Reference](Settings.md) - Complete settings documentation

## Quick Start Guide

### For New Users

1. **Enable Plex Integration**
   ```
   Settings → General → Use Plex: ✓
   ```

2. **Configure Authentication (OAuth Recommended)**
   ```
   Settings → Plex → Authentication Method: OAuth
   ```

3. **Authenticate**
   - Click "Authenticate with Plex"
   - Sign in when prompted
   - Grant permission to Bazarr

4. **Select Server & Configure Libraries**
   - Choose your Plex server from the dropdown
   - Enter your movie library name (e.g., "Movies")
   - Enter your TV shows library name (e.g., "TV Shows")

5. **Test & Save**
   - Use the "Test" button to verify connection
   - Save your settings

### For Existing API Key Users

If you're currently using API key authentication and want to upgrade to OAuth:

1. **Note Current Settings** (for backup)
   - Current server IP and port
   - Library names
   - Any custom configurations

2. **Switch to OAuth**
   ```
   Settings → Plex → Authentication Method: OAuth
   ```

3. **Complete OAuth Setup**
   - Follow the OAuth authentication flow
   - Select the same server you were using before
   - Verify library names match your previous setup

4. **Verify & Test**
   - Test the connection
   - Ensure all features work as expected
   - Your old API key settings are preserved as backup

## Example Configurations

### Home Network Setup (Recommended)
```
Authentication Method: OAuth
Server: [Auto-discovered from OAuth]
Movie Library: Movies
TV Shows Library: TV Shows
Set Added Date: ✓ (both Movies and Episodes)
Update Libraries: ✓ (both Movies and Series)
```

### Remote Access Setup
```
Authentication Method: OAuth (automatically handles remote access)
Server: [Select remote server from list]
Movie Library: Movies
TV Shows Library: TV Shows
Update Libraries: ✓ (recommended for remote setups)
```

### Legacy Setup (API Key)
```
Authentication Method: API Key
IP Address: 192.168.1.100
Port: 32400
SSL: ✗ (unless your Plex uses HTTPS)
API Key: [Your Plex token]
Movie Library: Movies
TV Shows Library: TV Shows
```

## Common Configuration Scenarios

### Single User, Local Plex Server
- Use **OAuth** for simplest setup
- Enable all integration features
- Use local network IP for best performance

### Multi-User Plex Server
- Each Bazarr user should use **OAuth** with their own Plex account
- Consider library access permissions
- Test with appropriate user accounts

### Dockerized Bazarr
- **OAuth** works seamlessly in containers
- No special network configuration needed
- Server discovery works automatically

### VPN/Remote Setup
- **OAuth** handles remote access automatically
- Ensure Plex server allows remote connections
- Consider connection timeouts for slower networks

---

*This documentation covers the new OAuth implementation available in recent Bazarr versions. For older versions, only API key authentication is available.*
