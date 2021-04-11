# MacOS

## LaunchAgent on MacOS

As-is, the LaunchAgent expects bazarr to be cloned or installed at `/Applications/bazarr`. If this is counter to other documentation I recommend amending the file contents.

The LaunchAgent should be named `com.github.morpheus65535.bazarr.plist` -  again, if you'd like something else, update the Label in the file itself as well.

The file is installed to `/Library/LaunchAgents` and the service will start when the user logs into the system. After installation, the service can be started immediately by running `launchctl load /Library/LaunchAgents/com.github.morpheus65535.bazarr.plist`. The service can be stopped by running the same command replacing load with unload.

Make sure that owner and permissions are properly defined on the plist:

```bash
sudo chown root:wheel /Library/LaunchAgents/com.github.morpheus65535.bazarr.plist
sudo chmod -R 0644 /Library/LaunchAgents/com.github.morpheus65535.bazarr.plist
```

Logs are written to `/usr/local/var/log/bazarr.log`.

Here's the file:

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
