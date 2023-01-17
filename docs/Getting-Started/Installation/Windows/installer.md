# Bazarr Installer

To install Bazarr on Windows 8 or greater, just use our automated installer: [Bazarr installer](https://github.com/bazarr/bazarr.github.io/releases/latest/download/bazarr.zip)

!!! Attention
    **Please keep in mind that, by default, the Bazarr service will run under Local System account that won't be able to access network shares. You need to change the account used for Bazarr service in `services.msc` console.**

!!! info
    If you install Bazarr in the `Program Files` directory, the account under which it runs **must have administrative privileges** for Bazarr to be able to update itself.

Bazarr settings, logs and db are stored in `C:\ProgramData\Bazarr`. Keep in mind you'll need to change permission on this directory if you change Bazarr service account to something else than System.

The start menu shortcut (it opens the web UI) won't work anymore if you change Bazarr listening port or IP address.

Bazarr installed through this installer won't update from any other branch other than master. If you've hard coded something else in `config.ini`, you must change it back to `master`.
