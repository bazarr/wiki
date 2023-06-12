# Linux

## Requirements and dependencies

- Python >= 3.7

=== "Ubuntu"

    ```shell
    apt-get install python3-dev python3-pip python3-distutils
    apt-get install unzip curl
    ```

=== "Fedora / CentOS"

    ```shell
    yum install python3-devel python3-pip python3-distutils
    ```

=== "Raspbian"

    ```shell
    sudo apt-get update
    sudo apt-get install libxml2-dev libxslt1-dev python3-dev python3-libxml2 python3-lxml unrar-free ffmpeg libatlas-base-dev
    ```

## Installation

!!! info
In this guide you are installing Bazarr in `/opt/bazarr` for the user `bazarr` and the group `media`.

    Change this configuration accordingly.

1. Create the `bazarr` directory and grant permissions for `bazarr` user and `media` group:

   ```shell
   sudo mkdir /opt/bazarr
   sudo chown -R bazarr:media /opt/bazarr
   ```

1. Download bazarr.zip and extract the contents:

   ```shell
   curl -L "https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip"
   sudo unzip bazarr.zip -d /opt/bazarr

   ```

1. Install Python requirements:

   ```shell
   python3 -m pip install -r /opt/bazarr/requirements.txt
   ```

1. You can now start Bazarr manually using the following command:

   ```shell
   python3 /opt/bazarr.py
   ```

1. To access the Bazarr UI, open your browser and go to [http://localhost:6767/](http://localhost:6767/).

Congratulations - you now have a working installation of Bazarr!

## Autostart

=== "Systemd"

    1. Create a `bazarr.service` file:

        You have to create a `bazarr.service` file in `/etc/systemd/system` that would contain the following text:

        ```php
        [Unit]
        Description=Bazarr Daemon
        After=syslog.target network.target

        # After=syslog.target network.target sonarr.service radarr.service

        [Service]
        WorkingDirectory=/opt/bazarr/
        User=bazarr
        Group=media
        UMask=0002
        Restart=on-failure
        RestartSec=5
        Type=simple
        ExecStart=/usr/bin/python3 /opt/bazarr/bazarr.py
        KillSignal=SIGINT
        TimeoutStopSec=20
        SyslogIdentifier=bazarr
        ExecStartPre=/bin/sleep 30

        [Install]
        WantedBy=multi-user.target
        ```

    2. Start the service:

        ```shell
        sudo systemctl start bazarr
        ```

    3. Check if the service is running:

        ```shell
        sudo systemctl status bazarr
        ```

    4. If it's running without errors enable the service:

        ```shell
        sudo systemctl enable bazarr
        ```

=== "Upstart"

    ```shell
    description "Upstart Script to run Bazarr as a service on Ubuntu/Debian based systems, as well as others"
    author "A Bazarr User"

    #Set user and group for the process if desired
    #setuid myUserID
    #setgid myGroupID

    #start after all services come up
    start on runlevel [2345]
    stop on runlevel [016]

    # Automatically restart process if crashed

    respawn

    # Make sure script is started with system locale

    script
    if [ -r /etc/default/locale ]; then
        . /etc/default/locale
        export LANG
    fi
    exec python /opt/bazarr/bazarr.py
    end script

    ```
