# Shared Kernel

Elementos genuinamente compartilhados entre bounded contexts — usados com
moderação, apenas quando duplicar custaria mais do que o acoplamento gerado.

## Conteúdo proposto

- **`EventBus` (porta):** interface publish/subscribe usada por todos os
  contextos para emitir e consumir eventos de domínio (ex.: `RelatoExcluido`,
  `CurtidaRealizada`, `ComentarioPublicado`). A implementação concreta
  (`RabbitMqEventBus`) vive na infraestrutura do contexto que a configura
  primeiro, mas a porta é compartilhada.
- **Erros de domínio comuns:** `NotFoundError`, `ForbiddenError`,
  `ValidationError` — hoje recriados de forma quase idêntica em
  `likeService.js`, `commentService.js` e `relatoService.js` (ver Violação 6
  da Questão 3 da avaliação: duplicação de `notFoundError()`/`parseId()`).
- **Value Objects reaproveitáveis:** `Id` (parsing/validação de identificadores
  numéricos), `Paginacao` (page/limit/offset).

> Regra geral: um elemento só entra aqui se **mais de um contexto** precisar
> dele com o mesmo significado. Caso contrário, ele pertence ao domínio do
> contexto que o usa.
