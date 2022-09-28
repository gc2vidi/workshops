# Hvordan virker vektorlag
Til forskel fra rasterlag, hvor et PNG billede af data sendes til Vidi, bliver selve dataene sendt i rå udgave til Vidi. Her i Vidi bliver laget så dannet som et "billede" i SVG format.
SVG står for Scalable Vector Graphic og et vektorbaseret billedformat, som browsere kan håndtere. Når først SVG billedet er dannet kan Vidi brugeren zoome rundt uden, at der skal hentes nye data fra GC2.
Det betyder, at presset på GC2/databasen er betydelig mindre end ved rasterlag. Til gengæld er det ude i brugernes browsere arbejdet sker.

Fordele ved vektorlag:
* Data hentes én gang fra GC2 hvorefter ingen yderligere requests bliver sendt. Heller ikke ved feature-info og mouse-over.
* Kan opskaleres til mange brugere uden, at server-ressourcer tilsvarende skal skaleres.
* Dannelsen af laget sker ude i Vidi og derfor kan vektorlag styles på forskellig vis for de enkelte brugere.
* Punktlag kan visualiseres som "markører", der kan være lettere at aflæse for "almindelige" web-brugere.
* Klassificering sker vha. JavaScript og kan derfor laves meget avanceret.
* Da dataene findes i Vidi kan de også vises i tabelformat sammen med kortet.

Men vektorlag har også nogle ulemper:
* Er ikke veleget til store mængder data. Alle data skal først flyttes fra GC2 til Vidi og SVG billedet bliver meget tungt.
* Ingen advanceret styling og labels. Styling er begrænset af SVG standarden og det underlæggende Leaflet framework.
* Klassificering sker vha. JavaScript og kan derfor være noget svær at håndtere.
