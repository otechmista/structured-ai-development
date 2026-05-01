# Go Coding Guide

## Estrutura

- Pacotes por responsabilidade.
- `main` pequeno.
- Sem ciclo entre pacotes.
- Evite `util`, `common`, `helper`.

## Estilo

- Rode `gofmt`.
- Rode `go vet`.
- Codigo direto.
- Interface no consumidor.
- Abstracao so se simplificar teste ou dependencia.

## Erros

- Retorne erro.
- Nao engula erro.
- Use contexto: `fmt.Errorf("acao: %w", err)`.
- Erro sentinela so se chamador compara.

## Contexto e concorrencia

- `context.Context` em IO, rede, banco e API externa.
- Goroutine so com motivo.
- Cancelamento claro.
- Estado mutavel protegido.
- Worker encerra limpo.

## API

- Handler sem regra de negocio.
- Valide payload na entrada.
- Mapeie erro de dominio para HTTP/CLI/fila.

## Testes

- Table tests para regras.
- Teste sucesso, validacao, erro e borda.
- Fake pequeno > mock complexo.

## Prompt

```txt
Gere Go idiomatico seguindo a Code View.
Pacotes pequenos. Erros explicitos. context.Context em IO externo.
Handlers finos. Testes de tabela.
Nao invente regra.
```
