# Windows

You can install Bazarr either by using the automated installer or by running it from source.

=== "Installer"

    Installing and running Bazarr comes with some caveats:

    !!! warning
        1. If you install Bazarr in the `Program Files` directory, the account under which it runs **must have administrative privileges** for Bazarr to be able to update itself.
        2. Bazarr is running as a Windows service. **Microsoft Windows doesn't support using mapped network drives for process running as a service.** You must either switch to UNC path for your Sonarr/Radarr root folders or use Bazarr path mapping to circumvent that.
        3. **Please keep in mind that, by default, the Bazarr service will run under Local System account that won't be able to access network shares. You need to change the account used for Bazarr service in `services.msc` console.**
        4. The start menu shortcut (it opens the web UI) won't work anymore if you change Bazarr listening port or IP address.
        5. Bazarr settings, logs and db are stored in `C:\ProgramData\Bazarr`. Keep in mind you'll need to change permission on this directory if you change Bazarr service account to something else than System.

    To install Bazarr on Windows 8 or greater, just use our automated installer: [Bazarr installer](https://github.com/bazarr/bazarr.github.io/releases/latest/download/bazarr.zip)

    ## Autostart

    When using the automated installer Bazarr will autostart per default.

=== "Run from source"

    ## Requirements

    * Python >= 3.7
    [comment]: <> (Is Python <= 3.8.6 outdated or still an active requirement?)

    ## Installation

    1. Make your that Python directory is included in the system PATH variable.
    1. Download the latest release of Bazarr [from here](https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip).
    1. Create a directory for your Bazarr installation.
        ```cmd
        mkdir C:\bazaar
        ```

        [comment]: <> (Why shouldnt you install to `C:\Program Files` or `C:\Program Files (x86)`?)
    1. Extract `bazarr.zip`s contents to the newly created directory.
    1. Open CMD, navigate to your Bazarr folder and install the Python requirements:

        ```shell
        cd bazarr
        pip install -r requirements.txt

        ```
    1. Start Bazarr:

        ```shell
        python bazarr.py
        ```

    1. Open your browser and go to [http://localhost:6767/](http://localhost:6767/).


    ## Autostart

    1. Download the latest NSSM from <https://nssm.cc/download>. It is recommended to grab the prerelease due to a slight issue with the Windows 10 Creators Update.
    [comment]: <> (What issue?)

    1. Either place the downloaded NSSM binary in `C:\Windows\System32`, or add it to your `PATH`. This is to allow you to use NSSM from any location.

    1. Run `cmd` as an Administrator and use the command `nssm install bazarr`

    1. A GUI should pop up. Use the following configuration

        - Path: Should be the location to your Python 3.8 executable. Example: `C:\Python38\python.exe`
        - Startup Directory: Should be the location of your `Python38` folder. Example: `C:\Python38`
        - Arguments: Should be the location of your `bazarr.py` file. Example: `C:\bazarr\bazarr.py`

        !!! Warning
            Please note that running Bazarr from the `Program Files` or `Program Files (x86)` directories may cause issues.
        [comment]: <> (Again, what issues?)

    1. Under `Process Tab`, make sure to uncheck `Console Windows`.

    1. Click the `Install Service` button

    1. Use the command `nssm start bazarr` to initiate bazarr. It should autostart going forward.

        - `nssm edit bazarr` will open up the GUI for further edits.
        - `nssm restart bazarr` will restart bazarr.
        - `nssm stop bazarr` will stop bazarr.
        - `nssm remove bazarr` will remove the Windows Service.
