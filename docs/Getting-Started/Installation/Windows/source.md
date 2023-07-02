# Run from source

bazarr requires Python 3.7 or greater and can be run from source.

1. Install Python 3.7 or greater (Till [Python 3.8.6](https://www.python.org/downloads/release/python-386/) Tested) from [this link](https://www.python.org/downloads/release/python-386/) and make sure to check the box to have Python directory added to the system path variable.
1. Open up CMD and go to the folder you want to install bazarr.

    !!! Attention
        Do not use `C:\Program Files` or `C:\Program Files (x86)` as you could run into strange issues. Something like `C:\bazarr` is a better choice.

1. Download the latest release of Bazarr [here](https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip)
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

Please see the [autostart page](../../Autostart/Windows/windows.md) for service installation instructions.
