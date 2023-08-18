# MacOS

bazarr requires Python 3.8 or greater and can be run from source.

1. Make sure you are logged in to your Mac as an admin user
1. Install Python from [this link](https://www.python.org/ftp/python/3.8.6/python-3.8.6-macosx10.9.pkg)
1. Download latest release of Bazarr from [this link](https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip) and expand the downloaded .zip file
1. Place the resulting `bazarr` directory in your /Applications directory
1. Open Terminal and change directory to `/Applications/bazarr`:

    ```bash
    cd /Applications/bazarr
    ```

1. Install bazarr requirements:

    ```bash
    python3.8 -m pip install -r requirements.txt
    ```

1. Run bazarr:

    ```bash
    python3.8 bazarr.py
    ```

bazarr will run in this Terminal session. Closing the session will stop bazarr. You can start it up again using steps 5 and 7.

Access bazarr via browser at [http://localhost:6767/](http://localhost:6767/)

Please see the [autostart page](../../Autostart/MacOS/macos.md) for launch agents configuration instructions.
