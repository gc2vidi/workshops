# Opsætning af formular

Attribut formularen bliver dannet ud fra typerne i PostGIS tabellen.

Fx bliver et `timestamp` felt til en datovælger i formularen og et `bolean` felt bliver til en tjek-boks. Det betyder,
at man fx ikke kan få en datovælger for et tekst-felt selvom, at man godt kan gemme en dato-streng i et tekstfelt.

Det er muligt at gøre tal- og tekst-felttyper til "drop-down"-felter. Værdier i listen kan angives på tre måder:

### Reference-tabel

Værdier kan komme fra en anden tabel i databasen. Dette angives ved tre parameter i et JSON objekt:

```json
{
  "_rel": "schema.tabel",
  "_value": "feltnavn",
  "_text": "feltnavn"
}  
``` 

`_rel` angiver reference-tabellen (eller view) som schema-kvalificeret (schema-navnet skal angives foran tabelnavnet).

`_value` angiver feltet, som indeholder værdierne.

`_text angiver feltet, som indeholder den tekst, der skal vises i drop-down-listen. Dette felt kan godt være det samme
som ovenstående _value felt.

### Værdi-tekst liste

Værdier kan angive som en liste af værdi-tekst par i et JSON objekt. Dvs. at det er teksten (venstre side), som bliver
vist i drop-down-listen, mens værdien (højre side) bliver anvendt.

```json
{"tekst_1": "1", "tekst_2": "2", "tekst_3": "3"}
```

### Værdi liste

Værdier kan angives som en liste i et JSON array. Værdierne bliver vist i drop-down-listen.

```json
[1, 2, 3]
```

Listen kan både bestå af tal og tekster.

### Wild card

Der kan dannes en drop-down-liste af samtlige unikke værdier som allerede findes i feltet. Det gøres ved at indsætte * i
feltet.

[Link til manualen](https://vidi.readthedocs.io/da/latest/pages/standard/92_gc2_meta_information.html#egenskaber)

## Øvelse

1. Upload csv filerne i databasen. Følgende forudsætter, at de er uploaded til schemaet `workshop`.

2. I fritidslagene er der en del Ja/nej felter. I laget `t_5800_fac_pkt` prøv først at indsætte i `Egenskaber` for feltet `vandhane_k`:

```json
[1, 0]
```

og skift ud med:

```json
{"Ja": 1, "Nej": 0}
```

> **_Bemærk:_** at `vandhane_k` er et heltalsfelt, så værdier må ikke have `"` omkring

Og til sidst prøv denne reference-tabel: 

```json
{"_rel":"workshop.d_basis_ja_nej", "_value":"ja_nej_kode", "_text":"ja_nej"}
```

3. Prøv at anvende en andre reference tabeller. 

I feltet `cvr_kode` kan `d_basis_ansvarlig_myndighed` anvendes således:

```json
{"_rel":"workshop.d_basis_ansvarlig_myndighed", "_value":"cvr_kode", "_text":"cvr_navn"}
```

I feltet `saeson_k` kan `d_5800_saeson` anvendes således:

```json
{"_rel":"workshop.d_5800_saeson", "_value":"saeson_k", "_text":"saeson"}
```

