# Vidi-opsætning

I en Vidi config kan du nu opsætte baggrundskortet. Det gøres med typen `gc2`. I `id` angives det schema, som indeholder baggrundskortet.

## Øvelse

- Opsæt dit baggrundskort i Vidi. Du kan medtage fx `osm` kortet for at kunne orienterede dig, hvis noget går galt.

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
            "name": "GeoDanmark kort",
            "db": "worshop",
            "host": "https://swarm.gc2.io",
            "config": {
                "minZoom": 8,
                "maxZoom": 22,
                "maxNativeZoom": 20,
                "attribution": "&copy; SDFE & MapCentia ApS"
            }
        }
    ]
}
```