# Webhooks

Webhooks can be configured to POST a JSON payload to the specified URL on multiple events.

## Where can I find the Bazarr API Key

You can find your Bazarr API KEY in `Settings` => `General` => `Security`

![!Bazarr API KEY](images/bazarr-api-key.png)

### Plex

!!! note

    Keep in mind Plex Webhooks is a Plex Pass perk

The URL provided will filter out events and if it got a `media.play` or `media.resume` event, it will search for missing subtitles for the episode or movie being played. you'll have to stop it and resume it for Plex to update the available subtitles.

Windows => `http://localhost:6767/api/webhooks/plex?apikey=YOUR_BAZARR_API_KEY`
Docker => `http://bazarr:6767/api/webhooks/plex?apikey=YOUR_BAZARR_API_KEY`

#### How to setup in Plex

`Settings` => `Webhooks` => click on `ADD WEBHOOK`

On the top right click on the ![plex-settings-icon](images/plex-settings-icon.png) `Settings` icon, and on the left sidebar select `Webhooks`
Click on the middle of the screen on ![plex-webhook-icon](images/plex-webhook-icon.png) and add the following info.

![!plex-settings-webhook](images/plex-settings-webhook.png)

1. Add the above URL.
2. Click on `SAVE CHANGES`
