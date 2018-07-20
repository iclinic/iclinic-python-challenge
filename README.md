## Desafio do iClinic


## Problema

Milhares de eventos são criados na Agenda do iClinic a todo momento, são muitos, mesmo! Com tantos eventos na agenda do profissional de saúde, surge o problema de como buscar esses agendamentos de uma maneira rápida e inteligente.

## Solução

Implemente uma API REST para que possamos realizar uma busca de pacientes dado uma query e essa API REST possa ser utilizada em um componente de autocomplete em nossa agenda.

O desafio é composto de duas partes:

1. O sistema de busca.

    - O sistema de busca pode ser visto como uma composição de 2 subcomponentes:

        1. Uma estrutura de dados para armazenar os nomes dos pacientes. Esta estrutura de dados deve ser rápida para lookups.

        2. Um algoritmo que, dado a estrutura de dados acima e uma query, retorna os possiveis resultados para aquela query.

2. Uma API REST que recebe uma query, faz uma busca no sistema de busca e retorna os possíveis resultados.

Exemplo de um endpoint para a API REST:

```bash
curl -X GET -H "Content-Type: application/json" http://localhost:8000/autocomplete/?q=Mar
```

Response:

```bash
{
    'patients': [
        'Marco Antonio',
        'Marcio Oliveira dos Santos',
        'Marco Aurelio',
        'Marco Nascimento',
        'Mario de Andrade da Silva',
    ]
}
```

## O que esperamos

- Que o desafio seja feito em **Python**.
- Que tenha testes automatizados.
- Que você implemente o sistema de buscas.
- Que você implemente a API REST para que, dado uma query, retorna os possiveis resultados.
- Que seja fácil de rodarmos seu desafio em um ambiente **Linux**.
- Que você utilize nosso dataset `events.csv` que está nesse repositório para popular sua estrutura de dados e podermos realizar as buscas.
- Que você popule o sistema de busca no setup/loading time e que ele permaneça imutável em memória durante o runtime.

## Dicas

**pode** não significa **deve**.

- Você **pode** implementar a API utilizando [Flask](https://github.com/pallets/flask).
- Você **pode** utilizar a biblioteca de testes [py.test](https://github.com/pytest-dev/pytest).
- Para implementar a estrutura de dados pesquise por `Tree` e outras variações dessa estrutura de dados.

Boa Sorte, 
Equipe iClinic DEV.