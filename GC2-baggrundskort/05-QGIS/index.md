# Tile caches i QGIS

Baggrundskortet fra GC2 kan bruges i QGIS på samme måde som i Vidi. QGIS kan hente tiles på tre måder:

| Forbindelse i QGIS | Tile-type | Grids |
|---|---|---|
| WMS/WMTS | Raster (PNG) | Alle – `g20`, `25832` osv. |
| XYZ Tiles | Raster (PNG) | Kun `g20` (web mercator) |
| Vector Tiles | Vektor (MVT) | `g20` (web mercator) |

Bruger dit QGIS-projekt `EPSG:25832`, er WMTS i grid'et `25832` det bedste valg til raster-tiles. Så skal QGIS ikke omprojicere tiles, og tekster og linjer bliver skarpe (se [Modul 06](../06-Tile-systemer)).

## WMTS

WMTS er den mest fleksible løsning, fordi QGIS selv læser hvilke tilesets og grids, der findes, i capabilities-dokumentet.

1. Vælg `Lag → Tilføj lag → Tilføj WMS/WMTS lag...`
2. Klik `Ny` og indtast:
   * Navn: `GC2 workshop`
   * URL: `https://test.admin.gc2.io/mapcache/[database]/wmts/1.0.0/WMTSCapabilities.xml`
3. Klik `Forbind` og vælg fanen `Tilesets`
4. Find `geodk` i listen. Hvert tileset optræder én gang pr. grid – vælg linjen med tile matrix set `25832` (eller `g20`)
5. Klik `Tilføj`

QGIS vælger selv det zoom-niveau i grid'et, der passer bedst til den aktuelle målestok.

## XYZ Tiles

XYZ er den hurtigste måde at tilføje et lag i web mercator:

1. Højreklik på `XYZ Tiles` i `Gennemse` (Browser) panelet og vælg `Ny forbindelse...`
2. Indtast:
   * Navn: `GeoDanmark (GC2)`
   * URL: `https://test.admin.gc2.io/mapcache/[database]/gmaps/geodk@g20/{z}/{x}/{y}.png`
   * Min. zoom: `0`, maks. zoom: fx `20`
3. Dobbeltklik på forbindelsen for at tilføje laget

Sæt maks. zoom til det højeste niveau, der er seedet eller giver mening. Zoomer man længere ind, forstørrer QGIS blot tiles fra det højeste niveau.

## Vector Tiles (MVT)

QGIS kan tegne MVT-tiles direkte og kan læse stylingen fra en MapLibre style-fil – den samme, som blev lavet i [Modul 04](../04-MVT-tiles).

1. Højreklik på `Vector Tiles` i `Gennemse` panelet og vælg `Ny generisk forbindelse...`
2. Indtast:
   * Navn: `GeoDanmark vektor (GC2)`
   * URL: `https://test.admin.gc2.io/mapcache/[database]/gmaps/geodk.mvt@g20/{z}/{x}/{y}.mvt`
   * Min. zoom: `0`, maks. zoom: `20`
   * Style URL: URL til din style-fil, fx `https://[bruger].github.io/[repo]/geodk.json`
3. Dobbeltklik på forbindelsen for at tilføje laget

Uden style-URL tegner QGIS alle source-layers med tilfældige farver. Du kan også indlæse stylingen bagefter: `Lagegenskaber → Symbologi → Stil → Indlæs stil...` og vælg style-filen (MapBox GL JSON).

Tips:

* QGIS oversætter style-filen til QGIS-symbologi. Det meste virker, men avancerede udtryk og visse label-indstillinger kan se anderledes ud end i Vidi/MapLibre. Når stylingen er indlæst, kan den rettes videre i QGIS
* Med `Identificer objekter` kan man klikke på features i vektor-tiles og se deres attributter – en god måde at undersøge hvad der ligger i tiles
* Et vektor-tile lag kan også gemmes som `.qml`/`.sld` stil, hvis QGIS-udgaven skal genbruges i andre projekter

## QGIS' egen cache

QGIS gemmer også selv tiles i en lokal netværkscache. Er cachen i GC2 ryddet eller re-seedet, kan QGIS derfor stadig vise de gamle tiles. Løsning:

* `Indstillinger → Indstillinger → Netværk → Cache-indstillinger` og klik på knappen for at rydde cachen
* Eller genstart QGIS

## Øvelse

- Sæt projektets CRS til `EPSG:25832` og tilføj baggrundskortet via WMTS i tile matrix set `25832`.

- Tilføj MVT-udgaven som Vector Tiles med din style-fil fra Modul 04. Hvad bliver oversat korrekt, og hvad ser anderledes ud?

- Brug `Identificer objekter` på vektor-tile laget og find attributterne for en vejmidte.

- Ryd cachen i GC2 for et af lagene (se [Modul 07](../07-Tile-backends)) og se om QGIS henter nye tiles – eller om du først skal rydde QGIS' netværkscache.
