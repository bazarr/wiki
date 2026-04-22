# Custom Notifications

The JSON, XML, and Form notification providers support Apprise's payload manipulation feature for custom notification payloads.

You can refer to the Apprise wiki for details on the specific syntax for the [JSON](https://appriseit.com/services/json/#payload-manipulation), [XML](https://appriseit.com/services/xml/#payload-manipulation), and [Form](https://appriseit.com/services/form/#payload-manipulation) providers.

## Media variables

Media variables can be combined with the payload manipulation syntax to include detailed media information within the notification.

These media variables are available for `movie`, `series`, and `episode` entities, and follow the format `{bazarr_movie_*}`, `{bazarr_series_*}`, and `{bazarr_episode_*}`.

When the notification is processed for sending, the media variables contained within the notification config string are dynamically replaced with the relevant value.

An example is shown for a JSON notification config with a custom `path` payload key and the resulting value.

```none
json://localhost:8000?:path={bazarr_movie_path}{bazarr_episode_path}
```

```none
json://localhost:8000?:path=%2Fmovies%2FExample%20Movie%20(2000)%2FExample%20Movie%20(2000).mkv
```

Note that the `{bazarr_movie_path}` media variable has been replaced by the actual movie file path relevant to this notification.

!!! note
    If a Bazarr media variable has no value for a given notification it will be replaced by an empty string. This enables combining both `movie` and `series` / `episode` media variables in the same notification config as shown in the example.

## List of media variables

### Movie

??? "Click to show/hide media variables"
    | Variable name                       |
    | :---------------------------------- |
    | {bazarr_movie_alternativeTitles}    |
    | {bazarr_movie_audio_codec}          |
    | {bazarr_movie_audio_language}       |
    | {bazarr_movie_created_at_timestamp} |
    | {bazarr_movie_failedAttempts}       |
    | {bazarr_movie_fanart}               |
    | {bazarr_movie_ffprobe_cache}        |
    | {bazarr_movie_file_size}            |
    | {bazarr_movie_format}               |
    | {bazarr_movie_imdbId}               |
    | {bazarr_movie_missing_subtitles}    |
    | {bazarr_movie_monitored}            |
    | {bazarr_movie_movie_file_id}        |
    | {bazarr_movie_overview}             |
    | {bazarr_movie_path}                 |
    | {bazarr_movie_poster}               |
    | {bazarr_movie_profileId}            |
    | {bazarr_movie_radarrId}             |
    | {bazarr_movie_resolution}           |
    | {bazarr_movie_sceneName}            |
    | {bazarr_movie_sortTitle}            |
    | {bazarr_movie_subtitles}            |
    | {bazarr_movie_tags}                 |
    | {bazarr_movie_title}                |
    | {bazarr_movie_tmdbId}               |
    | {bazarr_movie_updated_at_timestamp} |
    | {bazarr_movie_video_codec}          |
    | {bazarr_movie_year}                 |

### Series

??? "Click to show/hide media variables"
    | Variable name                        |
    | :----------------------------------- |
    | {bazarr_series_alternativeTitles}    |
    | {bazarr_series_audio_language}       |
    | {bazarr_series_created_at_timestamp} |
    | {bazarr_series_ended}                |
    | {bazarr_series_fanart}               |
    | {bazarr_series_imdbId}               |
    | {bazarr_series_lastAired}            |
    | {bazarr_series_monitored}            |
    | {bazarr_series_overview}             |
    | {bazarr_series_path}                 |
    | {bazarr_series_poster}               |
    | {bazarr_series_profileId}            |
    | {bazarr_series_seriesType}           |
    | {bazarr_series_sonarrSeriesId}       |
    | {bazarr_series_sortTitle}            |
    | {bazarr_series_tags}                 |
    | {bazarr_series_title}                |
    | {bazarr_series_tvdbId}               |
    | {bazarr_series_updated_at_timestamp} |
    | {bazarr_series_year}                 |

### Episode

??? "Click to show/hide media variables"
    | Variable name                          |
    | :------------------------------------- |
    | {bazarr_episode_absoluteEpisode}       |
    | {bazarr_episode_audio_codec}           |
    | {bazarr_episode_audio_language}        |
    | {bazarr_episode_created_at_timestamp}  |
    | {bazarr_episode_episode}               |
    | {bazarr_episode_episode_file_id}       |
    | {bazarr_episode_failedAttempts}        |
    | {bazarr_episode_ffprobe_cache}         |
    | {bazarr_episode_file_size}             |
    | {bazarr_episode_format}                |
    | {bazarr_episode_missing_subtitles}     |
    | {bazarr_episode_monitored}             |
    | {bazarr_episode_path}                  |
    | {bazarr_episode_resolution}            |
    | {bazarr_episode_sceneName}             |
    | {bazarr_episode_season}                |
    | {bazarr_episode_sonarrEpisodeId}       |
    | {bazarr_episode_sonarrSeriesId}        |
    | {bazarr_episode_subtitles}             |
    | {bazarr_episode_title}                 |
    | {bazarr_episode_tvdbId}                |
    | {bazarr_episode_updated_at_timestamp}  |
    | {bazarr_episode_video_codec}           |
