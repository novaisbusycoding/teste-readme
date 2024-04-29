---
id: drip
slug: /payment-methods/drip
title: Drip
sidebar_label: Drip
category: 662fa995369d3500194ad422
---

# Drip

Drip é uma solução de pagamento online que permite o pagamento de cobranças parceladas utilizando Pix e com cashback para os seus clientes. Para pagar com a Drip, o consumidor precisa abrir uma conta no sistema da Drip durante o processo de compra.

A Malga realiza o papel de intermediador de pagamentos, portanto não realiza a liquidação financeira, sendo necessária conta ativa da Drip para utilizar este método de pagamento.

## Os participantes de uma transação Drip

- O recebedor, cliente da Malga que deve possuir uma conta ativa da Drip.
- O pagador, um comprador que é um cliente pessoa física.

## Fluxo de Pagamento Drip utilizando a API de cobranças com integração direta

- Para criar uma transação com Drip basta informar o meio de pagamento `DRIP` na criação de uma cobrança e adicionar as informações dos itens presentes no checkout;
- Uma cobrança é registrada diretamente no sistema da Drip e é retornada uma URL de redirecionamento para o fluxo de cadastro e pagamento dentro da plataforma da Drip;
- O pagador deve realizar a confirmação da cobrança diretamente na plataforma da Drip;
- Após confirmação do pagamento, a Drip enviará uma notificação para a Malga informando que o pagamento foi realizado;
- Posteriormente o Cliente recebedor será notificado de que o pagamento foi efetuado e a cobrança finalizada com sucesso.

Este processo está descrito nos diagramas a seguir:

![Fluxo Drip](/img/drip-flux-ptbr.png)

![Fluxo Drip com Checkout](/img/drip-flux-checkout-ptbr.png)

Você pode consultar mais informações sobre a integração com checkout [neste link](/docs/intro-sdk).

:::info
As cobranças Drip são assíncronas por natureza, ou seja, ao criar uma nova cobrança você terá a confirmação apenas da criação da cobrança e não de sua conclusão. A autorização ou reprovação de uma cobrança é notificada através dos [webhooks](/docs/webhooks).
:::

## Notificação de alteração de status

| objeto        | evento       | descrição                                                                                               |
| ------------- | ------------ | ------------------------------------------------------------------------------------------------------- |
| _transaction_ | _pending_    | Evento enviado quando a cobrança é registrada e o link de pagamento na Drip está disponível             |
| _transaction_ | _authorized_ | Evento enviado quando é reconhecida a confirmação do pagamento da cobrança na Drip                      |
| _transaction_ | _failed_     | Evento enviado quando a cobrança não pôde ser registrada por motivo de falha durante a criação          |
| _transaction_ | _voided_     | Evento enviado quando a cobrança é cancelada após ter sido autorizada, produzindo um estorno financeiro |

## Testando recebimento de notificação de cobrança Drip paga

Para testar sua integração com os webhooks da Malga você pode desenvolver direto seu sistema ou utilizar algum serviço como request.bin ou pipedream.com para validar inicialmente os eventos enviados. Basta gerar um novo endpoint nestes serviços e cadastrar um webhook na Malga com o endpoint gerado que todos os eventos enviados ficarão registrados nestes serviços para consulta e debug.

Permitimos no ambiente de sandbox `sandbox-api.malga.io` a atualização manual de transações criadas para os status de `authorized` e `voided`, dessa forma você consegue criar uma transação e simular o evento desejado.

**Requisição para atualizar manualmente uma cobrança Drip como paga em sandbox**

```bash
curl --location --request POST 'https://sandbox-api.malga.io/v1/charges/<CHARGE_ID>' \
          --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
          --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
          --header 'Content-Type: application/json' \
          --data-raw '{
              "status": "authorized"
          }'
```

## Exemplo de cobrança

Realize uma cobrança Drip a partir dos dados do comprador usando o [Serviço de Charges](/api#operation/charge).

:::caution
Caso deseje informar a quantidade máxima de parcelas para determinada transação, basta informar o atributo
"maxInstallments" no objeto de paymentMethod.
:::

```bash
curl --location --request POST 'https://api.malga.io/v1/charges' \
--header 'X-Client-Id: <YOUR_CLIENT_ID>' \
--header 'X-Api-Key: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "d749c775-5c61-448e-9ed4-36b67d085301",
    "amount": 100,
    "currency": "BRL",
    "orderId": "98848683521",
    "paymentMethod": {
        "paymentType": "drip",
        "items": [
            {
                "id": "123",
                "quantity": 1,
                "title": "item title",
                "unitPrice": 100
            }
        ],
        "browser": {
            "ipAddress": "127.0.0.1",
            "browserFingerprint": "074c1ee676ed4998ab66491013c565e2"
        },
        "successRedirectUrl": "http://your-service.com/success",
        "cancelRedirectUrl": "http://your-service.com/cancel"
    },
    "paymentSource": {
        "sourceType": "customer",
        "customer": {
            "name": "Nome do Comprador",
            "phoneNumber": "xx999999999",
            "email": "email@docomprador.com",
            "document": {
                "number": "11111111111",
                "type": "cpf"
            },
            "address": {
                "street": "Rua do Comprador",
                "streetNumber": "1",
                "zipCode": "00000-140",
                "country": "BR",
                "state": "RJ",
                "district": "Bairro",
                "city": "Cidade",
                "complement": "complemento"
            }
        }
    }
}'

< HTTP/2 201
{
    "id": "c1248092-1769-4b93-9b1d-fabcec356c6c",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "d749c775-5c61-448e-9ed4-36b67d085301",
    "description": null,
    "orderId": "98848683521",
    "createdAt": "2023-07-18T23:03:53.035Z",
    "amount": 100,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": null,
    "capture": false,
    "isDispute": false,
    "status": "pending",
    "paymentMethod": {
        "paymentType": "drip",
        "paymentUrl": "https://sandbox-portal.dripapp.com.br/checkouts/0f6aa94e-8593-4662-bddd-7bfce2d4c935",
        "items": [
            {
                "id": "123",
                "quantity": 1,
                "title": "item title",
                "unitPrice": 100
            }
        ],
        "browser": {
            "ipAddress": "127.0.0.1",
            "browserFingerprint": "074c1ee676ed4998ab66491013c565e2"
        },
        "successRedirectUrl": "http://your-service.com/success",
        "cancelRedirectUrl": "http://your-service.com/cancel"
    },
    "paymentSource": {
        "sourceType": "customer",
        "customerId": "4362a975-9cc7-418f-918b-8ee9585f604e"
    },
    "transactionRequests": [
        {
            "id": "449043d9-dc7c-4456-bf58-d8d9a53f15bf",
            "createdAt": "2023-07-18T23:03:53.048Z",
            "updatedAt": "2023-07-18T23:03:55.501Z",
            "idempotencyKey": "9baef27a-9897-4541-9d84-d61b65ffe6fe",
            "providerId": "769bce00-678e-4d64-9617-9ce82f4dfcbf",
            "providerType": "DRIP",
            "transactionId": "9baef27a-9897-4541-9d84-d61b65ffe6fe",
            "amount": 100,
            "authorizationCode": "f03a3928-460e-42b3-8ece-76cdfdcb5924",
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "pending",
            "responseTs": "2436ms",
            "drip": {
                "paymentUrl": "https://sandbox-portal.dripapp.com.br/checkouts/0f6aa94e-8593-4662-bddd-7bfce2d4c935",
                "items": [
                    {
                        "id": "123",
                        "quantity": 1,
                        "title": "item title",
                        "unitPrice": 100
                    }
                ],
                "browser": {
                    "ipAddress": "127.0.0.1",
                    "browserFingerprint": "074c1ee676ed4998ab66491013c565e2"
                },
                "successRedirectUrl": "http://your-service.com/success",
                "cancelRedirectUrl": "http://your-service.com/cancel"
            }
        }
    ]
}
```

:::caution
Para criação de transações Drip é obrigatório o envio de paymentSource com customer ou identificador de um customer que contenha endereço completo e válido.
:::

## Estorno Drip

Requisições de estorno para cobranças Drip são tratadas de formas síncrona, ou seja, ocorrem no momento da requisição feita para a API da Malga.
Em caso de estornos parciais do valor da compra, a transação permanecerá com status `authorized` caso ainda haja valor na cobrança a ser estornado. Quando o valor restante em uma cobrança é 0 seu status é atualizado para `voided`.

## Exemplo de solicitação de estorno Drip

Quando uma requisição de estorno é feita, a API retorna uma transação com o status `voided` e uma `transactionRequest` do tipo `void` com o status `success`. Quando a requisição de estorno falha por algum motivo, a transação continua com o status `authorized` contendo uma `transactionRequest` do tipo `void` com o status `failed`.

```bash
curl --location --request POST 'https://api.malga.io/v1/charges/<CHARGE_ID>/void' \
--header 'X-Client-Id: <YOUR_CLIENT_ID>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "amount": 1200
}'

< HTTP/2 201
{
    "id": "7e4930f0-b23c-4378-a3ab-4db684434ded",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "d749c775-5c61-448e-9ed4-36b67d085301",
    "description": null,
    "orderId": "98848683521",
    "createdAt": "2023-07-26T12:26:03.938Z",
    "amount": 1200,
    "originalAmount": 1200,
    "currency": "BRL",
    "statementDescriptor": null,
    "capture": false,
    "isDispute": false,
    "status": "voided",
    "paymentMethod": {
        "paymentType": "drip",
        "paymentUrl": "https://sandbox-portal.dripapp.com.br/checkouts/61cad747-7bc3-41b0-a3c5-6a127eacd790",
        "browser": {
            "ipAddress": "127.0.0.1",
            "browserFingerprint": "ab123cd456ef789"
        },
        "items": [
            {
                "id": "12345",
                "title": "Product 1",
                "quantity": 1,
                "unitPrice": 300
            }
        ],
        "successRedirectUrl": "https://service-example.com/success",
        "cancelRedirectUrl": "https://service-example.com/cancel"
    },
    "paymentSource": {
        "sourceType": "customer",
        "customerId": "17491cfe-9952-40a9-a56f-c087be2ea693"
    },
    "transactionRequests": [
        {
            "id": "3a93edd9-473c-48dd-9b04-62c04d7a3d62",
            "createdAt": "2023-07-26T12:40:32.489Z",
            "updatedAt": "2023-07-26T12:40:32.489Z",
            "idempotencyKey": "d9160c55-000d-41b4-a6cd-0e2a4dd6439f",
            "providerId": "07fc7b49-6f27-42c4-b610-ba69af41403e",
            "providerType": "DRIP",
            "transactionId": "d9160c55-000d-41b4-a6cd-0e2a4dd6439f",
            "amount": 1200,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "void",
            "responseTs": "460ms"
        },
        {
            "id": "06e955f6-b648-4fdd-b9a8-84c1c75d0f26",
            "createdAt": "2023-07-26T12:26:53.557Z",
            "updatedAt": "2023-07-26T12:26:53.557Z",
            "idempotencyKey": "d9160c55-000d-41b4-a6cd-0e2a4dd6439f",
            "providerId": "07fc7b49-6f27-42c4-b610-ba69af41403e",
            "providerType": "DRIP",
            "transactionId": "d9160c55-000d-41b4-a6cd-0e2a4dd6439f",
            "amount": 1200,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": null
        },
        {
            "id": "72abedb9-ed78-49d3-98c9-2a83ab59f827",
            "createdAt": "2023-07-26T12:26:03.998Z",
            "updatedAt": "2023-07-26T12:26:04.717Z",
            "idempotencyKey": "d9160c55-000d-41b4-a6cd-0e2a4dd6439f",
            "providerId": "07fc7b49-6f27-42c4-b610-ba69af41403e",
            "providerType": "DRIP",
            "transactionId": "d9160c55-000d-41b4-a6cd-0e2a4dd6439f",
            "amount": 1200,
            "authorizationCode": "cd3a3d65-d096-4c4c-aef0-abd71b70b49b",
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "pending",
            "responseTs": "627ms",
            "drip": {
                "paymentUrl": "https://sandbox-portal.dripapp.com.br/checkouts/61cad747-7bc3-41b0-a3c5-6a127eacd790",
                "browser": {
                    "ipAddress": "127.0.0.1",
                    "browserFingerprint": "ab123cd456ef789"
                },
                "items": [
                    {
                        "id": "12345",
                        "title": "Product 1",
                        "quantity": 1,
                        "unitPrice": 300
                    }
                ],
                "successRedirectUrl": "https://service-example.com/success",
                "cancelRedirectUrl": "https://service-example.com/cancel"
            }
        }
    ]
}
```

## Testando atualização de estorno

Para testar uma atualização de estorno no sandbox da Malga você deve atualizar manualmente a transação com uma requisição, conforme descrito na sessão de [recebimento de notificações](/docs/payment-methods/drip#testando-recebimento-de-notificação-de-cobrança-drip-paga).

**Requisição para atualizar manualmente uma cobrança Drip como estornada em sandbox**

```bash
curl --location --request POST 'https://sandbox-api.malga.io/v1/charges/<CHARGE_ID>' \
          --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
          --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
          --header 'Content-Type: application/json' \
          --data-raw '{
              "status": "voided"
          }'
```
