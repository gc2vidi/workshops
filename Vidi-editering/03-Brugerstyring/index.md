# Brugerstyring

Typisk vil man have kontrol over hvem, der må editere de enkelte lag. Her er det en god ide, at oprette sub-brugere til
de forskellige personer og organisationer, der skal kunne editere.

En sub-bruger er en bruger, som er tilknyttet databasen. En sub-bruger har som udgangspunkt ikke rettigheder til nogle
lag.

Hvis flere sub-brugere skal have de samme privilegier, kan man oprette en sub-bruger, der kan fungere som "gruppe". De
andre sub-brugere kan så indstilles til at nedarve privilegierne fra denne.

# Øvelse

1. Opret en sub-bruger i Kontrolcenter. Prøv evt. at lade en "gruppe"-bruger styre privilegierne.
2. Tildel `Læse og skrive` privilegier til sub-brugeren på tre frilufts-lag.

![privilegier](../assets/privileges.png)
