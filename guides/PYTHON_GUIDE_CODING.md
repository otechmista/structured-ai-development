# Python Coding Guide

## Estrutura

- Modulos por responsabilidade.
- Entrada, regra e integracao separados.
- Arquivos pequenos.
- Imports sem efeito colateral pesado.

## Estilo

- Type hints em funcoes publicas.
- Funcoes pequenas.
- `dataclass` ou modelo da stack para dados.
- Sem mutacao global.
- Nome claro.

## Erros

- Excecao especifica para erro de dominio.
- Nao capture `Exception` sem tratar ou relancar.
- Preserve causa: `raise ... from exc`.
- Valide entrada na borda.

## Dependencias

- Injete cliente externo/repositorio quando ajudar teste.
- Chamada de rede/banco deve ser visivel.
- Biblioteca nova so com motivo.

## API/CLI/job

- API: schemas, handlers e services separados.
- CLI: parsing separado da regra.
- Job: log inicio, fim, metricas e falhas recuperaveis.

## Testes

- Use `pytest` se possivel.
- Teste regra com entrada/saida clara.
- Fixture pequena.
- Mocke IO externo.
- Cubra validacao e erro.

## Prompt

```txt
Gere Python seguindo a Code View.
Use type hints, responsabilidades separadas e pytest.
Valide entradas na borda. Preserve contexto dos erros.
Nao invente regra.
```
