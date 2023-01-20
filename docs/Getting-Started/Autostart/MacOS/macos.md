# MacOS

Install Bazarr following the [instructions](../../Installation/MacOS/macos)

## LaunchAgent on MacOS

1. From terminal:
```bash
cd /Users/<user name>/Library/LaunchAgents
nano com.github.morpheus65535.bazarr.plist
```
2. Paste the following into the document and change the location of the logs to `/Users/<user_name>/`: 
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
3. Save the document. You should receive a notification that “_Software from ‘Ned Daily’ added items that can run in the background”. This is totally normal.
4. To verify everything works, run to start Bazarr: `launchctl load /Users/<user name>/Library/LaunchAgents/com.github.morpheus65535.bazarr.plist`
5. Run to stop Bazarr: `launchctl unload /Users/<user name>/Library/LaunchAgents/com.github.morpheus65535.bazarr.plist`
6. No more terminal work

## Create the App

1. Go to _Launchpad_ and open _Automator_
2. Create a new document
3. Choose type _Application_
4. In the search bar, search for _Shell_
5. Choose _Run Shell Script_
6. Remove the contents of the shell script and paste: `launchctl load /Users/<user name>/Library/LaunchAgents/com.github.morpheus65535.bazarr.plist`
7. Go to the _File_ menu and choose _Save_
8. Name the file _Bazarr.app_ and save it in _Applications_

## Change the App icon

1. Copy the Bazarr icon from GitHub [repository](https://raw.githubusercontent.com/morpheus65535/bazarr/master/frontend/public/images/logo128.png)
2. In Finder, go to Applications
3. Right click on Bazarr.app and choose Get Info
4. Click on the robot icon
5. Paste the picture you want for the new icon
6. Close the info window

## Launch Bazarr at start

1. Open System Settings
2. Go to General > Login Items
3. At the bottom of the Open at Login section, click on the +
4. Choose Applications > Bazarr.app and click Open
5. Restart the computer and open http://localhost:6767 in your browser to test
