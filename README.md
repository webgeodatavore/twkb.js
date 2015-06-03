
### Note: 

This library is outdated and doesn't follow the new specification.
It will be updated soon. 
### End Note


this library allows to decode TWKB.
warning: this is a preview and may change without any notice

## usage

```
new TWKB(buffer).toGeoJSON()
```

## building

```
npm install
npm run build
```

## example

this is a simple example using leaflet

```
function get(url, callback) {
  var oReq = new XMLHttpRequest();
  oReq.open("GET", url, true);
  oReq.responseType = "arraybuffer";

  oReq.onload = function (oEvent) {
    callback(oReq.response);
  };

  oReq.send(null);
}

var map = L.map('map').setView([40.505, -3.09], 5);

get('file.twkb', function(data) {
    var b = new TWKB(new Uint8Array(data))
    L.geoJson({ features: b.toGeoJSON(), type: "FeatureSet"}).addTo(map)
})
```

# API

## TWKB 

### TWKB(buffer)

creates a new TWKB decoder

### toGeoJSON()

returns valid geojson for the features

### next() -> Feature
reads next feature, see ``Feature``

### forEach(callback)
calls callback for each feature

## Feature

### Feature.coordinates() -> Array
array of coordinates with format [x,y,z, x,y,z ...]. If ``type`` is a polygon this is an array of
arrays. As general rule this array follows the same rules that ``coordinates`` in geojson geometry

### Feature.bbox() -> Object 
geometry bounding box in format ``{ min: [minx, miny, minz], max: [maxx, maxy, maxz] }``

### Feature.ndims() -> int
dimensions, 2 for XY, 3 for XYZ

### Feature.type() -> int
one of the following: TWKB.POINT, TWKB.LINESTRING ...

### Feature.ids() -> int Array
when the geometry contains ids this method returns an array of id, one for each sub feature.
returns ``null`` if the feature does not contain ids.

NOTE: only multi geometries contain ids so for ``TWKB.POINT``, 
``TWKB.LINESTRING``, ``TWKB.POLYGON`` it allways returns ``null``

### Feature.features() -> Feature Array

then ``type()`` is ``TWKB.COLLECTION`` this method returns a ``Feature`` Array with the features
within the group, an empty array otherwise



## license

see LICENSE file
