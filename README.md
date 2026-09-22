# Painel UFABC Next

## Executar
Com Node.js 22.9+ no Windows, macOS ou Linux:

1. Copie `.env.example` para `.env` e preencha `MONGODB_URI` (com acesso de leitura), `MONGODB_DB` e `MONGODB_COLLECTION`.
2. `npm install`
3. `npm start` e abra http://localhost:3000.

## Docker (opcional)

```bash
docker compose up -d
```

Se mudar o .env reexecute o container com `docker compose up --force-recreate` para atualizar o .env
