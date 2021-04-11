# Linux

## (Ubuntu / Debian) Install requirements with

  ```bash
  apt-get install python3-pip python3-distutils
  ```

## (Fedora / CentOS) Install requirements with

  ```bash
  yum install python3-pip python3-distutils
  ```

## (Raspbian and maybe other ARM based distro)

thnx to @inquilino for the fixes/updates

  ```bash
  sudo apt-get update
  sudo apt-get install libxml2-dev libxslt1-dev python3-libxml2 python3-lxml unrar-free ffmpeg libatlas-base-dev
  ```

1. Upgrade Python to version 3.7 or greater.
1. Download latest release of Bazarr [here](https://github.com/morpheus65535/bazarr/archive/refs/heads/master.zip)
1. Extract the content of the zipped release to the previously created `bazarr` directory
1. Install Python requirements using:

    ```python
    python3 -m pip install -r requirements.txt
    ```

    !!! note
        (Raspbian) Don't worry about `lxml` not being installed at this step, you have installed the module through `apt-get` anyway.

1. Change ownership to your preferred user for running programs (replace both instances of `$user`)

    ```bash
    sudo chown -R $user:$user /opt/bazarr
    ```

1. You can now start bazarr using the following command:

    ```python
    python3 bazarr.py
    ```

1. Open your browser and go to [http://localhost:6767/](http://localhost:6767/)
