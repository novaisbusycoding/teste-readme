---
id: sessions
title: Sessões
category: 662fa995369d3500194ad422
---

# Sessões

O Serviço de Sessões foi construído para permitir a criação de pedidos e pagamento destes pela Malga. Além de ser uma ferramenta que pode ser combinada com o Malga Checkout para aumentar a segurança da implementação do checkout feita no front-end.
É possível criar sessões utilizando Pix, boleto e cartão de crédito, podendo cada sessão ser paga apenas uma única vez.

## Utilizando as sessões

Para utilizar o Serviço de Sessões, você deve primeiro criar uma sessão, definindo seus itens, valor e métodos de pagamento. Depois, utilizando a **publicKey** retornada na criação da sessão, que possui um escopo mais restrito, você poderá acessar o endpoint de pagamento de uma sessão e pagar ela, sendo sempre um pagamento por sessão. O diagrama abaixo ilustra o fluxo:

![Fluxo Sessões](/img/paying-session-ptbr.png)

### Criando uma sessão

Realize a criação e gestão de sessões usando o [Serviço de Sessões](/api#operation/createSession).

```bash
curl --location --request POST 'https://api.malga.io/v1/sessions/{id}/charge' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <PUBLIC_KEY_DA_SESSÃO>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "amount": 100,
        "name": "Loja 1",
        "merchantId": "884dd2a3-e400-4525-8188-2955ef403486",
        "dueDate": "2023-04-04T09:28:45.000Z",
        "paymentMethods": [
            {
            "paymentType": "pix",
            "expiresIn": 30
            }
        ],
        "items": [
            {
                "name": "Item",
                "description": "Item do carrinho",
                "unitPrice": 1000,
                "quantity": 1,
                "tangible": false
            }
        ]
    }'

< HTTP/2 201
{
  "id": "c1db83fa-723c-4e1f-9722-bc19d1be6791",
  "name": "Pedido 1",
  "status": "created",
  "isActive": true,
  "clientId": "39d2d314-5412-431a-b34b-74f9f0fbe7e1",
  "orderId": "b84b7694-d22f-4083-bee7-c1274b16eb4a",
  "amount": 100,
  "currency": "BRL",
  "capture": true,
  "merchantId": "9930c8d9-a7a8-4039-9faf-3715ad87baf8",
  "dueDate": "2022-10-26T19:32:08.000Z",
  "description": "Pedido Black Friday",
  "statementDescriptor": "LOJA JOAO",
  "items": [
    {
      "name": "Item 1",
      "description": "Item do carrinho",
      "unitPrice": 1000,
      "quantity": 1,
      "tangible": false
    }
  ],
  "paymentMethods": [
    {
      "paymentType": "pix",
      "expiresIn": 30
    }
  ],
  "createdAt": "2022-10-25T22:49:06.588Z",
  "updatedAt": "2022-10-25T22:49:06.588Z",
  "publicKey": "8be71cdf-01dc-4b1a-823a-4c58be6e4cf1"
}
```

### Pagando uma sessão

Pague uma sessão utilizando o [Serviço de Sessões](/api#operation/paySession).

```bash
curl --location --request POST 'https://api.malga.io/v1/sessions/{id}/charge' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <PUBLIC_KEY_DA_SESSÃO>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
        "amount": 150,
        "currency": "BRL",
        "statementDescriptor": "LOJA JOAO",
        "description": "Descrição longa da cobrança",
        "capture": false,
        "orderId": "32c68ff7-902c-408b-b464-cf487c7cda97",
        "paymentMethod": {
            "paymentType": "credit",
            "installments": 1
        },
        "paymentSource": {
            "sourceType": "card",
            "card": {
                "cardNumber": "5261424250184574",
                "cardCvv": "321",
                "cardExpirationDate": "06/2028",
                "cardHolderName": "JOAO DA SILVA"
            }
        }
        }'

< HTTP/2 201
{
   "id": "c1db83fa-723c-4e1f-9722-bc19d1be6791",
   "status": "paid",
   "clientId": "39d2d314-5412-431a-b34b-74f9f0fbe7e1",
   "orderId": "b84b7694-d22f-4083-bee7-c1274b16eb4a",
   "customerId": "eb70b146-85fd-4100-8fd4-a4dbb647aed3",
   "amount": 100,
   "originalAmount": 100,
   "currency": "BRL",
   "capture": true,
   "merchantId": "9930c8d9-a7a8-4039-9faf-3715ad87baf8",
   "statementDescriptor": "LOJA JOAO",
   "paymentMethods": [
      {
         "paymentType": "pix",
         "expiresIn": 30
      }
   ],
   "paymentSource": {
      "sourceType": "card",
      "cardId": "148d5db0-f1c3-439f-902d-f1f268086e1d"
   },
   "transactionRequests": [
      {
         "id": "78601913-a176-4d71-b7e8-abb6fc49a340",
         "idempotencyKey": "fafe857b176e45d6b12e32fcaf228996",
         "providerId": "2c3b57d8-ee43-4b19-bc8a-949a88c51df1",
         "providerType": "STRIPE",
         "transactionId": "ch_3JYE7MHjGFBGEeiP0lfTD3Ob",
         "amount": 100,
         "authorizationNsu": "1cc8391c-f0d5-4b7a-9fcf-653cea26be13",
         "requestStatus": "success",
         "requestType": "authorization",
         "responseTs": "2633ms",
         "createdAt": "2021-08-12T16:08:39.536Z",
         "updatedAt": "2021-08-12T16:08:42.212Z",
         "providerAuthorization": {
            "networkAuthorizationCode": "00",
            "networkResponseCode": ""
         }
      }
   ],
   "createdAt": "2022-10-25T22:49:06.588Z"
}
```

## Integrando o MalgaCheckout com sessões

Para utilizar o Malga Checkout em conjunto com o Serviço de Sessões de uma maneira mais segura, é possível criar uma sessão pelo seu back-end e, em conjunto com uma aplicação front-end, utilizar a `publicKey` de escopo restrito retornada para configurar o checkout sem expor a `publicKey` de escopo mais aberto que é utilizada normalmente. O diagrama abaixo ilustra o fluxo:

![Fluxo Sessões](/img/using-session-on-front-ptbr.png)

### Usando o MalgaCheckout com sessões

Depois de criar a sessão, é possível usar o `id` dela e a `publicKey` para configurar o MalgaCheckout. Para que fique seguro, recomendamos que a `publicKey` usada normalmente, de escopo mais aberto, não fique exposta no `front-end`, este só deve ter acesso a chave pública da sessão.

```html
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=5.0"
    />
    <script
      type="module"
      src="https://unpkg.com/@malga-checkout/core@latest/dist/malga-checkout/malga-checkout.esm.js"
    ></script>
    <title>Malga Checkout Components</title>
  </head>
  <body>
    <main>
      <malga-checkout
        sandbox="false"
        public-key="<PUBLIC_KEY_DA_SESSÃO>"
        client-id="<YOUR_CLIENT_ID>"
        session-id="<ID_DA_SESSÃO>"
      >
      </malga-checkout>
    </main>

    <script>
      const malgaCheckout = document.querySelector("malga-checkout");

      malgaCheckout.addEventListener("paymentSuccess", (data) => {
        // Your specifications here
      });

      malgaCheckout.addEventListener("paymentFailed", (error) => {
        // Your specifications here
      });
    </script>
  </body>
</html>
```
