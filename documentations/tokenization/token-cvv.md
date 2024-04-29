---
id: token-cvv
title: Token CVV
slug: /tokenization/token-cvv
category: 662fa995369d3500194ad422
---

O CVV (Valor de verificação do cartão), é o código de segurança compostos por três ou quatro dígitos,
utilizado nas compras online, o código substitui a senha usada em estabelecimentos físicos. Com a finalidade
de garantir que no ato da compra o cliente tenha o seu cartão em mãos, evitando fraudes.

A tokenização é um método de substituição dos dados sensíveis do portador (CVV), por um token, para que
a experiência de pagamento digital seja mais segura.

## Cenário de utilização

O recurso de tokenização do CVV é normalmente utilizado em transações usando dados de cartão de crédito já armazenados no cofre, no qual
os clientes desejam passar o CVV de forma segura atráves de um token, ao invés do CVV em texto plano. Para isso, utiliza-se a
solução de `Token CVV`, garantido que a aplicação dos clientes não consiga enxergar o conteúdo desse token.

:::caution
O token é invalidado após o seu uso e/ou pelo tempo de expiração, sendo impossível utiliza-lo futuramente.
Para isso é necessário gerar um novo token e informar esse na transação.
:::

## Exemplo de tokenização do CVV

```bash
curl --location 'https://api.malga.io/v1/tokens' \
     --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
     --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
     --header 'Content-Type: application/json' \
    --data '{
        "cvvUpdate": "410"
    }'
< HTTP/2 201
< content-type: application/json; charset=utf-8
{
    "tokenId": "2d930a61-67d5-45bb-8b90-24a993b292e3"
}
```

## Exemplo de transação utilizando a tokenização CVV

```bash
curl --location 'https://api.malga.io/v1/charges' \
     --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
     --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
     --header 'Content-Type: application/json' \
    --data '{
        "merchantId": "39a10bed-6ee1-46ea-94ce-565c1f52e2e6",
        "amount": 150,
        "currency": "BRL",
        "statementDescriptor": "Pedido #231 loja joão",
        "capture": true,
        "orderId": "7171717171",
        "description": "descrição"
        "paymentMethod": {
            "paymentType": "credit",
            "installments": 1
        },
        "paymentSource": {
            "sourceType": "card",
            "cardId": "8d930a61-67d5-45bb-8b90-24a993b292e3",
            "tokenCvv": "2d930a61-67d5-45bb-8b90-24a993b292e3"
        }
    }'
< HTTP/2 201
< content-type: application/json; charset=utf-8
{
   "id": "55d8b213-f3c9-4ef9-9476-4d874edb8ac1",
    "clientId": "523afbe7-36dc-4654-9dba-e7167d0e5e2d",
    "merchantId": "ba5cefe2-bf93-4fe3-bd4f-d87af60b21f8",
    "description": "d9965173-5543-41d1-9709-5d8347a36e53",
    "orderId": "ca0011e3-ff94-4fc3-a254-f72c8b86fe6e",
    "createdAt": "2023-08-04T14:02:05.082Z",
    "amount": 100,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": "Token CVV",
    "capture": true,
    "isDispute": false,
    "status": "failed",
    "paymentMethod": {
        "installments": 1,
        "paymentType": "credit"
    },
    "paymentSource": {
        "sourceType": "card",
        "cardId": "e98ea785-aa5e-4ce4-8cc7-812f1b55ba3d"
    },
    "transactionRequests": [
        {
            "id": "0559f1f2-ef81-46b7-8f7c-516428fd8a72",
            "createdAt": "2023-08-04T14:02:05.166Z",
            "updatedAt": "2023-08-04T14:02:05.271Z",
            "idempotencyKey": "d24296b2-dd60-49b5-bc28-0034fb04d293",
            "providerId": "cd5f2a7c-5fc2-48d3-87bd-ae6573184fb7",
            "providerType": "SANDBOX",
            "transactionId": "2767078",
            "amount": 100,
            "authorizationCode": "1547251",
            "authorizationNsu": "6783923",
            "requestStatus": "failed",
            "requestType": "authorization",
            "responseTs": "41ms",
            "providerError": {
                "retryable": false,
                "declinedCode": "invalid_cvv",
                "message": null,
                "networkDeniedMessage": "Invalid CVV",
                "networkDeniedReason": "sandbox invalid cvv"
            }
        }
    ]
}
```

:::tip SAIBA MAIS
Para saber mais sobre a Tokenização confira em [nossa documentação](/docs/tokenization).
:::
