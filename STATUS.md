# STATUS DO PROJETO

Data: 01/10/2026

Fase: **3 — Implementação**
Etapa atual: **Integração, validação e estabilização inicial do MVP**

## Fase 1 — Análise e definição

[x] Ambiguidades críticas resolvidas
[x] MVP/P1/P2 definidos
[x] Regras financeiras consolidadas
[x] Decisões registradas em `ASSUMPTIONS.md`

## Fase 2 — Planejamento técnico

[x] Arquitetura modular definida
[x] Modelo de dados especificado
[x] Divisão e arredondamento especificados
[x] Motor de liquidação especificado
[x] Estratégia de testes definida
[x] Critérios de aceitação definidos

## MVP — implementação e validação

[~] Bootstrap Django criado, ainda não executado neste sandbox
[~] Configuração PostgreSQL criada, ainda não validada neste sandbox
[~] Autenticação/cadastro criada, ainda não validada por teste Django
[~] Grupos criados, ainda não validados por fluxo HTTP
[~] Participantes criados, ainda não validados por fluxo HTTP
[~] Despesas criadas, camada de domínio validada parcialmente por testes puros
[~] Divisão igual implementada e testada
[~] Divisão personalizada implementada e testada em camada pura
[~] Divisão percentual implementada e testada em camada pura
[~] Cálculo de saldos implementado, pendente execução Django
[~] Motor de liquidação implementado e testado
[~] Acertos pendentes/pagos implementados, pendente execução Django
[~] Dashboard básico implementado, pendente validação visual/runtime
[~] Controle de acesso em views implementado, pendente execução Django
[~] Responsividade base implementada, pendente validação visual manual
[~] Seed de demonstração implementado, pendente execução Django/PostgreSQL
[~] Migrations escritas manualmente, pendentes de execução/checagem Django

## TESTADO REALMENTE NO SANDBOX

[x] 11 testes puros de cálculo e liquidação executados com sucesso
[x] Compilação sintática de todos os arquivos Python executada
[x] Verificação de branco em mudanças Git executada antes do checkpoint final

## AVALIAÇÃO REAL DO MOTOR

Execução de `tests/evaluate_algorithm.py` no sandbox deste checkpoint:

| Participantes | Transferências | Total liquidado | Tempo da função pura | Correto |
|---:|---:|---:|---:|:---:|
| 3 | 1 | 107,00 | 15,76 µs | Sim |
| 8 | 7 | 470,00 | 22,28 µs | Sim |
| 15 | 13 | 896,00 | 30,22 µs | Sim |
| 30 | 29 | 2.340,00 | 59,82 µs | Sim |

Os tempos acima são medições do ambiente de execução deste checkpoint e não devem ser tratados como benchmark universal.

## NÃO TESTADO NESTE SANDBOX

[ ] Suíte de testes Django
[ ] Fluxos HTTP ponta a ponta
[ ] Execução de migrations
[ ] Execução do seed
[ ] PostgreSQL real
[ ] Validação visual completa em navegador

## ESTÁVEL

[ ] MVP não deve ser declarado estável enquanto os testes e fluxos acima permanecerem pendentes.

## P1

[ ] Relatórios aprimorados
[ ] CSV
[ ] PDF
[ ] Despesas recorrentes
[ ] Notificações
[ ] Auditoria de alterações

## P2

[ ] Importação
[ ] Integrações externas
[ ] Comparação avançada de estratégias
[ ] IA para interpretação de linguagem natural

## Problemas conhecidos

- O sandbox atual não possui Django, Psycopg ou um servidor PostgreSQL instalados e não possui acesso de rede para `pip`.
- Portanto, o checkpoint contém código e testes preparados, mas não há evidência runtime local da camada Django/PostgreSQL neste ambiente.
- A interface depende de runtime Django para validação visual completa.

## Último checkpoint

**Checkpoint técnico de implementação.** O núcleo algorítmico foi validado; a validação do stack web/database permanece pendente por limitação do ambiente.
