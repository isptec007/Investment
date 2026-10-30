# Investment

Projeto de controle de investimentos, composto por uma API em AdonisJS e uma
aplicação web em Next.js.

## Stack

- **Backend:** AdonisJS 7, TypeScript, Lucid ORM e SQLite
- **Frontend:** Next.js 16, React, Tailwind CSS v4 e shadcn/ui
- **Gerenciamento do frontend:** pnpm e Turborepo
- **Autenticação:** AdonisJS Auth e sessões por cookie

## Estrutura do projeto

```text
Investment/
├── Investment_back/       # API AdonisJS
│   ├── app/               # Controllers, models, validators e middleware
│   ├── config/            # Configurações da aplicação e do banco
│   ├── database/          # Migrations e schema
│   ├── start/             # Rotas e bootstrap da aplicação
│   └── package.json
├── investment_front/      # Monorepo Next.js
│   ├── apps/web/          # Aplicação web
│   ├── packages/ui/       # Componentes compartilhados shadcn/ui
│   └── package.json
└── README.md
```

## Pré-requisitos

- Node.js `>=20.9.0`
- npm, para o backend
- pnpm, para o frontend (versão declarada: `11.25.0`)
- Git

Confira as versões instaladas:

```bash
node --version
npm --version
pnpm --version
```

## Configuração do backend

Abra um terminal na raiz do projeto e instale as dependências da API:

```bash
cd Investment_back
npm install
```

Crie o arquivo de ambiente a partir do exemplo:

```bash
cp .env.example .env
```

O scaffold utiliza SQLite por padrão. O banco será criado em
`Investment_back/tmp/db.sqlite3`. O `.env.example` já define:

| Variável | Valor padrão | Descrição |
| --- | --- | --- |
| `HOST` | `localhost` | Host da API. |
| `PORT` | `3333` | Porta da API. |
| `NODE_ENV` | `development` | Ambiente de execução. |
| `APP_KEY` | vazio | Chave usada pela aplicação; preencha para ambientes persistentes. |
| `SESSION_DRIVER` | `cookie` | Driver das sessões. |
| `CORS_ORIGIN` | comentado | Origens autorizadas para o frontend. |

Para desenvolvimento local, configure o CORS permitindo o frontend:

```env
CORS_ORIGIN=http://localhost:3000
```

Gere uma chave de aplicação antes de usar autenticação em um ambiente real:

```bash
node ace generate:key
```

Se houver migrations pendentes, execute:

```bash
node ace migration:run
```

## Configuração do frontend

Em outro terminal, instale as dependências do monorepo web:

```bash
cd investment_front
pnpm install
```

O scaffold atual não possui variáveis de ambiente obrigatórias no frontend.
Quando a integração com a API for adicionada, mantenha as variáveis públicas
do Next.js em `investment_front/apps/web/.env.local`, usando o prefixo
`NEXT_PUBLIC_` quando o valor precisar ser acessado no navegador.

## Executar o projeto

### Backend

```bash
cd Investment_back
npm run dev
```

A API ficará disponível em [http://localhost:3333](http://localhost:3333).
O endpoint de verificação atual é:

```text
GET http://localhost:3333/
```

### Frontend

```bash
cd investment_front
pnpm dev
```

A aplicação ficará disponível em [http://localhost:3000](http://localhost:3000).

Para usar os dois serviços, mantenha o backend e o frontend em terminais
separados. O frontend deve consumir a API em `http://localhost:3333`.

## Scripts do backend

Execute dentro de `Investment_back`:

| Comando | Descrição |
| --- | --- |
| `npm run dev` | Inicia o servidor com hot reload. |
| `npm run build` | Gera o build de produção da API. |
| `npm run start` | Inicia a API compilada. |
| `npm run test` | Executa os testes com Japa. |
| `npm run lint` | Executa o ESLint. |
| `npm run typecheck` | Verifica os tipos TypeScript. |
| `npm run format` | Formata os arquivos do backend. |

## Scripts do frontend

Execute dentro de `investment_front`:

| Comando | Descrição |
| --- | --- |
| `pnpm dev` | Inicia o frontend via Turborepo. |
| `pnpm build` | Gera o build de produção. |
| `pnpm lint` | Executa o ESLint. |
| `pnpm typecheck` | Verifica os tipos TypeScript. |
| `pnpm format` | Formata arquivos TypeScript e TSX. |
| `pnpm --filter web start` | Inicia o frontend compilado. |

## Validação antes do commit

Backend:

```bash
cd Investment_back
npm run lint
npm run typecheck
npm run test
npm run build
```

Frontend:

```bash
cd investment_front
pnpm lint
pnpm typecheck
pnpm build
```

## Componentes shadcn/ui

Os componentes compartilhados ficam em `investment_front/packages/ui`. Para
adicionar um novo componente, execute na raiz do frontend:

```bash
cd investment_front
pnpm dlx shadcn@latest add card -c apps/web
```

Use o componente na aplicação com o alias do workspace:

```tsx
import { Button } from "@workspace/ui/components/button"
```

Mais detalhes do desenvolvimento do frontend estão em
[`investment_front/README.md`](investment_front/README.md).

## Solução de problemas

Se a porta `3333` estiver ocupada, altere `PORT` no arquivo
`Investment_back/.env`. Se a porta `3000` estiver ocupada, inicie o frontend
em outra porta:

```bash
cd investment_front
pnpm --filter web dev -- -p 3001
```

Nesse caso, atualize também `CORS_ORIGIN` no `.env` do backend para
`http://localhost:3001`.
