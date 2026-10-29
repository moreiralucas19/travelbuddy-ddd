# TravelBuddy — Arquitetura DDD (Avaliação N2)

> Reorganização arquitetural do projeto **TravelBuddy** (Programação Web) segundo
> Domain-Driven Design, elaborada para a Avaliação N2 da disciplina Projeto e
> Arquitetura de Software — Católica SC.
>
> **Estudante:** Lucas da Silveira Branco Moreira
> **Professor:** Luiz C. Camargo, PhD

Este repositório documenta a **organização arquitetural** proposta (Questão 1 da
avaliação). Conforme o enunciado, não é necessário conter o código-fonte
funcional do projeto web — o objetivo é demonstrar a transformação da
arquitetura atual (módulos funcionais por tipo técnico: `model`, `service`,
`controller`) em **bounded contexts** organizados em camadas (Domain,
Application, Infrastructure, Interface), no espírito da arquitetura hexagonal.

O código-fonte real do TravelBuddy (Node.js/Express/Sequelize + Vue 3) segue
nos repositórios da disciplina de Programação Web; este repositório é
especificamente o artefato de arquitetura pedido na N2.

## Domínio

O TravelBuddy é uma rede social de relatos de viagem: viajantes se cadastram,
publicam relatos (título + descrição + foto), navegam por um feed, curtem e
comentam relatos de outras pessoas, e consultam perfis públicos.

## Bounded contexts

| Contexto | Pasta | Papel no domínio | Agregado(s) |
|---|---|---|---|
| **Identity & Access** | [`identity-access/`](identity-access/) | Subdomínio genérico — cadastro, autenticação, perfil | `Usuario` |
| **Travel Stories** | [`travel-stories/`](travel-stories/) | **Core domain** — o relato de viagem | `Relato` |
| **Social Interaction** | [`social-interaction/`](social-interaction/) | Subdomínio de apoio — curtidas e comentários | `Curtida`, `Comentario` |
| **Search** | [`search/`](search/) | Subdomínio genérico — busca global | *(read-model)* |
| **Shared Kernel** | [`shared-kernel/`](shared-kernel/) | Elementos compartilhados entre contextos | — |

Cada bounded context é independente e se comunica com os demais apenas por
**identidade** (ids) ou por **eventos de domínio** — nunca referenciando
entidades de outro contexto diretamente. Ver a Questão 2 da avaliação para a
camada de mensageria que liga *Social Interaction* a um novo serviço de
notificações.

## Camadas dentro de cada contexto

```
<contexto>/
├── domain/           # Entidades, agregados, value objects, regras de negócio puras
├── application/      # Casos de uso (orquestram o domínio), portas (interfaces)
├── infrastructure/   # Adaptadores concretos: Sequelize, sistema de arquivos, etc.
└── interface/        # Controllers, rotas, validators — a porta de entrada HTTP
```

A dependência sempre aponta **para dentro**: `interface` depende de
`application`, que depende de `domain`; `infrastructure` implementa portas
definidas em `application`/`domain`, nunca o contrário (princípio da inversão
de dependência).

## Documento completo

A justificativa detalhada de cada contexto, o mapa de bounded contexts, a
camada de mensageria (C4) e a inspeção de violações SOLID estão no PDF da
Avaliação N2 entregue no Teams.
