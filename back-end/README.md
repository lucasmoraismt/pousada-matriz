# Pousada Matriz — Back-end Workspace

Este workspace contém o servidor de API HTTP REST da **Pousada Matriz**. É o núcleo lógico do sistema, responsável por garantir a consistência das transações contábeis, governança de reservas e persistência de dados.

---

## 🏛️ Arquitetura e Regras de Desenvolvimento (Lightweight DDD)

Seguimos a arquitetura obrigatória de **Vertical Slices (Fatias Verticais)** com foco em desacoplamento e regras de negócio. Todo o código relacionado a um domínio de negócio reside agrupado em sua própria pasta sob `/back-end/src/domain/[nome-do-dominio]`. 

É proibido o agrupamento por tipo tecnológico (pastas como `controllers`, `services` ou `routes).

### 📁 Anatomia de uma Slice (Exemplo: `/reservations`)
*   `reservation.router.ts`: Porta de entrada e definição de rotas HTTP do Elysia.
*   `reservation.service.ts`: Core da lógica, regras de negócio puras e interações com o Prisma.
*   `reservation.schema.ts`: Validações de payload, DTOs e geração automatizada de documentação (Swagger).
*   `reservation.test.ts`: Testes unitários focados nas regras inquebráveis locais (ex: double-booking).

### ⚠️ Regra de Isolamento (ADR 002 & 004)
Os domínios devem ser modulares e desacoplados. A exclusão física de uma slice de negócio (exemplo: `rm -rf /back-end/src/domain/financial`) não deve quebrar a integridade interna nem impedir a compilação de outras slices independentes (ex: `/rooms`).

A comunicação entre fatias diferentes deve ocorrer exclusivamente pela chamada explícita de métodos públicos expostos por seus respectivos `.service.ts`.

---

## 🛠️ Comandos Locais (Escopo do Workspace)

Para executar os comandos diretamente nesta pasta, utilize o Bun:

```bash
# Iniciar o servidor de desenvolvimento do Elysia (porta 3000)
bun dev

# Rodar a suíte de testes unitários e de integração de regras de negócio via bun:test
bun test

# Sincronizar o banco de dados local com o schema do Prisma (dev)
bun prisma db push

# Gerar tipos e clientes atualizados do Prisma
bun prisma generate

# Executar a injeção de dados de seed no banco local
bun prisma db seed
```
O servidor de desenvolvimento expõe a documentação Swagger interativa em `http://localhost:3000/swagger`.
