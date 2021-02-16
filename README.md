# Nearmap vertical transformations

This repository contains the relevant transform grids to enable conversion
between ellipsoidal and geoidal heights.

## Background

Nearmap APIs typicall use geoidal heights. For example, [DSM and True Ortho API](https://docs.nearmap.com/display/ND/DSM+and+TrueOrtho+API#/staticmap/get_coverage_json)
returns heights in one of country local vertical datums:

* Australia - AHD (AusGeoid09)
* US - EGM2008
* Canada - CGVD2013

This is a deliberate design choice to provide the elevation data that follows
the mean sea level, and is therefore the most useful vertical
datum across many use cases.

In some cases, a conversion to ellipsoidal height is desirable. This repository
provides the transformation grids and example GDAL commands to perform these
transformations. The grids are as used internally at Nearmap.

## Dependencies
To perform these transformations, you need to install [GDAL](https://gdal.org/download.html). GDAL contains both the command line utitlies and programmatic access to the required subsystem which will perform the work.

## Australia

Expected values

|Datum|Longitude|Latitude|Altitude|
|---|---|---|---|
WGS84/GDA2020 Ellipsoidal Height|151.76523344167|-32.9295737833|52.857|
|AHD Height|151.76523344167|-32.9295737833|27.169|



Convert from AHD geoid to WGS84 Ellipsoid
```
$> echo  151.76523344 -32.92957378 27.14702027 | cs2cs +proj=longlat +datum=WGS84 +to +proj=longlat +geoidgrids=./AU/AUSGeoid09.gtx -f "%.8f"
151.76523344	-32.92957378 52.85700001
```

Convert from WGS84 Ellipsoid to AHD geoid
```
$> echo 151.76523344167 -32.9295737833 52.857 | cs2cs -I +proj=longlat +datum=WGS84 +to +proj=longlat +geoidgrids=./AU/AUSGeoid09.gtx -f "%.8f"
151.76523344	-32.92957378 27.14702027
```

## United States
From https://vdatum.noaa.gov/vdatumweb/vdatumweb

|Datum|Longitude|Latitude|Altitude|
|---|---|---|---|
|WGS84 Ellipsoidal Height|-122.30926|47.56949|0.0|
|EGM2008 Height|-122.30926|47.56949|22.65|

Convert from EGM2008 geoid to WGS84 ellipsoid
```
echo -122.30926 47.56949 0.0 | cs2cs +proj=longlat +datum=WGS84 +to +proj=longlat +geoidgrids=./US/USGG2012.gtx -f "%.6f"
-122.309260	47.569490 -22.652655
```

Convert from WGS84 ellipsoid to EGM2008 geoid
```
echo -122.30926 47.56949 0.0 | cs2cs -I +proj=longlat +datum=WGS84 +to +proj=longlat +geoidgrids=./US/USGG2012.gtx -f "%.6f"
-122.309260	47.569490 22.652655
```


## Canada
WIP

## New Zealand
WIP