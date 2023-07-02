# FreeBSD

1. Make sure you use /latest pkg repository

    * Ensure your pkg repo is configured to get packages from `/latest` and not `/quarterly`
    * Check `/usr/local/etc/pkg/repos/FreeBSD.conf`
    * If that does not exist, copy over `/etc/pkg/FreeBSD.conf` to that location, open it, and replace `quarterly` with `latest`

1. Install the required software:

    ```bash
    pkg update && pkg install bazarr
    ```

1. Enable bazarr during startup

    ```bash
    echo 'bazarr_enable="YES"' >> /etc/rc.conf
    ```

1. Start bazarr

    ```bash
    /usr/local/etc/rc.d/bazarr start
    ```

Please see the [autostart page](../../Autostart/FreeBSD/freebsd.md) for rc configuration instructions.
