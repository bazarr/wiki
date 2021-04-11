# MacOS

bazarr requires Python 3.7.x or 3.8.x (**system Python 2.7.10 not supported**) and can be run from source.

1. Install Python from [this link](https://www.python.org/ftp/python/3.8.6/python-3.8.6-macosx10.9.pkg)
1. Open Terminal
1. Change Directory To Applications:

    ```bash
    cd /Applications
    ```

    and create a `bazarr` directory.
1. Download latest release of Bazarr [here](https://github.com/morpheus65535/bazarr/archive/refs/heads/master.zip)
1. Extract the content of the zipped release to the previously created `bazarr` directory
1. Change Directory To bazarr:

    ```bash
    cd bazarr
    ```

1. Install bazarr requirements:

    ```bash
    python3.8 -m pip install -r requirements.txt
    ```

1. Run bazarr:

    ```bash
    python3.8 bazarr.py
    ```

bazarr will run in this Terminal session. Closing the session will stop bazarr. You can start it up again using 3, 4, 6, 7 and 8.

Access bazarr via browser at [https://localhost:6767/](https://localhost:6767/)
