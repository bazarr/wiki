# FreeBSD

Instruction as provided by @Derkades and fixes by @sindreruud

!!! info "Disclaimer"
    I don't know how rc.d works so the script is pretty crappy and doesn't have start/stop/status functionality. It only starts the program on startup, which was enough for me.

1. Install the required software:

    ```bash
    pkg update && pkg install python3 py3X-pip py3X-libxml2 libxslt py3X-sqlite3 unrar ffprobe
    ```

    !!! note
        Where `py3X` must be replaced with the Python3 version you use for Bazarr.

1. Goto your share folder

    ```bash
    cd /usr/local/share
    ```

1. Create a `bazarr` directory
1. Download latest release of Bazarr [here](https://github.com/morpheus65535/bazarr/archive/refs/heads/master.zip)
1. Extract the content of the zipped release to the previously created bazarr directory

1. change directory

    ```bash
    cd bazarr
    ```

1. Install Python requirements using:

    ```bash
    python3 -m pip install -r requirements.txt
    ```

1. Check if it works:

    ```bash
    python3 bazarr.py
    ```

   You should see `BAZARR is started and waiting for request on http://0.0.0.0:6767/`
