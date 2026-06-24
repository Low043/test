# Monorepo Express + NextJS

## Project Overview

Esta é uma estrutura inicial para um monorepo Express + NextJS. O projeto utiliza workspaces do `pnpm` para o gerenciamento de pacotes e segue uma arquitetura funcional baseada em módulos para a API. O objetivo é fornecer um ambiente padronizado, altamente escalável e com configurações compartilhadas.

## Tech Stack

- **Node.js**: v25.6
- **Package Manager**: pnpm v11.8
- **Database**: PostgreSQL v15 (via Docker)
- **Backend**: Express v5, Zod, tsup
- **ORM**: Prisma v7 (com driver adapter PostgreSQL)
- **Frontend**: NextJS

## Environment

Crie um arquivo `.env` dentro de `apps/api/` com as seguintes variáveis:

```env
PORT=3001

JWT_SECRET=super-secret-key
JWT_EXPIRES_IN=1d

DB_USER=postgres
DB_PASSWORD=postgres
DB_HOST=localhost
DB_PORT=5432
DB_NAME=express-db
```

## Directory Structure

```text
.
├── apps/
│   ├── api/
│   │   ├── src/
│   │   │   ├── modules/
│   │   │   │   └── {feature}/                     // Substitua {feature} pelo nome do domínio (ex: auth, user)
│   │   │   │       ├── {feature}.schema.ts        // Validações do Zod
│   │   │   │       ├── {feature}.types.ts         // Tipagens e interfaces
│   │   │   │       ├── {feature}.controller.ts    // Lógica de requisição/resposta
│   │   │   │       ├── {feature}.service.ts       // Regras de negócio e acesso a dados
│   │   │   │       └── {feature}.routes.ts        // Definição das rotas Express
│   │   │   ├── shared/                            // Middlewares, erros e utilitários globais
│   │   │   ├── index.ts                           // Bootstrap da aplicação
│   │   │   └── server.ts                          // Configuração do Express
│   │   ├── prisma.config.js                       // Configuração do Prisma CLI
│   │   └── tsup.config.ts                         // Configuração de build (bundler)
│   └── web/                                       // App frontend NextJS
└── packages/
    └── database/                                  // Pacote interno compartilhado
        ├── prisma/
        │   └── schema.prisma                     // Modelagem do banco de dados
        └── src/
            └── index.ts                           // Exporta o PrismaClient instanciado
```

## Command List

**Global Commands**

- `pnpm install`: Instala as dependências em todos os workspaces.
- `pnpm run dev`: Executa os servidores de desenvolvimento.
- `pnpm run build`: Compila as aplicações para produção.
- `pnpm run start`: Inicia as aplicações compiladas em modo de produção.
- `pnpm run lint`: Executa a verificação de código (ESLint).
- `pnpm run format`: Formata os arquivos do projeto (Prettier).

**Targeted Commands**
Qualquer um dos comandos globais acima pode ser direcionado para um pacote específico usando a flag `-F`.
Exemplo: `pnpm -F api run dev` ou `pnpm -F web run build`.

**Prisma Commands**
O Prisma é gerenciado a partir da API. Para executar qualquer comando da CLI do Prisma, utilize o `exec` direcionado para a API.
Exemplo: `pnpm -F api exec prisma migrate dev`.

## How to Run

1. Clone o repositório e instale as dependências:

   ```bash
   pnpm install
   ```

2. Crie o arquivo de ambiente:
   Crie um arquivo `.env` dentro da pasta `apps/api/` usando as variáveis listadas na seção Environment.

3. Inicie o banco de dados:
   Rode o container Docker, injetando as variáveis do `.env` da API:

   ```bash
   docker compose --env-file apps/api/.env up -d
   ```

4. Inicie o servidor em modo de desenvolvimento:
   A partir da pasta raiz:

   ```bash
   pnpm run dev
   ```

5. Build e Start (Produção):
   ```bash
   pnpm -F api build
   pnpm -F api start
   ```

## Git Flow / Commits

Este projeto segue o padrão de Conventional Commits (ex: `feat:`, `fix:`, `feat(auth):`).

**Estratégia de Branches:**

1. Crie uma branch usando o formato `{semantic_tag}/{feature}` (ex: `feat/auth-module` ou `fix/user-validation`).
2. Faça seus commits seguindo a estrutura semântica.
3. Abra um Pull Request apontando para a branch `develop` para revisão.
