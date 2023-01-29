# Starting with v1.1.5-beta.8, Bazarr can be configured to use Postgres as database engine.

## Database instance deployment
First, we need a Postgres instance. This guide is written for usage of the postgres:14 Docker image, but earlier version can be used if it's version >= 9.

_Do not even think about using the latest tag!_

```
docker create --name=postgres14 \
    -e POSTGRES_PASSWORD=_<postgres_password>_ \
    -e POSTGRES_USER=_<postgres_username>_ \
    -e POSTGRES_DB=bazarr \
    -p 5432:5432/tcp \
    -v /path/to/appdata/postgres14:/var/lib/postgresql/data \
    postgres:14
```
    
## Database creation
Bazarr needs one database. For example: `bazarr`

You can give the databases any name you want but make sure config.ini file has the correct names.

Bazarr will not create the databases for you. Make sure you create it with your favourite tool before trying to start Bazarr.

## Schema creation
We need to tell Bazarr to use Postgres. The config.ini should already be populated with the entries we need:

```
[postgresql]
enabled = True
host = localhost
port = 5432
database = bazarr
username = _<postgres_username>_
password = _<postgres_password>_
```

If you do not want to migrate an existing SQLite database to Postgres then you have already reached the end of this guide and you can simply start Bazarr!

## Data migration
Once the database has been created, you can start the Bazarr migration from SQLite to Postgres. Make sure that your SQLite database has been used with Bazarr 1.1.5 or greater prior to migration.

To migrate data we can use PGLoader. It does, however, have some gotchas:
1. By default, transactions are case-insensitive, we use --with "quote identifiers" to make them case-sensitive.
1. The version packaged in Debian and Ubuntu's apt repo are too old for newer versions of Postgres (Roxedus has not tested packages in other distros).
1. Roxedus built a binary to enable this support (no code modification was needed, simply had to be built with updated dependencies).
1. Do not drop any tables in the Postgres instance

Before starting a migration please ensure that you have run Bazarr against the previously created Postgres databases at least once successfully.

Begin the migration by doing the following:

1. Stop Bazarr
1. Open your preferred database management tool and connect to the Postgres database instance
1. Run the following commands:

```
DELETE FROM "system" WHERE 1=1;
DELETE FROM "table_settings_languages" WHERE 1=1;
DELETE FROM "table_settings_notifier" WHERE 1=1;
```

Start the migration by using either of these options:

with docker:

```
docker run --rm -v /absolute/path/to/bazarr.db:/bazarr.db --network=host ghcr.io/roxedus/pgloader --with "quote identifiers" --with "data only" --cast "column table_blacklist.timestamp to timestamp" --cast "column table_blacklist_movie.timestamp to timestamp" --cast "column table_history.timestamp to timestamp" --cast "column table_history_movie.timestamp to timestamp" /bazarr.db "postgresql://_<postgres_username>_:_<postgres_password>_@hostname:5432/bazarr"
```

without docker:

```
pgloader --with "quote identifiers" --with "data only" --cast "column table_blacklist.timestamp to timestamp" --cast "column table_blacklist_movie.timestamp to timestamp" --cast "column table_history.timestamp to timestamp" --cast "column table_history_movie.timestamp to timestamp" /bazarr.db "postgresql://_<postgres_username>_:_<postgres_password>_@hostname:5432/bazarr"
```

You can now start Bazarr with Postgres support without losing anything in the process!