# Bazarr Installer

To install Bazarr on Windows 8 or greater, just use our automated installer: [Bazarr installer](https://github.com/bazarr/bazarr.github.io/releases/latest/download/bazarr.zip)

!!! Attention
    **Please keep in mind that, by default, the Bazarr service will run under Local System account that won't be able to access network shares. You need to change the account used for Bazarr service in `services.msc` console or `nssm edit`.** Once that's done, delete `%PROGRAMDATA\Bazarr` and restart the service (via the console or `nssm restart`.

!!! warning
    Bazarr is running as a Windows service. **Microsoft Windows doesn't support using mapped network drives for process running as a service.** You must either switch to UNC path for your Sonarr/Radarr root folders or use Bazarr path mapping to circumvent that.

!!! info
    If you install Bazarr in the `Program Files` directory, the account under which it runs **must have administrative privileges** for Bazarr to be able to update itself.

Bazarr settings, logs and db are stored in `C:\ProgramData\Bazarr`. Keep in mind you'll need to change permission on this directory if you change Bazarr service account to something else than System.

The start menu shortcut (it opens the web UI) won't work anymore if you change Bazarr listening port or IP address.
