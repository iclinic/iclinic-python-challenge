![iClinic logo](https://d1ydp7gtfj5fb9.cloudfront.net/static/img/views/home_v2/header/logo.png?1525283729)


## Problema

Milhares de pacientes são atendidos no iClinic a todo momento, são muitos, mesmo! Com tantos pacientes, surge o problema de prover uma busca rápida e inteligente para nossos clientes.

## Solução

Implemente uma API REST para realizar uma busca de pacientes dado uma query e retornar os possíveis resultados.

O desafio é composto de duas partes:

1. O sistema de autocomplete.

    1. Criar uma Estrutura de Dados para armazenar os nomes dos pacientes.

    2. Implementar um algoritmo que, dado a estrutura de dados acima e uma query, retornará possiveis pacientes.

2. Uma API REST (Você pode utilizar qualquer biblioteca que quiser) que recebe uma query, faz uma busca no sistema de autocomplete utilizando o algoritmo de busca e retorne os possíveis pacientes.

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

- Que o desafio seja feito em **Python** 3+.
- Que tenha testes automatizados.
- Que você implemente o sistema de autocomplete.
- Que você implemente a API REST para que, dado uma query, retorne os possíveis pacientes a partir do sistema de autocomplete.
- Que seja fácil de rodarmos seu desafio em um ambiente **Linux**.
- Que você utilize nosso dataset `patients.csv` que está nesse repositório para popular sua Estrutura de Dados.
- Que você popule a Estrutura de Dados quando a API REST inicializar, em memória.

## O que não esperamos

- Que você utilize ORMs e Banco de Dados.


Boa Sorte,  
Equipe iClinic DEV.