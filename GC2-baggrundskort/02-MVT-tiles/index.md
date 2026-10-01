# MVT vektor-tiles

Indtil nu har baggrundskortet bestået af raster-tiles (PNG eller JPEG), som renderes færdige på serveren. GC2 kan også udstille de samme lag som vektor-tiles i formatet MVT (Mapbox Vector Tiles).

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

Den vigtigste pointe: **Med MVT er data og styling adskilt**. GC2 leverer data, styling foregår i klienten via en style-fil. Den styling, som er lavet i GC2 (klasser, farver osv.), bruges derfor ikke for MVT.

## Hvordan GC2 laver MVT tiles

MVT tiles genereres af MapServer ud fra schemaets MapFile og caches i MapCache ligesom PNG-tiles. For hvert lag og hvert schema opretter GC2 automatisk et tileset med endelsen `.mvt`:

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
https://test.admin.gc2.io/mapcache/workshop/gmaps/[tileset]@[grid]/{z}/{x}/{y}.mvt
```

Fx for hele schemaet `geodk` i Google Maps-grid'et `g20`:

```
https://test.admin.gc2.io/mapcache/workshop/gmaps/geodk.mvt@g20/{z}/{x}/{y}.mvt
```

De samme tiles kan også hentes via TMS og WMTS:

```
# TMS (bemærk: y-aksen er vendt – brug "scheme": "tms" i style-filen)
https://test.admin.gc2.io/mapcache/workshop/tms/1.0.0/geodk.mvt@g20/{z}/{x}/{y}.mvt

# WMTS (bemærk rækkefølgen {y}/{x})
https://test.admin.gc2.io/mapcache/workshop/wmts/1.0.0/geodk.mvt/default/g20/{z}/{y}/{x}.mvt
```

Om grid'et (`@g20`) – se [Modul 03](../03-Tile-systemer).

## Style-filen (MapLibre style JSON)

Styling af vektor-tiles beskrives i en [MapLibre style](https://maplibre.org/maplibre-style-spec/) – en JSON-fil med tre centrale dele:

* `sources` – hvor tiles hentes fra
* `layers` – hvordan hvert source-layer tegnes (fill, line, symbol, circle ...) og i hvilken rækkefølge (først i listen tegnes nederst)
* `glyphs` – hvor skrifttyper til labels hentes fra (kun nødvendig ved labels)

Et minimalt eksempel til workshoppens data:

Filen kan hentes fra [https://mapcentia.github.io/vidi_configs_common/geodk.json](https://mapcentia.github.io/vidi_configs_common/geodk.json):

```json
{
  "version": 8,
  "name": "geodk",
  "glyphs": "https://cdn.dataforsyningen.dk/assets/vector_tiles_assets/latest/glyphs/{fontstack}/{range}.pbf",
  "sources": {
    "geodk": {
      "type": "vector",
      "tiles": [
        "https://test.admin.gc2.io/mapcache/workshop/gmaps/geodk.mvt@g20/{z}/{x}/{y}.mvt"
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

## Udtynding af tiles med klasser og målestok

Styling i GC2 bruges ikke til MVT – men **klasserne** gør. MVT tiles laves af MapServer ud fra schemaets MapFile, og den indeholder lagets klasser med deres `Expression`, `Min scale denominator` og `Max scale denominator`. MapServer lægger kun en feature i tilen, hvis den matcher en klasse, der er "tændt" i tilens målestok.

Det kan bruges til at tynde ud i tiles på lave zoom-niveauer. Fx behøver en tile på zoom 12 ikke at indeholde alle boligveje, når der kun skal vises overordnede veje. Det giver mindre tiles, hurtigere tegning og mindre data at seede.

Eksempel for `geodk.vejmidte` med feltet `vejklasse_`:

| Klasse | Expression | Max scale denominator | Resultat |
|---|---|---|---|
| Overordnede veje | `'[vejklasse_]' IN 'Trafikvej-Gennemfart,Trafikvej-Fordeling'` | (tom) | Med på alle zoom-niveauer |
| Lokalveje | `'[vejklasse_]' IN 'Lokalvej-Primær,Lokalvej-Sekundær'` | `60000` | Med fra zoom 13 |
| Boligveje mv. | (tom) | `20000` | Med fra zoom 15 |

Klasserne opsættes i GC2 Admin under lagets klasser (`Klasser` → vælg klasse → `Min/Max scale denominator`). Sidste klasse uden expression opsamler alle øvrige features.

Vigtigt:

* **Features uden klasse forsvinder.** Har laget klasser, kommer kun features, som matcher mindst én klasse, med i tilen. Husk en opsamlingsklasse, ellers mangler der data
* **Klasserne gælder også PNG-tiles og WMS.** Det er de samme klasser, som styrer raster-kortet, så udtynding i MVT slår også igennem i raster-udgaven
* **Ryd MVT-cachen** efter ændringer i klasserne (se [Modul 04](../04-Tile-backends)), ellers bliver de gamle tiles ved med at blive leveret

### Hvilken målestok har en tile?

MapServer beregner målestokken ud fra tilens størrelse (256 px) og en opløsning på 72 dpi. For `g20` giver det:

| Zoom | Resolution (m/px) | MapServer målestok |
|---|---|---|
| 10 | 152,87 | 1:433.000 |
| 11 | 76,44 | 1:217.000 |
| 12 | 38,22 | 1:108.000 |
| 13 | 19,11 | 1:54.200 |
| 14 | 9,55 | 1:27.100 |
| 15 | 4,78 | 1:13.500 |
| 16 | 2,39 | 1:6.800 |
| 17 | 1,19 | 1:3.400 |
| 18 | 0,60 | 1:1.700 |

Vælg en værdi mellem to zoom-niveauer, fx `20000` for at skære mellem zoom 14 og 15. Så er det entydigt, hvilke niveauer featuren er med på. Et andet grid, fx `25832`, har andre målestokke pr. zoom (se [Modul 03](../03-Tile-systemer)).

Style-filen skal stadig have egne `minzoom`/`maxzoom` og filtre for at styre, hvordan vejene tegnes. Klasserne styrer kun, hvad der er med i tilen.

## Maputnik – visuel style-editor

[Maputnik](https://maplibre.org/maputnik/) er en gratis browser-baseret editor til style-filer. Åbn din style-fil, ret farver/filtre visuelt og eksportér JSON igen. Maputnik viser også hvilke source-layers og attributter, der findes i tiles (`View → Inspect`).

## Nyttigt at vide

* **Cache og styling:** Ændres der kun i style-filen, skal cachen *ikke* ryddes. Ændres der i data eller i hvilke lag schemaet indeholder, skal cachen ryddes/re-seedes som for PNG.
* **Datamængde:** Vektor-tiles på lave zoom-niveauer kan blive store, fordi alle geometrier i tilen kommer med. Brug klasser til Udtynding af features i tiles.
* **Seeding:** MVT tilesets seedes med GC2-cli/GC2-app på samme måde som PNG – brug tileset-navnet med `.mvt`, fx `--layer geodk.mvt`.
* **Sikkerhed:** Tiles hentes med de samme rettigheder som lagene. Beskyttede lag kræver login/token – bedst er at baggrundskortets lag er offentlige.

## Øvelse

- Opsæt klasser så der sker udtynding af features i tiles.
- Åbn `geodk.json` i [Maputnik](https://maplibre.org/maputnik/) (`Open → Upload`) og zoom ind over dataene.
