# Dividi — Despesas compartilhadas

Aplicação web monolítica em Django para registrar despesas compartilhadas, calcular saldos e gerar uma estratégia determinística de liquidação entre participantes.

## Objetivo

O Dividi reduz cálculos manuais em grupos pequenos. O núcleo do produto é o motor financeiro: divisão de despesas, saldo líquido, arredondamento determinístico e geração de transferências por um algoritmo guloso explicável.

## Stack

- Python 3.14.8
- Django 6.1.1
- PostgreSQL 18.6
- Psycopg 3.3.6
- WhiteNoise 6.12.0
- HTML5, CSS3 e JavaScript vanilla

As versões acima são as versões fixadas neste projeto. A seleção considera versões estáveis/suportadas verificadas em 1º de outubro de 2026; Django 6.1 suporta Python 3.12–3.14 e PostgreSQL 18.6 está em série suportada.

## Requisitos

### Com Docker

```bash
cp .env.example .env
docker compose up --build
```

Em outro terminal:

```bash
docker compose exec web python manage.py migrate
docker compose exec web python manage.py seed_demo
```

A aplicação fica em `http://localhost:8000/`.

### Sem Docker

É necessário instalar Python 3.14.x, PostgreSQL 18.x e as dependências:

```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows PowerShell
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Configure as variáveis de `.env.example` no ambiente do processo e execute:

```bash
python manage.py migrate
python manage.py seed_demo
python manage.py runserver
```

O projeto não carrega um arquivo `.env` automaticamente; isso evita introduzir uma dependência adicional no MVP.

## Seed de demonstração

```bash
python manage.py seed_demo
```

Para recriar os dados:

```bash
python manage.py seed_demo --reset
```

Contas fictícias: `ana`, `bruno`, `carla`, `diego`, `elisa`, `felipe`, `gabi`, `henrique`.

Senha local de demonstração: `Demo123!`

Não use essa senha em ambiente real.

## Testes

Suíte Django:

```bash
python manage.py test
```

Testes puros da camada de cálculo e do motor, que não dependem de Django:

```bash
python -m unittest tests.test_calculator tests.test_engine -v
```

## Arquitetura

A aplicação é um monólito modular:

- `accounts/`: cadastro e autenticação;
- `groups/`: grupos e memberships;
- `expenses/`: despesas, categorias e divisão;
- `settlements/`: acertos e motor de liquidação;
- `core/`: infraestrutura compartilhada e dashboard;
- `templates/` e `static/`: interface server-rendered.

As regras críticas ficam em camadas de serviço e módulos puros de cálculo, não em templates ou JavaScript.

## Regras financeiras principais

```text
saldo da despesa = valor pago - valor devido
saldo financeiro = saldo das despesas - acertos pagos enviados + acertos pagos recebidos
```

A soma dos saldos deve ser zero.

O motor recebe saldos líquidos, separa devedores e credores e usa uma estratégia gulosa determinística. O projeto não declara que ela minimiza globalmente o número de transferências.

## Limitações do MVP

- uma única moeda por grupo;
- sem conversão cambial;
- participantes são usuários cadastrados;
- sem processamento de dinheiro real;
- sem integrações externas;
- sem IA no núcleo;
- despesas históricas ficam protegidas contra edição/exclusão após o primeiro acerto registrado no grupo.

## Documentação

Consulte `docs/` para requisitos, especificação técnica, arquitetura, banco, algoritmo, segurança, acessibilidade, plano de testes, guia de demonstração, avaliação de UX, assunções e roadmap.

## Estado do projeto

O estado operacional é mantido em `STATUS.md`. O projeto diferencia explicitamente código implementado de comportamento testado e estado estável.

## Validação do checkpoint

O núcleo puro de cálculo/liquidação possui testes executados no sandbox. A suíte Django, migrations, seed e validação contra PostgreSQL dependem das dependências/runtime externos. No sandbox utilizado para este checkpoint, Django e PostgreSQL não estavam instalados e a instalação via rede estava indisponível; essas lacunas permanecem registradas em `STATUS.md` e não são apresentadas como testes aprovados.
