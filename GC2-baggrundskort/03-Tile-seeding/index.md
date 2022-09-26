# Tile-seeding

Tiles kan seedes, som betyder, at tiles bliver skabt på forhånd. Seeding sker ved brugen af gc2-cli værktøjet.

Her er "help" outputtet fra seed:start kommandoen:

```
$ gc2 seed:start --help

Starts a seed job

USAGE
  $ gc2 seed:start

OPTIONS
  -e, --end=end          (required) End zoom level (the higher number)
  -f, --force            Force seed job - overwrites existing tiles
  -g, --grid=grid        (required) Grid to use
  -h, --help             show CLI help
  -l, --layer=layer      (required) Layer to seed [schema].[relation]
  -n, --name=name        (required) Name of seed job
  -s, --start=start      (required) Start zoom level (the lower number)
  -t, --threads=threads  Number of parallel threads that should be used to request tiles from the WMS source
  -x, --extent=extent    (required) Polygon layer which set the extent for the seeding [schema].[relation]
  ```

  ## Installation af gc2-cli i Windows

  Downbload gc2-cli:   

  https://gc2-cli.s3-eu-west-1.amazonaws.com/gc2-win32-x64.tar.gz

  I Powershell udpak filen:    

  `tar -xvzf gc2-win32-x64.tar.gz`
  
  Test af gc2-cli:    
  
  `cd gc2\bin`    
  `.\gc2`
  
  Typisk vil man tilføje `gc2` exec-filen til "system-stien", så værktøjet kan started med `gc2` fra alle steder i filsystemet.

## Øvelse

- Forbind gc2-cli til GC2 med: 
`gc2 connect`

- Login med gc2-cli med: 
`gc2 login`

- Start et seed job med (tilret evt. argumenterne): 
`gc2 seed:start --end 19 --start 14 --grid g20 --layer geodk --name geodk --threads 2 --extent geodk.extent`

- Se om jobbet kører: 
`gc2 seed:list`

- Se hvor langt jobbet er kommet: 
`gc2 seed:log -uuid=874b7493-fe7f-412e-b8ba-96fe731b8ef7` skift uuid til det, som vises i seed:list kommandoen)

