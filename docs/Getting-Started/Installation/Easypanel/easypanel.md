# Easypanel

[Easypanel](https://easypanel.io/) is a self-hosted Docker deployment platform. It has a one-click Easypanel template for Bazarr, which runs the `linuxserver/bazarr` image with persistent volumes for `/config`, `/movies` and `/tv` configured automatically.

[![Deploy on Easypanel](https://easypanel.io/img/deploy-on-easypanel-40.svg)](https://easypanel.io/templates/bazarr)

!!! info
    Bazarr does not scan the disk to detect series and movies - point the `/movies` and `/tv` volumes at libraries already managed by Sonarr/Radarr.
