# Pousada Matriz — Plataforma Integrada

Plataforma digital integrada e responsiva sob o conceito mobile-first para a gestão e divulgação da **Pousada Matriz**. O projeto unifica a experiência do hóspede e o painel operacional da gerência, combinando um site público de alta conversão a uma plataforma administrativa interna de alta confiabilidade contábil.

Orquestrado em um **Monorepo com Bun Workspaces**, o ecossistema é logicamente dividido em dois subprojetos autônomos e desacoplados: `/front-end` (SPA cliente de alto desempenho) e `/back-end` (servidor de API REST focado em integridade de dados).

---

## 🏗️ Stack Tecnológica

### 1. Infraestrutura e Qualidade de Código (Global)
*   **Runtime e Orquestração:** [Bun](https://bun.sh/) (Bun Workspaces para gerenciamento de dependências e scripts).
*   **Qualidade e Formatação:** [BiomeJS](https://biomejs.dev/) (Linter e Formatter unificado).
*   **Controle de Hooks:** Git + Husky.

### 2. Back-end Workspace (`/back-end`)
*   **Framework HTTP:** [Elysia.js](https://elysia.js.org/) (Rápido, leve e com segurança de tipos de ponta a ponta).
*   **Documentação da API:** `@elysiajs/swagger` (Interface OpenAPI autogerada).
*   **Persistência e ORM:** PostgreSQL + [Prisma ORM](https://www.prisma.io/).
*   **Suíte de Testes:** `bun:test` (Test Runner nativo do Bun).

### 3. Front-end Workspace (`/front-end`)
*   **Biblioteca Principal:** [React](https://react.dev/) + [Vite](https://vitejs.dev/) (SPA de alta velocidade de carregamento).
*   **Estilização:** Tailwind CSS + [Shadcn/UI](https://ui.shadcn.com/) (Componentes acessíveis, responsivos e refinados).
*   **Integração de Tipos (E2E):** Elysia Eden (Consumo da API com tipagem estrita compartilhada do backend).
*   **Suíte de Testes:** Vitest + React Testing Library.

### 4. Testes Ponta a Ponta (E2E)
*   **Framework:** [Playwright](https://playwright.dev/) (Simulação e teste de fluxos críticos de ponta a ponta).

---

## 📦 Primeiros Passos (Execução Local)

### Pré-requisitos
Certifique-se de ter o **Bun** e o **Docker** instalados na sua máquina.

### 1. Clonagem e Instalação de Dependências
Clone o repositório e execute a instalação na raiz do projeto (o Bun instalará os pacotes de ambos os workspaces de uma só vez):
```bash
bun install
```

### 2. Configuração do Banco de Dados
Certifique-se de ter uma instância do PostgreSQL rodando (ou suba via docker-compose). Configure a variável `DATABASE_URL` no arquivo `.env` dentro da pasta `/back-end`.

Gere o cliente do Prisma e sincronize a estrutura física do banco:
```bash
# Gerar o cliente Prisma
bun --filter back-end prisma generate

# Sincronizar o banco de dados (ambiente de dev)
bun --filter back-end prisma db push

# Injetar dados iniciais (seeder de teste)
bun --filter back-end prisma db seed
```

### 3. Execução dos Servidores de Desenvolvimento
Para iniciar simultaneamente o back-end (porta `3000`) e o front-end (porta `5173`) em modo *watch* de forma concorrente, execute na raiz do repositório:
```bash
bun run dev
```

*   **Painel Administrativo / Landing:** Acesse `http://localhost:5173`
*   **Documentação Swagger (API):** Acesse `http://localhost:3000/swagger`

---

## 🧪 Suíte de Testes

### Testes do Back-end (Lógica Pura e Integração)
Para rodar os testes unitários e de integração do back-end:
```bash
bun --filter back-end test
```

### Testes do Front-end (Componentes de UI)
Para executar os testes de componentes isolados do front-end com Vitest:
```bash
bun --filter front-end test
```

### Testes de Fluxo Crítico (E2E Playwright)
Para executar a simulação ponta a ponta que valida os fluxos críticos de reservas e faturamento:
```bash
bunx playwright test
```

---

## 🧹 Qualidade de Código (Biome)

Para verificar o código em busca de erros de linting ou problemas de formatação:
```bash
# Apenas checar conformidade
bunx biome check .

# Aplicar correções e formatações automaticamente
bunx biome check --write .
```
O Husky interceptará automaticamente os commits locais via pré-commit hook para garantir que nenhum código fora dos padrões estritos do Biome seja empurrado para o repositório.
