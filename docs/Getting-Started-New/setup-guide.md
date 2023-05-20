# Setup Guide

In this guide we go over essential settings for running `Bazarr` with `Radarr` / `Sonarr`:

* we connect the services
* we define a language profile

!!! info 
    It's assumed that both services run with their default configuration.

    [comment]: <> (add extra pages going more in-depth in regards to caveats when using Docker, reverse Proxies, SSL)

[For a complete documentation of all settings, read this page.](link-to-docs)

After installing Bazarr, open [http://0.0.0.0:6767](http://0.0.0.0:6767).

## Service Configuration
=== "Sonarr"

    The default configuration of Bazaar matches the default Sonarr settings.

    1. Navigate to `Settings` > `Sonarr`
    1. Enable the `Use Sonarr` toggle.
    1. Enter you Sonarr API key into the `API key` field.
    1. Test your configuration.
    1. Hit `Save`.

=== "Radarr"

    The default configuration of Bazaar matches the default Sonarr settings.

    1. Navigate to `Settings` > `Radarr`
    1. Enable the `Use Radarr` toggle.
    1. Enter you Radarr API key into the `API key` field.
    1. Test your configuration.
    1. Hit `Save`.



## Setting a Language Profile

Bazarr starts searching for matching subtitles once a language profile is assigned to a movie / series.

[comment]: <> (What about new movies, do they really need to be added manually?)


1. Create a language profile.
1. Open Bazarr on [http://0.0.0.0:6767/](http://0.0.0.0:6767/) and navigate to `movies` / `series`.
1. Select `Mass Edit` and select either all or individual movies.
1. Select your language profile in the `Change Profile` dropdown menu.
1. Hit `Save`.

You are done! For all checked movies / shows subtitles are searched and downloaded according to your settings.