---
id: pix
slug: /payment-methods/pix
title: Pix
sidebar_label: Pix
category: 662fa995369d3500194ad422
---

# Pix

Pix é o pagamento instantâneo brasileiro. O meio de pagamento criado pelo Banco Central (BC) em que os recursos são transferidos entre contas em poucos segundos, a qualquer hora ou dia. É prático, rápido e seguro. O Pix pode ser realizado a partir de uma conta corrente, conta poupança ou conta de pagamento pré-paga.

O Pix foi criado para ser um meio de pagamento bastante amplo. Qualquer pagamento ou transferência que hoje é feito usando diferentes meios (TED, cartão, boleto etc.), poderá ser feito com o Pix, simplesmente com o uso do aparelho celular.

A Malga realiza o papel de hub de pagamentos, porém não realiza a liquidação financeira sendo necessário conta em uma instituição financeira integrante do sistema de pagamentos. Dessa forma, recebemos os dados enviados pelo cliente no [Serviço de Charges](https://docs.malga.io/api#tag/Charges) e fazemos a comunicação com os adquirentes e PSPs.

## Os participantes do Pix

- O recebedor, cliente da Malga que deve possuir uma conta em instituição financeira parceira para receber pagamentos via Pix.
- Provedor de pagamento, instituição financeira onde o cliente possui conta, responsável pela emissão de um QR Code dinâmico com os dados bancários do recebedor e dados do produto ofertado.
- O pagador, um comprador qualquer que deverá realizar o scan do QR Code dinâmico no aplicativo de instituição financeira de sua escolha para realizar o pagamento.

## Fluxo de pagamento Pix

- Para criar uma transação com Pix basta o cliente informar o meio de pagamento Pix na criação de uma cobrança, sua data de expiração e dados que identifiquem o comprador;
- Uma cobrança é registrada no sistema de pagamentos instântenos do banco central e fica disponível para pagamento. Os dados do QR Code são retornados no formato de imagem, que pode ser scaneado pelo pagador, e código, que pode ser copiado pelo pagador;
- O Cliente deve apresentar os dados da cobrança (imagem do qrcode ou código para copia e cola) ao pagador que deve efetuar o pagamento dentro do tempo de validade definido na cobrança pelo recebedor;
- O pagador deve realizar o pagamento do QR Code em instituição financeira de sua escolha;
- Após confirmação do pagamento, o provedor de pagamento irá realizar a transferência de fundos via novo sistema de pagamentos instantâneos para a conta do recebedor;
- Posteriormente o cliente recebedor será notificado de que o pagamento foi efetuado e a cobrança finalizada com sucesso.

## Fluxo de pagamento associado ao customer

- Criar um [customer](https://docs.malga.io/api#operation/createCustomer);
- Criar um novo [charge](https://docs.malga.io/api#operation/charge) informando como `paymentSource` o customer criado previamente, dessa forma iremos utilizar os dados do comprador para geração da cobrança.

## Provedores suportados para cobrança via Pix

Veja os provedores que suportam cobrança via pix na nossa [tabela de provedores e meios de pagamento suportados](/api#section/Provedores-e-meios-de-pagamentos-suportados).

:::tip
A cobrança tipo Pix quando criada na Malga é registrada com status `pending`, sendo atualizada automaticamente para o status de `authorized` quando somos notificados pela instituição financeira da confirmação do pagamento, esse tempo pode variar de provedor para provedor, não podendo exceder o tempo de expiração definido na criação da cobrança.
:::

## Notificação de alteração de status

| objeto        | evento           | descrição                                                                                                                  |
| ------------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------- |
| _transaction_ | _pending_        | Evento enviado quando a cobrança é registrada e os dados para pagamento estão disponíveis                                  |
| _transaction_ | _authorized_     | Evento enviado quando é reconhecido a confirmação do pagamento da cobrança                                                 |
| _transaction_ | _failed_         | Evento enviado quando a cobrança é negada pela instituição financeira antes de ter sido autorizada, sem estorno financeiro |
| _transaction_ | _refund_pending_ | Evento enviado quando é solicitado o estorno do Pix e ele ainda está em processamento                                      |
| _transaction_ | _voided_         | Evento enviado quando é recebida a confirmação de sucesso de estorno total ou parcial do Pix                               |

:::caution
É possível que o valor de pagamento de uma _transaction_ Pix com status _authorized_ seja diferente do valor original de emissão do mesmo, como em caso de juros, multa ou desconto sendo aplicados. Sempre verifique o campo `amount` informado nas atualizações de _transactions_ para confirmar o valor pago.
:::

## Estorno Total ou Parcial

No fluxo de pagamento por Pix é possível realizar um estorno no valor total ou parcial da cobrança, dependendo da instituição financeira. No caso de estorno parcial o lojista opta por realizar um estorno com valor inferior ao valor da venda, sendo feito o reembolso do valor parcial estornado para o comprador, permanecendo a cobrança como autorizada para liquidação do valor restante para o lojista.

Para realizar um estorno parcial, basta enviar na [requisição de void](/api#operation/refundCharge) um `amount` inferior ao valor original da transação, sendo este `amount` do estorno o valor a ser estornado. Uma vez concluído o estorno parcial pelo provedor, o `amount` da transação é atualizado para o valor residual da transação após o estorno.

:::caution
O atributo `originalAmount` do objeto `charge` se mantém como o valor originalmente autorizado na transação, é o valor inicial da transação quando aprovada. Já o atributo `amount` do objeto `charge` é alterado a cada solicitação de estorno parcial, mantendo o saldo residual a receber pelo lojista.
:::

É importante ressaltar que podem ser realizadas diversas requisições de estorno parcial para uma mesma transação, e o valor a ser estornado não pode ser superior ao valor residual existente na transação, não sendo possível estornar um valor superior. O histórico de requisições de estorno pode ser encontrado na listagem de `transactionRequests` do objeto `charge`.

:::info
O status da transação permanecerá como `authorized` enquanto restar valor a ser recebido pelo lojista, sendo alterado para `voided` somente quando todo o valor original da transação for estornado.
:::

Quando é feita a solicitação do estorno do Pix será adicionada no `transactionRequests` uma nova request com `requestType` `void` e `requestStatus` `processing`. Quando o estorno for finalizado será adicionada à lista de `transactionRequests` uma request com `requestType` `void` e `requestStatus` `success`.

A operação de estorno de Pix é realizada de forma assíncrona. Quando recebida a requisição para solicitar estorno, o status da transação passa a ser `refund_pending` e é automaticamente atualizado para `voided` quando somos notificados pela instituição financeira da confirmação do estorno.

![Fluxo Estorno do Pix](/img/pix-chargeback-flux-ptbr.png)

## Provedores suportados para estorno de Pix

| ProviderType   | Estorno total | Estorno parcial |
| -------------- | ------------- | --------------- |
| `MERCADO_PAGO` | X             | X               |
| `PAGARME`      | X             | X               |
| `BS2`          |               |                 |
| `BB`           | X             | X               |
| `ZOOP`         | X             | X               |
| `PAGSEGURO`    | X             | X               |
| `ADYEN`        | X             | X               |
| `GETNET`       |               |                 |
| `NUPAY`        | X             | X               |

## Testando recebimento de notificação de Pix pago

Para testar sua integração com os webhooks da Malga você pode desenvolver direto seu sistema ou utilizar algum serviço como request.bin ou pipedream.com para validar inicialmente os eventos enviado. Basta gerar um novo endpoint nestes serviços e cadastrar um webhook na Malga com o endpoint gerado que todos os eventos enviados ficarão registrados nestes serviços para consulta e debug.

Permitimos no ambiente de sandbox `sandbox-api.malga.io` a atualização manual de transações criadas para os status de authorized, voided, refund_pending e charged_back. Desta forma você consegue criar uma transação e simular o evento desejado.

**Requisição para atualizar manualmente um Pix como pago em sandbox**

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

Realize uma cobrança Pix a partir dos dados do comprador usando o [Serviço de Charges](/api#operation/charge).

```bash
curl --location --request POST 'https://api.malga.io/v1/charges' \
  --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
  --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
    "amount": 150,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "paymentMethod": {
      "paymentType": "pix",
      "expiresIn": 3600
    },
    "paymentSource": {
      "sourceType": "customer",
      "customer": {
        "name": "Jose Bonifacio Da Silveira",
        "phoneNumber": "21 98889999099",
        "email": "jose@gmail.com",
        "document": {
            "number": "72912053013",
            "type": "cpf"
        }
      }
    }
  }'

< HTTP/2 201
{
  "id": "148d5db0-f1c3-439f-902d-f1f268086e1d",
  "clientId": "<YOUR_CLIENT_ID>",
  "createdAt": "2012-06-30 23:59:59 +0000",
  "amount": 150,
  "originalAmount": 150,
  "currency": "BRL",
  "statementDescriptor": "Pedido #231 loja joão",
  "capture": true,
  "status": "pending",
  "paymentMethod": {
    "paymentType": "pix",
    "expiresIn": 3600,
    "qrCodeData": "00020101021126510014BR.GOV.BCB.PIX0129K89VdiUgWN1B3p0IHrgHkNHg9tX5F52040000530398654040.155802BR5913Customer test600062070503***630431C0",
    "qrCodeImageUrl": "https://...."
  },
  "paymentSource": {
    "sourceType": "customer",
    "customerId": "1cdcf0c9-eb04-4e43-b9b2-b7a4acdead1f"
  }
}
```

## Exemplo de estorno do Pix

Realize uma estorno do Pix a partir do id da transação usando a [Requisição de Estorno](/api#operation/refundCharge).

```bash
curl --location --request POST 'https://api.malga.io/v1/charges/<CHARGE_ID>/void' \
--header 'X-Client-Id: <YOUR_CLIENT_ID>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "amount": 150
}'

< HTTP/2 200
{
    "id": "1484a29e-4208-42cb-ba03-f9839b98690e",
    "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
    "clientId": "<YOUR_CLIENT_ID>",
    "description": null,
    "orderId": null,
    "createdAt": "2022-09-08T15:27:44.245Z",
    "amount": 150,
    "originalAmount": 150,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "status": "refund_pending",
    "paymentMethod": {
        "paymentType": "pix"
    },
    "paymentSource": {
        "sourceType": "customer",
        "customerId": "52a20b27-caae-4875-b455-a66f0fddcfc1"
    },
    "transactionRequests": [
        {
            "id": "4c379644-50b2-44e4-af03-62fbfaebc94a",
            "createdAt": "2022-09-08T15:29:26.950Z",
            "updatedAt": "2022-09-08T15:29:26.950Z",
            "idempotencyKey": "261fccac24c1412f9d23d5eab73418c6",
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": "MERCADO_PAGO",
            "transactionId": null,
            "amount": 150,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "processing",
            "requestType": "void",
            "responseTs": "713ms"
        },
        {
            "id": "8742b17c-47d6-464a-968e-e3f2a99eaffd",
            "createdAt": "2022-09-08T15:28:48.199Z",
            "updatedAt": "2022-09-08T15:28:48.199Z",
            "idempotencyKey": "261fccac24c1412f9d23d5eab73418c6",
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": null,
            "transactionId": null,
            "amount": 150,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "authorization",
            "responseTs": null,
            "pix": {
                "expiresIn": 3600,
                "qrCodeData": "00020101021126510014BR.GOV.BCB.PIX0129K89VdiUgWN1B3p0IHrgHkNHg9tX5F52040000530398654040.155802BR5913Customer test600062070503***630431C0",
                "qrCodeImageUrl": "https://...."
            }
        },
        {
            "id": "21fa762b-8634-4129-a25d-e8c8bdb64cb1",
            "createdAt": "2022-09-08T15:27:45.893Z",
            "updatedAt": "2022-09-08T15:27:52.406Z",
            "idempotencyKey": "261fccac24c1412f9d23d5eab73418c6",
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": "MERCADO_PAGO",
            "transactionId": "25593931617",
            "amount": 150,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "pending",
            "responseTs": "1356ms"
        }
    ]
}
```

## Exemplo de retorno do Webhook

Retorno do webhook quando é solicitado o estorno do Pix e o status do Pix é atualizado.

```bash
{
   "clientId":"<YOUR_CLIENT_ID>",
   "event":"transaction.voided",
   "payload":{
      "status":"voided",
      "id":"1484a29e-4208-42cb-ba03-f9839b98690e",
      "updatedAt":"2022-09-08T15:31:13.462Z",
      "createdAt":"2022-09-08T15:27:44.245Z",
      "idempotencyKey":null,
      "requestId":null,
      "orderId":null,
      "amount":150,
      "originalAmount":150,
      "currency":"BRL",
      "installments":null,
      "clientId":"<YOUR_CLIENT_ID>",
      "statementDescriptor":"Pedido #231 loja joão",
      "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
      "capture":true,
      "isProcessing":false,
      "customerId":null,
      "description":null,
      "fee":null,
      "feeAmount":null,
      "transactionRequests":[
         {
            "id":"21fa762b-8634-4129-a25d-e8c8bdb64cb1",
            "updatedAt":"2022-09-08T15:27:52.406Z",
            "createdAt":"2022-09-08T15:27:45.893Z",
            "idempotencyKey":"261fccac24c1412f9d23d5eab73418c6",
            "requestId":null,
            "providerId":"5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType":"MERCADO_PAGO",
            "networkTransactionId":"25593931617",
            "amount":150,
            "authorizationCode":null,
            "authorizationNsu":null,
            "requestStatus":"success",
            "requestType":"void",
            "responseTs":"1356ms",
            "pix":null,
            "boleto":null,
            "providerError":null,
            "providerAuthorization":null,
            "fraudAnalysis":null
         },
         {
            "id": "4c379644-50b2-44e4-af03-62fbfaebc94a",
            "createdAt": "2022-09-08T15:29:26.950Z",
            "updatedAt": "2022-09-08T15:29:26.950Z",
            "idempotencyKey": "261fccac24c1412f9d23d5eab73418c6",
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": "MERCADO_PAGO",
            "transactionId": null,
            "amount": 150,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "processing",
            "requestType": "void",
            "responseTs": "713ms"
         },
         {
            "id":"8742b17c-47d6-464a-968e-e3f2a99eaffd",
            "updatedAt":"2022-09-08T15:28:48.199Z",
            "createdAt":"2022-09-08T15:28:48.199Z",
            "idempotencyKey":"261fccac24c1412f9d23d5eab73418c6",
            "requestId":null,
            "providerId":"5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType":null,
            "networkTransactionId":null,
            "amount":150,
            "authorizationCode":null,
            "authorizationNsu":null,
            "requestStatus":"success",
            "requestType":"authorization",
            "responseTs":null,
            "pix":{
               "id":"71808df4-327f-493e-a046-48489b71e356",
               "updatedAt":"2022-09-08T15:28:48.199Z",
               "createdAt":"2022-09-08T15:27:49.543Z",
               "qrCodeData": "00020101021126510014BR.GOV.BCB.PIX0129K89VdiUgWN1B3p0IHrgHkNHg9tX5F52040000530398654040.155802BR5913Customer test600062070503***630431C0",
               "qrCodeImageUrl": "https://....",
               "expiresIn":3600
            },
            "boleto":null,
            "providerError":null,
            "providerAuthorization":null,
            "fraudAnalysis":null
         },
         {
            "id":"88f28d71-a64b-42f3-ac57-1cecbbaf1bb4",
            "updatedAt":"2022-09-08T15:31:13.488Z",
            "createdAt":"2022-09-08T15:31:13.488Z",
            "idempotencyKey":"261fccac24c1412f9d23d5eab73418c6",
            "requestId":null,
            "providerId":"5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType":"MERCADO_PAGO",
            "networkTransactionId":null,
            "amount":150,
            "authorizationCode":null,
            "authorizationNsu":null,
            "requestStatus":"success",
            "requestType":"void",
            "responseTs":null,
            "pix":null,
            "boleto":null,
            "providerError":null,
            "providerAuthorization":null,
            "fraudAnalysis":null
         }
      ],
      "transactionError":null,
      "fraudAnalysisMetadata":null
   }
}
```
