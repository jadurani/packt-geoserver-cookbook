# Using different WFS versions in OpenLayers

## Chapter Link
https://subscription.packtpub.com/book/application_development/9781783289615/1/ch01lvl1sec10/using-different-wfs-versions-in-openlayers

## Notes

### 1 - Create a symlink from for this file to work

- Item 9 instructs me to save the file `ch01_wfsVersion.html` in a folder published on my server, such as `TOMCAT_HOME/webapps/ROOT` and point my browser to it
- To my knowledge, I do not have TOMCAT installed in my local machine and geoserver runs directly as its own java-based app by running `./$GEOSERVER_DIR/bin/startup.sh`.
  - `$GEOSERVER_DIR` does have a `webapps` directory in it. I cd to that and see `geoserver/`
  - This must be where I can also save the file `ch01_wfsVersion.html` and have it served in my browser.
- But I do not want to save my file to that directory and I want to keep them in a place where I keep all my codes.
- Solution: Create a symlink of my original file to the `sudo ln -s /path/to/original /path/to/link`

### 2 - <source_name>.shp does not exist (WFS)

- Make sure all the sources of your layers are still in their original paths. Otherwise you'll have a very unrelated error that goes along the lines of "shapefile.shp does not exist" when you're trying to retrieve a layer for a WFS request that is not at all related to the error
  - Seems like this is more of a Geoserver error

### 3 - Update the code in step 6

Code in Step 6:

```
            new OpenLayers.Layer.Vector("countries", {
              strategies: [new OpenLayers.Strategy.BBOX()],
              protocol: new OpenLayers.Protocol.WFS({
                url: "http://localhost/geoserver/wfs",
                featureType: "countries",
                featureNS: "http://www.naturalearthdata.com/",
                // Mind the geometry column name
                geometryName: "geom"
              }),
```

Update the above code to:

```
            new OpenLayers.Layer.Vector("countries", {
              strategies: [new OpenLayers.Strategy.BBOX()],
              protocol: new OpenLayers.Protocol.WFS.v1_1_0({
                url: "http://localhost:8080/geoserver/wfs",
                featurePrefix: "NaturalEarth",
                featureType: "countries",
                geometryName: "the_geom"
              }),
```

Changes included:

- add the field `featurePrefix` and set the value to the workspace of the layer
- remove the `featureNS` field
- update the geometry name to `the_geom`


Reference:

https://gis.stackexchange.com/a/340134