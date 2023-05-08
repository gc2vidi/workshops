# Templates fortsat

I dette modul vil vi dykke lidt mere ned i HTML delen af templates.

## Link

Hvis man har en felt med web-side adresser, kan man bruges a-tagget (anchor) til at skabe hyper-links.   

Her støder vi på de såkaldte HTML "attributter" (ikke at forveksle med attributter i GIS sammenhæng): 

```html
<a href="{{link}}">Link til hjemmeside</a>
```

Ofte vil man gerne have linket til at åbne i et nyt fane, som kan klares med `target` attributten:

```html
<a target="_blank" href="{{link}}">Link til hjemmeside</a>
```

> **_NOTE:_**  Det er brugerens browseropsætning, der styres om siden skal åbnes i en ny fane eller nyt vindue.

## Billed

Ligeledes hvis man har et felt med links til billeder, kan man bruge img-tagget (image) til at vise billeder

```html
<img src="{{geofafoto}}" />
```

Typisk vil man gerne have skaleret billedet ned til bredden af pop-up'en. Det kan gøres med `style` attributten, som indeholder 
css regler (dem er der mange af!). Her bruger vi `width` reglen:

```html
<img style="width:100%" src="{{geofafoto}}" />
```

> **_NOTE:_**  Det er kun nødvendigt at skalere i bredden, da browseren selv sørger for at tilpasse højden.

## Komplet eksempel

Vi kan du sammensatte en komplet template af forskellige felt-værdier:

```html
<img style="width:100%" src="{{geofafoto}}" />
<h5>{{navn}}</h5>
<p>{{beskrivels}}</p>
<a target="_blank" href="{{link}}">Link til hjemmeside</a>
```

Ved at omslutte det hele i en generisk container kaldet et `div` element, kan vi styre fx tekstjustering: 

```html
<div style="text-align:center">
    <img style="width:100%; margin-bottom: 10px" src="{{geofafoto}}" />
    <h5>{{navn}}</h5>
    <p>{{beskrivels}}</p>
    <a target="_blank" href="{{link}}">Link til hjemmeside</a>
</div>
```

> **_NOTE:_**  Indrykningen har ingen teknisk betydning, men gør templaten lettere at læse.

## Øvelse

1. Skriv en pop-up template, som bruger flere felter. Tag evt. udgangspunkt i ovenstående eksempler.
