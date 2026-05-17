# Estrutura Atual dos Repositórios

Esta página descreve o estado atual dos repositórios após a integração da arquitetura modular do backend.

## Visão Geral

```text
Projeto-Advocacia-TPPE/
├── tppe-advocacia-backend/
│   ├── app/
│   │   ├── api/
│   │   ├── config/
│   │   ├── db/
│   │   ├── modules/
│   │   └── shared/
│   ├── docs/
│   ├── tests/
│   │   ├── unit/
│   │   ├── integration/
│   │   └── e2e/
│   ├── docker-compose.yml
│   ├── Dockerfile
│   ├── pyproject.toml
│   ├── requirements.txt
│   └── requirements-dev.txt
├── tppe-advocacia-frontend/
│   ├── src/
│   │   ├── components/
│   │   └── services/
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
└── tppe-advocacia-docs/
    ├── docs/
    ├── mkdocs.yml
    └── requirements.txt
```

## Backend

### Stack

- Python + FastAPI
- SQLAlchemy
- PostgreSQL
- Alembic para migrations
- Pydantic Settings
- JWT com PyJWT
- Resend para envio de e-mail
- Docker Compose
- Ruff, Pytest, Pytest-Cov, Bandit e Pip Audit

### Padrão Arquitetural

O backend segue um **monólito modular**. Cada domínio do sistema fica isolado em `app/modules/<dominio>/`, com camadas internas próprias.

```text
app/modules/<dominio>/
├── model.py       # modelo ORM SQLAlchemy
├── schema.py      # contratos Pydantic de entrada e saída
├── repository.py  # acesso ao banco de dados
├── service.py     # regras de negócio
├── controller.py  # orquestração da operação
└── router.py      # endpoints HTTP FastAPI
```

Nem todos os módulos precisam de todas as camadas. O módulo `email`, por exemplo, possui um `protocol.py`, uma implementação real com Resend e uma implementação fake para testes.

### Módulos Atuais

| Módulo | Responsabilidade |
|---|---|
| `auth` | Login, emissão de JWT e recuperação de senha |
| `users` | Cadastro, edição, listagem e desativação de usuários |
| `audit_logs` | Registro e consulta de eventos de auditoria |
| `office_config` | Dados institucionais do escritório e conteúdo da landing page |
| `media` | Upload e servimento de imagens |
| `articles` | Artigos, rascunhos, publicação e preview |
| `leads` | Recepção e gestão de leads |
| `clients` | Cadastro, edição, busca e observações de clientes |
| `processes` | Processos, movimentações, status e anotações internas |
| `datajud` | Cliente e endpoints de sincronização individual e em lote de movimentações pela API pública DataJud |
| `external_api_logs` | Registro e consulta administrativa de chamadas a APIs externas |
| `email` | Abstração de envio de e-mail via Resend |
| `health` | Health check da API e do banco |

### Fluxo de Requisição

```text
HTTP Request
  └─► router.py
        └─► controller.py
              └─► service.py
                    └─► repository.py
                          └─► model.py
  ◄── Response serializada por schema.py
```

### Código Compartilhado

| Caminho | Uso |
|---|---|
| `app/api/router.py` | Agrega os routers dos módulos |
| `app/config/settings.py` | Configurações e variáveis de ambiente |
| `app/db/database.py` | Engine, sessão e inicialização do banco |
| `app/shared/auth_deps.py` | Dependências de autenticação e autorização |
| `app/shared/base_model.py` | Base declarativa do SQLAlchemy |
| `app/shared/email_deps.py` | Injeção do serviço de e-mail |
| `app/shared/exceptions.py` | Exceções de negócio padronizadas |
| `app/shared/responses.py` | Envelopes de resposta da API |
| `app/shared/types.py` | Tipos compartilhados, como `Role` |

### Testes e Qualidade

O backend possui três camadas de teste:

- `tests/unit/`: regras de negócio e validações isoladas.
- `tests/integration/`: repositórios e persistência.
- `tests/e2e/`: endpoints HTTP com FastAPI e PostgreSQL.

O CI executa:

- `ruff format --check`
- `ruff check`
- `bandit -r app/ -ll`
- `pip-audit -r requirements.txt`
- `pytest` para unitários, integração e e2e com coverage.

## Frontend

### Stack

- React
- Vite
- Fetch API

### Estado Atual

O frontend ainda é uma base inicial. Ele possui:

- health check da API;
- formulário público de envio de lead;
- configuração por `VITE_API_URL`.

As telas administrativas previstas no backlog, como painel de usuários, clientes, processos, artigos e configurações, ainda estão pendentes.

## Documentação

### Stack

- MkDocs
- Material for MkDocs

### Organização

- `docs/backlog/`: backlog, requisitos e status da implementação.
- `docs/arquitetura/`: estrutura técnica e modelo físico do banco.
- `mkdocs.yml`: navegação e configuração do site.
