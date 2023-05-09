# Logiske udtryk i templates

Det er muligt at anvende logiske udtryk og løkker i templates. Det mest almindelig er brugen af logiske udtryk, så 
denne workshop vil kun gennemgå dette.

## Hvad er et logisk udtryk

Et logisk udtryk vil enten resultere i `sand` eller `falsk` og kan bruges til at teste om en felt-værdi er tom.

{% raw %}
```handlebars
<div style="text-align:center">
    <img style="width:100%; margin-bottom: 10px" src="{{geofafoto}}" />
    <h5 style="color: darkgray">{{navn}}</h5>
    <p>{{beskrivels}}</p>
    {{#link}}
        <a target="_blank" href="{{link}}">Link til hjemmeside</a>
    {{/link}}
    {{^link}}
        <p><i style="color: silver">Ingen link</i></p>
    {{/link}}
</div>
```
{% endraw %}

Den første {% raw %}`{{#link}} {{/link}}` {% endraw %} tester om feltet `link` er udfyldt og den sidste {% raw %}`{{#link}} {{/link}}`{% endraw %} tester om det er tomt. 
Hvis udtrykket er sandt, udskrives det, der står mellem markeringerne. I første tilfælde udskrives et HTML a-elementet (link) og hvis sidste er 
sandt udskrives en kursiv tekst, med oplysning om, at der ingen link er.

Template systemet, der anvendes i Vidi, hedder [Mustache](https://mustache.github.io/mustache.5.html), som er meget simpelt. 

Det dog muligt, at anvende en overbygning på `Mustache`, der [Handlebars](https://handlebarsjs.com), der udvider template systemet. 

> **_NOTE:_**  Fejl i åbn/lukke markeringen ved logiske udtryk, vil resultere i en template fejl. Denne fejl bliver vist i browserens konsol.

## Øvelse

1. Udbyg din template med et logisk udtryk.
