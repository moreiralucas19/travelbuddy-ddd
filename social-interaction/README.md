# Social Interaction

**Tipo de subdomínio:** apoio (supporting)
**Agregados raiz:** `Curtida`, `Comentario`

Reage a relatos publicados no contexto Travel Stories, mas não os modifica.
Referencia `relatoId` e `userId` **por identidade**, nunca por referência de
objeto — respeitando o limite do agregado `Relato` e o limite do agregado
`Usuario`.

## domain/
- `Curtida`: userId, relatoId (par único).
- `Comentario`: userId, relatoId, conteúdo.

## application/
- Casos de uso: `AlternarCurtidaUseCase` (toggle like), `PublicarComentarioUseCase`,
  `ListarComentariosUseCase`, `ListarRelatosCurtidosUseCase`.
- Portas: `CurtidaRepository`, `ComentarioRepository`.
- Escuta o evento `RelatoExcluido` (publicado pelo contexto Travel Stories)
  para remover curtidas/comentários órfãos — em vez de ser o contexto
  Travel Stories a fazer isso diretamente (ver nota em `travel-stories/README.md`).
- Publica os eventos `CurtidaRealizada` e `ComentarioPublicado` na camada de
  mensageria (RabbitMQ) consumidos pelo novo serviço de Notificações — ver
  Questão 2 da avaliação (diagramas C4).

## infrastructure/
- `SequelizeLikeRepository`, `SequelizeCommentRepository`.
- `RabbitMqEventPublisher` (implementa a porta de publicação de eventos).

## interface/
- `likeController`, `commentController`, rotas correspondentes.

> Equivalente, no código atual do TravelBuddy, a `modules/like/*` e
> `modules/comment/*`.
