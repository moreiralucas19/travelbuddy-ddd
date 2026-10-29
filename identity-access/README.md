# Identity & Access

**Tipo de subdomínio:** genérico
**Agregado raiz:** `Usuario`

Responsável pelo cadastro, autenticação (JWT) e manutenção do perfil do
viajante. É um subdomínio genérico porque, em tese, poderia ser substituído
por um provedor de identidade de terceiros sem alterar o valor central do
produto (que é o relato de viagem, não a autenticação em si).

## domain/
- `Usuario` (entidade/agregado): id, username, email, senha (hash), nome,
  bio, foto de perfil, contadores (seguidores, seguindo, relatos).
- Value Objects: `Email`, `Senha` (regras de formato/força).
- Regras de unicidade de `username`/`email`.

## application/
- Casos de uso: `RegistrarUsuarioUseCase`, `AutenticarUseCase`,
  `AtualizarPerfilUseCase`, `ObterPerfilPublicoUseCase`.
- Portas (interfaces) consumidas pelo domínio/aplicação: `UsuarioRepository`,
  `PasswordHasher`, `TokenService`.

## infrastructure/
- `SequelizeUsuarioRepository` (implementa `UsuarioRepository`).
- `BcryptPasswordHasher` (implementa `PasswordHasher`).
- `JwtTokenService` (implementa `TokenService`).

## interface/
- `userController`, `userRoutes`, `userValidator` — camada HTTP que traduz
  requisições Express em chamadas aos casos de uso acima.

> Equivalente, no código atual do TravelBuddy, a `modules/user/*`.
