---
id: nupay
slug: /payment-methods/nupay
title: NuPay
sidebar_label: NuPay
category: 662fa995369d3500194ad422
---

# NuPay

NuPay é uma solução de pagamento online disponibilizada pela Nubank que visa reduzir custos operacionais e melhorar a experiência de compra com parcelamento, sem atrito e sem cartão de crédito. Para pagar com a NuPay, o consumidor precisa ser um cliente do Nubank e possuir pelo menos um cartão de crédito emitido pela instituição em seu nome.

A Malga realiza o papel de intermediador de pagamentos, portanto não realiza a liquidação financeira, sendo necessária conta ativa da NuPay for Business para utilizar este método de pagamento.

:::caution
O processo de credenciamento da NuPay ainda está restrito para credenciamento de muitos negócios. Caso tenha interesse em utilizar este método de pagamento entre em contato com contato@malga.io.
:::

## Os participantes de uma transação NuPay

- O recebedor, cliente da Malga que deve possuir uma conta ativa da NuPay for Business.
- O pagador, um comprador que é um cliente Nubank.

## Fluxo de pagamento NuPay

- Para criar uma transação com NuPay basta informar o meio de pagamento `NUPAY` na criação de uma cobrança, sua data de expiração e documentos que identifiquem o comprador;
- Uma cobrança é registrada diretamente no sistema da NuPay e uma notificação é enviada para o celular do comprador;
- O pagador deve realizar o pagamento da cobrança diretamente em seu aplicativo Nubank;
- Após confirmação do pagamento, a NuPay enviará uma notificação para a Malga informando que o pagamento foi realizado;
- Posteriormente o Cliente recebedor será notificado de que o pagamento foi efetuado e a cobrança finalizada com sucesso.

Este processo está descrito no diagrama a seguir.

![Fluxo NuPay](/img/nupay-flux-ptbr.png)

:::info
As cobranças NuPay são assíncronas por natureza, ou seja, ao criar uma nova cobrança você terá a confirmação apenas da criação da cobrança e não de sua conclusão. A autorização ou reprovação de uma cobrança é notificada através dos [webhooks](/docs/webhooks).
:::

## Notificação de alteração de status

| objeto        | evento       | descrição                                                                                                 |
| ------------- | ------------ | --------------------------------------------------------------------------------------------------------- |
| _transaction_ | _pending_    | Evento enviado quando a cobrança é registrada e a notificação de cobrança foi enviada para o pagador      |
| _transaction_ | _authorized_ | Evento enviado quando é reconhecida a confirmação do pagamento da cobrança                                |
| _transaction_ | _failed_     | Evento enviado quando a cobrança é negada pela NuPay antes de ter sido criada, sem estorno financeiro     |
| _transaction_ | _canceled_   | Evento enviado quando a cobrança é negada pela NuPay antes de ter sido autorizada, sem estorno financeiro |
| _transaction_ | _voided_     | Evento enviado quando a cobrança é cancelada após ter sido autorizada, produzindo um estorno financeiro   |

## Testando recebimento de notificação de cobrança NuPay paga

Para testar sua integração com os webhooks da Malga você pode desenvolver direto seu sistema ou utilizar algum serviço como request.bin ou pipedream.com para validar inicialmente os eventos enviados. Basta gerar um novo endpoint nestes serviços e cadastrar um webhook na Malga com o endpoint gerado que todos os eventos enviados ficarão registrados nestes serviços para consulta e debug.

Permitimos no ambiente de sandbox `sandbox-api.malga.io` a atualização manual de transações criadas para os status de `authorized`, `voided` e `charged_back`, dessa forma você consegue criar uma transação e simular o evento desejado.

**Requisição para atualizar manualmente uma cobrança NuPay como paga em sandbox**

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

Realize uma cobrança NuPay a partir dos dados do comprador usando o [Serviço de Charges](/api#operation/charge).

```bash
curl --location --request POST 'https://api.malga.io/v1/charges' \
--header 'X-Client-Id: <YOUR_CLIENT_ID>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "d5b792ba-4d76-46b4-a94d-c7ddae75cf9e",
    "orderId": "5556",
    "amount": 100,
    "currency": "BRL",
    "paymentMethod": {
        "paymentType": "nupay",
        "taxValue": 1,
        "delayToAutoCancel": 200,
        "orderUrl": "https://order.com.br",
        "returnUrl": "https://return-url.com.br",
        "cancelUrl": "https://cancel-url.com.br"
    },
    "paymentSource": {
        "sourceType": "customer",
        "customer": {
            "document": {
                "number": "36436702067",
                "type": "cpf"
            },
            "name": "Jose das Flores",
            "email": "jose@gmail.com",
            "phoneNumber": "11998324141",
            "address": {
                "street": "Rua 1",
                "streetNumber": "120",
                "zipCode": "01714140",
                "state": "SP",
                "city": "São Paulo"
            }
        }
    },
    "fraudAnalysis": {
        "customer": {
            "browser": {
                "ipAddress": "127.0.0.1"
            }
        },
        "cart": {
            "items": [
                {
                    "sku": "123",
                    "name": "desc",
                    "quantity": 1,
                    "unitPrice": 1,
                    "risk": "Low"
                }
            ]
        }
    }
}'

< HTTP/2 201
{
    "id": "4cbF2516-b8c0-4222-a28d-2c7e22a9ebe1",
    "clientId": "11111111-36dc-4654-9dba-e7167d0e5e2d",
    "merchantId": "7cCf07e8-9798-4bf6-a97e-7f0e0822c176",
    "description": null,
    "orderId": "8222",
    "createdAt": "2022-09-30T21:33:42.955Z",
    "amount": 100,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": null,
    "status": "pending",
    "paymentMethod": {
        "paymentType": "nupay"
    },
    "paymentSource": {
        "sourceType": "customer",
        "customerId": "ae1b1f52-ee01-4014-9eb6-e529dd6d3f5f"
    },
    "transactionRequests": [
        {
            "id": "1c57caad-136d-4bc4-a265-c72d408d5ef7",
            "createdAt": "2022-09-30T21:33:42.969Z",
            "updatedAt": "2022-09-30T21:33:44.105Z",
            "idempotencyKey": "84d0ed42-0239-4618-b3b9-e31114bba17b",
            "providerId": "72cecc64-2079-4128-8cb0-c5a4ed8fa995",
            "providerType": "NUPAY",
            "transactionId": "2c57caad-136d-4bc4-a265-c72d408d5ef8",
            "amount": 100,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "pending",
            "responseTs": "1084ms",
            "nupay": {
                "expiresIn": 200
            }
        }
    ]
}
```

:::caution
Para criação de transações NuPay é obrigatório o envio das informações de `cart`.
:::

## Estorno NuPay

O estorno NuPay, assim como a cobrança, acontece de forma assíncrona. Isto significa que uma vez solicitado é iniciado o processo de estorno, que terá seu prazo máximo para finalização determinado na propriedade `delayToCompose`, em dias.

Isto acontece pois o processo de liquidação da NuPay é diário e é necessário utilizar saldo gerado em transações posteriores para compor o valor necessário do estorno. Caso queira mais informações sobre este processo, consulte a [documentação da NuPay sobre estorno](https://docs.nupaybusiness.com.br/#/checkout/sellers/refund/).

O fluxo de um estorno está representado no diagrama a seguir.

![Fluxo de Estorno NuPay](/img/nupay-void-flux-ptbr.png)

## Exemplo de solicitação de estorno NuPay

Quando uma solicitação de estorno é feita, é recebida de volta uma transação com status `refund_pending` e uma `transactionRequest` do tipo `void` com status `processing` informando que o estorno está sendo processado.

```bash
curl --location --request POST 'https://api.malga.io/v1/charges/<CHARGE_ID>/void' \
--header 'X-Client-Id: <YOUR_CLIENT_ID>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "amount": 100,
    "delayToCompose": 100
}'

< HTTP/2 201
{
    "id": "4cbF2516-b8c0-4222-a28d-2c7e22a9ebe1",
    "clientId": "11111111-36dc-4654-9dba-e7167d0e5e2d",
    "merchantId": "7cCf07e8-9798-4bf6-a97e-7f0e0822c176",
    "description": null,
    "orderId": "8222",
    "createdAt": "2022-09-30T21:33:42.955Z",
    "amount": 100,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": null,
    "status": "refund_pending",
    "paymentMethod": {
        "paymentType": "nupay"
    },
    "paymentSource": {
        "sourceType": "customer",
        "customerId": "ae1b1f52-ee01-4014-9eb6-e529dd6d3f5f"
    },
    "transactionRequests": [
        {
            "id": "1c57caad-136d-4bc4-a265-c72d408d5ef7",
            "createdAt": "2022-09-30T21:33:42.969Z",
            "updatedAt": "2022-09-30T21:33:44.105Z",
            "idempotencyKey": "84d0ed42-0239-4618-b3b9-e31114bba17b",
            "providerId": "72cecc64-2079-4128-8cb0-c5a4ed8fa995",
            "providerType": "NUPAY",
            "transactionId": "72cecc64-2079-4128-8cb0-c5a4ed8fa995",
            "amount": 100,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "processing",
            "requestType": "void",
            "responseTs": "1123ms",
            "nupay": {
                "refundId": "72cecc64-2079-4128-8cb0-c5a4ed8fa444"
            }
        },
        {
            "id": "1c57caad-136d-4bc4-a265-c72d408d5ef7",
            "createdAt": "2022-09-30T21:33:42.969Z",
            "updatedAt": "2022-09-30T21:33:44.105Z",
            "idempotencyKey": "84d0ed42-0239-4618-b3b9-e31114bba17b",
            "providerId": "72cecc64-2079-4128-8cb0-c5a4ed8fa995",
            "providerType": "NUPAY",
            "transactionId": "72cecc64-2079-4128-8cb0-c5a4ed8fa995",
            "amount": 100,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": null,
            "nupay": {
                "installmentsNumber": 1,
                "paymentType": "spintest-link"
            }
        },
        {
            "id": "1c57caad-136d-4bc4-a265-c72d408d5ef7",
            "createdAt": "2022-09-30T21:33:42.969Z",
            "updatedAt": "2022-09-30T21:33:44.105Z",
            "idempotencyKey": "84d0ed42-0239-4618-b3b9-e31114bba17b",
            "providerId": "72cecc64-2079-4128-8cb0-c5a4ed8fa995",
            "providerType": "NUPAY",
            "transactionId": "72cecc64-2079-4128-8cb0-c5a4ed8fa995",
            "amount": 100,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "pending",
            "responseTs": "1084ms",
            "nupay": {
                "expiresIn": 200
            }
        }
    ]
}
```

## Testando atualização de estorno

Ao criar um estorno para uma transação NuPay em nosso sandbox, o mesmo será criado com o status `refund_pending`. Para testar uma atualização de estorno no sandbox da Malga você deve atualizar manualmente a transação com uma requisição, conforme descrito na sessão de [recebimento de notificações](/docs/payment-methods/nupay#testando-recebimento-de-notificação-de-cobrança-nupay-paga).

**Requisição para atualizar manualmente uma cobrança NuPay como estornada em sandbox**

```bash
curl --location --request POST 'https://sandbox-api.malga.io/v1/charges/<CHARGE_ID>' \
          --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
          --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
          --header 'Content-Type: application/json' \
          --data-raw '{
              "status": "voided"
          }'
```
