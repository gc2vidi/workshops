# Klassificering

Indtil nu har vi anvendt én stilart på alle objekter. Men det er muligt at returnere forskellige stilarter ud fra
objektets attribut-værdier.

Følgende er en unik klassificering af laget `t_5801_fac_fl` med feltet `facil_ty_k` bruges (bemærk at stien i feltet
er `feature.properties.facil_ty_k`)

```javascript
function (feature) {
    switch (feature.properties.facil_ty_k) {
        case 2121:
            return {
                color: 'ForestGreen',
                weight: 2,
                opacity: 0.8,
                fillColor: 'ForestGreen',
                fillOpacity: 0.5
            }
            break;
        case 2111:
            return {
                color: 'YellowGreen',
                weight: 2,
                opacity: 0.8,
                fillColor: 'YellowGreen',
                fillOpacity: 0.5
            }
            break;
        case 2171:
            return {
                color: 'DarkSeaGreen',
                weight: 2,
                opacity: 0.8,
                fillColor: 'DarkSeaGreen',
                fillOpacity: 0.5
            }
            break;
    }
}
```

Det er kun farven, der ændres i de forskellige klasser, så man kan gøre det lidt kortere ved at bruge en variabel:

```javascript
function (feature) {
    var color;
    switch (feature.properties.facil_ty_k) {
        case 2121:
            color = 'ForestGreen';
            break;
        case 2111:
            color = 'YellowGreen';
            break;
        case 2171:
            color = 'DarkSeaGreen';
            break;
    }
    return {
        color: color,
        weight: 2,
        opacity: 0.8,
        fillColor: color,
        fillOpacity: 0.5
    }
}
```

På samme måde kan Point-to-layer funktionen anvendes til at tildele forskellige markører. Her bruges `default` case, som
returnerer en circleMarker til de punkter, der ikke skal have en ExtraMarkers ikon. Denne Point-to-layer funktion kan
anvendes på laget `t_5801_fac_fl`:

```javascript
function (feature, latlng) {
    var icon, color, shape;
    switch (feature.properties.facil_ty_k) {
        case 1222:
            icon = 'fas-faucet';
            color = 'blue';
            shape = 'circle'
            break;
        case 1122:
            icon = 'fas-biking';
            color = 'orange';
            shape = 'penta'
            break;
        default:
            return L.circleMarker(latlng)
    }
    return L.marker(latlng, {
        icon: L.ExtraMarkers.icon({
            icon: icon,
            markerColor: color,
            shape: shape,
            iconColor: '#fff',
            prefix: 'fas'
        })
    });
}
```

## Øvelse

Indsæt en Style funktion og Point-to-layer funktion i et lagene. Prøv at ændre på nogle egenskaber.

Du kan også prøve det [direkte i Vidi](https://vidi.swarm.gc2.io/app/demo/workshop)
