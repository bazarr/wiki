# Synology

It's recommended to run Bazarr with Docker in case your NAS supports it. 

A package is also available from SynoCommunity <https://synocommunity.com/package/bazarr>.


[comment]: <> ("If you decide to mix packages with docker you will need to mess with [Path Mappings](/Additional-Configuration/Settings/#path-mappings)." What does this exactly mean, running Sonarr/Radarr in Docker as well?)

## PUID and PGID

In order for the Docker container to access the shares on the Synology, you need to give it the same permissions as your main user who has access to those shares.
For this you need to find the PUID and PGID of the user having access to your shares.

You will need to SSH into your Synology. Here's how to enable SSH:

1. Open `Control Panel` > `Terminal & SNMP` > `Terminal`
1. Tick `Enable SSH service`

Then use a program like Putty and SSH to your Synology:

1. Login if you get a popup asking if you want to trust the key, press `OK` or `ACCEPT`.

1. Enter the login information of your main Synology user account.

1. Once logged in type:

    ```shell
    id
    ```

This will show your UID / PUID (`1026` in the screenshot) and your GID / PGID (`101` in the screenshot) for you admin.


!!! Info
    *yes we know it's not recommended to use the admin account but if you already know this then you wouldn't need to read this* ;)

[comment]: <> (Why is it not recommended to use the admin account? Is there an alternative?)

## Installation

=== "Synology GUI"

    1. Install Docker on the Synology device.
    1. Open Docker, navigate to `Registry` and search for bazarr.
    1. Select either the `linuxserver/bazarr` or `hotio/bazarr` image.
        1. In the popup, select `latest` for the stable build or `nightly` for the dev build. 
        1. Confirm your choice with `Select`.
    1. Nagivate to `Image` and wait for the build to finish.
    1. Configure your container:

        **General**:

        - Container name: bazaar
        - Execute container using high privilege: Ticked
        [comment]: <> ("not sure if it's needed but you can test that later yourself" Is it really needed?)
        
        **Advanced Settings**:

        - Enable auto-restart: Tick

        **Advanced Settings > Volume**:

         - Add folder: Create and select the directory `docker/config/bazarr` .

        This will be used for the database, config and log files.
        **Also add your `tv` and your `movies` folder locations,
        In this Guide we used the preferred path setup that's why we used `/data/media`.**

        **Advanced Settings > Port Settings**:

        - Local port: Set to 6767

        **Advanced Settings > Environment**:

        - PUID: Your PUID
        - PGID: Your PGID

    1. Click `Apply` and `Next`.
    1. Check your settings and click `Apply`.
    1. Select `Container` and check if your container state is `Running`.

    Now you can access the Bazarr docker container by typing in your browser
    <http://your_synology_ip_or_your_synology_hostname:6767>
    and then follow the [Setup-Guide](/Getting-Started/First-time-installation-configuration/).


=== "SSH"

    First create a `config` folder in your `docker`  folder and create a `bazarr` folder in it.
    Then you ssh into your Synology and you type one of the the following depending which image you want to use.

    Parameters:

    - `--name bazarr` = Container name.
    - `-v /volume1/docker/config/bazarr:/config` = Your path to your config location.
    - `-v /volume1/data/media:/data/media` = Your path to your tv shows/series and movies.
    - `-e PUID=1026` = Your PUID (*that we found earlier*).
    - `-e PGID=101` = Your PGID (*that we found earlier*).
    - `-p 6767:6767` = The ports Bazarr is going to use.
    - `hotio/bazarr:xxx` = Which image and build is going to be used.

    === "Stable build"

        hotio/bazarr

        ```bash
        sudo docker run -d \
            && --name bazarr \
            && -v /volume1/docker/config/bazarr:/config  \
            && -v /volume1/data/media:/data/media \
            && -e PUID=1026 \
            && -e PGID=101 \
            && -p 6767:6767 \
            && hotio/bazarr:latest
        ```

        linuxserver/bazarr

        ```bash
        sudo docker run -d \
            && --name bazarr \
            && -v /volume1/docker/config/bazarr:/config \
            && -v /volume1/data/media:/data/media \
            && -e PUID=1026 \
            && -e PGID=101 \
            && -p 6767:6767 \
            && linuxserver/bazarr:latest
        ```

    === "Development build"

        hotio/bazarr

        ```bash
        sudo docker run -d \
            && --name bazarr \
            && -v /volume1/docker/config/bazarr:/config  \
            && -v /volume1/data/media:/data/media \
            && -e PUID=1026 \
            && -e PGID=101 \
            && -p 6767:6767 \
            && hotio/bazarr:nightly
        ```

        linuxserver/bazarr

        ```bash
        sudo docker run -d \
            && --name bazarr \
            && -v /volume1/docker/config/bazarr:/config \
            && -v /volume1/data/media:/data/media \
            && -e PUID=1026 \
            && -e PGID=101 \
            && -p 6767:6767 \
            && linuxserver/bazarr:nightly
        ```
=== "Docker compose"

    [Install Bazarr and the rest with docker compose](https://trash-guides.info/Misc/how-to-set-up-hardlinks-and-atomic-moves/)
    
------

For his Wiki/Guide I used the following sources being that I don't have a Synology myself.

[Dr_Frankenstein's Tech Stuff](https://drfrankenstein.co.uk/2017/03/28/setting-up-radarr-in-docker-on-a-synology-nas/)

[NAS Guides](https://nasguides.wordpress.com/)

[DSM 6.2 Online Demo](https://demo.synology.com/en-global/dsm)

[Docker Guide](https://wiki.servarr.com/Docker_Guide) (thnx to @fryfrog)

Help from some Synology users on the [Discord Server](https://discord.gg/MH2e2eb).
