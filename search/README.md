# Search

**Tipo de subdomínio:** genérico
**Relação com os demais contextos:** *Conformist* — apenas lê projeções
publicadas por Identity & Access e Travel Stories, sem poder influenciá-los.

Hoje é um stub no código real (`globalSearch` sempre retorna listas vazias).
Conceitualmente, é um *read-model*: não possui agregados próprios, apenas
consulta dados já existentes em outros contextos (ou, futuramente, um índice
de busca dedicado alimentado por eventos).

## application/
- Caso de uso: `BuscarGlobalUseCase`.

## infrastructure/
- Hoje: consulta direta (stub). Evolução futura: adaptador para um índice de
  busca dedicado (ex. Elasticsearch/MeiliSearch), populado de forma
  assíncrona a partir dos eventos de domínio dos outros contextos.

## interface/
- `searchController`, `searchRoutes`.

> Equivalente, no código atual do TravelBuddy, a `modules/search/*`.
