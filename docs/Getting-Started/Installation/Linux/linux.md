# Linux

## (Ubuntu / Debian) Install requirements with

  ```bash
  apt-get install python3-dev python3-pip python3-distutils unrar unzip
  ```

## (Fedora / CentOS) Install requirements with

  ```bash
  yum install python3-devel python3-pip python3-distutils
  ```

## (Raspbian and maybe other ARM based distro)

thnx to @inquilino for the fixes/updates

  ```bash
  sudo apt-get update
  sudo apt-get install libxml2-dev libxslt1-dev python3-dev python3-libxml2 python3-lxml unrar-free ffmpeg libatlas-base-dev
  ```

1. Upgrade Python to version 3.8 or greater.
1. Download latest release of Bazarr [here](https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip)

    ```shell
    wget https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip
    ```

1. Create the `bazarr` directory

    ```shell
    sudo mkdir /opt/bazarr
    ```

1. Extract the content of the zipped release to the previously created `bazarr` directory

    ```shell
    sudo unzip bazarr.zip -d /opt/bazarr
    ```

1. cd into /opt/bazarr
    ```shell
    cd /opt/bazarr
    ```

1. Install the Python requirements:

    ```python
    python3 -m pip install -r requirements.txt
    ```

    !!! note
        (Raspbian) Don't worry about `lxml` not being installed at this step, you have installed the module through `apt-get` anyway.

    !!! note "Older Raspberry Pi (ARMv6)"
        On ARMv6 devices (e.g. older Raspberry Pis, find out with `uname -m`), `numpy` installed from pip might contain instruction set that is not compatible with the architecture ([Ref](https://github.com/morpheus65535/bazarr/issues/1671)). The solution is to replace it with the version from apt repository:

        ```bash
        python3 -m pip uninstall numpy
        sudo apt-get install python3-numpy
        ```

1. Change ownership to your preferred user for running programs (replace both instances of `$USER`, or leave it to change ownership to current user)

    ```bash
    sudo chown -R $USER:$USER /opt/bazarr
    ```

1. You can now start bazarr using the following command:

    ```python
    python3 bazarr.py
    ```

1. Open your browser and go to [http://localhost:6767/](http://localhost:6767/)

Please see the [autostart page](../../Autostart/Linux/linux.md) for systemd/upstart configuration instructions.
