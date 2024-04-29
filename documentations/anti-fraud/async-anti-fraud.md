---
id: async-anti-fraud
slug: anti-fraud/async
title: Antifraudes Assíncronos
category: 662fa995369d3500194ad422
---

## Fluxo do Antifraude Assíncrono

Os provedores Antifraude assíncronos na Malga possuem um ciclo de vida que percorre quatro etapas, separadas em dois momentos:

1. Pré Autorização
2. Solicitação de Análise Antifraude
3. Recebimento da Análise
4. Captura e envio do webhook

No primeiro momento a transação é enviada para o provedor de cobrança para ser pré capturada e enviada para o provedor de antifraude como ilustra o diagrama a seguir.

![Fluxo Antifraude Assíncrono](/img/antifraud/async-antifraud-flux-ptbr.jpg)

Num segundo momento, após a avaliação de risco ser feita pelo provedor antifraude, o mesmo retorna o status através de um webhook onde a automação de captura ou cancelamento acontece.

![Fluxo Notificação Antifraude Assíncrono](/img/antifraud/async-antifraud-webhook-flux-ptbr.jpg)

:::info
O comportamento automatizado do ciclo de vida da transação pode ser escolhido utilizando as configurações de `captureOnApprove` e `refundOnReprove` feitas diretamente no setup de provedor de antifraude para uso na Malga.
:::

:::caution
Não é possível configurar a opção `runBeforeCharge=true` utilizando produtos de antifraude que possuam fluxo de avaliação assíncronos ou híbridos. Caso tenha dúvidas sobre o suporte desta variável com o seu produto antifraude entre em contato com o nosso suporte.
:::

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
            "providerType": "SANDBOX_ANTIFRAUD",
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
            "id": "f4c59668-c513-4e81-a5b2-9988866f3af4",
            "createdAt": "2022-08-25T20:56:37.645Z",
            "updatedAt": "2022-08-25T20:56:37.645Z",
            "idempotencyKey": null,
            "providerId": "5de239ec-b724-453a-96e1-229fe29cdedd",
            "providerType": "SANDBOX_ANTIFRAUD",
            "transactionId": "9475312",
            "amount": 100,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "anti_fraud",
            "responseTs": "19ms",
            "fraudAnalysis": {
                "score": null,
                "status": "pending"
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
            "id": "3973bbb2-21e0-453e-8c2b-09c2234079e4",
            "createdAt": "2023-11-08T20:02:20.249Z",
            "updatedAt": "2023-11-08T20:02:20.249Z",
            "idempotencyKey": null,
            "providerId": "91850c78-8739-4417-95f4-6a830d10dadc",
            "providerType": "SANDBOX",
            "transactionId": null,
            "amount": null,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "void",
            "responseTs": null
        },
        {
            "id": "3dd57c15-09b2-48b8-825f-848d76f12411",
            "createdAt": "2023-11-08T20:02:19.984Z",
            "updatedAt": "2023-11-08T20:02:20.249Z",
            "idempotencyKey": null,
            "providerId": "f1370767-28c7-45fb-af45-8163c5d2e6b2",
            "providerType": "SANDBOX_ANTIFRAUD",
            "transactionId": null,
            "amount": null,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "anti_fraud",
            "responseTs": null,
            "fraudAnalysis": {
                "score": null,
                "status": "reproved"
            }
        },
        {
            "id": "5ac0f30b-7c42-4885-8edd-e75677aadff4",
            "createdAt": "2023-11-08T20:01:35.769Z",
            "updatedAt": "2023-11-08T20:01:35.769Z",
            "idempotencyKey": "6cf90159-9b51-462d-9eeb-010537a28ebc",
            "providerId": "f1370767-28c7-45fb-af45-8163c5d2e6b2",
            "providerType": "SANDBOX_ANTIFRAUD",
            "transactionId": "3262306",
            "amount": 500,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "anti_fraud",
            "responseTs": "90ms",
            "fraudAnalysis": {
                "score": null,
                "status": "pending"
            }
        },
        {
            "id": "7fe317d9-02b0-413e-ad19-17cce732c237",
            "createdAt": "2023-11-08T20:01:34.157Z",
            "updatedAt": "2023-11-08T20:01:34.454Z",
            "idempotencyKey": "6cf90159-9b51-462d-9eeb-010537a28ebc",
            "providerId": "91850c78-8739-4417-95f4-6a830d10dadc",
            "providerType": "SANDBOX",
            "transactionId": "3b0625b8-8947-4c4f-ab5e-852be2394bdd",
            "amount": 500,
            "authorizationCode": "2234373",
            "authorizationNsu": "6727421",
            "requestStatus": "success",
            "requestType": "pre_authorization",
            "responseTs": "113ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "9761119",
                "networkResponseCode": "5098365"
            }
        }
    ]
}
```

:::caution
Sempre que a transação for reprovada no antifraude, faremos o cancelamento da pré autorização automaticamente. Caso a tentativa de cancelamento falhe, a transação manterá o status `pre_authorized` e poderá ser cancelada ou capturada manualmente, de acordo com a sua preferência.
:::

### Exemplo de cobrança com erro a acessar Antifraude

Caso um erro ocorra no provedor antifraude, o comportamento padrão do fluxo de pagamentos é retornar a transação ainda pré-autorizada.

Exemplo de erro

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
            "id": "8b685a49-7a75-475a-91be-b8d12302aa14",
            "createdAt": "2023-11-08T20:03:41.481Z",
            "updatedAt": "2023-11-08T20:03:41.565Z",
            "idempotencyKey": null,
            "providerId": "f1370767-28c7-45fb-af45-8163c5d2e6b2",
            "providerType": "SANDBOX_ANTIFRAUD",
            "transactionId": null,
            "amount": null,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "failed",
            "requestType": "anti_fraud",
            "responseTs": null,
            "fraudAnalysis": {
                "score": null,
                "status": "failed"
            }
        },
        {
            "id": "6f55ec9a-764e-4878-b530-c56869847525",
            "createdAt": "2023-11-08T20:03:30.962Z",
            "updatedAt": "2023-11-08T20:03:30.962Z",
            "idempotencyKey": "447853da-d045-4b5d-901b-e20610570f04",
            "providerId": "f1370767-28c7-45fb-af45-8163c5d2e6b2",
            "providerType": "SANDBOX_ANTIFRAUD",
            "transactionId": "4027144",
            "amount": 500,
            "authorizationCode": null,
            "authorizationNsu": null,
            "requestStatus": "success",
            "requestType": "anti_fraud",
            "responseTs": "106ms",
            "fraudAnalysis": {
                "score": null,
                "status": "pending"
            }
        },
        {
            "id": "93aba631-96fc-4925-8119-f76927ddeadc",
            "createdAt": "2023-11-08T20:03:28.948Z",
            "updatedAt": "2023-11-08T20:03:29.756Z",
            "idempotencyKey": "447853da-d045-4b5d-901b-e20610570f04",
            "providerId": "91850c78-8739-4417-95f4-6a830d10dadc",
            "providerType": "SANDBOX",
            "transactionId": "f967511b-89f3-43ed-91fb-1aa90563d692",
            "amount": 500,
            "authorizationCode": "8128049",
            "authorizationNsu": "7221186",
            "requestStatus": "success",
            "requestType": "pre_authorization",
            "responseTs": "301ms",
            "providerAuthorization": {
                "networkAuthorizationCode": "8852082",
                "networkResponseCode": "3142846"
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

:::info
Para testar os diversos comportamentos das propriedades descritas acima, veja sobre [Alterar o status do antifraude no ambiente de sandbox](https://docs.malga.io/api#operation/changeAntifraudStatusTransaction)
:::
