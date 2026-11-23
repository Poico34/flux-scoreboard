# Flux Scoreboard 6.0

Placar esportivo para OBS com controle pelo celular, editor de camadas, régua de posicionamento e integração Canva.

## Site de produção
https://flux-scoreboard.vercel.app/

## Canva: dois modos

1. **Incorporação** (funciona sem OAuth): publique o design no Canva em Compartilhar → Incorporar, e cole o link público no editor. O Canva controla a atualização do iframe. Não garante transparência.
2. **Canva Connect API** (precisa ser ativado): conecte a conta pelo OAuth PKCE; o backend consulta o design, exporta como PNG e armazena a arte no Vercel Blob. O placar usa a versão da imagem sem reiniciar os dados da partida. Para fundo transparente, a conta Canva precisa ter capacidade premium.

## Configurar Canva Developers

1. Crie uma aplicação apropriada em https://www.canva.dev/ e habilite a Canva REST API.
2. No app, habilite os scopes `design:meta:read` e `design:content:read`.
3. Adicione o redirect URI **exatamente** `https://flux-scoreboard.vercel.app/api/canva/callback`.
4. No painel Vercel do projeto `flux-scoreboard`, em **Settings → Environment Variables**, adicione:
   - `CANVA_CLIENT_ID`: ID da integração no Canva Developers.
   - `CANVA_CLIENT_SECRET`: segredo da integração (marque como Sensitive).
   - `CANVA_ENCRYPTION_KEY`: uma chave aleatória **com pelo menos 32 bytes** (marque como Sensitive), nunca a armazene no repositório.
   - `CANVA_REDIRECT_URI`: `https://flux-scoreboard.vercel.app/api/canva/callback`
5. Implante novamente após adicionar as variáveis para ativar a integração.

**Nunca envie Client Secret ou tokens no chat.** Mantenha os segredos apenas no dashboard da Vercel. A conexão entre Canva e o jogo é armazenada criptografada no Blob.

## Fluxo da versão 6.0

- Crie uma partida e mantenha o link de controle com chave em segredo.
- Vá ao **Editor gráfico** → **Canva Connect API — Sincronizar PNG** → **Conectar conta Canva**.
- Autorize no Canva; retorne ao editor e informe o URL de um design com acesso autorizado.
- Clique **Sincronizar PNG agora** e, se o Canva ainda estiver exportando, clique novamente para concluir.
- Marque **Usar PNG exportado na saída OBS**. Abra o link público da saída OBS.
- No editor, pode selecionar sincronização manual ou verificação a cada 5/15 minutos **enquanto o editor estiver aberto**.

## Observações de produção

- As funções de servidor devem ter acesso ao Vercel Blob privado e ao token `BLOB_READ_WRITE_TOKEN`.
- O endpoint de criação de partidas ainda é público e sem rate limit; para tráfego amplo, adicione autenticação e limite de requisições.
- As contas de Canva são conectadas **por partida** nesta versão. Não há ainda um sistema de usuários.
- O monitoramento automático não funciona quando o editor está fechado. Para operações contínuas, configure tarefa agendada e controle de custos.
- O API do Canva tem limites e a exportação em transparência pode exigir Canva Pro.
- Teste a integração OAuth e exportação em um design real antes de usar numa transmissão oficial.

## Rodar localmente

`npm install` e `npm start` para o antigo servidor Node. A versão de produção está estruturada em funções serverless em `api/` e Vercel Blob, com deploy pela Vercel.
