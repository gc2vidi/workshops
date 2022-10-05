# Hvordan virker editering

Vidi har et editor-modul, som gør det muligt at oprette, ændre og slette features i et hvilket som helst lag.

Vidi har et avanceret system til at sikre, at transaktioner bliver gennemført. Følgende trin sker når der editeres.

1. Vidi brugeren starter med at editere et lag og laver en indsæt, opdatering eller slet transaktion.
2. Transaktionen bliver IKKE sendt direkte til GC2/databasen, men bliver lagt i en "kø", som anvender browserens
   indbyggede database (IndexdDB). Dvs. en transaktion altid kan sendes uanset om der er netværk eller ej.
3. En baggrundsprocess tjekker løbende køen og forsøger at sende transaktionerne til GC2, hvis følgende kriterier er
   opfyldt:
    * Der er netværk.
    * Laget transaktionerne tilhører ikke er sat til offline.
4. Efter baggrundsprocessen har sendt en transaktion til GC2 bliver responsen tjekket. Hvis den er gennemført, slettes
   transaktionen fra køen. Hvis der er sket en fejl eller brugeren ikke har rettigheder til at gøre det, beholdes
   transaktionen i køen, og brugeren bliver notificeret og kan rette i transaktionen eller logge ind med de korrekte
   rettigheder.




