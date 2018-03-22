# Nominatim West Africa Docker

100% working container for [Nominatim](https://github.com/openstreetmap/Nominatim).

[![](https://images.microbadger.com/badges/image/mediagis/nominatim.svg)](https://microbadger.com/images/gpproton/nominatim-westafrica/ "Get your own image badge")

# Supported tags and respective `Dockerfile` links #

- [`3.0.1`, `3.0`, `latest`  (*3.0/Dockerfile*)](https://github.com/gpproton/nominatim-westafrica/tree/master/3.0)
- [`2.5.0`, `2.5`, `latest`  (*2.5/Dockerfile*)](https://github.com/gpproton/nominatim-westafrica/tree/master/2.5)

Run [http://wiki.openstreetmap.org/wiki/Nominatim](http://wiki.openstreetmap.org/wiki/Nominatim) in a docker container. Clones the current master and builds it. This is always the latest version, be cautious as it may be unstable.

Uses Ubuntu 16.04 and PostgreSQL 9.5

# Country
To check that everything is set up correctly, download and load to Postgres PBF file with minimal size - Africa/WestAfrica (latest)

If a different country should be used you can set `PBF_DATA` on build.

1. Clone repository

  ```
  # git clone --recursive https://github.com/gpproton/nominatim-westafrica.git
  # cd nominatim-docker/3.0
  ```

2. Modify Dockerfile, set your url for PBF

  ```
  ENV PBF_DATA https://track-api.realtrack.ml/westafrica.osm.pbf
  ```
3. Configure incrimental update. By default CONST_Replication_Url configured for Monaco.
If you want a different update source, you will need to declare `CONST_Replication_Url` in local.php. Documentation [here] (https://github.com/openstreetmap/Nominatim/blob/master/docs/Import-and-Update.md#updates). For example, to use the daily country extracts diffs for Gemany from geofabrik add the following:
  ```
  @define('CONST_Replication_Url', 'http://download.geofabrik.de/europe/germany-updates');
  ```

4. Build 

  ```
  docker build -t nominatim-westafrica .
  ```
5. Run

  ```
  docker run --restart=always -d -p 8084:8084 --name nominatim-westafrica gpproton/nominatim-westafrica
  ```
  If this succeeds, open [http://localhost:8084/](http:/localhost:8084) in a web browser

# Running

You can run Docker image from docker hub.

```
docker run --restart=always -d -p 8084:8084 --name nominatim-westafrica gpproton/nominatim-westafrica:latest
```
Service will run on [http://localhost:8084/](http:/localhost:8084)

# Update

Full documentation for Nominatim update available [here](https://github.com/openstreetmap/Nominatim/blob/master/docs/Import-and-Update.md#updates). For a list of other methods see the output of:
  ```
  docker exec -it nominatim-westafrica sudo -u nominatim ./src/build/utils/update.php --help
  ```

The following command will keep your database constantly up to date:
  ```
  docker exec -it nominatim-westafrica sudo -u nominatim ./src/build/utils/update.php --import-osmosis-all
  ```
If you have imported multiple country extracts and want to keep them
up-to-date, have a look at the script in
[issue #60](https://github.com/openstreetmap/Nominatim/issues/60).
