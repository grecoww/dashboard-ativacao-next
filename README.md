# Painel UFABC Next

Servidor Node.js e frontend em HTML, CSS e JavaScript. Identidade Nexus: Exo 2 local, fundo #efefef, preto #000009, azul #3852ac e vermelho #ec0a00.

## Executar

Com Node.js 22.9+ no Windows, macOS ou Linux:

1. Copie `.env.example` para `.env` e preencha `MONGODB_URI` (com acesso de leitura), `MONGODB_DB` e `MONGODB_COLLECTION`.
2. Execute `npm ci` na primeira utilização.
3. Execute `npm start` e abra http://localhost:3000.

Ctrl+C encerra. O navegador não abre automaticamente.

## Estrutura

- `server.mjs`: cria o MongoClient, fornece `GET /api/comments` e serve os arquivos de `public/` no mesmo localhost. A conexão ocorre na primeira consulta.
- `public/app.js`: consulta a API a cada 5 segundos, atualiza o painel e controla a meta (variável `GOAL` no `.env`, padrão 100).
- `public/style.css`: guia visual; Exo 2 e sua licença estão incluídas em `public/fonts/`.

O banco e a coleção são definidos por `MONGODB_DB` e `MONGODB_COLLECTION` no `.env`. Conta comentários ativos, com texto não vazio, criados hoje em São Paulo. Retorna apenas `day`, `total`, `latestCommentAt` e `goal`. Não consulta RA nem envia conteúdo, professores ou IDs ao navegador. Edições de comentários antigos não aumentam a contagem de hoje.

O primeiro resultado estabelece a base. Quando o horário do último comentário avança, aparece um aviso genérico. Vários comentários entre consultas são agrupados: não há histórico individual, códigos, demonstração ou sorteio. Em erro, a API retorna 503; o frontend mantém o último total com aviso de desconexão. Comentários removidos deixam de contar; contribuições criadas e removidas entre consultas não são detectadas.

A contagem inclui toda a plataforma no dia, não apenas a ativação.

## Docker opcional

```bash
docker compose up --build -d
```

Ou sem Compose:

```bash
docker build -t painel-next .
docker run --rm --env-file .env -p 127.0.0.1:3000:3000 painel-next
```

Abra http://localhost:3000. MongoDB continua externo. O container executa apenas o painel. A connection string não entra na imagem. O serviço não possui autenticação e deve permanecer local.

O Compose monta `server.mjs` e `public/` do notebook no container. Salvar o servidor aciona o `node --watch`; para mudanças no frontend, recarregue o navegador. Após mudar o `.env`, execute `docker compose up -d --force-recreate`. Mudanças nas dependências ou no Dockerfile exigem `docker compose up --build -d`.
