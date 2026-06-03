# ADR 002: Escolha da Stack Tecnológica Definitiva e Estratégia de Testes

**Data:** 2026-04-03
**Status:** Aceito

## Contexto
Temos que atender a requisições via internet 4G móvel (Juazeiro do Norte) operadas primariamente via celular pela gerência. O design exige a aparência limpa, prática e focada no input direto. O Backend necessita tratar o rigor financeiro e a concorrência dos quartos via banco de dados relacional.

## Decisão
O ecossistema oficial é formado pelas seguintes ferramentas:
1.  **Back-end:** Servidor leve `Elysia.js` (rodando em Bun) com o plugin `@elysiajs/swagger` empacotado para documentação viva OpenAPI análoga ao FastAPI.
2.  **Banco de Dados:** `PostgreSQL` mapeado estritamente por `Prisma ORM`.
3.  **Front-end:** SPA gerado por `React` usando `Vite` com responsividade progressiva regida pelo `Tailwind CSS`. Os componentes base sairão do pacote visual `Shadcn/UI`.
4.  **Testes Automatizados:** `bun:test` como test runner do back-end, `Vitest` integrando o React, e `Playwright` na linha dos testes E2E do *painel de checkout/financeiro*.

## Consequências
- **Positivo:** A harmonia gerada por esse bundle provê a famosa Tipagem *End-to-End* sem esforço adicional ao longo do tempo. 
- **Negativo:** O uso da abordagem arquitetural "Lightweight DDD" demandará alta resiliência mental vertical (divisão correta das *Slices*) em detrimento de jogar Controllers num arquivo genérico sem nexo.
