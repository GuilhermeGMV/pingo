# Pingo

> [Português](#português) | [English](#english)

---

## English

Full-stack monorepo application built with **React** + **Fastify**, managed with **pnpm workspaces** and **Turborepo**.

### Tech Stack

| Layer            | Technology                                              |
| ---------------- | ------------------------------------------------------- |
| Frontend         | React 18, Vite, Tailwind CSS 4, shadcn/ui, React Router |
| Backend          | Fastify, Prisma 7, Zod, Pino                            |
| Database         | PostgreSQL 16                                           |
| Cache            | Redis 7                                                 |
| Monorepo         | pnpm Workspaces, Turborepo                              |
| Language         | TypeScript 5                                            |
| Formatting       | Prettier                                                |
| Containerization | Docker, Docker Compose                                  |

### Project Structure

```
pingo/
├── apps/
│   ├── backend/          # Fastify API (port 3001)
│   └── frontend/         # React SPA (Vite dev server)
├── packages/
│   └── shared/           # Shared types & schemas (Zod)
├── docker-compose.yml    # Full stack with Postgres, Redis, backend & frontend
├── turbo.json            # Turborepo pipeline config
└── pnpm-workspace.yaml   # Workspace definition
```

### Prerequisites

- **Node.js** >= 18
- **pnpm** >= 10
- **Docker** & **Docker Compose** (for containerized setup)

### Running Locally

1. **Install dependencies**

```bash
pnpm install
```

2. **Start infrastructure** (PostgreSQL + Redis)

```bash
docker compose up postgres redis -d
```

3. **Set environment variables**

Create `apps/backend/.env`:

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5434/pingo
JWT_SECRET=your-secret-key-at-least-32-characters-long
```

4. **Run database migrations**

```bash
cd apps/backend
pnpm exec prisma migrate dev
```

5. **Start all apps in dev mode**

```bash
pnpm dev
```

This starts both frontend and backend via Turborepo:

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:3001`

### Running with Docker

1. **Create a `.env` file** in the project root:

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=pingo
JWT_SECRET=your-secret-key-at-least-32-characters-long
VITE_API_URL=http://localhost:3001
```

2. **Build and start all services**

```bash
docker compose up --build -d
```

This will start:

- **PostgreSQL** on port `5434`
- **Redis** on port `6379`
- **Backend** on port `3001`
- **Frontend** on port `80`

3. **Stop all services**

```bash
docker compose down
```

### Useful Commands

| Command             | Description                        |
| ------------------- | ---------------------------------- |
| `pnpm dev`          | Start all apps in development mode |
| `pnpm build`        | Build all apps and packages        |
| `pnpm lint`         | Type-check all projects            |
| `pnpm format`       | Format code with Prettier          |
| `pnpm format:check` | Check code formatting              |
| `pnpm clean`        | Remove all build outputs           |

---

## Português

Aplicação full-stack monorepo construída com **React** + **Fastify**, gerenciada com **pnpm workspaces** e **Turborepo**.

### Tecnologias

| Camada          | Tecnologia                                              |
| --------------- | ------------------------------------------------------- |
| Frontend        | React 18, Vite, Tailwind CSS 4, shadcn/ui, React Router |
| Backend         | Fastify, Prisma 7, Zod, Pino                            |
| Banco de Dados  | PostgreSQL 16                                           |
| Cache           | Redis 7                                                 |
| Monorepo        | pnpm Workspaces, Turborepo                              |
| Linguagem       | TypeScript 5                                            |
| Formatação      | Prettier                                                |
| Containerização | Docker, Docker Compose                                  |

### Estrutura do Projeto

```
pingo/
├── apps/
│   ├── backend/          # API Fastify (porta 3001)
│   └── frontend/         # SPA React (servidor dev Vite)
├── packages/
│   └── shared/           # Tipos e schemas compartilhados (Zod)
├── docker-compose.yml    # Stack completa com Postgres, Redis, backend e frontend
├── turbo.json            # Configuração do pipeline Turborepo
└── pnpm-workspace.yaml   # Definição do workspace
```

### Pré-requisitos

- **Node.js** >= 18
- **pnpm** >= 10
- **Docker** e **Docker Compose** (para rodar containerizado)

### Rodando Localmente

1. **Instalar dependências**

```bash
pnpm install
```

2. **Subir a infraestrutura** (PostgreSQL + Redis)

```bash
docker compose up postgres redis -d
```

3. **Configurar variáveis de ambiente**

Crie o arquivo `apps/backend/.env`:

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5434/pingo
JWT_SECRET=sua-chave-secreta-com-pelo-menos-32-caracteres
```

4. **Rodar as migrations do banco**

```bash
cd apps/backend
pnpm exec prisma migrate dev
```

5. **Iniciar todas as apps em modo dev**

```bash
pnpm dev
```

Isso inicia frontend e backend via Turborepo:

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:3001`

### Rodando com Docker

1. **Crie um arquivo `.env`** na raiz do projeto:

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=pingo
JWT_SECRET=sua-chave-secreta-com-pelo-menos-32-caracteres
VITE_API_URL=http://localhost:3001
```

2. **Construir e iniciar todos os serviços**

```bash
docker compose up --build -d
```

Isso vai iniciar:

- **PostgreSQL** na porta `5434`
- **Redis** na porta `6379`
- **Backend** na porta `3001`
- **Frontend** na porta `80`

3. **Parar todos os serviços**

```bash
docker compose down
```

### Comandos Úteis

| Comando             | Descrição                           |
| ------------------- | ----------------------------------- |
| `pnpm dev`          | Inicia todas as apps em modo dev    |
| `pnpm build`        | Compila todas as apps e pacotes     |
| `pnpm lint`         | Verifica tipos em todos os projetos |
| `pnpm format`       | Formata o código com Prettier       |
| `pnpm format:check` | Verifica a formatação do código     |
| `pnpm clean`        | Remove todos os artefatos de build  |
