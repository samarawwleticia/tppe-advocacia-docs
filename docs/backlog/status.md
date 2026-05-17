# Status da Implementação

Esta matriz compara o backlog planejado com o estado atual do código. A avaliação considera a `main` publicada do backend após a integração da branch de desenvolvimento.

## Legenda

| Status | Critério |
|---|---|
| Feito | Há implementação funcional no backend, com testes cobrindo o fluxo principal |
| Parcial | Existe parte da funcionalidade, mas falta algum requisito, tela, integração ou regra relevante |
| Pendente | Não há implementação identificada no código atual |

## Resumo por Épico

| Épico | Status geral | Observação |
|---|---|---|
| Autenticação | Parcial | Login, reset e usuários existem; papéis ainda estão simplificados em `ADMIN` e `USER` |
| Landing Page | Parcial | Backend de conteúdo, mídia e artigos existe; frontend público e painel editorial ainda são iniciais |
| Gestão de Leads e Clientes | Parcial | Leads, clientes e observações existem; histórico completo e conversão lead-cliente ainda faltam |
| Core Jurídico | Parcial | Processos, movimentações, status e anotações existem; campos como advogado responsável/comarca ainda não estão completos |
| APIs Externas | Parcial | Há sincronização individual e em lote com a API pública DataJud, retentativa para falhas temporárias e log persistido de chamadas externas; ainda faltam agendamento periódico e notificações |
| Notificações | Parcial | Há envio de e-mail para reset/criação de usuário; faltam notificações configuráveis e eventos jurídicos |
| Backup, LGPD e Compliance | Pendente | Não há anonimização, consentimento registrado, exportação LGPD ou backup automatizado |
| Agenda e Prazos | Pendente | Não há agenda, compromissos, prazos, feriados ou alertas |
| Workflow e Tarefas | Pendente | Não há tarefas nem Kanban |

## Matriz por História

| ID | Status | Evidência atual | O que falta |
|---|---|---|---|
| US-01 | Feito | `POST /api/v1/auth/login` com JWT e testes unit/e2e | Expandir matriz de permissões por papel real |
| US-02 | Feito | Solicitação e confirmação de reset de senha por e-mail | Melhorar template/fluxo de frontend |
| US-03 | Parcial | CRUD de usuários, filtros, ativação/desativação e alteração de role | Modelar papéis Administrador, Advogado e Secretária |
| US-04 | Parcial | Auditoria para criação e desativação de usuários | Auditar outras operações críticas do sistema |
| US-05 | Parcial | `office_config` permite consultar e editar dados institucionais | Criar painel frontend de administração |
| US-06 | Parcial | Campos de hero, sobre, diferenciais e imagens existem no backend | Criar edição visual no painel |
| US-07 | Parcial | Endpoints públicos de artigos publicados existem | Criar seção pública de artigos no frontend |
| US-08 | Feito | Backend cria/edita artigos com status `draft`/`published` e imagem de capa | Agendamento automático de publicação, se mantido no requisito |
| US-09 | Feito | Endpoint de preview autenticado para artigos | Tela de preview no frontend |
| US-10 | Parcial | `POST /api/v1/leads` e formulário simples no frontend | Landing page definitiva e consentimento LGPD |
| US-11 | Feito | Listagem de leads com filtros, status e responsável | Tela administrativa |
| US-12 | Feito | CRUD de clientes com CPF/CNPJ, contato e endereço | Tela administrativa |
| US-13 | Parcial | Há clientes, notas e processos por cliente | Endpoint/tela agregada de histórico completo, origem como lead, documentos e compromissos |
| US-14 | Parcial | Busca de clientes por nome, CPF e CNPJ; processos por cliente | Busca direta de cliente por número de processo |
| US-15 | Feito | Observações internas de cliente com criação, listagem e edição | Melhorar tela de uso |
| US-16 | Parcial | Cadastro de processo com CNJ, vara/court, tipo, parte contrária e cliente | Advogado responsável, comarca/área e regras mais completas |
| US-17 | Feito | Movimentações manuais, movimentações externas DataJud e timeline por processo | Melhorar tela de uso |
| US-18 | Feito | Alteração de status com movimentação `SYSTEM` na mesma transação | Regras de transição mais específicas, se exigidas |
| US-19 | Feito | Anotações internas de processo com criação, listagem e edição | Melhorar tela de uso |
| US-20 | Parcial | `POST /api/v1/processes/{process_id}/datajud/sync-movements` sincroniza um processo e `POST /api/v1/datajud/sync-active-processes` sincroniza processos ativos em lote | Agendar execução periódica automática |
| US-21 | Parcial | Tabela `external_api_logs`, `GET /api/v1/external-api-logs` e retentativa automática para falhas temporárias do DataJud | Notificação ao administrador e política mais completa de retentativas assíncronas |
| US-22 | Parcial | Resend envia e-mails de reset e boas-vindas | Notificações configuráveis por usuário/evento |
| US-23 | Pendente | Não identificado | Notificar eventos de processos vinculados |
| US-24 | Pendente | Não identificado | Anonimização de ex-clientes e regras de retenção |
| US-25 | Pendente | Não identificado | Termo de consentimento no formulário e persistência do aceite |
| US-26 | Pendente | Não identificado | Modelo, endpoints e telas de compromissos |
| US-27 | Pendente | Não identificado | Cálculo de prazo em dias úteis e feriados por comarca |
| US-28 | Pendente | Não identificado | Alertas escalonados para prazos |
| US-29 | Pendente | Não identificado | Modelo, endpoints e vínculo de tarefas |
| US-30 | Pendente | Não identificado | Quadro Kanban e atualização de status |

## Lacunas Técnicas Importantes

1. O backend já possui migration inicial com Alembic e não executa `Base.metadata.create_all` em `APP_ENV=production`; falta incorporar `alembic upgrade head` ao processo automatizado de deploy.
2. O modelo físico está documentado e versionado em migration inicial; futuras alterações de schema devem manter o DER e as migrations sincronizados.
3. A integração externa jurídica existe via DataJud e possui retentativa para falhas temporárias; a sincronização em lote existe, mas ainda depende de acionamento manual.
4. O frontend não acompanha a amplitude do backend; a maior parte das funcionalidades está disponível apenas via API.
5. RBAC precisa ser refinado para os papéis reais do backlog.

## Próximos Passos Sugeridos

1. Transformar a sincronização em lote DataJud em execução periódica agendada.
2. Automatizar `alembic upgrade head` no fluxo de deploy.
3. Criar telas administrativas mínimas para usuários, leads, clientes e processos.
4. Expandir auditoria para clientes, processos, artigos e configurações.
5. Implementar consentimento LGPD no formulário de leads.
