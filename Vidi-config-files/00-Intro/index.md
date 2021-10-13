# Introduktion Vidi config-filer og lag-/felt-indstillinger

Vidi config-filer og lag-/felt-indstillinger giver mulighed for at konfigurere hhv. Vidi og kortlagene. Mulighederne for at konfigurere er mange og forskelliagartede og denne workshop tager dig igennem alle mulighederne, men går ikke i dybden, da dette vil blive for omfangsrigt.

## Forudsætninger

For at kunne gennemføre denne workshop kræves adgang til GC2/Vidi med minimum et lag.

Du kan anvende denne [GC2/Vidi installation](https://swarm.gc2.io/) hvor du kan oprette en database og uploade et fire datasæt, som kan hentes [her](https://github.com/mapcentia/workshops/raw/main/Vidi-config-files/data/data.zip)

Dataene skal unzippes før upload.

Du skal oprette et schema kaldet `workshop` og uploade hertil.

Dataene består af:

* KOMMUNE.shp
* REGION.shp
* NAVNE_P.shp
* ja_nej.csv

Alle datasæt skal uploades som EPSG:25832 med encoding UTF8.


