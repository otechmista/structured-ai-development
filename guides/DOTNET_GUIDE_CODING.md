# .NET Coding Guide

## Estrutura

```txt
MyProduct/
+-- MyProduct.Api/
+-- MyProduct.Application/
+-- MyProduct.Domain/
+-- MyProduct.Infrastructure/
+-- MyProduct.Tests/
```

## Criar

```bash
dotnet new sln -n MyProduct
dotnet new webapi -n MyProduct.Api
dotnet new classlib -n MyProduct.Application
dotnet new classlib -n MyProduct.Domain
dotnet new classlib -n MyProduct.Infrastructure
dotnet new xunit -n MyProduct.Tests
dotnet sln add **/*.csproj
```

## Camadas

| Camada | Faz | Nao faz |
|---|---|---|
| Api | endpoints, auth, DTOs, mapear erros | regra de negocio |
| Application | casos de uso, contratos, transacao | HTTP/banco direto |
| Domain | entidades, VOs, regras, erros | framework ou infra |
| Infrastructure | EF/Dapper, clientes, filas, config | regra de negocio |
| Tests | unidade e integracao | testar detalhe inutil |

## Regras

- Controllers/endpoints finos.
- DTO != entidade.
- Regra no Domain/Application.
- Infra implementa interfaces.
- Config sensivel fora do codigo.
- DI para repositorios e clientes.
- `record` para DTO simples.
- `required` quando fizer sentido.
- Evite `Manager`, `Helper`, `Utils`.

## API

- Request/response explicitos.
- Status codes consistentes.
- Erro padrao.
- Valide payload antes do caso de uso.
- Nao exponha stack trace.

## Persistencia

- Migrations para schema.
- Constraints importantes no banco.
- Queries explicitas para leitura complexa.
- Transacao no limite do caso de uso.
- Evite carregar dados demais.

## Comandos

```bash
dotnet restore
dotnet build
dotnet test
dotnet run --project MyProduct.Api
```

## Testes

- Domain rules.
- Use cases.
- Validacao.
- Erros esperados.
- Auth quando API.
- Integracao com banco/infra critica.

## Prompt

```txt
Gere codigo .NET seguindo a Code View.
Use Api, Application, Domain, Infrastructure e Tests quando houver regra relevante.
Endpoints finos. Regra no domain/application. Infra isolada. Contratos explicitos.
Inclua testes. Nao invente regra.
```
