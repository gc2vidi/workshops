# Brug af CCS framework i templates

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
      <img style="width:100%; margin-bottom: 10px" src="{{geofafoto}}" />
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
