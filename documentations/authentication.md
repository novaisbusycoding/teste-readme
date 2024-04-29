---
title: Autenticação
category: 662fa995369d3500194ad422
---

Os serviços da Malga são protegidos através de chaves de acesso. Você pode gerenciar suas chaves de acesso através do seu <a href="https://dashboard.malga.io/">dashboard</a>.

É importante armazenar suas chaves de maneira privada e segura, uma vez que elas possuem privilégios de alteração na sua conta. Não compartilhe suas chaves, não deixe elas fixadas no seu código e nem armazene elas no seu servidor de controle de versão. Recomendamos utilizar variáveis de ambiente secretas para deixar a chave disponível para sua aplicação.

:::info
A Autenticação para todas as chamadas da API é feita através de headers HTTP, sendo necessário informar seu identificador de cliente na Malga e a chave secreta de acesso.
:::

```bash
  curl --location --request GET 'https://api.malga.io/v1/' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>'
```

## Client Token

É possível criar chaves públicas de acesso temporária a API com escopo e tempo de expiração limitados.

Recomendamos o uso deste tipo de chave quando você tiver que expor a chave em uma aplicação client side.

:::tip
Neste caso, você deve fazer uma chamada no [Serviço de Autenticação](/api#section/Authentication) a partir de sua chave secreta, solicitando a criação de uma chave pública com escopo limitado.
:::

```bash
  curl --location --request POST 'https://api.malga.io/v1/auth' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
      "scope":["tokens"],
      "expires": 31104000"
    }'

  < HTTP/2 201
  < content-type: application/json; charset=utf-8
  {
    "clientId":"<YOUR_CLIENT_ID>",
    "publicKey":"<YOUR_PUBLIC_KEY>",
    "scope":["tokens"],
    "expires": 31104000,
    "createdAt": "20200110 00:00:00"
  }
```

:::caution
A sua chave pública criada pode ser usada normalmente como se fosse a chave secreta da sua conta, porém com a restrição imposta pelo escopo e sendo invalidada após a expiração.
:::

```bash
  curl --location --request GET 'https://api.malga.io/v1/tokens' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_PUBLIC_KEY>'
```
