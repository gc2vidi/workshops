# Tile-backends (SQLite, Disk, S3)

Når MapCache har lavet en tile, ved seeding eller første gang en klient beder om den, gemmes den i en "cache-backend". Backend'en afgør hvor og hvordan tiles ligger, og har betydning for performance, diskforbrug, deling mellem servere og rydning af cache.

GC2 understøtter disse backends:

| Backend | Lagring |
|---|---|
| SQLite | Én SQLite-databasefil pr. tileset |
| Disk | Én fil pr. tile i en mappestruktur |
| S3 | Ét objekt pr. tile i en S3 bucket (AWS eller S3-kompatibel) |
| Memcache | I hukommelsen på en memcached-server (midlertidig) |

## Hvor vælges backend'en?

**Globalt** i GC2's `app/conf/App.php`. Standard er `sqlite`:

```php
"mapCache" => [
    // Type of cache back-end. "disk" or "sqlite"
    "type" => "sqlite",
    ...
],
```

**Pr. lag** i fanen `Tile cache` under `Cache`, hvor man kan vælge Disk, SQLite, S3 eller Memcache.

Vigtigt for baggrundskort: Det sammensmeltede schema-lag (`geodk` og `geodk.mvt`) bruger schemaets tile-indstillinger, som kan sættes gennem GC2-app. Indstillingen pr. lag gælder kun lagets egne tilesets (fx `geodk.bygning` og `geodk.bygning.mvt`).

## SQLite

Tiles ligger i én fil pr. tileset:

```
app/wms/mapcache/sqlite/workshop/geodk.sqlite3
app/wms/mapcache/sqlite/workshop/geodk.mvt.sqlite3
```

Alle grids (`g20`, `25832` osv.) for tilesettet ligger i den samme fil.

Fordele:

* Få filer – nemt at flytte, tage backup af eller kopiere en færdigseedet cache til en anden server
* Hurtig at slette
* Identiske tiles (fx tomt hav) gemmes kun én gang (`symlink_blank`)

Ulemper:

* Kun én proces kan skrive ad gangen. Seeding med mange tråde giver derfor ikke nødvendigvis mere fart
* Filen skrumper ikke, når cachen ryddes – pladsen genbruges, når cachen fyldes op igen

## Disk

Tiles ligger som almindelige filer i en mappestruktur pr. tileset og grid:

```
app/wms/mapcache/disk/workshop/geodk/g20/14/...
app/wms/mapcache/disk/workshop/geodk.mvt/g20/14/...
```

Fordele:

* Simpel – tiles kan ses og kopieres direkte i filsystemet
* Mange processer kan skrive samtidig, så seeding skalerer godt med flere tråde

Ulemper:

* Et stort baggrundskort kan blive til millioner af små filer. Det kan løbe tør for *inodes* og giver langsom backup
* Rydning af cachen tager tid, fordi hver fil skal slettes (sker i baggrunden)

## S3

Tiles gemmes som objekter i en S3 bucket. Opsætning af bucket og adgangsnøgler sker i `App.php`:

```php
"s3" => [
    "host" => "[bucket].s3-eu-west-1.amazonaws.com",
    "id" => "[access key id]",
    "secret" => "[secret access key]",
    "region" => "eu-west-1",
],
```

Tiles får stien:

```
https://[host]/workshop/[tileset]/[grid]/{z}/{x}/{y}/[ext]
```

Med `S3 tile set name` i `Tile cache` fanen kan et lag få sin egen sti i bucket'en i stedet for `workshop/[tileset]`. Det er fx nyttigt, når flere databaser skal dele den samme cache.

Fordele:

* Ubegrænset plads, og serverens disk fyldes ikke op
* Cachen deles af alle GC2/MapCache-servere – oplagt når GC2 kører på flere noder (fx Docker Swarm)
* Overlever at serveren genopbygges

Ulemper:

* Hvert tile-opslag er et netværkskald, så cache-hits er langsommere end på lokal disk
* Der betales for lagring og requests
* Hele cachen kan ikke ryddes fra GC2 – se afsnittet om rydning nedenfor

**Bemærk sikkerheden:** GC2 lægger tiles i S3 med ACL'en `public-read`. Kender man stien, kan tiles hentes direkte fra S3 uden om GC2's adgangskontrol. Brug derfor kun S3 til offentlige lag, hvilket et baggrundskort typisk er.

## Memcache

Tiles gemmes i hukommelsen og forsvinder, når de udløber eller serveren genstartes. Det er nyttigt til lag, der ændrer sig hele tiden, hvor man vil aflaste databasen uden at have en permanent cache. Memcache er **ikke** egnet til baggrundskort, og der er ingen grund til at seede til den.

## Rydning af cache

`Ryd tile cache` i GC2 App rydder SQLite- og Disk-caches for raster-tiles.

| Backend | Ryd hele tilesettet | Ryd del (`bbox`/`zoom`) |
|---|---|---|
| SQLite | Ja, med det samme | Ja, som baggrundsjob |
| Disk | Ja, som baggrundsjob | Ja, som baggrundsjob |
| S3 | Nej – brug `bbox`/`zoom` eller en *lifecycle rule* på bucket'en | Ja, som baggrundsjob |
| Memcache | Nej – tiles udløber af sig selv | Ja, som baggrundsjob |

Med `Lock` i `Tile cache` fanen kan en cache låses, så den ikke ryddes ved en fejl – fx efter en lang seeding.

## Hvilken backend skal jeg vælge?

| Situation | Anbefaling |
|---|---|
| Én server, almindeligt baggrundskort | SQLite |
| Stor seeding med mange tråde på én server | Disk |
| Flere GC2-servere eller meget store caches | S3 |
| Hurtigt skiftende data, ingen seeding | Memcache (eller ingen cache) |

## Øvelse

- Hvilken backend ville du vælge til et landsdækkende baggrundskort i både `g20` og `25832`, seedet til zoom 19? Tænk på antal tiles, diskplads og hvordan cachen skal ryddes.
