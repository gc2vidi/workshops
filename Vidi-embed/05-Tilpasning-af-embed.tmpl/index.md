# Tilpasning af embed.tmpl

Standard templaten for indlejring hedder embed.tmpl. Dennes udseende kan til at vis grænse styres gennem et eksternt style-sheet. Hvordan eksterne style-sheets anvendes kan ses [her](https://vidi.readthedocs.io/en/latest/pages/standard/91_run_configuration.html#cssfiles). 

Typisk vil man gerne have fjernet visse knapper.

Her fjernes "Find mig", "Baggrundskort" og "Log ind" knapperne:

```css
#find-me-btn,
#base-layers-btn,
#burger-btn,
#full-screen-btn,
#about-btn,
#session,
#mapcontrols-history-backward,
#mapcontrols-history-forward
{
    display:none !important;
}
```

Hvis alle knapper (udover navigationsknapperne) skal fjernes, så kan det gøres med én regel:

```css
#floating-container-secondary
{
    display:none !important;
}
```

Navigationsknapperne kan fjerne ved:

```css
#floating-container-primary
{
    display:none !important;
}
```
