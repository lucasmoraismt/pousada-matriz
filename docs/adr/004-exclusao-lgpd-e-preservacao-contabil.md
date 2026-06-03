# ADR 004: Expurgo Físico de Hóspedes (LGPD) com Anulação de FK em Reservas

**Data:** 2026-05-27
**Status:** Aceito

## Contexto
O projeto **Pousada Matriz** deve operar em conformidade com a LGPD (Lei Geral de Proteção de Dados Pessoais). Hóspedes titulares de reservas devem ter o direito de solicitar a exclusão total e física (Hard Delete) de suas informações pessoais (Nome, CPF/RG, WhatsApp) do banco de dados da pousada.

No entanto, o sistema administrativo requer dados históricos contábeis (balanços mensais e anuais) consistentes. A exclusão física de uma reserva inteira ou de suas transações associadas corromperia o faturamento financeiro histórico da pousada, distorcendo relatórios críticos para a administração familiar.

## Decisão
Implementamos uma modelagem relacional de nulidade na relação entre as entidades `Reservation` e `Guest`:
1.  A chave estrangeira `guest_id` na tabela `Reservation` é definida como opcional (`guest_id: String?`).
2.  Quando uma solicitação de exclusão total de dados de um `Guest` for acionada:
    *   O registro correspondente é deletado fisicamente da tabela `Guest` (`Hard Delete`).
    *   A chave `guest_id` em todas as reservas associadas a esse hóspede é redefinida para `null` (`ON DELETE SET NULL` no banco de dados).
3.  O valor financeiro acordado (`total_agreed_price` na reserva) e todas as suas transações correspondentes (`Transaction` na tabela financeira) permanecem intactos e imutáveis.

## Consequências
- **Positivo:**
    - **Conformidade Legal:** Atende integralmente à requisição de eliminação de dados pessoais prevista no Artigo 18 da LGPD.
    - **Integridade Contábil Blindada:** Preserva a soma total histórica de receitas no fechamento financeiro, garantindo que relatórios anuais de anos fiscais passados permaneçam exatos.
    - **DX Limpa:** Implementado de forma nativa e segura no nível do banco de dados via comportamento relacional `SET NULL`.
- **Negativo:**
    - **Perda de Rastreabilidade Nominal:** Uma vez excluído o hóspede, não haverá qualquer registro legível ou chave sobre *quem* efetuou e pagou aquela reserva no passado (consequência inerente e necessária da lei).
