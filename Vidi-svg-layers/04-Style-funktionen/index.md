# Style funktionen
Et vektorlag dannes gennem en funtionen. Man skal forestille sig, at hver feature (geometri og attributer) i laget bliver sendt til en funktion. Funktion returnere en style, som det enkelte objekt får.

Ud udgangspunkt har vektorlag en blå farve

```javascript
function(feature) {
    return {
        color: 'green',
        weight: 2,
        fillColor: 'red',
        opacity: 0.5,
        fillOpacity: 0.5,
        radius: 15
        }
    }
```

