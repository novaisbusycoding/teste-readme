---
id: split
slug: /split/split
title: Cobrança com Split
category: 662fa995369d3500194ad422
---

# Cobranças com Split

O split foi criado com o objetivo de fazer o repasse automático dos valores de uma transação entre diversos parceiros recebedores com as regras estabelecidas pelo nosso cliente.

## Criando uma cobrança com Split

Uma configuração de Split contém uma ou mais regras que definem taxas de comissão e condições quando a taxa se aplica.

Para criar uma transação com split, basta adicionar o campo **splitRules** que representará as regras daquela cobrança, dentro do corpo da requisição da [criação de uma transação](/docs/payment-methods/credit-card).

### Regras de split

Cada regra (**splitRules**) em uma configuração dividida deve ter:

- **Identificador do destinatário (sellerId)**: O valor de ID dentro de **splitRules** deve ser correspondente ao ID gerado na [criação de seller](/docs/split/seller).

- **Porcentagem (percentage)**: porcentagem do valor da transação original que será enviada ao destinatário

- **Valor (amount)**: valor que será enviado ao destinatário

- **Sinalizador de taxa de processamento (processingFee)**: o destinatário será cobrado pelas taxas de processamento, que serão descontadas de seu saldo, definido como falso para desconto do marketplace

- **Responsabilidade por reembolso (liable)** : o destinatário será responsável pelo reembolso (charge_back), que será descontado de seu saldo, definido como falso para desconto do marketplace.
- **Tarifas (fares)** : o destinatário será responsável pelas tarifas, que serão descontadas de seu saldo, definido como falso para desconto do marketplace.
  - **mdr(Merchant Discount Rate)**: percentual a ser descontado do valor de uma transação, definido por produto (crédito/débito/boleto), bandeira e faixa de parcelamento.
  - **Tarifa fixa(fee)**: também chamada de fee transacional. Valor em centavos a ser cobrado por transação capturada. É descontado no momento da “montagem” da agenda financeira

:::caution

- Não é possível realizar uma transação com split misto, isto é, utilizando as duas regras de divisão: porcentagem e valor.
- A informação de tarifas no momento é apenas para split que transaciona utilizando provider Braspag
  :::

### Exemplo de cobrança com Split aprovado usando o campo percentage

```bash
curl -X POST 'https://api.malga.io/v1/charges' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
      "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
      "amount": 150,
      "currency": "BRL",
      "statementDescriptor": "Pedido #231 loja joão",
      "capture": false,
      "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
      },
      "paymentSource": {
        "sourceType": "card",
        "card": {
          "cardHolderName": "JOAO DA SILVA",
          "cardNumber": "4929564637987814",
          "cardCvv": "320",
          "cardExpirationDate": "06/2028"
        }
      },
	  "splitRules": [{
        "sellerId": "5323ece6-816d-11ed-a1eb-0242ac120002",
        "percentage": 10,
        "processingFee": true,
        "liable": true
        },
        {
         "sellerId": "616605b2-816f-11ed-a1eb-0242ac120002",
         "percentage": 10,
         "processingFee": false,
         "liable": false,
      }]
  }'
< HTTP/2 201
{
    "id": "3768a9bf-68fd-43e8-b7f6-e7d12ee22139",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "46b433bf-79aa-4cb1-9eaa-cdaea42cb955",
    "description": null,
    "orderId": null,
    "createdAt": "2023-02-04T19:02:36.769Z",
    "amount": 150,
    "originalAmount": 150,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": false,
    "isDispute": false,
    "status": "authorized",
    "paymentMethod": {
        "installments": 1,
        "paymentType": "credit"
    },
   "paymentSource": {
        "sourceType": "card",
        "cardId": "5afcdeb3-9e56-4dd9-bfee-e6b70c75b200"
    },
    "splitRules": [
        {
            "id": "214f1995-35f0-411f-9694-0cd56ef31db9",
            "updatedAt": "2023-10-04T19:02:36.814Z",
            "createdAt": "2023-10-04T19:02:36.814Z",
            "sellerId": "5323ece6-816d-11ed-a1eb-0242ac120002",
            "percentage": 10,
            "amount": null,
            "processingFee": true,
            "liable": true,
            "fareMdr": null,
            "fareFee": null
        },
        {
            "id": "e8444bb9-ba89-4cd7-808c-9c9a8dd8af4c",
            "updatedAt": "2023-10-04T19:02:36.814Z",
            "createdAt": "2023-10-04T19:02:36.814Z",
            "sellerId": "616605b2-816f-11ed-a1eb-0242ac120002",
            "percentage": 10,
            "amount": null,
            "processingFee": false,
            "liable": false,
            "fareMdr": null,
            "fareFee": null
        }
    ],
    "transactionRequests": [
        {
            "id": "7adfc4c6-567a-4ce0-a9a3-a08a21beff7f",
            "createdAt": "2023-10-04T19:02:37.106Z",
            "updatedAt": "2023-10-04T19:02:37.205Z",
            "idempotencyKey": "40efd5a0-1010-5c8d-9f75-0cd455553388",
            "providerId": "c6686322-f27b-90b2-a61b-106e7a564087",
            "providerType": "SANDBOX",
            "transactionId": "eb80f69e-de28-5060-8daf-da0fecc3b18f",
            "amount": 150,
            "authorizationCode": "9301661",
            "authorizationNsu": "4244270",
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": "62ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "7909027",
                "networkResponseCode": "8168918"
            }
        }
    ],
    "appInfo": null
   }
}
```

### Exemplo de cobrança com Split aprovado usando o campo amount

```bash
curl -X POST 'https://api.malga.io/v1/charges' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
      "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
      "amount": 150,
      "currency": "BRL",
      "statementDescriptor": "Pedido #231 loja joão",
      "capture": false,
      "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
      },
      "paymentSource": {
        "sourceType": "card",
        "card": {
          "cardHolderName": "JOAO DA SILVA",
          "cardNumber": "4929564637987814",
          "cardCvv": "320",
          "cardExpirationDate": "06/2028"
        }
      },
	  "splitRules": [{
        "sellerId": "5323ece6-816d-11ed-a1eb-0242ac120002",
        "amount": 50,
        "processingFee": true,
        "liable": true
        },
        {
         "sellerId": "616605b2-816f-11ed-a1eb-0242ac120002",
         "amount": 50,
         "processingFee": false,
         "liable": false,
      }]
  }'
  < HTTP/2 201 {
    "id": "3768a9bf-68fd-43e8-b7f6-e7d12ee22139",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "46b433bf-79aa-4cb1-9eaa-cdaea42cb955",
    "description": null,
    "orderId": null,
    "createdAt": "2023-02-04T19:02:36.769Z",
    "amount": 150,
    "originalAmount": 150,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": false,
    "isDispute": false,
    "status": "authorized",
    "paymentMethod": {
        "installments": 1,
        "paymentType": "credit"
    },
   "paymentSource": {
        "sourceType": "card",
        "cardId": "5afcdeb3-9e56-4dd9-bfee-e6b70c75b200"
    },
    "splitRules": [
        {
            "id": "214f1995-35f0-411f-9694-0cd56ef31db9",
            "updatedAt": "2023-10-04T19:02:36.814Z",
            "createdAt": "2023-10-04T19:02:36.814Z",
            "sellerId": "5323ece6-816d-11ed-a1eb-0242ac120002",
            "percentage": null,
            "amount": 50,
            "processingFee": true,
            "liable": true,
            "fareMdr": null,
            "fareFee": null
        },
        {
            "id": "e8444bb9-ba89-4cd7-808c-9c9a8dd8af4c",
            "updatedAt": "2023-10-04T19:02:36.814Z",
            "createdAt": "2023-10-04T19:02:36.814Z",
            "sellerId": "616605b2-816f-11ed-a1eb-0242ac120002",
            "percentage": null,
            "amount": 50,
            "processingFee": false,
            "liable": false,
            "fareMdr": null,
            "fareFee": null
        }
    ],
    "transactionRequests": [
        {
            "id": "7adfc4c6-567a-4ce0-a9a3-a08a21beff7f",
            "createdAt": "2023-10-04T19:02:37.106Z",
            "updatedAt": "2023-10-04T19:02:37.205Z",
            "idempotencyKey": "40efd5a0-1010-5c8d-9f75-0cd455553388",
            "providerId": "c6686322-f27b-90b2-a61b-106e7a564087",
            "providerType": "SANDBOX",
            "transactionId": "eb80f69e-de28-5060-8daf-da0fecc3b18f",
            "amount": 150,
            "authorizationCode": "9301661",
            "authorizationNsu": "4244270",
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": "62ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "7909027",
                "networkResponseCode": "8168918"
            }
        }
    ],
    "appInfo": null
   }
}
```

### Exemplo de cobrança com Split aprovado usando o campo de tarifa

```bash
curl -X POST 'https://api.malga.io/v1/charges' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
      "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
      "amount": 150,
      "currency": "BRL",
      "statementDescriptor": "Pedido #231 loja joão",
      "capture": false,
      "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
      },
      "paymentSource": {
        "sourceType": "card",
        "card": {
          "cardHolderName": "JOAO DA SILVA",
          "cardNumber": "4929564637987814",
          "cardCvv": "320",
          "cardExpirationDate": "06/2028"
        }
      },
	  "splitRules": [{
        "sellerId": "5323ece6-816d-11ed-a1eb-0242ac120002",
        "amount": 50,
        "processingFee": true,
        "liable": true,
        "fares": {
            "mdr": 2,
            "fee": 100
          }
        },
        {
         "sellerId": "616605b2-816f-11ed-a1eb-0242ac120002",
         "amount": 50,
         "processingFee": false,
         "liable": false,
         "fares": {
            "mdr": 2,
            "fee": 100
          }
      }]
  }'
  < HTTP/2 201 {
    "id": "3768a9bf-68fd-43e8-b7f6-e7d12ee22139",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "46b433bf-79aa-4cb1-9eaa-cdaea42cb955",
    "description": null,
    "orderId": null,
    "createdAt": "2023-02-04T19:02:36.769Z",
    "amount": 150,
    "originalAmount": 150,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": false,
    "isDispute": false,
    "status": "authorized",
    "paymentMethod": {
        "installments": 1,
        "paymentType": "credit"
    },
   "paymentSource": {
        "sourceType": "card",
        "cardId": "5afcdeb3-9e56-4dd9-bfee-e6b70c75b200"
    },
    "splitRules": [
        {
            "id": "214f1995-35f0-411f-9694-0cd56ef31db9",
            "updatedAt": "2023-10-04T19:02:36.814Z",
            "createdAt": "2023-10-04T19:02:36.814Z",
            "sellerId": "5323ece6-816d-11ed-a1eb-0242ac120002",
            "percentage": null,
            "amount": 50,
            "processingFee": true,
            "liable": true,
            "fareMdr": "2.00",
            "fareFee": 100
        },
        {
            "id": "e8444bb9-ba89-4cd7-808c-9c9a8dd8af4c",
            "updatedAt": "2023-10-04T19:02:36.814Z",
            "createdAt": "2023-10-04T19:02:36.814Z",
            "sellerId": "616605b2-816f-11ed-a1eb-0242ac120002",
            "percentage": null,
            "amount": 50,
            "processingFee": false,
            "liable": false,
            "fareMdr": "2.00",
            "fareFee": 100
        }
    ],
    "transactionRequests": [
        {
            "id": "7adfc4c6-567a-4ce0-a9a3-a08a21beff7f",
            "createdAt": "2023-10-04T19:02:37.106Z",
            "updatedAt": "2023-10-04T19:02:37.205Z",
            "idempotencyKey": "40efd5a0-1010-5c8d-9f75-0cd455553388",
            "providerId": "c6686322-f27b-90b2-a61b-106e7a564087",
            "providerType": "SANDBOX",
            "transactionId": "eb80f69e-de28-5060-8daf-da0fecc3b18f",
            "amount": 150,
            "authorizationCode": "9301661",
            "authorizationNsu": "4244270",
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": "62ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "7909027",
                "networkResponseCode": "8168918"
            }
        }
    ],
    "appInfo": null
   }
}
```
