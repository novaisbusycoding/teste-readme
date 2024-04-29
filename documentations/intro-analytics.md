---
sidebar_position: 1
id: intro-analytics
title: Analytics API
category: 662fa995369d3500194ad422
---

# Analytics API

## O que é?

A **Analytics API Malga** é um serviço que disponibiliza os dados dos pagamentos realizados através da nossa API,
retornando as informações registradas durante o seu processamento pelos provedores.

Desenvolvida em linguagem de consulta <a href="https://graphql.org"> GraphQL</a> e banco de dados **NoSQL OpenSearch**, oferece _alto desempenho_ nas consultas.

A Analytics API possibilita uma visão completa e acessível dos dados de transações e suas descrições, permitindo a realização de queries.

Através de uma requisição na Analytics API, é possível retornar e combinar dados de diferentes serviços da Malga, obtendo o conjunto de informações necessárias para **análises gerenciais e acompanhamento dos pagamentos processados** em nossa API.

:::info Atualização diária dos dados
Os dados da Analytics API estão disponíveis no padrão **D-1**, correspondendo às informações das cobranças processadas até o dia anterior. Nossa base de dados é atualizada diariamente entre **01:00 e 04:00 (UTC-3)** e, durante a janela diária de atualização, a API permanece indisponível para requisições.
:::

## Como usar este guia?

Neste guia, você encontrará informações sobre:

- Linguagem de consulta **GraphQL** e o playground **GraphiQL** da Analytics API;
- Como fazer chamadas na API _através do seu browser_ usando suas **credenciais da Malga**;
- **Lista completa de entidades, queries e tipos de objetos** disponibilizados pela Analytics API.

Para aprender mais sobre GraphQL, confira a <a href="https://graphql.org/learn/"> documentação oficial</a>, e para utilizar o playground GraphiQL da Analytics API, <a href="https://graphql.malga.io">acesse aqui</a>.
