# ADR 001: Adoção de Arquitetura Monorepo Logicamente Separada com Bun Workspaces

**Data:** 2026-04-03
**Status:** Aceito

## Contexto
O projeto Pousada Matriz exige alta agilidade de desenvolvimento por ser guiado por um engenheiro solo (ou Agente auxiliador). Há um forte desejo de evitar overengineering como Microsserviços puros, mas manter o limite de camadas de código claro. Opções de Monolito com Next.js foram descartadas pelo excessivo acoplamento *framework-centric* dificultando manutenção e criação isolada. A separação completa (Multirepo) cobra um alto preço de produtividade na manutenção de Linter iterativo.

## Decisão
Adotamos o **Bun Workspaces** configurado como um Monorepo na pasta raiz do repositório. Criamos duas pastas fisicamente separadas: `/front-end` (como SPA focado UI) e `/back-end` (como servidor API HTTP puro). 

## Consequências
- **Positivo:** Excelente *Developer Experience* (DX). O Linter (BiomeJS) varre os dois projetos simultaneamente. Tipos do lado do servidor podem ser exportados interativamente para a pasta do frontend sem geradores adicionais. O Git controla a história inteira de ponta a ponta de uma vez.
- **Negativo:** Configuração inicial dos scripts (`package.json`) exige certo rigor na execução (uso do comando `--filter`) em detrimento do NPM clássico.
