# Bootcamp Treinos API

API em Node.js (Fastify + Prisma + PostgreSQL) com ambiente de desenvolvimento via Docker Compose.

## Requisitos

- Docker
- Docker Compose

## Subir o projeto

```bash
docker compose up -d
```

Serviços criados:

- `postgres`: banco PostgreSQL (`localhost:5432`)
- `backend`: API Node/Fastify (`localhost:3000`)
- `prisma-studio`: Prisma Studio (`localhost:51212`)

## URLs úteis

- API: http://localhost:3000
- Docs (Scalar): http://localhost:3000/docs
- Prisma Studio: http://localhost:51212

## Logs

Ver todos os logs:

```bash
docker compose logs -f
```

Ver só backend:

```bash
docker compose logs -f backend
```

Ver só prisma studio:

```bash
docker compose logs -f prisma-studio
```

## Banco de dados (Prisma)

Os comandos abaixo executam dentro do container `backend`.

### Criar/aplicar migration local

```bash
docker compose exec backend pnpm prisma migrate dev --name nome_da_migration
```

Exemplo:

```bash
docker compose exec backend pnpm prisma migrate dev --name add_workout_table
```

### Aplicar migrations existentes (fluxo mais próximo de produção)

```bash
docker compose exec backend pnpm prisma migrate deploy
```

### Atualizar banco sem migration (db push)

```bash
docker compose exec backend pnpm prisma db push
```

### Gerar Prisma Client

```bash
docker compose exec backend pnpm prisma generate
```

### Abrir Prisma Studio manualmente (opcional)

Já existe um serviço dedicado no compose, mas se quiser rodar manualmente:

```bash
docker compose exec backend pnpm prisma studio --port 51212 --browser none
```

## Acesso ao PostgreSQL

Abrir `psql` dentro do container do banco:

```bash
docker compose exec postgres psql -U postgres -d bootcamp-treinos
```

Comandos úteis no `psql`:

```sql
\dt
SELECT now();
```

## Variáveis de ambiente

O projeto usa `.env`, e no Docker Compose o `DATABASE_URL` do container é sobrescrito para apontar para o serviço `postgres`:

- interno no Docker: `postgresql://postgres:password@postgres:5432/bootcamp-treinos`

Se você mudar o nome do banco, atualize:

- `POSTGRES_DB` no `docker-compose.yml`
- `DATABASE_URL` em `backend.environment`
- `DATABASE_URL` em `prisma-studio.environment`
- (opcional) `.env` local para uso fora do Docker

## Parar e limpar

Parar serviços:

```bash
docker compose down
```

Parar e remover volumes (apaga dados do banco):

```bash
docker compose down -v
```
