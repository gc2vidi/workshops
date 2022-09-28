# Intro til vektorlag
Vidi kan udstille data som både rasterlag og vektorlag. Der er stor forskel på hvordan disse teknologier virker og hvad de kan.

Denne workshop går i dybden med hvordan vektorlag virker, opsættes og kan anvendes.

Dette meste indhold i workshoppen er dokumenteret under [GC2 Meta Information > Vector setting](https://vidi.readthedocs.io/da/latest/pages/standard/92_gc2_meta_information.html#vector-settings)   

> **_Note:_** Flere opsætninger er fælles for vektor- og rasterlag, som fx 'Mouse over', 'Info pop-up', 'Filters'. Disse gennemgås ikke i denne workshop.

## Terminologi
Vi benytter ofte 'raster' og 'vektor' til at beskrive metoder til visualisering af lag. Dette er også korrekt, men kan misforstås, da raster og vektor også beskriver to datamodeller i GIS.
Så når termerne benyttes i denne workshop, handler det udelukkede om hvilken metode, der anvendes til visualisering af lag og IKKE datamodel.

## Forudsætninger
For at kunne gennemføre denne workshop kræves adgang til GC2/Vidi.

Du kan anvende denne [GC2/Vidi installation](https://swarm.gc2.io/) hvor du kan oprette en database og uploade workshoppens [tre datasæt](https://github.com/gc2vidi/workshops/raw/main/Vidi-svg-layers/data/data.zip)

Dataene skal unzippes før upload til GC2.

Du kan oprette et schema kaldet `workshop` og uploade hertil.

Dataene består af:

* t_5801_fac_fl.shp
* t_5802_fac_li.shp
* t_5800_fac_pkt.shp

Alle datasæt skal uploades med EPSG:25832 og med encoding UTF8.   

[Denne Vidi instans](https://vidi.swarm.gc2.io/app/demo/workshop/) med de tre lag kan anvendes til flere af øvelserne.

