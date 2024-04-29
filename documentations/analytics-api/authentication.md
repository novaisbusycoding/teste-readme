---
id: authentication
title: Autenticação
category: 662fa995369d3500194ad422
---

# Autenticação API

Como acessar a Analytics API Malga?

Você pode utilizar a sua aplicação ou o <a href="https://graphql.malga.io/">playground GraphiQL</a> para interagir com a API em tempo real.

Através dela, é possível visualizar os objetos, lista de campos e realizar as queries.

A autenticação para todos os chamadas da API é feita através de headers HTTP, sendo necessário informar seu identificador de cliente na Malga e a chave secreta de acesso.

Todas as requisições em GraphQL devem ser enviadas ao endpoint HTTP utilizando o método POST;

## Ambientes

<b>Sandbox</b>:

```
curl --location --request POST 'https://api.dev.malga.io/v1/graphql' \
--header 'X-Client-Id: YOUR_CLIENT_ID' \
--header 'X-Api-Key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{YOUR_QUERY}'
```

<b>Produção</b>:

```
curl --location --request POST 'https://api.malga.io/v1/graphql' \
--header 'X-Client-Id: YOUR_CLIENT_ID' \
--header 'X-Api-Key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{YOUR_QUERY}'
```

Se você ainda não possui uma conta de sandbox na Malga, você pode <a href="https://dashboard.malga.io/sign-up">criá-la aqui</a> e imediatamente obterá as credenciais para acesso ao ambiente de testes;

## Credenciais

### X-Client-ID

Identificador único da sua conta na Malga.

Deve ser enviado no header obrigatoriamente em todas as requisições feitas a API.

```
Security Scheme Type	API Key
Header parameter name	X-Client-ID
```

### X-Api-Key

Sua chave de acesso à API. Funciona em par com o client-id devendo ser enviado no header obrigatoriamente em todas as requisições feitas a API.

```
Security Scheme Type	API Key
Header parameter name	X-Client-ID
```

Exemplo de requisição autenticada

```
curl --location --request GET 'https://api.malga.io/v1/graphql' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    ...
```

## Autenticação no Playground GraphiQL

Para utilizar o Playground e realizar os testes na API, é necessário inserir o seu 'X-Client-Id' e 'X-Api-Key' no Headers localizado na janela inferior do ambiente, conforme o exemplo abaixo:

![Autenticação no Playground GraphiQL](/img/analytics/authentication-graphiQL.png)

:::info
Após inserir as chaves, para visualização da documentação e schema de dados, você deve realizar a atualização do ambiente, clicando no botão <b>Re-fecht GraphQL schema</b>.
:::
