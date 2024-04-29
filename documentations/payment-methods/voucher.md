---
id: voucher
slug: /payment-methods/voucher
title: Voucher
sidebar_label: Voucher
category: 662fa995369d3500194ad422
---

# Voucher

São cartões de benefícios que as empresas dão aos seus colaboradores, utilizados para compras de produtos
específicos como: **vale refeição** e **vale alimentação**.

Antes o voucher só era utilizado por meio de maquininhas de cartões em compras presenciais, mas se tornou
um método de pagamento muito requisitado no varejo online.

Sua aceitação é uma forma de atender a essa demanda e oferecer mais comodidade e liberdade para os consumidores,
fazendo com que os negócios tenham diversificação em suas opções de pagamento, ampliando assim seus potenciais
clientes e aumentando as suas conversões.

## Fluxo do Voucher

- Para uma transação com Cartão utilizando o metódo _Voucher_, é necessário informar o método de pagamento que seria voucher, source type card e os dados de identificação do cartão, em sua criação;
- A cobrança será realizada no cartão através da instituição de pagamentos escolhida no caso o VR;
- Após a confirmação do pagamento, o provedor de pagamento irá realizar a transferência de fundos para a conta do recebedor;
- Posteriormente o cliente recebedor poderá ser notificado de que o pagamento foi realizado com sucesso e/ou estornado caso solicitado.

## Status do pagamento

| objeto        | evento       | descrição                                                                                                                  |
| ------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------- |
| _transaction_ | _authorized_ | Status enviado quando é autorizado o pagamento                                                                             |
| _transaction_ | _failed_     | Status enviado quando a cobrança é negada pela instituição financeira antes de ter sido autorizada, sem estorno financeiro |
| _transaction_ | _voided_     | Status enviado quando a cobrança é cancelada após ter sido autorizada, produzindo um estorno financeiro                    |

## Provedores e voucher suportados

Veja os provedores que suportam cobrança via voucher na nossa [tabela de provedores e meios de pagamento suportados](/api#section/Provedores-e-meios-de-pagamentos-suportados).

## Estorno Total

É possível realizar um estorno no valor total da cobrança. O histórico de requisições de
estorno pode ser encontrado na listagem de `transactionRequests` do objeto `charge`.

:::warning
Não é possível realizar o _estorno parcial_ no provedor VR.
:::

## Exemplo de transação do voucher realizada com sucesso

:::caution
O voucher não utiliza o _capture_, portanto não precisa ser informado.
:::

```bash
  curl --location 'https://api.malga.io/v1/charges' \
       --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
       --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
       --header 'Content-Type: application/json' \
       --data '{
            "merchantId": "<merchantId>",
            "amount": 100,
            "currency": "BRL",
            "statementDescriptor": "Voucher",
            "orderId": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
            "description": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
            "paymentMethod": {
                "customer": {
                    "name": "Jose das Flores",
                    "identity": "11111111111",
                    "billingAddress": {
                        "zipCode": "01714140",
                        "city": "São Paulo",
                        "state": "SP",
                        "country": "BR",
                        "complement": "complement"
                    }
                },
                "paymentType": "voucher",
                "items": [
                    {
                        "id": "123",
                        "title": "ItemTeste1",
                        "quantity": 1,
                        "unitPrice": 100
                    }
                ]
            },
            "paymentSource": {
                "sourceType": "card",
                "card": {
                    "cardHolderName": "Jose das Flores",
                    "cardNumber": "4000000000000010",
                    "cardCvv": "351",
                    "cardExpirationDate": "12/2024"
                }
            }
       }'

  < HTTP/2 201
  < content-type: application/json; charset=utf-8
  {
    "id": "97443bea-fb76-43c2-afb1-2cb45abbe673",
    "clientId": "<CLIENT_ID>",
    "merchantId": "<MERCHANT_ID>",
    "orderId": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
    "createdAt": "2023-07-17T19:46:08.597Z",
    "amount": 100,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": "Voucher",
    "capture": true,
    "isDispute": false,
    "status": "authorized",
    "paymentMethod": {
        "paymentType": "voucher"
    },
    "paymentSource": {
        "sourceType": "card"
    },
    "transactionRequests": [
        {
            "id": "e18cf62e-aa43-4db5-abfe-0f63e2749fef",
            "createdAt": "2023-07-17T19:46:08.627Z",
            "updatedAt": "2023-07-17T19:46:10.861Z",
            "idempotencyKey": "5b011a93-2eaf-4b9c-9f22-f3676c7cba74",
            "providerId": "56a83eaf-f9b2-4d2e-97f3-53933331b0e5",
            "providerType": "SANDBOX",
            "transactionId": "f4ae4d32-350d-4135-9b50-b259e937ca03",
            "amount": 100,
            "authorizationCode": "011056",
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": "2212ms"
        }
    ]
}
```

## Exemplo de transação do voucher realizada com falha

```bash
curl --location 'https://api.malga.io/v1/charges' \
     --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
     --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
     --header 'Content-Type: application/json' \
     --data '{
        "merchantId": "<merchantId>",
        "amount": 100,
        "currency": "BRL",
        "statementDescriptor": "Voucher",
        "capture": true,
        "orderId": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
        "description": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
        "paymentMethod": {
            "paymentType": "voucher"
        },
        "paymentSource": {
            "sourceType": "card",
            "card": {
                "cardHolderName": "ARACHANE CORTAS",
                "cardNumber": "6274160007023102",
                "cardCvv": "527",
                "cardExpirationDate": "12/2024"
            }
        }
     }'
 < HTTP/2 201
 < content-type: application/json; charset=utf-8
 {
    "id": "ef19739f-953b-488e-b653-623b80a88392",
    "clientId": "<CLIENT_ID>",
    "merchantId": "<MERCHANT_ID>",
    "description": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
    "orderId": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
    "createdAt": "2023-07-17T22:09:53.502Z",
    "amount": 100,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": "Voucher",
    "capture": true,
    "isDispute": false,
    "status": "failed",
    "paymentMethod": {
        "paymentType": "voucher"
    },
    "paymentSource": {
        "sourceType": "card"
    },
    "transactionRequests": [
        {
            "id": "e6cad844-ae11-44ab-a83d-e1abfe10ac5c",
            "createdAt": "2023-07-17T22:09:53.542Z",
            "updatedAt": "2023-07-17T22:09:55.048Z",
            "idempotencyKey": "8ef45126-2faa-487c-9367-e8722f5d0d6c",
            "providerId": "56a83eaf-f9b2-4d2e-97f3-53933331b0e5",
            "providerType": "SANDBOX",
            "transactionId": "d00eb5d6-e99f-4c56-a42d-a3d557c27a52",
            "amount": 100,
            "authorizationCode": "011056",
            "authorizationNsu": null,
            "requestStatus": "failed",
            "requestType": "authorization",
            "responseTs": "1413ms"
        }
    ]
}
```

## Exemplo de transação do voucher com estorno parcial realizado com sucesso

```bash
curl --location 'https://api.malga.io/v1/charges/271a0ef0-5543-4ac9-83e9-f31fc5c174b5/void' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \ \
    --header 'Content-Type: application/json' \
    --data '{
    "amount": 50
    }'

< HTTP/2 201
< content-type: application/json; charset=utf-8
{
    "id": "271a0ef0-5543-4ac9-83e9-f31fc5c174b5",
    "clientId": "<CLIENT_ID>",
    "merchantId": "<MERCHANT_ID>",
    "description": null,
    "orderId": "orderId",
    "createdAt": "2023-08-16T18:54:01.706Z",
    "amount": 50,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": "Descrição do Pedido",
    "capture": false,
    "isDispute": false,
    "status": "authorized",
    "paymentMethod": {
        "paymentType": "voucher"
    },
    "paymentSource": {
        "sourceType": "card",
        "cardId": "77ed7c5b-a1d7-4ac2-ab3d-0a7e4681f152"
    },
        "transactionRequests": [
            {
            "id": "52984c70-514c-4204-9ee0-3af715eab63b",
            "createdAt": "2023-08-16T18:54:51.434Z",
            "updatedAt": "2023-08-16T18:54:51.434Z",
            "idempotencyKey": null,
            "providerId": "a14c0109-de62-4f09-a613-70567c5697fa",
            "providerType": "PAGARME_V5",
            "transactionId": "or_6ylmd9ZH6Ho50aRo",
            "amount": 50,
            "authorizationCode": "621",
            "authorizationNsu": "27073",
            "requestStatus": "success",
            "requestType": "void",
            "responseTs": "439ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "621",
                "networkResponseCode": "00"
                }
            },
            {
            "id": "266ed9ea-beef-45ef-a145-d2f050777988",
            "createdAt": "2023-08-16T18:54:01.751Z",
            "updatedAt": "2023-08-16T18:54:02.294Z",
            "idempotencyKey": "76162962-1bff-4a56-9f21-c83ad70247a8",
            "providerId": "a14c0109-de62-4f09-a613-70567c5697fa",
            "providerType": "PAGARME_V5",
            "transactionId": "or_6ylmd9ZH6Ho50aRo",
            "amount": 100,
            "authorizationCode": "621",
            "authorizationNsu": "58785",
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": "515ms"
            }
        ]
    }
```

## Exemplo de transação do voucher com estorno realizado com sucesso

```bash
curl --location 'https://api.malga.io/v1/charges/97443bea-fb76-43c2-afb1-2cb45abbe673/void' \
     --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    --header 'Content-Type: application/json' \
    --data '{
      "amount": 100
    }'
< HTTP/2 201
< content-type: application/json; charset=utf-8
{
   {
    "id": "a1321f1a-be77-4639-85c8-d0257b2d93af",
    "clientId": "<CLIENT_ID>",
    "merchantId": "<MERCHANT_ID>",
    "description": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
    "orderId": "15ad136a-520b-40d0-9c0e-05ff008b0fa7",
    "createdAt": "2023-07-18T12:52:48.393Z",
    "amount": 0,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": "Voucher",
    "capture": true,
    "isDispute": false,
    "status": "voided",
    "paymentMethod": {
        "paymentType": "voucher"
    },
    "paymentSource": {
        "sourceType": "card",
        "cardId": "692598f6-410b-4f12-b2df-5b3e653c7b9f"
    },
    "transactionRequests": [
        {
            "id": "c6d3cd78-d562-42d8-83b7-4bc6ea362dac",
            "createdAt": "2023-07-18T12:53:16.692Z",
            "updatedAt": "2023-07-18T12:53:16.692Z",
            "idempotencyKey": null,
            "providerId": "bce85756-9016-4483-81b8-b1a2a37c3ac4",
            "providerType": "VR",
            "transactionId": "128898e2ab7d41df99960859",
            "amount": 100,
            "authorizationCode": "011043",
            "authorizationNsu": "128898e2ab7d41df99960859",
            "requestStatus": "success",
            "requestType": "void",
            "responseTs": "2157ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "011043",
                "networkResponseCode": "00"
            }
        },
        {
            "id": "9e01d010-dbb6-497a-bf3c-7c64ff823605",
            "createdAt": "2023-07-18T12:52:48.579Z",
            "updatedAt": "2023-07-18T12:52:52.857Z",
            "idempotencyKey": "205c60c4-c4dc-40c7-8ad5-4339aa61f31a",
            "providerId": "bce85756-9016-4483-81b8-b1a2a37c3ac4",
            "providerType": "VR",
            "transactionId": "128898e2ab7d41df99960859",
            "amount": 100,
            "authorizationCode": "011056",
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": "4236ms"
        }
    ]
}
```

## Exemplo de transação do voucher com estorno realizado com falha

```bash
curl --location 'https://api.malga.io/v1/charges/4b26b3cf-a9a9-45d7-9629-5a97c1f02b88/void' \
     --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
     --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
     --header 'Content-Type: application/json' \
    --data '{
        "amount": 500
    }'

< HTTP/2 422
< content-type: application/json; charset=utf-8
{
    "error": {
        "type": "unprocessable_entity",
        "code": 422,
        "message": "Transaction failed"
    }
}
```
