# FreeBSD


## Installation 

1. Make sure you use /latest pkg repository

    * Ensure your pkg repo is configured to get packages from `/latest` and not `/quarterly`
    * Check `/usr/local/etc/pkg/repos/FreeBSD.conf`
    * If that does not exist, copy over `/etc/pkg/FreeBSD.conf` to that location, open it, and replace `quarterly` with `latest`

1. Install the required software:

    ```shell
    pkg update && pkg install bazarr
    ```


[comment]: <> (In the other guides Bazarr is usually ran manually - is there an equivalent in BSD as well? Is the)

## Autostart

1. Enable Bazarr during startup:

    ```shell
    echo 'bazarr_enable="YES"' >> /etc/rc.conf
    ```

1. Start Bazarr

    ```shell
    /usr/local/etc/rc.d/bazarr start
    ```