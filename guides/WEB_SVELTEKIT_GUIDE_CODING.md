# Web Coding Guide with SvelteKit

## Estrutura

- Rotas do SvelteKit para paginas/endpoints.
- Componentes reutilizaveis separados.
- Regra sensivel no servidor.
- Sem dominio complexo em `.svelte`.

## SvelteKit

- `+page.server.ts`: dado/action server-only.
- `+page.ts`: dado que pode rodar client/server.
- `+server.ts`: endpoint.
- Use helpers do SvelteKit para redirect/error.
- Nunca exponha secret ao browser.

## UI

- Componentes pequenos.
- Estados: loading, empty, error, success.
- Forms com label, erro e foco previsivel.
- Sem duplicar formatacao.

## Dados

- Valide actions, endpoints e integracoes.
- Schema fora do componente visual.
- Normalize API antes da UI.
- Erro de dominio deve virar mensagem util.

## Seguranca

- Server valida auth e autorizacao.
- UI nao e barreira de seguranca.
- Sanitize HTML/markdown externo.
- Variavel privada nunca no cliente.

## Testes

- Unidade para regra/validacao.
- Componente para comportamento visual relevante.
- E2E para fluxo critico.
- Cubra form action, validacao e autorizacao.

## Prompt

```txt
Gere SvelteKit seguindo a Code View.
Regras sensiveis no servidor. Use load/actions/endpoints corretamente.
Valide entradas. Cubra estados de UI e erros importantes.
Nao exponha secrets. Nao invente regra.
```
