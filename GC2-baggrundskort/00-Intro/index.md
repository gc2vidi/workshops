# Introduktion GC2 baggrundskort og vektor-tiles

GC2 giver mulighed for at lave egne baggrundskort bestående af en række lag, der "sammensmeltes" til ét tile-lag – enten som raster-tiles (PNG) eller som vektor-tiles (MVT). De enkelte lag kan opsætning i GC2 gennem MapServer (raster og vektor) eller QGIS Server (kun raster) og GC2-cli og GC2-app kan anvendes til at "seed" cachen.

## Forudsætninger

For at kunne gennemføre denne workshop kræves adgang til GC2/Vidi med flere lag, som skal udgøre baggrundskortet.

Du kan anvende denne [GC2/Vidi installation](https://test.admin.gc2.io/) hvor du kan logge ind i databasen `workshop`, skabe et nyt schema og uploade fem datasæt, som kan hentes [her](https://github.com/gc2vidi/workshops/blob/main/GC2-baggrundskort/data/data.zip)

Dataene skal unzippes før upload.

Dataene består af:

* baggrund.shp
* bygning.shp
* vejmidte.shp
* bykerne.shp
* extent.shp (EPSG:3857)
* geodk.json (MapLibre style fil)

Alle datasæt skal uploades som EPSG:25832 med encoding UTF8 med undtagelse af `extent.shp` som er projekteret i EPSG:3857.


