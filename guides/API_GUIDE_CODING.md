# API Coding Guide

## Use para

- HTTP APIs.
- REST.
- RPC.
- Endpoints internos.

## Antes

- Defina consumidores.
- Defina recursos e operacoes.
- Defina contratos: request, response, error.
- Defina auth, permissoes e rate limit.
- Defina versionamento se houver consumidor externo.

## Camadas

| Camada | Faz | Nao faz |
|---|---|---|
| API | request, auth inicial, validacao, response | regra de negocio |
| Application | caso de uso, transacao, orquestracao | HTTP ou banco direto |
| Domain | regra, entidade, invariante, erro | framework, banco, fila |
| Infrastructure | banco, cache, fila, clientes externos | regra de negocio |

## Contrato

- Payload claro.
- Campos obrigatorios explicitos.
- Erro padrao.
- Paginacao em listas.
- Nao exponha modelo interno.
- Nao retorne dado inutil.

## Endpoints

- Nome por recurso.
- Um objetivo por endpoint.
- Metodo HTTP semantico.
- Status code consistente.
- Comportamento documentado.

## Validacao

- Formato na API.
- Regra no domain/application.
- Rejeite campo desconhecido se isso reduzir risco.
- Normalize antes de aplicar regra.

## Erros

- `400`: request invalido.
- `401`: nao autenticado.
- `403`: sem permissao.
- `404`: nao encontrado.
- `409`: conflito.
- `422`: regra invalida.
- `429`: rate limit.
- `500`: erro inesperado.
- `503`: dependencia indisponivel.

## Seguranca

- Auth no servidor.
- Autorizacao no servidor.
- Menor privilegio.
- HTTPS externo.
- CORS explicito.
- Nunca logar segredo, token ou dado sensivel sem motivo.

## Operacao

- Timeout em chamada externa.
- Retry so quando seguro.
- Idempotencia para operacao critica.
- Paginacao obrigatoria.
- Evite N+1.
- Cache so com expiracao e invalidacao claras.

## Testes

- Sucesso.
- Validacao.
- Auth e permissao.
- Erro esperado.
- Contrato publico.
- Integracao critica.

## Prompt

```txt
Gere API seguindo a Code View.
Separe API, Application, Domain e Infrastructure.
Contratos explicitos. Validacao na borda. Regras no domain/application.
Erros padronizados. Testes para sucesso, validacao, auth e erros.
Nao invente regra.
```
