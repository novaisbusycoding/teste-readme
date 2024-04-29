---
id: example
title: Exemplo
category: 662fa995369d3500194ad422
---

# Exemplo

Para conhecer a API em GraphQL e visualizar a estrutura de uma requisição, exibimos abaixo um exemplo de consulta (query)
realizada através da Analytics API, demonstrando a operação:

```
curl --location --request POST 'https://api.malga.io/v1/graphql' \
--header 'X-Client-Id: YOUR_CLIENT_ID' \
--header 'X-Api-Key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query allCharges {\n  charges {\n    allCharges(\n      first: 1\n      startDate: \"2023-09-01 00:00:00\"\n      endDate: \"2023-10-01 00:00:00\"\n    ) {\n      edges {\n        cursor\n        node {\n          clientId\n          id\n          createdAt\n          description\n          orderId\n          originalAmount\n          paymentMethod {\n            installments\n            paymentType\n          }\n          status\n          transactionRequests {\n            id\n            amount\n            createdAt\n            transactionId\n            idempotencyKey\n            requestStatus\n            requestType\n          }\n        }\n      }\n      pageInfo {\n        endCursor\n        hasNextPage\n        hasPreviousPage\n      }\n      totalCount\n    }\n  }\n}","variables":{}}'
```

Que retornará (_response_):

```
{
    "data": {
        "charges": {
            "allCharges": {
                "edges": [
                    {
                        "cursor": "eyJpZCI6IDE2OTM2OTMxMzU4NjN9",
                        "node": {
                            "clientId": "523afbe7-36dc-4654-9dba-e7167d0e5e2d",
                            "id": "2098ba98-7ad4-4d2c-a6b6-a6b911fdcacd",
                            "createdAt": "2023-09-02T22:18:55.863042",
                            "description": "Loja 1",
                            "orderId": null,
                            "originalAmount": 100,
                            "paymentMethod": {
                                "installments": 1,
                                "paymentType": "credit"
                            },
                            "status": "failed",
                            "transactionRequests": [
                                {
                                    "id": "b6eb472b-1d2f-4648-8b96-b00ea61276e7",
                                    "amount": "100",
                                    "createdAt": "2023-09-02T22:18:56.007904",
                                    "transactionId": "e18a7106-db5f-4662-b80b-f6faaf20a00e",
                                    "idempotencyKey": "22035412-9d5e-4635-b72b-7893c5cd34c0",
                                    "requestStatus": "failed",
                                    "requestType": "pre_authorization"
                                }
                            ]
                        }
                    }
                ],
                "pageInfo": {
                    "endCursor": "eyJpZCI6IDE2OTM2OTMxMzU4NjN9",
                    "hasNextPage": true,
                    "hasPreviousPage": false
                },
                "totalCount": 418417
            }
        }
    }
}
```

A seguir, confira como realizar a autenticação e utilizar a Analytics API Malga
