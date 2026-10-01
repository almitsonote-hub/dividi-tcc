# Premissas e decisões consolidadas

Data: 01/10/2026

## Participantes

**Decisão:** somente usuários cadastrados participam de grupos no MVP.

**Motivo:** reduz ambiguidade de identidade, simplifica autorização, evita duplicatas e mantém o modelo de dados pequeno. Convidados sem conta ficam para P1/P2.

## Moeda

**Decisão:** cada grupo possui uma única moeda, sem conversão cambial no MVP. BRL é o padrão.

## Dinheiro

**Decisão:** `DecimalField` no banco e `Decimal` no Python. `float` não é usado para valores financeiros.

## Arredondamento

**Decisão:** valores monetários têm 2 casas. Divisões são convertidas para centavos sem perda silenciosa.

- divisão igual: calcula a parcela-base por truncamento para centavos e distribui os centavos restantes, em ordem crescente de `Membership.id`;
- divisão percentual: calcula valores brutos, trunca cada parcela para centavos e distribui centavos restantes pela maior parte fracionária, com `Membership.id` crescente como desempate;
- divisão personalizada: exige soma exata do total, portanto não cria resíduo de arredondamento;
- comparação de valores monetários ocorre depois de quantização a centavos.

## Saldos

O saldo bruto é:

`valor efetivamente pago - valor que deveria pagar`

Para refletir pagamentos já concluídos, o saldo financeiro usado pelo motor acrescenta os fluxos de acertos pagos:

`saldo financeiro = saldo bruto - acertos pagos enviados + acertos pagos recebidos`

## Liquidação

**Decisão:** estratégia gulosa determinística, com ordenação por maior valor absoluto e `Membership.id` como desempate.

A mesma entrada produz a mesma saída. Não é afirmada optimalidade global do número de transferências.

## Acertos

Acertos pendentes são compromissos registrados. Eles não alteram o saldo financeiro de caixa, mas são descontados da liquidação sugerida para evitar duplicação da recomendação. Quando marcados como pagos, passam a integrar o saldo financeiro e preservam o mesmo efeito líquido.

## Edição de despesas

Depois que qualquer acerto existe no grupo, despesas históricas não podem ser editadas/excluídas no MVP. Novas despesas podem ser adicionadas e são incorporadas ao saldo corrente.

Essa restrição privilegia explicabilidade e integridade em vez de uma rotina de reversão mais complexa.

## Segurança

Todo acesso de negócio é restringido pela associação `request.user -> Membership -> ExpenseGroup`. O frontend nunca é considerado fonte de autorização.

## Estado quitado

Um grupo só é mostrado como quitado quando não há transferência necessária na liquidação corrente e não existem acertos pendentes.

## Testes

Django Test Framework é a base dos testes de integração. A camada pura de cálculo/algoritmo também possui testes com `unittest` para poder validar invariantes mesmo em ambientes sem dependências web instaladas.

## Concorrência

Operações que alteram o estado financeiro usam `transaction.atomic` e bloqueio da linha do grupo/acerto quando necessário, reduzindo a possibilidade de dois comandos concorrentes calcularem o mesmo saldo disponível e registrarem acertos incompatíveis.
