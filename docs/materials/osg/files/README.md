Worker Node Geolocation
=======================

The contents of this repository are for the OSG Virtual School Grid worker node (WN) geolocation exercises.
It contains the following:

- **data/** - GeoLite2 city database in `mmdb` format from [MaxMind](https://dev.maxmind.com/geoip/geoip2/geolite2/)
- **src/** - The `wn-geoip` library for returning lat/lon coordinates from worker nodes in the CHTC pool and CEs out on
  the OSG.
  It also contains the [geoip2](https://pypi.org/project/geoip2/) pip module, its requirements, and the `ipaddress`
  library, installed with:

    ```
    pip install --ignore-installed --no-compile --target src/ geoip2
    ```

To prepare the tarball for the exercises, perform the following steps:

1.  Create the tarball with the expected dir structure:

        tar -czvf wn-geoip.tar.gz wn-geoip/

1.  Copy the tarball to the CHTC Squid location referenced by the Grid exercises
