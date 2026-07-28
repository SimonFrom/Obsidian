[Link til Vite docs](https://vite.dev/)
[Intro til Vite](https://scrimba.com/intro-to-vite-c03p6pbbdq?via=vite)

1. [[Vite#^02452a |Beskrivelse]]
2. [[Vite#^edd94f |Kommandoer]]



### Beskrivelse: 
^02452a
Vite er en bundler/build tool/dev server til moderne webudvikling. 
Normalt vil en bundler samle *hele* ens app før at noget bliver sendt videre til serveren, selv i dev miljøer. 
Jo større projekt, jo længere tid tager det. 

Vite gør det anderledes. Det bliver splittet op i to miljøer, dev og prod. 
- **Dev**: 
  Vite tager kun det mest nødvendige og sender til browseren. Når noget andet skal bruges, kan man via native ES moduler (import / export) nemt hente det nye indhold ind. Det giver en ekstremt hurtig opstart og kan derved gøre brug af **HMR / Hot Module Replacement** fordi at laver man ændring i en fil, er det kun det nødvendigt at hente nøjagtig den fil ind igen.
- **Prod**:
  Beder man derimod om et **prod** build, vil Vite gøre som man normalt kender det, ved hjælp **Rollup**. Nemlig bygge alt sammen så alt bliver hentet på samme tid og er klar til brug. Brugerne har ikke brug for **HMR** og det er mere effektivt at gøre det på denne måde. 


### Kommandoer:
^edd94f

> Start dev server med AirPlate theme sat
> `bun dev:default`

> Start prod build
> `bun run build`