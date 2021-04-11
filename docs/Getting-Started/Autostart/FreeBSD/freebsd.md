# FreeBSD

## Starting on boot

- A user needs to be created for Bazarr to run as a daemon, the script is written for a user called bazarr but this can be changed by modifying `bazarr_user="bazarr"` in `/etc/rc.conf`
- Change permissions on Bazarr directory by running the following command: `chown -R bazarr_user /usr/local/share/bazarr` where bazarr_user is the user you've created.
- `nano /usr/local/etc/rc.d/bazarr`
- Enter this:

```bash
#!/bin/sh
# PROVIDE: bazarr
# REQUIRE: LOGIN
# KEYWORD: shutdown

# Add the following lines to /etc/rc.conf to enable bazarr:
# bazarr_enable="YES"
# Optionally add:
# Note: The bazarr_user must be unique as the stop_postcmd kills the other running process
# bazarr_user="bazarr"

. /etc/rc.subr

name="bazarr"
rcvar=bazarr_enable

load_rc_config $name

: ${bazarr_enable="NO"}
: ${bazarr_user:="bazarr"}
: ${bazarr_data_dir:="/usr/local/bazarr"}

pidfile="/usr/local/share/bazarr/bazarr.pid"
procname="/usr/local/bin/python3"
command="/usr/sbin/daemon"
command_args="-f -p ${pidfile} ${procname} /usr/local/share/bazarr/bazarr.py"

start_precmd=bazarr_precmd
stop_postcmd=bazarr_postcmd

bazarr_precmd()
{
 export XDG_CONFIG_HOME=${bazarr_data_dir}
        export GIT_PYTHON_REFRESH=quiet

 if [ ! -d ${bazarr_data_dir} ]; then
  install -d -o ${bazarr_user} ${bazarr_data_dir}
 fi
}

bazarr_postcmd()
{
 killall -u ${bazarr_user}
}

run_rc_command "$1"
```

- Save and exit (ctrl+x - y - enter)
- Make the file executable `chmod +x /usr/local/etc/rc.d/bazarr`
- `nano /etc/rc.conf`
- On the last line, add bazarr_enable="YES"
- Reboot
