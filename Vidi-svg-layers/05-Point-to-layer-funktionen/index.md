

```javascript
function(feature, latlng) {
    return L.marker(latlng, {
        icon: L.ExtraMarkers.icon({
            icon: 'fa-home',
            markerColor: 'blue',
            shape: 'circle',
            prefix: 'fa',
            iconColor: '#fff'
        })
    });
}
```

https://fontawesome.com/v4/icons/