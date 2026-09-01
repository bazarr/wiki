# Settings

This comprehensive guide covers all available settings in Bazarr. Settings are organized into several main categories accessible from the Settings menu.

## Table of Contents

- [Library](#library)
- [Integrations](#integrations)
- [Languages](#languages)
- [Providers](#providers)
- [Subtitles](#subtitles)
- [Notifications](#notifications)
- [Application](#application)

---

## Library

Connect and manage your media libraries through Sonarr (TV series) and Radarr (movies). Configure synchronization, subtitle searching, and path mappings.

**Topics covered:**

- Sonarr connection and synchronization options
- Sonarr path mappings for different file access paths
- Radarr connection and synchronization options
- Radarr path mappings for different file access paths
- Webhook integration for automated subtitle searches
- Monitoring and troubleshooting
- Best practices for large libraries

[View Library Settings →](Settings/Library.md)

---

## Integrations

Configure connections to external media services like Plex and Jellyfin.

**Topics covered:**

- Plex Media Server integration
- Plex library configuration and automation
- Jellyfin integration
- Jellyfin library configuration and automation
- Webhook configuration for media events

[View Integration Settings →](Settings/Integrations.md)

---

## Languages

Configure subtitle languages, create language profiles, and manage language mappings.

**Topics covered:**

- Language selection and filtering
- Embedded track language handling
- Language mappings for provider compatibility
- Language profiles creation and management
- Tag-based profile assignment
- Default profile configuration for new content

[View Language Settings →](Settings/Languages.md)

---

## Providers

Configure subtitle providers, translation services, anti-captcha services, and metadata sources.

**Topics covered:**

- Enabled subtitle providers selection
- Automatic subtitle translation (Google Translate, Gemini, Lingarr)
- Anti-captcha services (Anti-Captcha.com, Death by Captcha, CaptchaAI)
- Metadata provider integration
- Advanced provider settings and SSL configuration

[View Provider Settings →](Settings/Providers.md)

---

## Subtitles

Comprehensive subtitle file management, search optimization, and post-processing.

**Topics covered:**

- Subtitle file storage options
- Embedded subtitle handling
- Search behavior and score thresholds
- Adaptive searching optimization
- Performance optimization
- Audio synchronization (ffsubsync)
- Subtitle content modifications (Sub-Zero)
- Whisper AI fallback
- Custom post-processing scripts

[View Subtitle Settings →](Settings/Subtitles.md)

---

## Notifications

Set up notification channels and configure when notifications are sent using Apprise.

**Topics covered:**

- Notification channel configuration (Email, Discord, Slack, Telegram, etc.)
- Apprise integration and configuration
- Notification filters and options
- Silent mode for manual actions

[View Notification Settings →](Settings/Notifications.md)

---

## Application

Core Bazarr configuration including host settings, security, authentication, proxy, jobs manager, scheduler, UI preferences, and maintenance options.

**Topics covered:**

- Host configuration (address, port, base URL, hostname)
- Security settings (authentication, API keys, CORS)
- Jobs Manager configuration
- Incoming Webhooks
- Proxy configuration
- UI settings
- Scheduler configuration
- Maintenance and logging

[View Application Settings →](Settings/Application.md)

---

## Quick Start Guide

### Initial Setup

1. **Configure Library First** - Connect Sonarr/Radarr (TV and movies)
2. **Test Connections** - Use test buttons to verify connectivity
3. **Select Languages** - Enable the languages you want subtitles for
4. **Create Language Profiles** - Define automatic subtitle selection rules
5. **Enable Providers** - Select multiple subtitle providers for better coverage
6. **Configure Integrations** - Set up Plex/Jellyfin if using them
7. **Save Settings** - Always save after making changes

### Optimization Tips

- **Large Libraries**: Enable Adaptive Searching to reduce API calls
- **Low-Power Devices**: Disable concurrent jobs and parallel provider searches
- **Fast Networks**: Enable simultaneous provider searches
- **Slow Storage**: Enable cached embedded subtitle results
- **Large Series**: Enable "Sync Only Monitored Episodes" to reduce processing load

### Troubleshooting

- **Subtitles Not Found**: Check minimum score, enabled providers, and language profiles
- **Slow Performance**: Reduce concurrent jobs, disable simultaneous provider searches
- **Missed Episodes**: Check Excluded Tags and Series Types in Library settings
- **Wrong Languages**: Verify language profile configuration and enabled languages
- **API Rate Limiting**: Enable Adaptive Searching to reduce provider API calls

### Regular Maintenance

- **Monitor Logs**: Check debug logs for errors (enable Debug mode in Maintenance)
- **Review Backups**: Verify automatic backups are working
- **Test Connections**: Periodically test Sonarr/Radarr connections
- **Update Branch**: Keep Bazarr updated for bug fixes and improvements

---
