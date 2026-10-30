# Investment Frontend

Frontend do projeto de controle de investimentos. A aplicação utiliza Next.js
com App Router, Tailwind CSS v4 e shadcn/ui em um monorepo gerenciado por
pnpm e Turborepo.

## Pré-requisitos

- Node.js `>=20.9.0`
- pnpm (a versão declarada pelo projeto é `11.25.0`)
- Git

Confira as versões instaladas:

```bash
node --version
pnpm --version
```

## Estrutura do projeto

```text
investment_front/
├── apps/
│   └── web/                 # Aplicação Next.js
│       ├── app/             # Rotas e layouts do App Router
│       ├── components/      # Componentes específicos da aplicação
│       └── lib/             # Utilitários e regras do app
├── packages/
│   ├── ui/                  # Componentes compartilhados e estilos
│   │   └── src/components/  # Componentes shadcn/ui
│   ├── eslint-config/       # Configurações compartilhadas do ESLint
│   └── typescript-config/   # Configurações compartilhadas do TypeScript
└── package.json             # Scripts do workspace
```

## Configuração inicial

A partir da raiz deste projeto (`investment_front`), instale as dependências:

```bash
pnpm install
```

No estado atual do scaffold, não é necessário criar um arquivo `.env`: não há
variáveis de ambiente declaradas para iniciar a aplicação.

## Fluxo de desenvolvimento

1. Acesse a pasta do frontend:

	```bash
	cd investment_front
	```

2. Inicie o servidor de desenvolvimento:

	```bash
	pnpm dev
	```

	O Turbo executa o script `dev` do app `web`. Abra
	[http://localhost:3000](http://localhost:3000) no navegador.

3. Faça as alterações em `apps/web` ou nos pacotes compartilhados.

4. Antes de abrir um pull request, execute as validações:

	```bash
	pnpm lint
	pnpm typecheck
	pnpm build
	```

Para executar um comando apenas no app web, use o filtro do pnpm:

```bash
pnpm --filter web lint
pnpm --filter web typecheck
pnpm --filter web build
```

## Scripts disponíveis

| Comando | Descrição |
| --- | --- |
| `pnpm dev` | Inicia o desenvolvimento de todos os apps do workspace. |
| `pnpm build` | Gera o build de produção com Turborepo. |
| `pnpm lint` | Executa o ESLint nos pacotes e apps. |
| `pnpm typecheck` | Executa a verificação de tipos do TypeScript. |
| `pnpm format` | Formata arquivos TypeScript e TSX com Prettier. |
| `pnpm --filter web start` | Inicia o build de produção localmente. |

Depois do build, o fluxo de produção local é:

```bash
pnpm build
pnpm --filter web start
```

## Tailwind CSS e estilos

Os estilos globais e os tokens de tema ficam em
`packages/ui/src/styles/globals.css`. O Tailwind CSS v4 é carregado por meio
do PostCSS, sem um arquivo `tailwind.config.*` tradicional.

O layout raiz de `apps/web` importa os estilos compartilhados:

```tsx
import "@workspace/ui/globals.css"
```

Prefira as classes utilitárias existentes e os tokens CSS do tema (`bg-background`,
`text-foreground`, `border-border` etc.) para manter os modos claro e escuro
consistentes.

## shadcn/ui

Os componentes shadcn/ui são mantidos no pacote compartilhado `@workspace/ui`.
Para adicionar um componente, execute na raiz de `investment_front`:

```bash
pnpm dlx shadcn@latest add card -c apps/web
```

Substitua `card` pelo componente desejado. O componente será criado em
`packages/ui/src/components`.

Use os componentes dentro da aplicação por meio do alias do workspace:

```tsx
import { Button } from "@workspace/ui/components/button"
```

As configurações do shadcn/ui estão em `apps/web/components.json` e
`packages/ui/components.json`. A biblioteca de ícones configurada é
`lucide-react`.

## Convenções de implementação

- Rotas, layouts e páginas ficam em `apps/web/app`.
- Componentes reutilizáveis de interface ficam em `packages/ui`.
- Componentes específicos de uma tela ficam em `apps/web/components`.
- Use TypeScript e mantenha os imports compartilhados com os aliases
  `@workspace/ui/...`.
- Execute `pnpm format` após alterações de formatação e rode `pnpm lint` e
  `pnpm typecheck` antes de considerar a tarefa concluída.

## Solução de problemas

Se as dependências estiverem inconsistentes, remova apenas `node_modules` e o
lockfile gerado localmente não versionado e reinstale:

```bash
rm -rf node_modules apps/*/node_modules packages/*/node_modules
pnpm install
```

Se a porta `3000` estiver ocupada, inicie o Next.js em outra porta:

```bash
pnpm --filter web dev -- -p 3001
```
