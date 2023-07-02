# Linux

!!! attention
    **In any case, the user must have a home directory!!!**

## systemd service file for Debian Ubuntu

!!! note
    **Be aware that this is provided as-is without any support from the team.**

This is a systemd service file created by users of Bazarr. It assumes you've installed Bazarr in: `/opt/bazarr`.

You have to create a `bazarr.service` file in `/etc/systemd/system` that would contain the following text:

```php
[Unit]
Description=Bazarr Daemon
After=syslog.target network.target

# After=syslog.target network.target sonarr.service radarr.service

[Service]
WorkingDirectory=/opt/bazarr/
User=your_user(username of your choice)
Group=your_group(group of your choice)
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

Start the service using `sudo systemctl start bazarr`

Check if the service is running using `sudo systemctl status bazarr`

If it's running without errors then you need to enable the service using `sudo systemctl enable bazarr`

## Upstart script for Debian Ubuntu

This is an init upstart file. It assumes you've installed Bazarr in: `/opt/bazarr`

You have to create a `bazarr.conf` file in `/etc/init/` (`sudo nano /etc/init/bazarr.conf`) that would contain the following text:

```php
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
