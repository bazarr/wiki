# Docker

!!! WARNING

    **You CANNOT store your config directory over an NFS share as it is unsupported by SQLITE. You'll face a locked database error.**

Feel free to use any of the following well maintained images, in no particular order:

## [hotio/bazarr](https://hub.docker.com/r/hotio/bazarr)

- Maintained by: [hotio](https://hotio.dev/containers/bazarr/)
- Available tags: `latest` (=`stable`), `nightly`
- Versioned tags: `stable-0.8.3.4`, `nightly-0.9.4-beta.18`
- Updates: `every 30 minutes for apps and every hour for upstream image updates`
- Configuration files for Bazarr are stored in `/config`.

```bash
hotio/bazarr:latest
```

## [linuxserver/bazarr](https://hub.docker.com/r/linuxserver/bazarr)

- Maintained by: [linuxserver](https://github.com/linuxserver)
- Available tags: `latest` and `development`
- Versioned tags: `v0.8.3.4-ls59` and `600ef3ab-ls62`
- Updates: `regular and timely application updates`

- Configuration files for Bazarr are stored in `/config`.

!!! info
    For more info on how to configure the images, info about their used tags and their correlation to bazarr branches, visit their respective Docker pages.

```bash
linuxserver/bazarr
```