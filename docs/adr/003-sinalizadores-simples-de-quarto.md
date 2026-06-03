# ADR 003: Simplificação de Status do Quarto via Flags Booleanas Persistentes

**Data:** 2026-05-27
**Status:** Aceito

## Contexto
Devido à natureza altamente informal e imprevisível da chegada e saída dos romeiros na pousada (check-ins e checkouts fora de horários padrão) requerem um sistema de altíssima flexibilidade e baixíssima fricção operacional. A modelagem complexa de overlaps de intervalos temporais para faxina aumentaria a probabilidade de falhas e sobrecarga cognitiva desnecessária.

## Decisão
Adotamos uma modelagem simplificada baseada em sinalizadores persistentes diretamente na entidade `ROOM`:
1.  Dois campos booleanos físicos na tabela `Room`: `needs_cleaning: Boolean` e `under_maintenance: Boolean`.
2.  Um quarto desbloqueado será definido estritamente pela igualdade de ambos os campos a `false`.
3.  **Sem Bloqueio de Software:** As flags ativas de limpeza ou manutenção **não travam** a criação de novas reservas. O sistema exibe apenas avisos visuais na interface para alertar a gerência. 
4.  O único bloqueio sistêmico absoluto para uma nova reserva é o cruzamento direto de datas com outra reserva concomitante no mesmo quarto.

## Consequências
- **Positivo:**
    - **Alta Flexibilidade:** Facilita realocações ágeis e reservas de última hora de romeiros, mesmo que a faxina física de um quarto ainda não conste como finalizada no sistema.
    - **UX Ergonômica:** Permite a resolução em lote (marcar múltiplos quartos como limpos de uma só vez) de forma ágil após a saída de romarias.
- **Negativo:**
    - **Perda de Histórico Cronológico:** Não registramos no banco de dados o histórico cronológico exato de quanto tempo cada quarto ficou sob manutenção ou faxina.
