# Hvordan virker rasterlag
Vi indleder lige med en forklaring på hverdan rasterlag virker, så forskellen til vektorlag bliver tydeliggjort.

I alle tilfælde findes data i en vektormodel i databasen (som geometrier). Når et lag tændes i Vidi sendes en request til GC2, som beder MapServer eller QGIS server om at skabe et billede af det valgte lag i det valgte udsnit.
MapServer/QGIS Server forbinder så til databasen og henter de nødvendige data. Derefter skaber den et billede (typisk en PNG) ud fra den styling, som findes i MapFilen eller QGIS projekrfilen.
Når billedet er klart sendes det tilbage til Vidi. Zoomer eller panorerer Vidi brugeren gentages hele processen med at skabe et billede og sende det til Vidi.

Så hver gang en Vidi bruger tænder og zoomer rundt i lag, vil der blive dannet billeder på GC2 serveren, som passer til de ønskede udsnit i Vidi. Så denne metode kræver en del netværkstrafik og processerkraft, som stiger med antallet af Vidi brugere.

Så ulemperne kan opsummeres således:

* Kræver en del netværkstrafik og processerkraft og kan ikke opskaleres uden tilsvarende opskalering af ressourcer.
* Styling er fastsat centralt i GC2 og kan ikke ændres af Vidi brugeren - alle får den samme styling af et givet lag.
* Rasterlag er "døde" billeder, og feature-info, mouse-over og editering kræver yderligere kald til GC2 og databasen.

Men rasterlag har også en række fordele:

* Kan hurtigt visualiserer store mængder data. Det er intet problem at skabe et billede af en million punkter.
* Kan skabe meget avanceret styling og labels.
* Stiller ingen krav til Vidi brugerens computer, da denne bare viser billedet af data (det er lige meget om det indeholder 10 eller en million punkter).

