# FreeBSD

1. Install the required software:

    ```bash
    pkg update && pkg install bazarr
    ```

    If you want to use the latest beta use

    ```bash
    pkg update && pkg install bazarr-devel
    ```

1. Enable bazarr during startup

    ```bash
    echo 'bazarr_enable="YES"' >> /etc/rc.conf
    ```

1. Start bazarr

    ```bash
    /usr/local/etc/rc.d/bazarr start
    ```
