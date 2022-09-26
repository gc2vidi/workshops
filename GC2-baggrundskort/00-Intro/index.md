# Introduktion GC2 baggrundskort

GC2 giver mulighed for at lave egne baggrundskort bestående af en række lag, der "sammensmeltes" til ét tile-lag. De enkelte lag kan opsætning i GC2 gennem MapServer eller QGIS Server og GC2-cli kan anevendes til at "seed" cachen.

## Forudsætninger

For at kunne gennemføre denne workshop kræves adgang til GC2/Vidi med flere lag, som skal udgøre baggrundskortet.

Du kan anvende denne [GC2/Vidi installation](https://swarm.gc2.io/) hvor du kan oprette en database og uploade fem datasæt, som kan hentes [her](https://github.com/mapcentia/workshops/raw/main/GC2-baggrundskort/data/data.zip)

Dataene skal unzippes før upload.

Du kan oprette et schema kaldet `geodk` og uploade hertil.

Dataene består af:

* baggrund.shp
* bygning.shp
* vejmidte.shp
* bykerne.shp
* extent.shp (EPSG:3857)

Alle datasæt skal uploades som EPSG:25832 med encoding UTF8 med undtagelse af `extent.shp` som er projekteret i EPSG:3857.


