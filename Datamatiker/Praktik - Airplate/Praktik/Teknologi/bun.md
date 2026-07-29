1. [[bun#^d51967 |Beskrivelse]]
2. [[bun#^54d8de |Kommandoer]]



### Beskrivelse:
^d51967
Bun er en package manager til Node.js, ligesom npm, yarn eller pnpm. 

Kendetegn:
- **Hastighed**. Installerer pakker op til 25x gange hurtigere end andre pga af en global cache og optimerende system fil skrivninger.
- **Alsidig** Passer ind i alle projekter med en package.json fil.
- **bunx**. Starter npm pakker næsten med det samme, ligesom npx. 
### Kommandoer:
^54d8de
> 
>`bun run bringup --dev  --mode traefik --skip-alerts --skip-live-listener-check`

>Start server og dashboard på port 80 lokalt
>Stå i /main i airplateserver
>`bun api --local 80`

>Installer projekt dependencies
>`bun install`

>Installer pakke
>`bun add <pakke navn>`

> Start en pakke binary uden at installere den globalt
> `bunx <pakke navn>`

> Start dev frontend
>  VITE_API_HOST=localhost:3001 bun dev:default

