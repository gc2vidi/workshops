# Brug af CCS framework i templates

Vidi anvender et CSS framework kaldet [Bootstrap](https://getbootstrap.com/docs/5.2/getting-started/introduction/), som 
definerer en række såkaldte css-classes. Classes er en eller flere css regler, 
der samles under et class-navn og derigennem kan tilknyttes et html-element.  

Det er en god ide at anvende classes i stedet for eksplicitte style attributter. Derved følger dine templates de ændringer, 
der evt. bliver fortaget i det overordnet. Fx i stedet for at skrive Vidi primær-farve direkte ind i en style-attribut, kan 
der anvendes en class, som tildeler den anvendte primær-farven. Derved behøver du ikke rette i templates, hvis du ændrer primær-farven.  

{% raw %}
```handlebars
<div class="text-center">
    <img class="w-100 mb-4" src="{{geofafoto}}" />
    <h5 class="text-primary">{{navn}}</h5>
    <p>{{beskrivels}}</p>
    <a target="_blank" href="{{link}}">Link til hjemmeside</a>
</div>
```
{% endraw %}

> **_NOTE:_** De viste eksempler bruger classes fra Bootstrap 5.2, og kan derfor kun anvendes i Ny Vidi.

Bootstrap' css classes gør det også let at lave egentlige widgets i templates. Nedenfor er et eksempel på en faneblad struktur:

{% raw %}
```handlebars
<ul class="nav nav-tabs" id="myTab">
  <li class="nav-item">
    <button class="nav-link active" id="home-tab" data-bs-toggle="tab" data-bs-target="#billed" type="button" role="tab" aria-selected="true">Billed</button>
  </li>
  <li class="nav-item">
    <button class="nav-link" id="profile-tab" data-bs-toggle="tab" data-bs-target="#tekst" type="button" aria-selected="false">Tekst</button>
  </li>
</ul>
<div class="tab-content" id="myTabContent" style="height: 250px">
  <div class="tab-pane fade show active" id="billed" tabindex="0">
      <img class="w-100 mb-4" src="{{geofafoto}}" />
  </div>
  <div class="tab-pane fade" id="tekst" tabindex="0">
      <div style="text-align:center">
          <h5>{{navn}}</h5>
          <p>{{beskrivels}}</p>
          <a target="_blank" href="{{link}}">Link til hjemmeside</a>
      </div>
  </div>
</div>
```
{% endraw %}

## Øvelse

Hvis man skal afprøve css-classes kræver det, at du anvender Ny Vidi med Bootstrap 5.2.

1. Prøv at tilføje nogle classes til din template

