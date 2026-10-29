# Travel Stories

**Tipo de subdomínio:** core domain
**Agregado raiz:** `Relato`

É o motivo de o TravelBuddy existir: o relato de viagem (título, descrição,
foto) publicado por um viajante. Todo o valor de negócio diferenciado do
produto está aqui — por isso é o **core domain**, e recebe a maior atenção de
modelagem.

## domain/
- `Relato` (agregado raiz): título, descrição, imagem, autorId (referência
  por identidade a `Usuario`, nunca o objeto inteiro).
- Value Object: `ContadoresRelato` (visualizações, curtidas, comentários).
- Regras: limites de tamanho de título/descrição, obrigatoriedade de imagem.

## application/
- Casos de uso: `PublicarRelatoUseCase`, `EditarRelatoUseCase`,
  `ExcluirRelatoUseCase`, `ObterFeedUseCase`, `ObterDetalheRelatoUseCase`.
- Porta: `RelatoRepository`, `ArmazenamentoDeImagem`.
- Publica eventos de domínio (`RelatoExcluido`, `RelatoPublicado`) consumidos
  por outros contextos (ex.: Social Interaction, Notification — ver Questão 2
  da avaliação) em vez de chamar diretamente os módulos desses contextos.

## infrastructure/
- `SequelizeRelatoRepository` (implementa `RelatoRepository`).
- `LocalFileStorageAdapter` (implementa `ArmazenamentoDeImagem`, hoje via
  disco local/`express.static`).

## interface/
- `relatoController`, `relatoRoutes`, `relatoValidator`.

> Equivalente, no código atual do TravelBuddy, a `modules/relato/*`.
>
> **Nota de refatoração (Questão 3 da avaliação):** no código atual,
> `relatoService.deleteRelato` importa e manipula diretamente os models
> `Like` e `Comment` do contexto Social Interaction — uma violação de SRP e
> do limite entre contextos. Nesta reorganização, a exclusão de um relato
> apenas publica o evento `RelatoExcluido`; quem apaga as curtidas/comentários
> associados é o próprio contexto Social Interaction, que escuta esse evento.
