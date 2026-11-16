# Flux Scoreboard 2.0

Sistema web de placares ao vivo para OBS Studio. Painel de controle para computador e celular, sincronização Socket.IO e editor gráfico com posicionamento de elementos.

## Executar

Requer Node.js 20+. Execute `npm install` e `npm start`. Acesse `http://localhost:3000` e crie uma partida. O painel privado contém uma chave `?key=` que deve ser mantida em segredo. O painel oferece o link público `/overlay/ID` para adicionar no OBS como **Fonte de navegador**, em 1920 × 1080.

## Render

O arquivo `render.yaml` configura uma instância de teste no plano `free`. No Render, importe este repositório ou utilize o Blueprint. A hospedagem pode exigir cadastramento de pagamento, mesmo para o plano gratuito.

### Limitações importantes

- A versão gratuita pode entrar em suspensão, interrompendo a conexão entre operadores e OBS.
- Os dados são gravados em `games.json` no sistema de arquivos local e podem desaparecer quando a instância reiniciar ou for reimplantada.
- O endpoint de criação `/api/games` não tem autenticação ou limites por usuário, portanto não é adequado para uso público intensivo.
- Para transmissões oficiais, implemente banco de dados persistente, contas de usuários e infraestrutura que permaneça ativa.
- A saída pública mostra somente dados de partida; **não divulgue links do controle com a chave**.

## Endpoints

- `/`: criar partida
- `/control/:id?key=...`: painel do operador
- `/editor/:id?key=...`: editor visual
- `/overlay/:id`: fonte para OBS
- `/health`: saúde do servidor
