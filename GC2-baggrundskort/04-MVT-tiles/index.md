# MVT vektor-tiles

Indtil nu har baggrundskortet bestået af raster-tiles (PNG eller JPEG), som renderes færdige på serveren. GC2 kan også udstille de samme lag som **vektor-tiles** i formatet MVT (Mapbox Vector Tiles).

## Hvad er en vektor-tile?

En vektor-tile er ligesom en raster-tile et "stykke" af kortet i et fast tile-system (zoom/x/y). Forskellen er indholdet:

| | Raster-tile (PNG) | Vektor-tile (MVT) |
|---|---|---|
| Indhold | Færdigt billede | Geometrier + attributter (protobuf) |
| Styling | På serveren (MapServer/QGIS) | I klienten (MapLibre style JSON) |
| Skift af styling | Kræver ny cache | Bare ret style-filen |
| Skarphed | Pixeleret mellem zoom-niveauer | Skarp i alle zoom og ved rotation |
| Labels | Brændt ind, kan blive klippet ved tile-kanter | Placeres i klienten henover tile-kanter |
| Interaktion | Ingen | Features kan forespørges i browseren |

Den vigtigste pointe: **Med MVT er data og styling adskilt**. GC2 leverer data – styling foregår i klienten via en *style*-fil. Den styling, som er lavet i GC2 (klasser, farver osv.), bruges derfor ikke for MVT.

## Hvordan GC2 laver MVT tiles

MVT tiles genereres af MapServer ud fra schemaets WFS-mapfil og caches i MapCache – præcis som PNG-tiles. For hvert lag og hvert schema opretter GC2 automatisk et tileset med endelsen `.mvt`:

| Tileset | Indhold |
|---|---|
| `geodk.mvt` | Hele schemaet `geodk` – alle lag i én tile |
| `geodk.bygning.mvt` | Kun laget `geodk.bygning` |

I en tile for hele schemaet ligger hvert lag som sit eget *source-layer* med navnet `schema.tabel`, fx `geodk.bygning` og `geodk.vejmidte`. Det er de navne, der skal refereres til i style-filen.

Attributterne i tiles er de samme felter, som laget udstiller via WFS.

Bemærk: MVT kan slås fra på serveren. Hvis `mapCache.formats` er sat i GC2's `App.php`, skal `mvt` være med på listen.

## URL'er til MVT tiles

MapCache udstiller tiles gennem flere "services". Til vektor-tiles bruges typisk `gmaps` (XYZ-skema, som MapLibre forventer):

```
https://swarm.gc2.io/mapcache/[database]/gmaps/[tileset]@[grid]/{z}/{x}/{y}.mvt
```

Fx for hele schemaet `geodk` i Google Maps-grid'et `g20`:

```
https://swarm.gc2.io/mapcache/workshop/gmaps/geodk.mvt@g20/{z}/{x}/{y}.mvt
```

De samme tiles kan også hentes via TMS og WMTS:

```
# TMS (bemærk: y-aksen er vendt – brug "scheme": "tms" i style-filen)
https://swarm.gc2.io/mapcache/workshop/tms/1.0.0/geodk.mvt@g20/{z}/{x}/{y}.mvt

# WMTS RESTful (bemærk rækkefølgen {y}/{x})
https://swarm.gc2.io/mapcache/workshop/wmts/1.0.0/geodk.mvt/default/g20/{z}/{y}/{x}.mvt
```

Om grid'et (`@g20`) – se [Modul 06](../06-Tile-systemer).

## Style-filen (MapLibre style JSON)

Styling af vektor-tiles beskrives i en [MapLibre style](https://maplibre.org/maplibre-style-spec/) – en JSON-fil med tre centrale dele:

* `sources` – hvor tiles hentes fra
* `layers` – hvordan hvert source-layer tegnes (fill, line, symbol, circle ...) og i hvilken rækkefølge (først i listen tegnes nederst)
* `glyphs` – hvor skrifttyper til labels hentes fra (kun nødvendig ved labels)

Et minimalt eksempel til workshoppens data:

```json
{
  "version": 8,
  "name": "geodk",
  "glyphs": "https://cdn.dataforsyningen.dk/assets/vector_tiles_assets/latest/glyphs/{fontstack}/{range}.pbf",
  "sources": {
    "geodk": {
      "type": "vector",
      "tiles": [
        "https://swarm.gc2.io/mapcache/workshop/gmaps/geodk.mvt@g20/{z}/{x}/{y}.mvt"
      ],
      "minzoom": 0,
      "maxzoom": 20
    }
  },
  "layers": [
    {
      "id": "baggrund",
      "type": "fill",
      "source": "geodk",
      "source-layer": "geodk.baggrund",
      "paint": {
        "fill-color": "#f4f1ea"
      }
    },
    {
      "id": "bykerne",
      "type": "fill",
      "source": "geodk",
      "source-layer": "geodk.bykerne",
      "paint": {
        "fill-color": "#e8dccb",
        "fill-opacity": 0.6
      }
    },
    {
      "id": "sti",
      "type": "line",
      "source": "geodk",
      "source-layer": "geodk.vejmidte",
      "minzoom": 14,
      "filter": ["==", ["get", "trafikart_"], "Sti"],
      "paint": {
        "line-color": "#9a8f7d",
        "line-width": 1,
        "line-dasharray": [2, 2]
      }
    },
    {
      "id": "vej",
      "type": "line",
      "source": "geodk",
      "source-layer": "geodk.vejmidte",
      "filter": ["!=", ["get", "trafikart_"], "Sti"],
      "paint": {
        "line-color": "#ffffff",
        "line-width": ["interpolate", ["linear"], ["zoom"], 12, 1, 16, 4, 19, 10]
      }
    },
    {
      "id": "bygning",
      "type": "fill",
      "source": "geodk",
      "source-layer": "geodk.bygning",
      "minzoom": 14,
      "paint": {
        "fill-color": "#d9d0c1",
        "fill-outline-color": "#b5a993"
      }
    }
  ]
}
```

Bemærk hvordan:

* `filter` bruges til at tegne det samme source-layer på flere måder (vej og sti)
* `interpolate` giver en linjebredde, der vokser med zoom – uden at røre serveren
* `minzoom`/`maxzoom` på et layer styrer, hvornår det vises

## Maputnik – visuel style-editor

[Maputnik](https://maplibre.org/maputnik/) er en gratis browser-baseret editor til style-filer. Åbn din style-fil, ret farver/filtre visuelt og eksportér JSON igen. Maputnik viser også hvilke source-layers og attributter, der findes i tiles (`View → Inspect`).

## Nyttigt at vide

* **Cache og styling:** Ændres der kun i style-filen, skal cachen *ikke* ryddes. Ændres der i data eller i hvilke lag schemaet indeholder, skal cachen ryddes/re-seedes som for PNG.
* **Datamængde:** Vektor-tiles på lave zoom-niveauer kan blive store, fordi alle geometrier i tilen kommer med. Brug fx views med forenklede geometrier eller begræns hvilke lag, der ligger i schemaet.
* **Seeding:** MVT tilesets seedes med gc2-cli på samme måde som PNG – brug tileset-navnet med `.mvt`, fx `--layer geodk.mvt`.
* **Sikkerhed:** Tiles hentes med de samme rettigheder som lagene. Beskyttede lag kræver login/token – bedst er at baggrundskortets lag er offentlige.

## Øvelse

- Hent en enkelt tile i browseren for at se, at det virker (du får en binær fil):  
`https://swarm.gc2.io/mapcache/[din database]/gmaps/geodk.mvt@g20/14/8643/5015.mvt`

- Gem style-eksemplet ovenfor som `geodk.json` og ret database-navnet i `tiles`.

- Åbn `geodk.json` i [Maputnik](https://maplibre.org/maputnik/) (`Open → Upload`) og zoom ind over dataene.

- Leg med stylingen: skift farver, tilføj en `line`-kontur på bygningerne, og tilføj et `filter` på `bygning` for fx `bygningsty`.

- Eksportér den færdige style-fil – den skal bruges i næste modul.
