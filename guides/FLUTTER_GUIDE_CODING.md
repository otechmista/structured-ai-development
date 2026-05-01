# Flutter Coding Guide

## Estrutura

- Organize por feature.
- Separe UI, estado, caso de uso e integracao.
- Widgets pequenos.
- Evite arquivo gigante.

## UI

- Estados: loading, empty, error, success.
- Use tema do app.
- Acessibilidade basica.
- Sem trabalho pesado no UI thread.
- Nao chame API no `build`.

## Estado

- Use padrao ja adotado.
- Regra fora de widget.
- Estados explicitos.
- Tela nao deve conhecer cliente externo direto.

## Navegacao

- Rotas centralizadas quando houver varios fluxos.
- Parametros validados.
- Back/deep link previsivel.

## Dados

- Modelo API separado do modelo UI quando semantica divergir.
- Trate timeout, rede e resposta invalida.
- Ignore resposta obsoleta se tela foi descartada.

## Testes

- Unidade para regra e mapper.
- Widget tests para estados visuais.
- Teste loading, empty, error, success.
- Fake para repositorio/cliente.

## Prompt

```txt
Gere Flutter seguindo a Code View.
Widgets pequenos. Estados explicitos. Regra fora da UI.
Siga o state management do projeto.
Teste estados visuais importantes.
Nao invente regra.
```
