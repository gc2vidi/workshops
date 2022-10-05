# Intro til editering

Man kan gennem Vidi editere et hvilket som helst lag baseret på PostGIS vektor-data. Editering omfatter skabelsen af nye
features samt opdatering og sletning af eksisterende. Både geometri og attributter kan editeres.

Editering kan ske på både raster- og vektorudgaven af et lag, men der er forskel på hvordan det opleves for brugeren og
hvorvidt offline funktionalitet kan anvendes.

Gennem denne workshop læres hvordan lag kan opsættes til editering, og hvordan input-felter for attributter kan gives
forskellige egenskaber.

## Terminologi

Vi benytter ofte 'raster' og 'vektor' til at beskrive metoder til visualisering af lag. Dette er også korrekt, men kan
misforstås, da raster og vektor også beskriver to datamodeller i GIS.
Så når termerne benyttes i denne workshop, handler det udelukkede om hvilken metode, der anvendes til visualisering af
lag og IKKE datamodel.

## Forudsætninger

For at kunne gennemføre denne workshop kræves adgang til GC2/Vidi.

Du kan anvende denne [GC2/Vidi installation](https://swarm.gc2.io/) hvor du kan oprette en database og uploade
workshoppens [datasæt](https://github.com/gc2vidi/workshops/raw/main/Vidi-svg-layers/data/data.zip)

Dataene skal udpakkes før upload til GC2.

Du kan oprette et schema kaldet `workshop` og uploade hertil.

Dataene består af disse geometri datasæt:

* t_5801_fac_fl.shp
* t_5802_fac_li.shp
* t_5800_fac_pkt.shp
* vejmidte_brudt.zip

Alle ovenstående datasæt skal uploades med EPSG:25832 og med encoding UTF8. `vejmidte_brudt.zip` skal ikke udpakkes før upload, da den fylder en del. 

Derudover er der disse datasæt uden geometri:

* d_5800_facilitet.csv
* d_basis_ansvarlig_myndighed.csv
* d_basis_ja_nej.csv

Disse datasæt uploades med encoding UTF8. EPSG koden er underordnet da de ikke indeholder geometrier.


