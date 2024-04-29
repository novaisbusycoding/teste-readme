---
id: glossary
title: Glossário
category: 662fa995369d3500194ad422
---

# Glossário

![Fluxo de criação de token](/img/analytics/glossary.png)

| Object Type | Schema | Queries | Field | Argument |
| ----------- | ------ | ------- | ----- | -------- |

### Objeto &lsqb;type&rsqb;

Cada parte de um esquema em GraphQL é um tipo de objeto, que consiste em uma lista de campos e o valor de cada campo;

### Esquema &lsqb;schema&rsqb;

O esquema (schema), ou sistema de tipos (types), é a base de qualquer serviço em GraphQL e descreve o escopo de todas entidades (types) e consultas (queries) disponíveis pela API.

Com o esquema, é possível realizar a validação de cada requisição enviada, além de permitir a relação de consultas de introspecção, possibilitando o entendimento das entidades descritas no esquema.

### Consulta &lsqb;queries&rsqb;

A consulta (query) descreve como é feita a leitura dos dados em GraphQL e como a operação irá retornar os dados do servidor, permitindo a consulta de diferentes entidades em cada requisição.

Na Analytics API da Malga, estão disponíveis as entidades de Charges.

### Campo &lsqb;field&rsqb;

Cada campo corresponde a uma unidade de dados que é retornada dentro do objeto.

### Argumento &lsqb;argument&rsqb;

Os campos de um objeto em GraphQL podem ser descritos como uma função que retorna um valor e que aceitam argumentos pré-definidos, onde cada argumento pode ser qualquer campo que não seja um objeto.

Na Analytics API Malga, disponibilizamos argumentos como filtros por ID e status de transação, que podem ser aplicados aos conjuntos de dados das entidades;
