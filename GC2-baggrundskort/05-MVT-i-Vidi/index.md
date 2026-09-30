# MVT baggrundskort i Vidi

Vidi kan vise vektor-tiles som baggrundskort med typen `MVT`. Vidi bruger [MapLibre GL](https://maplibre.org/) til at tegne kortet ud fra en style-fil – den samme style-fil, som blev lavet i [Modul 04](../04-MVT-tiles).

## Opsætning

I modsætning til typen `gc2` peger `url` ikke på GC2, men på **style-filen**. Det er style-filen, der fortæller hvor tiles hentes fra (under `sources`).

```json
{
    "type": "MVT",
    "url": "https://mit-domæne.dk/styles/geodk.json",
    "id": "geodk_mvt",
    "name": "GeoDanmark (vektor)",
    "description": "GeoDanmark baggrundskort som vektor-tiles",
    "attribution": "&copy; SDFE & MapCentia ApS",
    "minZoom": 8,
    "maxZoom": 22,
    "maxNativeZoom": 20
}
```

| Egenskab | Betydning |
|---|---|
| `type` | Skal være `MVT` |
| `url` | URL til MapLibre style JSON |
| `id` | Unikt id for baggrundskortet |
| `name`/`description` | Vises i baggrundskort-vælgeren |
| `attribution` | Kildehenvisning |
| `minZoom`/`maxZoom`/`maxNativeZoom` | Som for de øvrige baggrundskort |

Style-filen kan også pege på helt eksterne vektor-tiles, fx fra MapTiler:

```json
{
    "type": "MVT",
    "url": "https://api.maptiler.com/maps/topo/style.json?key=[din nøgle]",
    "id": "maptiler_topo",
    "name": "Open Street Map (vektor)",
    "attribution": "&copy; MapTiler &copy; OpenStreetMap contributors",
    "minZoom": 8,
    "maxZoom": 20,
    "maxNativeZoom": 19
}
```

## Hvor skal style-filen ligge?

Style-filen skal kunne hentes af browseren, og serveren skal tillade CORS. Muligheder:

* **Vidi's egen `public` mappe** – fx `public/mvt/geodk.json`, som så kan tilgås på `https://[vidi-host]/mvt/geodk.json`. Kræver adgang til Vidi-installationen.
* **GitHub Pages** – læg filen i et repo med Pages slået til (på samme måde som Vidi config-filer, fx `https://[bruger].github.io/[repo]/geodk.json`).
* **En hvilken som helst webserver** med CORS-headeren `Access-Control-Allow-Origin`.

Da stylingen ligger i en separat fil, kan kortets udseende ændres ved blot at opdatere filen – uden at røre Vidi-config eller tile-cache.

## Begrænsninger

* MVT baggrundskort i Vidi bruger altid **web mercator** (grid `g20` i GC2). Et UTM-grid som `25832` (se [Modul 06](../06-Tile-systemer)) kan ikke bruges til MVT i Vidi.
* Der er ingen gennemsigtigheds-skyder på MVT baggrundskort i baggrundskort-vælgeren. Gennemsigtighed styres i style-filen (`fill-opacity` osv.).
* Labels kræver at `glyphs` er sat i style-filen, og at skrifttypen i `text-font` findes på glyph-serveren.

## Øvelse

- Læg den style-fil, du lavede i Modul 04, et sted hvor den kan hentes (fx GitHub Pages).

- Tilføj MVT-baggrundskortet til din Vidi config ved siden af raster-versionen fra [Modul 02](../02-Vidi-opsaetning):

```json
{
    "schemata": [
        "public"
    ],
    "brandName": "Base layer test",
    "baseLayers": [
        {
            "id": "osm",
            "name": "Open Street Map"
        },
        {
            "type": "gc2",
            "id": "geodk",
            "name": "GeoDanmark kort (raster)",
            "db": "workshop",
            "host": "https://swarm.gc2.io",
            "config": {
                "minZoom": 8,
                "maxZoom": 22,
                "maxNativeZoom": 20,
                "attribution": "&copy; SDFE & MapCentia ApS"
            }
        },
        {
            "type": "MVT",
            "url": "https://[bruger].github.io/[repo]/geodk.json",
            "id": "geodk_mvt",
            "name": "GeoDanmark kort (vektor)",
            "attribution": "&copy; SDFE & MapCentia ApS",
            "minZoom": 8,
            "maxZoom": 22,
            "maxNativeZoom": 20
        }
    ]
}
```

- Skift mellem raster- og vektor-udgaven. Zoom ind og ud og læg mærke til forskellen i skarphed – især mellem zoom-niveauerne.

- Ret en farve i style-filen, publicér den igen og genindlæs Vidi. Bemærk at cachen i GC2 ikke skulle ryddes.
