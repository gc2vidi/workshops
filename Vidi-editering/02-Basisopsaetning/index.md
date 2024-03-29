# Basisopsætning

Editeringsfunktionaliteten kommer gennem en extension kaldet `editor`. Når denne er aktiveret i en config, kan lag
editeres. Dog skal de enkelt lag have editering aktiveret gennem Meta funktionen.

[Manual for Editor indstillingen i Meta](https://vidi.readthedocs.io/da/latest/pages/standard/92_gc2_meta_information.html#editor)

I fleste tilfælde skal brugeren have rettigheder til at ændre på et lag, så `session` extension skal også være
aktiveret, således brugeren kan logge sig ind.

## Øvelse

1. Start med at upload datasættene. Opret et schama kaldet `workshop` til det. Alle data skal uploades med encoding UTF8 og
   EPSG:25832 (csv filerne indeholder ikke geometri, så her er EPSG underordnet).
2. Opret en ny config i Kontrolcenter med følgende indhold:

```json
{
  "schemata": [
    "workshop"
  ],
  "enabledExtensions": [
    "editor",
    "session"
  ]
}
```

3. Tjek af i Meta > Editor > Editable

![Editor i Meta](../assets/editor.png)

4. Vælg både Tile og Vector i Meta > Layer type

![Layertype i Meta](../assets/layertype.png)

5. Placer alle geometri-lag i grupper og søg for at `Authentication` er sat til `Write`.

![Grupper](../assets/groups.png)

