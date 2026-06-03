# Pousada Matriz — Front-end Workspace

Este workspace contém a aplicação cliente SPA (Single Page Application) da **Pousada Matriz**. Focada no conceito **mobile-first**, a interface é dividida em duas grandes frentes:
1.  **Landing Page Pública:** Site de alta velocidade voltado para hóspedes e romeiros, com foco em facilidade de contato via WhatsApp, além de informações gerais sobre a pousada, história da família Morais e atrações turísticas da região.
2.  **Painel Administrativo Interno:** Interface protegida para uso da gerência, voltada à operação diária de reservas, visualização do calendário de quartos, controle de faxina e fechamentos financeiros da família.

---

## 🎨 Especificações Técnicas e Arquitetura

*   **Framework base:** [React](https://react.dev/) + [Vite](https://vitejs.dev/).
*   **Design System & UI:** Tailwind CSS, integrado a componentes base do [Shadcn/UI](https://ui.shadcn.com/) (Radix UI + Lucide Icons).
*   **Consumo da API:** **Elysia Eden Client** (promove integração e validação estrita de tipos fim-a-fim, espelhando os esquemas do backend em tempo de compilação sem gerar arquivos estáticos intermediários).
*   **Gestão de Estado:** React Context e State local para fluxos operacionais rápidos.

---

## 📁 Estrutura de Pastas

```text
/front-end
├── src
│   ├── components   # Componentes visuais atômicos/reutilizáveis (inputs, tabelas, ui-base)
│   ├── pages        # Telas completas da aplicação (LandingPage, Dashboard, Calendario)
│   ├── hooks        # Custom hooks dedicados a queries, estados e listeners
│   ├── lib          # Utilitários do projeto (cn, setup do cliente Eden API)
│   ├── assets       # Imagens institucionais, logos e ícones locais
│   ├── App.tsx      # Roteador e envelopamento do painel admin
│   └── main.tsx     # Ponto de entrada de renderização do React
├── tailwind.config.js
├── vite.config.ts
└── tsconfig.json
```

---

## 🛠️ Comandos Locais (Escopo do Workspace)

Para executar os comandos diretamente nesta pasta, utilize o Bun da seguinte forma:

```bash
# Iniciar o servidor de desenvolvimento do Vite (porta 5173)
bun dev

# Executar testes unitários e de renderização de componentes com Vitest
bun test

# Formatar e analisar a conformidade do código via Biome
bunx biome check --write .

# Gerar o build estático otimizado para produção (/dist)
bun run build
```
