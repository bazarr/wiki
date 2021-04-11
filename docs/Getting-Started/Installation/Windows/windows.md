# Windows

## Bazarr Installer

To install Bazarr on Windows 7 or greater, just use our automated installer: [Bazarr installer](https://github.com/bazarr/bazarr.github.io/releases/latest/download/bazarr.zip)

!!! Attention
    **Please keep in mind that, by default, the Bazarr service will run under Local System account that won't be able to access network shares. You need to change the account used for Bazarr service in `services.msc` console.**

!!! info
    If you install Bazarr in the `Program Files` directory, the account under which it runs **must have administrative privileges** for Bazarr to be able to update itself.

Bazarr settings, logs and db are stored in `C:\ProgramData\Bazarr`. Keep in mind you'll need to change permission on this directory if you change Bazarr service account to something else than System.

The start menu shortcut (it opens the web UI) won't work anymore if you change Bazarr listening port or IP address.

Bazarr installed through this installer won't update from any other branch other than master. If you've hard coded something else in `config.ini`, you must change it back to `master`.

## Run from source

bazarr requires Python 3.7 or greater and can be run from source.

1. Install Python 3.7 or greater (Till [Python 3.8.6](https://www.python.org/downloads/release/python-386/) Tested) from [this link](https://www.python.org/downloads/release/python-386/) and make sure to check the box to have Python directory added to the system path variable.
1. Open up CMD and go to the folder you want to install bazarr.

    !!! Attention
        Do not use `C:\Program Files` or `C:\Program Files (x86)` as you could run into strange issues. Something like `C:\bazarr` is a better choice.

1. Download latest release of Bazarr [here](https://github.com/morpheus65535/bazarr/archive/refs/heads/master.zip)
1. Extract the content of the zipped release to the previously created `bazarr` directory
1. Go to the bazarr folder:

    ```bash
    cd bazarr
    ```

1. Install Python requirements using:

    ```bash
    pip install -r requirements.txt
    ```

1. You can now start bazarr with the following command:

    ```bash
    python bazarr.py
    ```

1. Open your browser and go to [http://localhost:6767/](http://localhost:6767/)
