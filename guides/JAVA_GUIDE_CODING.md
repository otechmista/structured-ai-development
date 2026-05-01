# Java Coding Guide

## Estrutura

- Pacotes por dominio/feature.
- Controllers, services, entities, repositories e integrations separados.
- Classes coesas.
- Sem regra em controller, DTO ou mapper.

## Estilo

- Nomes do dominio.
- Prefira imutabilidade.
- Use `record` para DTO simples quando disponivel.
- Composicao > heranca.
- Framework/annotation so com motivo.

## Erros

- Excecao de dominio para falha esperada.
- Mapeie erro na borda.
- Nao use excecao para fluxo comum se resultado explicito for melhor.
- Mensagem util, sem dado sensivel.

## API

- Valide DTO de entrada.
- Contrato estavel.
- Modelo externo != modelo interno.
- Nao exponha entidade de persistencia.

## Persistencia

- Transacao com limite claro.
- Evite query implicita cara.
- Constraint importante tambem no banco.
- Teste repositorio critico.

## Testes

- Unidade para regra.
- Integracao para persistencia, contrato e wiring.
- Builders/factories para objeto complexo.
- Cubra validacao, erro e transacao.

## Prompt

```txt
Gere Java seguindo a Code View.
Organize por dominio. Controllers finos. Services explicitos. Contratos estaveis.
Teste regras e erros. Nao coloque regra em DTO, mapper ou controller.
Nao invente regra.
```
