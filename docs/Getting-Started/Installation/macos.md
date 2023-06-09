# MacOS

## Requirements and dependencies

- Python >= 3.7

[comment]: <> (Is this up to date? Is true for Python > 3.8?)

## Installation

1. Login as admin user.
1. Install Python from [this link](https://www.python.org/ftp/python/3.8.6/python-3.8.6-macosx10.9.pkg)

[comment]: <> (Does Python have to be this specific version or is this outdated?)

1. Download latest release of Bazarr from [this link](https://github.com/morpheus65535/bazarr/releases/latest/download/bazarr.zip) and expand the downloaded .zip file
1. Place the resulting `bazarr` directory in your `/Applications` directory
1. Open Terminal and change directory to `/Applications/bazarr`:

   ```shell
   cd /Applications/bazarr
   ```

1. Install Bazarr requirements:

   ```shell
   python3.8 -m pip install -r requirements.txt
   ```

1. Run Bazarr:

   ```shell
   python3.8 bazarr.py
   ```

Bazarr will run in this Terminal session. Closing the session will stop Bazarr. You can start it up again using steps 5 and 7.

Access Bazarr via browser at [http://localhost:6767/](http://localhost:6767/)

## Autostart

### LaunchAgent on MacOS

From terminal:

```shell
cd /Users/<user name>/Library/LaunchAgents
nano com.github.morpheus65535.bazarr.plist
```

Paste the following into the document and change the location of the logs to `/Users/<user_name>/`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>com.github.morpheus65535.bazarr</string>
    <key>ProgramArguments</key>
    <array>
      <string>/usr/local/bin/python3.8</string>
      <string>/Applications/bazarr/bazarr.py</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
    <key>StandardErrorPath</key>
    <string>/usr/local/var/log/bazarr.log</string>
    <key>StandardOutPath</key>
    <string>/usr/local/var/log/bazarr.log</string>
  </dict>
</plist>
```

1. Save the document. You should receive a notification that “\_Software from ‘Ned Daily’ added items that can run in the background”. This is totally normal.

1. To verify everything works, run to start Bazarr:

   ```shell
   launchctl load /Users/<user name>/Library/LaunchAgents/com.github.morpheus65535.bazarr.plist
   ```

1. Run to stop Bazarr:

   ```shell
   launchctl unload /Users/<user name>/Library/LaunchAgents/com.github.morpheus65535.bazarr.plist
   ```

### Create the App

1. Go to _Launchpad_ and open _Automator_
1. Create a new document
1. Choose type _Application_
1. In the search bar, search for _Shell_
1. Choose _Run Shell Script_
1. Remove the contents of the shell script and paste:

   ```shell
   launchctl load /Users/<user name>/Library/LaunchAgents/com.github.morpheus65535.bazarr.plist
   ```

1. Go to the _File_ menu and choose _Save_
1. Name the file _Bazarr.app_ and save it in _Applications_

### Change the App icon

1. Copy the Bazarr icon from GitHub [repository](https://raw.githubusercontent.com/morpheus65535/bazarr/master/frontend/public/images/logo128.png)
1. In Finder, go to Applications
1. Right click on Bazarr.app and choose Get Info
1. Click on the robot icon
1. Paste the picture you want for the new icon
1. Close the info window

### Launch Bazarr at start

1. Open `System Settings`
1. Go to `General` > `Login Items`
1. At the bottom of the `Open at Login` section, click on the `+`
1. Choose `Applications` > `Bazarr.app` and click `Open`
1. Restart the computer and open `http://localhost:6767` in your browser to test
