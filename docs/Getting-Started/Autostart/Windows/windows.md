# Windows

!!! info

    If you used the installer then it's already set to autostart.

    If you installed it in another way in windows you can use the following.

## Installing Bazarr as a Windows Service using NSSM

1. Download the latest NSSM from <https://nssm.cc/download>. It is recommended to grab the prerelease due to a slight issue with the Windows 10 Creators Update.

1. Either place the downloaded NSSM binary in `C:\Windows\System32`, or add it to your `PATH`. This is to allow you to use NSSM from any location.

1. Run `cmd` as an Administrator and use the command `nssm install bazarr`

1. A GUI should pop up. Use the following configuration

    - Path: Should be the location to your Python 3.8 executable. Example: `C:\Python38\python.exe`
    - Startup Directory: Should be the location of your `Python38` folder. Example: `C:\Python38`
    - Arguments: Should be the location of your `bazarr.py` file. Example: `C:\bazarr\bazarr.py`

    !!! Warning
        Please note that running Bazarr from the `Program Files` or `Program Files (x86)` directories may cause issues.

1. Under `Process Tab`, make sure to uncheck `Console Windows`.

1. Click the `Install Service` button

1. Use the command `nssm start bazarr` to initiate bazarr. It should autostart going forward.

    - `nssm edit bazarr` will open up the GUI for further edits.
    - `nssm restart bazarr` will restart bazarr.
    - `nssm stop bazarr` will stop bazarr.
    - `nssm remove bazarr` will remove the Windows Service.

!!! note

    This guide will work in essence for any Python script, and you can use NSSM to run most things as Windows Services through some tweaking of this overall config.
