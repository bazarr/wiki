# Linux

## (Ubuntu / Debian) Install requirements with

  ```bash
  apt-get install python3-dev python3-pip python3-distutils
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

1. Upgrade Python to version 3.7 or greater.
1. Download latest release of Bazarr [here](https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip)
1. Extract the content of the zipped release to the previously created `bazarr` directory
1. Install Python requirements using:

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

1. Change ownership to your preferred user for running programs (replace both instances of `$user`)

    ```bash
    sudo chown -R $user:$user /opt/bazarr
    ```

1. You can now start bazarr using the following command:

    ```python
    python3 bazarr.py
    ```

1. systemctl auto start service using virtual env
   1. setup virtual env
      * `sudo apt install python3-virtualenv` install virtual env
      * `sudo virtualenv -p python3 /opt/venv/bazarr`
      * `sudo unzip ~/Downloads/bazarr.zip -d /opt/venv/bazarr/install` unzip download to install location
      * `sudo chown -R bazarr:bazarr /opt/venv/bazarr/install` setup permissions
      * `cd /opt/venv/bazarr`
      * `source bin/activate` activate venv
      * (venv) `cd install`
      * (venv) `sudo python3 -m pip install -r requirements.txt` install requirements
      * (venv) `python3 bazarr.py` run bazarr in virtual env
   1. setup service to auto start
      * `sudo touch /etc/systemd/system/bazarr.service` init service file
      * populate contents with
      * ```properties
        [Unit]
        Description=Bazarr Daemon
        After=syslog.target network.target

        [Service]
        User=bazarr
        Group=bazarr
        UMask=0003

        Type=simple
        WorkingDirectory=/opt/venv/bazarr
        ExecStart=/opt/venv/bazarr/bin/python3 /opt/venv/bazarr/install/bazarr.py
        Restart=on-abort

        [Install]
        WantedBy=multi-user.target
        ```
      * `systemctl status bazarr` check service status
      * `systemctl restart bazarr` start/ restart bazarr, run status and check for errors to fix and try again.

1. Open your browser and go to [http://localhost:6767/](http://localhost:6767/)
