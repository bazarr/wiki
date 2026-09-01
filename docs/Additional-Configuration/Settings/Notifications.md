# Notification Settings

Set up notification channels and configure when notifications are sent.

## Notification Channels

Configure where notifications are sent. Bazarr supports notifications powered by [Apprise](https://github.com/caronc/apprise).

Available channels include:

- **Email**: SMTP email notifications
- **Discord**: Discord channel or DM webhooks
- **Slack**: Slack channel or bot messages
- **Telegram**: Telegram bot messages
- **Pushbullet**: Mobile push notifications
- **XMPP**: Jabber/XMPP messaging
- **Windows Notifications**: Native Windows alerts
- **And many more...**: See [Apprise Wiki](https://github.com/caronc/apprise/wiki) for full list

## Configuration

For detailed configuration instructions for your specific notification service, refer to the [Apprise Wiki](https://github.com/caronc/apprise/wiki).

Advanced usage patterns are covered in the [Custom Notifications guide](../Custom-Notifications.md).

## Options

**Silent for Manual Actions**

Suppress notifications when you manually download or upload subtitles from the UI.

!!! note

    - **Effect**: Only automatic searches/downloads trigger notifications
    - **Use Case**: Reduce notification noise from manual operations

**Notify When There Are No Missing Subtitles**

Send notifications even when a synced episode/movie already has all required subtitles.

!!! note

    - **Default**: Off
    - **Use Case**: Useful for workflow confirmation that Bazarr processed the content
