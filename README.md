# Projeto 01

Aplicação web construída com **Next.js 16 (App Router)**, **React 19** e **TypeScript**, seguindo uma arquitetura server-first com separação clara entre UI, lógica de negócio e acesso a dados.

> Projeto em estágio inicial (scaffold). A estrutura de pastas e convenções abaixo definem o padrão a ser seguido conforme as páginas forem implementadas.

## Stack

- **Framework:** Next.js 16 (App Router)
- **UI:** React 19 + TypeScript
- **Estilização:** TailwindCSS 4 + shadcn/ui
- **Formulários e validação:** React Hook Form + Zod
- **Lint:** ESLint (eslint-config-next)

## Pré-requisitos

- Node.js 18+
- npm

## Como rodar

```bash
npm install
npm run dev
```

Abra [http://localhost:3000](http://localhost:3000) no navegador.

## Scripts disponíveis

| Comando           | Descrição                          |
| ------------------ | ----------------------------------- |
| `npm run dev`       | Inicia o servidor de desenvolvimento (porta 3000) |
| `npm run build`     | Gera o build de produção            |
| `npm run start`     | Inicia o servidor em modo produção  |
| `npm run lint`      | Roda o ESLint                       |

## Arquitetura

O projeto usa **App Router** com Server Components por padrão. Componentes só devem usar `"use client"` quando precisarem de hooks, eventos ou APIs de navegador.

```
app/                  # Rotas (App Router), agrupadas por (grupo/)
  <rota>/
    page.tsx          # Server Component (entrada da página)
    _components/      # Componentes específicos da página
    _actions/          # Server Actions ("use server")
    _data-access/      # Data Access Layer (busca de dados)
components/
  ui/                 # Primitivos reutilizáveis (shadcn/ui)
  ...                 # Componentes de feature compartilhados
lib/                  # Helpers, clients (Supabase, Stripe), configurações
types/                # Tipos globais e schemas Zod compartilhados
```

### Camadas por página

- **`page.tsx`** — Server Component; busca dados via `_data-access`, valida autenticação e repassa para os componentes client.
- **`_components/`** — componentes da página (privados, não geram rota); interatividade e estado ficam aqui via Client Components.
- **`_actions/`** — Server Actions (`"use server"`) responsáveis por mutações, validadas com Zod.
- **`_data-access/`** — funções server-only de leitura de dados, encapsulando queries e verificação de permissões.

Mutações **nunca** acessam o banco diretamente a partir de Client Components — sempre passam por Server Actions.

## Convenções de código

- Sem `any` explícito — usar `unknown` + type guard.
- Imports via ES modules (sem `require`).
- Estilização apenas com Tailwind (sem CSS inline ou styled-components); novos design tokens vão em `tailwind.config.ts` antes de usar.
- Arquivos em kebab-case; componentes React em PascalCase.
- Handlers de evento prefixados com `handle` (`handleClick`, `handleSubmit`); booleanos com verbo (`isLoading`, `hasError`); hooks customizados com `use`.
- Formulários seguem o padrão shadcn/ui + React Hook Form + Zod.

## Variáveis de ambiente

- Copie `.env.example` para `.env.local` ao clonar o projeto.
- `NEXT_PUBLIC_*` apenas para valores seguros no client.
- Segredos (banco de dados, API keys) só em Server Actions ou Route Handlers.

## Workflow de contribuição

- Branches: `feat/`, `fix/`, `chore/` + descrição em kebab-case.
- Commits em inglês, no imperativo (ex.: `add OAuth callback handler`).
- Após alterações, rodar `npm run lint` (e `type-check`, quando disponível) antes de commitar.

## Saiba mais

- [Documentação do Next.js](https://nextjs.org/docs)
- [shadcn/ui — Forms com React Hook Form](https://ui.shadcn.com/docs/forms/react-hook-form)
