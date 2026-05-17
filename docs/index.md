# Documentação Técnica

Esta é a documentação do projeto de advocacia desenvolvido na disciplina TPPE.

## Sobre o Projeto

O sistema foi desenvolvido com o objetivo de apoiar a gestão de um escritório de advocacia, cobrindo presença online, recepção de leads, cadastro de clientes, controle de processos, movimentações, anotações internas e trilhas de auditoria.

Nesta etapa, o projeto está organizado em três repositórios na organização do Git:

- `tppe-advocacia-backend` com FastAPI, PostgreSQL, SQLAlchemy e monólito modular
- `tppe-advocacia-frontend` com React e Vite para a interface inicial de integração
- `tppe-advocacia-docs` com as páginas da documentação técnica em MkDocs

## Estado Atual

A implementação mais avançada está no backend. A branch `main` do backend contém módulos de autenticação, usuários, auditoria, configuração institucional, mídia, artigos, leads, clientes, processos, movimentações, anotações, integração DataJud e logs de APIs externas.

O frontend ainda está em estágio inicial, com health check e envio público de leads. As telas administrativas previstas no backlog ainda não foram implementadas.

## Como navegar

Utilize o menu de navegação no topo para acessar as seções da documentação:

- **Inicio** — Esta página de apresentação
- [**Backlog**](./backlog/index.md) — Histórias de usuário planejadas
- [**Status da Implementação**](./backlog/status.md) — O que está feito, parcial ou pendente
- **Arquitetura** — Estrutura atual do backend e modelo físico do banco

## Links úteis

- [Organização no GitHub](https://github.com/Projeto-Advocacia-TPPE)
- [Repositório da documentação](https://github.com/Projeto-Advocacia-TPPE/tppe-advocacia-docs)
- [Protótipo web no Figma](https://www.figma.com/proto/Rmih83396wf3H6qCvkzieO/Prot%C3%B3tipo-TPPE?node-id=34-4263&p=f&t=2zZdNgLHAasZedhN-1&scaling=scale-down&content-scaling=fixed&page-id=0%3A1&starting-point-node-id=34%3A4263&show-proto-sidebar=1)
- [Protótipo mobile no Figma](https://www.figma.com/proto/Rmih83396wf3H6qCvkzieO/Prot%C3%B3tipo-TPPE?node-id=598-7967&p=f&t=w2cbP6sN47bXqYEM-1&scaling=scale-down&content-scaling=fixed&page-id=439%3A3085&starting-point-node-id=598%3A7967&show-proto-sidebar=1)
