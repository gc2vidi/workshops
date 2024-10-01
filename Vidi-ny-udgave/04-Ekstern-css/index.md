# Ekstern CSS

Vidi kan hente eksterne CSS filer ved opstart. Formålet er at ændre udseende på Vidi - fx farver, størrelse, placering mv. af de forskellige elementer.

I en config ser det sådan ud:

```json
{
    "cssFiles": [
         "myStyles1.css",
         "myStyles2.css"
    ]
}
```

Eksisterende CSS-filer lavet til den gamle udgave af Vidi vil efter al sandsynlighed IKKE VIRKE i den nye udgave af Vidi. Enten vil filerne ikke have nogen effekt eller så vil effekten ikke være den tilsigtede.

Det skyldes, at CSS klasser og id'er er blevet ændret, så selectors i CSS filerne ikke matche noget eller det forskerte i Ny Vidi.

## CSS i Ny Vidi

Ny Vidi er baseret på Bootstrap 5.3 og planen er at opgradere løbende.

Der er ikke anvendt yderligere frontend toolkits eller plugins/extension til Bootstap, hvilket den gamle Vidi gjorde. Der er ej heller ændret på Bootstraps CSS gennem overskrivninger. Det gør, at det er veldokumenteret hvordan et givet element kan ændres gennem CSS.

Dokumentation til Boostrap 5.3 findes [her](https://getbootstrap.com/docs/5.3/getting-started/introduction/)

Boostrap er begyndt at [anvende CSS variabler](https://getbootstrap.com/docs/5.3/customize/css-variables/#component-variables) men desvære er det ikke muligt (endnu) at ændre fx den primære farve ved at overskrive en css variabel. Det skal stadig gøre via en SCSS variabel, som er et system, der "compiler" et mere avanceret stylesheet sprog til CSS, som browseren kan forstå. Men CSS variabler er taget i brug rundt omkring.

Man kan lave fx dette (kan sættes i et eksternt stylesheet):

```css
:root {
    --bs-body-bg: lightgreen;
    --bs-body-font-family: 'Montserrat', sans-serif;
}
```

Men man kan altså ikke gøre dette i et eksternt style-sheet:

```css
:root {
    --bs-primary: lightgreen;
}
````

Her skal man stadig lave denne SCSS overskrivning i Vidi's byggeproces, dvs. at det er noet, der sker når Vidi bliver installeret:

```scss
$primary: rgba(67,67,67,255);
```

> **_NOTE:_**  Scss kan ikke forstås af browseren, men bliver "kompileret" til CSS af et program kaldet Sass. SCSS har en masse udvidelser til CSS, såsom variabler.

Hvis man vil teste noget af, kan man bruge Chrome eller Edge udvilker-værktøj til at lave overrides med.

## Øvelse
1. Åben udvikler-værtøjet i Chrome/Edge og opret et inspector-stylesheet.
2. Prøv at ændre på nogle variable.





