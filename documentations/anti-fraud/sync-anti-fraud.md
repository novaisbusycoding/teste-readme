---
id: sync-anti-fraud
slug: anti-fraud/sync
title: Antifraudes Síncronos
category: 662fa995369d3500194ad422
---

## Fluxo do Antifraude Síncrono

Os provedores Antifraude síncronos na Malga podem ser configurados para serem executados antes ou depois das transações através da opção `runBeforeCharge`.

Para `runBeforeCharge=false`, o ciclo de vida de uma transação com avaliação antifraude percorre três etapas:

1. Pré Autorização
2. Análise Antifraude
3. Captura

Para garantir que a venda não será perdida e que está apta a ser processada pelo antifraude, o pagamento é pré-autorizado e, em seguida, é feita uma verificação no provedor de risco. Por padrão, caso a análise seja aprovada, o pagamento é capturado. Caso contrário, ela é estornada automaticamente.

![Fluxo Antifraude](/img/antifraud/antifraud-flux-ptbr.jpg)

:::info
O comportamento automatizado do ciclo de vida da transação pode ser escolhido utilizando as configurações de `captureOnApprove` e `refundOnReprove` feitas diretamente no setup de provedor de antifraude para uso na Malga.
:::

Para `runBeforeCharge=true`, o ciclo de vida de uma transação com avaliação antifraude percorre duas etapas:

1. Análise Antifraude
2. Captura

![Fluxo Antifraude](/img/antifraud/antifraud-flux-runBeforeCharge-ptbr.jpg)

Com esta configuração, todas as transações serão avaliadas pelo antifraude antes de serem processadas nos provedores de cobrança.

## Exemplos de uso do antifraude

Para utilizar análises antifraude em suas transações basta estar credenciado em um provedor antifraude e configurá-lo em seu fluxo de pagamentos. Assim, será possível processar uma transação enviando as informações adicionais de antifraude.

### Exemplo de cobrança com Antifraude aprovado

Após a pré autorização, se a cobrança passar pela avaliação de risco, é aprovada e capturada na sequência.

```bash
curl --location --request POST 'https://api.malga.io/v1/charges' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "5002bdcd-e816-4741-871c-6de4f31ea966",
    "amount": 100,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
    },
    "paymentSource": {
        "sourceType": "card",
        "card": {
            "cardHolderName": "JOSE DAS NEVES",
            "cardNumber": "4929564637987814",
            "cardCvv": "120",
            "cardExpirationDate": "12/2026"
        }
    },
    "fraudAnalysis": {
        "sla": 10,
        "customer": {
            "name": "Joao Torres",
            "phone": "11 99329899",
            "email": "joao@gmail.com",
            "identity": "45762964851",
            "identityType": "CPF",
            "registrationDate": "2022-07-26T16:11:35.756Z",
            "billingAddress": {
                "street": "Rua Um",
                "number": "200",
                "zipCode": "02010010",
                "country": "BR",
                "state": "SP",
                "district": "Jardim Paraiso",
                "city": "Sao Paulo",
                "complement": "ap 10"
            },
            "browser": {
                "browserFingerprint": "074c1ee676ed4998ab66491013c565e2",
                "cookiesAccepted": false,
                "email": "comprador@test.com.br",
                "hostName": "google.com",
                "ipAddress": "127.0.0.1",
                "type": "Chrome"
            }
        },
        "cart": {
            "items": [
                {
                    "name": "ItemTeste1",
                    "quantity": 1,
                    "sku": "20170511",
                    "unitPrice": 100,
                    "risk": "High",
                    "locality": "Local do evento",
                    "date": "2017-03-21T22:36:53",
                    "type": 1,
                    "genre": "Concerto de música",
                    "tickets": {
                        "quantityTicketSale": 800,
                        "quantityEventHouse": 2,
                        "convenienceFeeValue": 0,
                        "quantityFull": 100,
                        "quantityHalf": 50,
                        "batch": 1234123
                    },
                    "location": {
                        "country": "BR",
                        "street": "Rua Um",
                        "number": "2",
                        "complement": "ap 1",
                        "zipCode": "02417140",
                        "city": "Sao Paulo",
                        "state": "SP",
                        "district": "Bairro"
                    }
                }
            ]
        }
    }
}'

< HTTP/2 201
< content-type: application/json; charset=utf-8
{
    "id": "713d1e9d-3e05-4258-9c38-d5b474cdca7f",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "5002bdcd-e816-4741-871c-6de4f31ea966",
    "description": null,
    "orderId": null,
    "createdAt": "2022-08-25T20:56:37.265Z",
    "amount": 100,
    "originalAmount": 100,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "status": "authorized",
    "paymentMethod": {
        "installments": 1,
        "paymentType": "credit"
    },
    "paymentSource": {
        "sourceType": "card",
        "cardId": "2aba421f-db81-4c2d-a6e8-3379c192a7c5"
    },
    "fraudAnalysisMetadata": {
        "sla": 10,
        "customer": {
            "name": "Joao Torres",
            "identity": "45762964851",
            "identityType": "CPF",
            "birthdate": null,
            "phone": "11 99329899",
            "billingAddress": {
                "street": "Rua Um",
                "number": "200",
                "complement": "ap 10",
                "zipCode": "02010010",
                "city": "Sao Paulo",
                "state": "SP",
                "district": "Jardim Paraiso"
            }
        },
        "cart": {
            "items": [
                {
                    "name": "ItemTeste1",
                    "quantity": 1,
                    "sku": "20170511",
                    "unitPrice": 100,
                    "risk": "High",
                    "locality": "Local do evento",
                    "date": "2017-03-21T22:36:53",
                    "type": 1,
                    "genre": "Concerto de música",
                    "tickets": {
                        "quantityTicketSale": 800,
                        "quantityEventHouse": 2,
                        "convenienceFeeValue": 0,
                        "quantityFull": 100,
                        "quantityHalf": 50,
                        "batch": 1234123
                    },
                    "location": {
                        "country": "BR",
                        "street": "Rua Um",
                        "number": "2",
                        "complement": "ap 1",
                        "zipCode": "02417140",
                        "city": "Sao Paulo",
                        "state": "SP",
                        "district": "Bairro"
                    }
                }
            ]
        }
    },
    "transactionRequests": [
        {
            "id": "72e788f6-c767-4688-8b23-4f3c9313458f",
            "createdAt": "2022-08-25T20:56:37.796Z",
            "updatedAt": "2022-08-25T20:56:37.796Z",
            "idempotencyKey": null,
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": "SANDBOX",
            "transactionId": "1737284",
            "amount": 100,
            "authorizationCode": "6076640",
            "authorizationNsu": "3028818",
            "requestStatus": "success",
            "requestType": "capture",
            "responseTs": "8ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "7354551",
                "networkResponseCode": "5759371"
            }
        },
        {
            "id": "f4c59668-c513-4e81-a5b2-9988866f3af4",
            "createdAt": "2022-08-25T20:56:37.645Z",
            "updatedAt": "2022-08-25T20:56:37.645Z",
            "idempotencyKey": null,
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": "SANDBOX",
            "transactionId": "9475312",
            "amount": 100,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "anti_fraud",
            "responseTs": "19ms",
            "fraudAnalysis": {
                "score": 85,
                "status": "approved"
            }
        },
        {
            "id": "67a55426-defc-4f75-92d0-97f93ef55e39",
            "createdAt": "2022-08-25T20:56:37.432Z",
            "updatedAt": "2022-08-25T20:56:37.866Z",
            "idempotencyKey": "97cfcd40-d9f0-469a-bc7f-b5cda1e2d8ac",
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": "SANDBOX",
            "transactionId": "6498444",
            "amount": 100,
            "authorizationCode": "3698706",
            "authorizationNsu": "4897050",
            "requestStatus": "success",
            "requestType": "pre_authorization",
            "responseTs": "36ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "7376566",
                "networkResponseCode": "2946159"
            }
        }
    ]
}
```

### Exemplo de cobrança com Antifraude reprovado

Após a pré autorização, se a análise de risco for reprovada, a transação é rejeitada e a pré autorização é cancelada automaticamente.

```bash
curl --location --request POST 'https://api.malga.io/v1/charges/v1/charges' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "2f837bc1-9dc5-4683-9610-6e75ed9e5aa5",
    "amount": 991,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
    },
    "paymentSource": {
        "sourceType": "card",
        "card": {
            "cardHolderName": "JOSE DAS NEVES",
            "cardNumber": "4929564637987814",
            "cardCvv": "120",
            "cardExpirationDate": "12/2026"
        }
    },
    "fraudAnalysis": {
        "sla": 10,
        "customer": {
            "name": "João Neves",
            "identity": "62871519005",
            "identityType": "CPF",
            "birthdate": null,
            "phone": "219988776654",
            "billingAddress": {
                "street": "Rua CJ-14",
                "number": "100",
                "zipCode": "69314702",
                "country": "BR",
                "state": "PR",
                "district": "Jardim Tropical",
                "city": "Boa vista",
                "complement": "ap 110"
            },
            "browser":{
                "browserFingerprint":"074c1ee676ed4998ab66491013c565e2",
                "cookiesAccepted":false,
                "email":"comprador@test.com.br",
                "hostName":"google.com",
                "ipAddress":"127.0.0.1",
                "type":"Chrome"
            }
        },
        "cart": {
            "items": [
                {
                    "name": "ItemTeste1",
                    "quantity": 1,
                    "sku": "20170511",
                    "unitPrice": 991,
                    "risk": "High"
                }
            ]
        }
    }
}'

< HTTP/2 201
< content-type: application/json; charset=utf-8
{
    "id": "72dd8608-3741-48fd-9aab-5573408820fa",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "2f837bc1-9dc5-4683-9610-6e75ed9e5aa5",
    "description": null,
    "orderId": null,
    "createdAt": "2022-08-10T19:21:19.452Z",
    "amount": 0,
    "originalAmount": 991,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "status": "canceled",
    "paymentMethod": {
        "installments": 1,
        "paymentType": "credit"
    },
    "paymentSource": {
        "sourceType": "card",
        "cardId": "4a986611-0534-4628-8c2e-2845a3f6d185"
    },
    "fraudAnalysisMetadata": {
        "sla": 10,
        "customer": {
            "id": "c3eb8208-760d-4f21-b54f-70601db69aee",
            "updatedAt": "2022-08-10T14:31:37.045Z",
            "createdAt": "2022-08-10T14:31:37.045Z",
            "idempotencyKey": null,
            "requestId": null,
            "name": "João Neves",
            "identity": "62871519005",
            "identityType": "CPF",
            "birthdate": null,
            "phone": "219988776654",
            "billingAddress": {
                "street": "Rua CJ-14",
                "number": "100",
                "zipCode": "69314702",
                "country": "BR",
                "state": "PR",
                "district": "Jardim Tropical",
                "city": "Boa vista",
                "complement": "ap 110"
            }
        },
        "cart": {
            "items": [
                {
                    "name": "ItemTeste1",
                    "quantity": 1,
                    "sku": "20170511",
                    "unitPrice": 991,
                    "risk": "High"
                }
            ]
        }
    },
    "transactionRequests": [
        {
            "id": "e03d2e8f-a279-4227-8753-ad7e248e242e",
            "createdAt": "2022-08-10T19:21:21.910Z",
            "updatedAt": "2022-08-10T19:21:21.910Z",
            "idempotencyKey": null,
            "providerId": "91d0191d-75fa-4abc-bc8b-f2c5d0170ebc",
            "providerType": "PAGARME",
            "transactionId": "17979806",
            "amount": 991,
            "authorizationCode": "386630",
            "authorizationNsu": "17979806",
            "requestStatus": "success",
            "requestType": "void",
            "responseTs": "333ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "386630",
                "networkResponseCode": "authorized"
            }
        },
        {
            "id": "9e15983b-eb1f-4407-b00f-e28c2a98bd51",
            "createdAt": "2022-08-10T19:21:21.393Z",
            "updatedAt": "2022-08-10T19:21:21.393Z",
            "idempotencyKey": null,
            "providerId": "8355537a-1d61-4426-aa8e-8905c15a2489",
            "providerType": "CLEARSALE",
            "transactionId": "0AE2DE127C8242579EF7D192DC4EAB16",
            "amount": 991,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "anti_fraud",
            "responseTs": "1340ms",
            "fraudAnalysis": {
                "score": 97,
                "status": "reproved"
            }
        },
        {
            "id": "f3e23e9e-52ff-40e4-b858-cffa32f10622",
            "createdAt": "2022-08-10T19:21:19.532Z",
            "updatedAt": "2022-08-10T19:21:21.982Z",
            "idempotencyKey": "c9b417bd-fd21-42fc-9123-88dcb035449a",
            "providerId": "91d0191d-75fa-4abc-bc8b-f2c5d0170ebc",
            "providerType": "PAGARME",
            "transactionId": "17979806",
            "amount": 991,
            "authorizationCode": "386630",
            "authorizationNsu": "17979806",
            "requestStatus": "success",
            "requestType": "pre_authorization",
            "responseTs": "418ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "386630",
                "networkResponseCode": "authorized"
            }
        }
    ]
}
```

:::caution
Sempre que a transação for reprovada no antifraude, por padrão faremos o cancelamento da pré autorização automaticamente. Caso a tentativa de cancelamento falhe, a transação manterá o status `pre_authorized` e poderá ser cancelada ou capturada manualmente, de acordo com a sua preferência.
:::

### Exemplo de cobrança com erro a acessar Antifraude

Caso um erro ocorra no provedor antifraude, o comportamento padrão do fluxo de pagamentos é retornar a transação ainda pré-autorizada.

```bash
curl --location --request POST 'https://api.malga.io/v1/charges' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "2f837bc1-9dc5-4683-9610-6e75ed9e5aa6",
    "amount": 991,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
    },
    "paymentSource": {
        "sourceType": "card",
        "card": {
            "cardHolderName": "JOSE DAS NEVES",
            "cardNumber": "4929564637987814",
            "cardCvv": "120",
            "cardExpirationDate": "12/2026"
        }
    },
    "fraudAnalysis": {
        "sla": 10,
        "customer": {
            "name": "João Neves",
            "identity": "62871519005",
            "identityType": "CPF",
            "birthdate": null,
            "phone": "219988776654",
            "billingAddress": {
                "street": "Rua CJ-14",
                "number": "100",
                "zipCode": "69314702",
                "country": "BR",
                "state": "PR",
                "district": "Jardim Tropical",
                "city": "Boa vista",
                "complement": "ap 110"
            }
        },
        "cart": {
            "items": [
                {
                    "name": "ItemTeste1",
                    "quantity": 1,
                    "sku": "20170511",
                    "unitPrice": 991,
                    "risk": "High"
                }
            ]
        }
    }
}'

< HTTP/2 201
< content-type: application/json; charset=utf-8
{
    "id": "9b7f2329-5f0b-417d-8e43-022a1057fded",
    "clientId": "<YOUR_CLIENT_ID>",
    "merchantId": "2f837bc1-9dc5-4683-9610-6e75ed9e5aa6",
    "description": null,
    "orderId": null,
    "createdAt": "2022-08-10T19:26:55.701Z",
    "amount": 991,
    "originalAmount": 991,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": true,
    "status": "pre_authorized",
    "paymentMethod": {
        "installments": 1,
        "paymentType": "credit"
    },
    "paymentSource": {
        "sourceType": "card",
        "cardId": "d96fbcae-065e-4cd0-ac1e-b27714618c95"
    },
    "fraudAnalysisMetadata": {
        "sla": 10,
        "customer": {
            "id": "c3eb8208-760d-4f21-b54f-70601db69aee",
            "updatedAt": "2022-08-10T14:31:37.045Z",
            "createdAt": "2022-08-10T14:31:37.045Z",
            "idempotencyKey": null,
            "requestId": null,
            "name": "João Neves",
            "identity": "62871519005",
            "identityType": "CPF",
            "birthdate": null,
            "phone": "219988776654",
            "billingAddress": {
                "street": "Rua CJ-14",
                "number": "100",
                "zipCode": "69314702",
                "country": "BR",
                "state": "PR",
                "district": "Jardim Tropical",
                "city": "Boa vista",
                "complement": "ap 110"
            }
        },
        "cart": {
            "items": [
                {
                    "name": "ItemTeste1",
                    "quantity": 1,
                    "sku": "20170511",
                    "unitPrice": 991,
                    "risk": "High"
                }
            ]
        }
    },
    "transactionRequests": [
        {
            "id": "52efd018-c005-4b20-b28f-2cf390c794ef",
            "createdAt": "2022-08-10T19:26:56.388Z",
            "updatedAt": "2022-08-10T19:26:56.388Z",
            "idempotencyKey": "11f4bd50-4110-43bf-91e7-28ae8d0d243b",
            "providerId": "8355537a-1d61-4426-aa8e-8905c15a2489",
            "providerType": "CLEARSALE",
            "transactionId": null,
            "amount": 991,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "timeout",
            "requestType": "anti_fraud",
            "responseTs": "9ms",
            "providerError": {
                "retryable": false,
                "declinedCode": null,
                "networkDeniedMessage": null,
                "networkDeniedReason": null
            }
        },
        {
            "id": "65199fff-b961-473a-a75f-2b5717d73089",
            "createdAt": "2022-08-10T19:26:55.802Z",
            "updatedAt": "2022-08-10T19:26:56.412Z",
            "idempotencyKey": "11f4bd50-4110-43bf-91e7-28ae8d0d243b",
            "providerId": "91d0191d-75fa-4abc-bc8b-f2c5d0170ebc",
            "providerType": "PAGARME",
            "transactionId": "17981570",
            "amount": 991,
            "authorizationCode": "430210",
            "authorizationNsu": "17981570",
            "requestStatus": "success",
            "requestType": "pre_authorization",
            "responseTs": "467ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "430210",
                "networkResponseCode": "authorized"
            }
        }
    ]
}
```

:::caution
Caso ocorra alguma instabilidade com o provedor antifraude, existem duas opções para lidar com este cenário que devem ser configuradas em seu fluxo de pagamentos:

- `captureOnError`: Faz a captura mesmo que ocorra instabilidade no provedor de antifraude.
- `refundOnError`: Faz o estorno da transação em caso de instabilidade no provedor de antifraude. Não pode ser ligada ao mesmo tempo que `captureOnError`.

Nos casos onde `captureOnError` e `refundOnError` estão desligadas, a transação é retornada como `pre_authorized`.
:::
