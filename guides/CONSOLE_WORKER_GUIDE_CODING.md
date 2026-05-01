# Console App and Worker Coding Guide

## Use para

- CLI.
- Console app.
- Worker.
- Job agendado.
- Batch.
- Importador/exportador.
- Consumer de fila/evento.

## Antes

- Defina gatilho.
- Defina entrada e saida.
- Defina efeito colateral.
- Defina sucesso e falha.
- Defina retry, timeout e cancelamento.
- Defina idempotencia.
- Defina logs, metricas e alerta.

## Camadas

| Camada | Faz | Nao faz |
|---|---|---|
| Entrypoint | args, config, logging, DI, cancelamento | regra de negocio |
| Application | fluxo, lote, retry, checkpoint, transacao | detalhe externo |
| Domain | regra e invariante | fila, banco, framework |
| Infrastructure | fila, banco, arquivo, API externa | regra de negocio |

## CLI

- Tenha `--help`.
- Valide args antes de executar.
- Suporte `--dry-run` quando altera dados.
- Use exit codes claros.
- Evite prompt interativo em automacao.

## Exit codes

```txt
0 sucesso
1 erro inesperado
2 argumentos invalidos
3 config invalida
4 dependencia indisponivel
5 falha parcial
```

## Worker

- Encerramento gracioso.
- Cancelamento por sinal/contexto.
- Concorrencia limitada.
- Backoff em falha ou fila vazia.
- Sem loop quente.
- Ack so apos sucesso.
- Dead-letter para mensagem invalida.

## Batch/job

- Lotes pequenos.
- Checkpoint.
- Idempotencia.
- Progresso em log.
- Timeout por item/lote.
- Falha parcial rastreavel.

## Config

- Config obrigatoria validada no startup.
- Segredo via env/secret manager.
- Batch size, concorrencia, timeout e dry-run configuraveis.
- Nada critico hardcoded.

## Observabilidade

- Log inicio, progresso, fim e erro.
- Inclua job id, batch id, message id ou correlation id.
- Metricas: sucesso, falha, retry, lag, latencia.
- Alertas para falha repetida, fila acumulada e job travado.

## Testes

- Regra.
- Args/flags.
- Dry-run.
- Retry.
- Timeout.
- Cancelamento.
- Falha parcial.
- Idempotencia.

## Runbook

- Como rodar.
- Config obrigatoria.
- Permissoes.
- Como ver sucesso.
- Como investigar falha.
- Como reprocessar.
- Como pausar/parar.

## Prompt

```txt
Gere console app/worker seguindo a Code View.
Separe entrypoint, application, domain e infrastructure.
Inclua config, logs, cancelamento, erro, retry com limite e testes.
Se houver efeito colateral, use idempotencia e dry-run.
Nao invente regra.
```
