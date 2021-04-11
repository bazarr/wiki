# Performance Tuning

We gathered some info from people that use Bazarr on a low powered device like a RaspberryPi or when you have your media stored in the cloud.
And when you got allot of missing Subtitles.

## Disable periodic scan for existing Subtitles

![something](images/image-20200724170813991.png)
`Settings` => `Scheduler` => `Disk Indexing`
Change the settings to Manually to Disable it.
This means that Bazarr won't scan your drive for existing subtitles and only knows about subtitles that Bazarr added !!!

### Reduce the frequency of searching for missing Subtitles

![image-20200724171225229](images/image-20200724171225229.png)
`Settings` => `Scheduler` => `Search and Upgrade Subtitles`
If you got allot of missing/wanted subtitles change the  search frequency to a bigger interval.
6 - 12/24 Hours could give better results.

> Change this option also if you often see in the logs something like:
> `Execution of job "Update movies list from Radarr (trigger: interval[0:05:00], next run at: 2019-08-04 11:23:44 CEST)" skipped: maximum number of running instances reached (1)`

### Enable Adaptive Searching

![image-20200724174903909](images/image-20200724174903909.png)
`Settings` => `Subtitles` => `Performance / Optimization`
Enable `Adaptive Searching`

This setting reduces the searching frequency of specific media files after some time. This prevents searching every x hours for subtitles that may not exist at all for now. Adaptive searching should be (Always) enabled when you have many missing subtitles.

### Disable scan for embedded Subtitles

`Settings` => `Subtitles` => `Performance / Optimization`
Disable `Use Embedded Subtitles`
Embedded subtitles are subtitles that are in the video container (mkv, mp4, etc)
Bazarr needs to look inside the video container to know which subtitles are in it this can be resource intensive for some low powered devices and also give you issues with API Limits if you store your media on the cloud.

> This does mean it will ignore the embedded subtitles and will search for matching subtitles but that uses less resources.

### Disable Search Enabled Providers Simultaneously

Disable this option if you use a low performance device.
Example a RaspberryPi.

### Random small tweaks that can help with several issues

- Set scanning to only monitored and unmonitor ended shows in Sonarr/Radarr.
  And enable them later.
- Disable shows, movies that won't have subs ever in your preferred language some shows won't get subs ever. Keep in mind a lot of subs are user made. And some shows or movies are just not worth the time to create.
- More Providers
- Pay for anti-captha for the other providers that need it
