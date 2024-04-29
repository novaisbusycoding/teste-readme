---
id: seller
slug: /split/seller
title: Gerenciar Recebedores
category: 662fa995369d3500194ad422
---

# Recebedor (seller)

Para a utilização da funcionalidade do **SPLIT**, foi desenvolvido um novo serviço o de **SELLER**,
com a finalidade em realizar o cadastramento do "Recebedor" e/ou "Recebedores" da divisão do split.
Para que as transações que utilizam o SPLIT sejam realizadas com sucesso, é necessário haver o cadastro dos recebedores.

## Possíveis status de recebedor

A criação de um seller dentro dos provedores não é feita de forma imediata, uma vez que critérios precisam ser analisados, assim como uma transação financeira.

Os status possíveis para um seller são:

| Status       | Descrição                                                     |
| ------------ | ------------------------------------------------------------- |
| **pending**  | Informações recebidas porém pendente de validação no provedor |
| **active**   | Recebedor pode transacionar e sacar valores                   |
| **inactive** | Recebedor não pode transacionar nem sacar valores             |

## Provedores suportados

Veja os provedores que suportam criação de `sellers` na nossa [tabela de provedores e meios de pagamento suportados](/api#section/Provedores-e-meios-de-pagamentos-suportados).

## Exemplo de criação de recebedor aprovado

Caso se trate de um parceiro **PESSOA FÍSICA**, utilize a propriedade **owner**:

```bash
curl --location --request POST 'https://api.malga.io/v1/sellers' \
--header 'X-Client-Id: <YOUR_SECRET_KEY>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "b1612460-0fef-447d-9590-97825cf60cf6",
    "owner": {
        "name": "Seller",
        "email": "seller@email.com",
        "phoneNumber": "85988350264",
        "birthdate": "1995-01-27",
        "document": {
            "type": "cpf",
            "number": "36243319067",
            "country": "BR"
        },
        "address": {
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "apto 208 bloco c",
            "zipCode": "61000-320",
            "country": "BR",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        }
    },
    "mcc": 1,
     "bankAccount": {
        "holderName": "Seller name",
        "holderDocument": "36243319067",
        "bank": "077",
        "branchNumber": "492",
        "branchCheckDigit": "1",
        "accountNumber": "4929",
        "accountCheckDigit": "22",
        "type": "conta_corrente"
    },
    "metadata": {[
      "key": "768093",
      "value": "informação adicional"
    ]},
    "transferPolicy": {
        "transferDay": 5,
        "transferEnabled": true,
        "transferInterval": "weekly",
        "automaticAnticipationEnabled": false,
        "anticipatableVolumePercentage": "",
        "automaticAnticipationType": "",
        "automaticAnticipationDays": "",
        "automaticAnticipation1025Delay": ""
    }
}'

< HTTP/2 201
{
    "id": "19d05a45-0e92-478e-8366-955231bcf3d6",
    "providers": {
        "providerType": "SANDBOX",
        "externalId": "1966811",
        "externalStatus": "active",
        "externalStatusReason": "ok",
        "status": "pending",
        "createdAt": "2022-12-21T23:10:13.498Z",
        "updatedAt": "2022-12-21T20:10:13.951Z"
    },
    "merchantId": "b1612460-0fef-447d-9590-97825cf60cf6",
    "clientId": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
    "metadata": null,
    "owner": {
        "id": "1ae4bc8a-f8b2-4050-bc32-a90534a803c1",
        "updatedAt": "2023-07-11T22:06:14.078Z",
        "createdAt": "2023-07-11T22:06:14.078Z",
        "name": "Seller",
        "email": "seller@email.com",
        "phoneNumber": "85988350264",
        "birthdate": "1995-01-27T02:00:00.000Z",
        "address": {
            "country": "BR",
            "id": "ecccaa6c-63ac-4edf-8f5e-897f4cd56f76",
            "updatedAt": "2023-07-11T22:06:14.080Z",
            "createdAt": "2023-07-11T22:06:14.080Z",
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "apto 208 bloco c",
            "zipCode": "61000-320",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB",
        }
        "document": {
            "country": "BR",
            "id": "afcb459f-168c-4b74-a2cc-85b1756cb97f",
            "updatedAt": "2022-12-21T23:10:13.334Z",
            "createdAt": "2022-12-21T23:10:13.334Z",
            "type": "cpf",
            "number": "36243319067"
        }
    },
    "business": null,
    "bankAccount": {
        "id": "924ab8c7-df93-465b-97e3-c211c75a3e6e",
        "updateAT": "2023-02-28T18:00:00.573Z",
        "createdAt": "2023-02-28T18:00:00.573Z",
        "holderName": "Seller name",
        "holderDocument": "36243319067",
        "bank": "077",
        "branchNumber": "492",
        "branchCheckDigit": "1",
        "accountNumber": "4929",
        "accountCheckDigit": "22",
        "type": "conta_corrente"
    },
    "transferPolicy": {
        "id": "6d76b361-a9a8-4e26-865e-d1c790ad5c72",
        "updatedAt": "2023-07-05T18:55:29.809Z",
        "createdAt": "2023-07-05T18:55:29.809Z",
        "transferDay": 5,
        "transferEnabled": true,
        "transferInterval": "weekly",
        "automaticAnticipationEnabled": false,
        "anticipatableVolumePercentage": null,
        "automaticAnticipationType": null,
        "automaticAnticipationDays": null,
        "automaticAnticipation1025Delay": null
    },
    "mcc": 1,
    "status": "active"
}
```

Caso se trate de um parceiro **PESSOA JURÍDICA**, utilize as propriedades **owner** e **business**:

```bash
curl --location --request POST 'https://api.malga.io/v1/sellers' \
--header 'X-Client-Id: <YOUR_SECRET_KEY>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "b1612460-0fef-447d-9590-97825cf60cf6",
    "owner": {
        "name": "Seller",
        "email": "seller@email.com",
        "phoneNumber": "85988350264",
        "birthdate": "1995-01-27",
        "document": {
            "type": "cpf",
            "number": "36243319067",
            "country": "BR"
        },
        "address": {
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "casa 4",
            "zipCode": "61000-320",
            "country": "BR",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        }
    },
    "business": {
        "name": "Seller business",
        "phoneNumber": "85988350264",
        "email": "sellerbusiness@email.com",
        "website": "www.sellerbusiness.com.br",
        "description": "Seller business",
        "facebook": "facebook Seller business",
        "twitter": "twitter Seller business",
        "openingDate": "1995-01-27",
        "address": {
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "sala 100",
            "zipCode": "61000-320",
            "country": "BR",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        },
        "document": {
            "type": "cnpj",
            "number": "94938591000196",
            "country": "BR"
        }
    },
    "mcc": 4040,
    "bankAccount": {
        "holderName": "Seller name",
        "holderDocument": "36243319067",
        "bank": "077",
        "branchNumber": "492",
        "branchCheckDigit": "1",
        "accountNumber": "4929",
        "accountCheckDigit": "22",
        "type": "conta_corrente"
    },
    "transferPolicy": {
        "transferDay": 5,
        "transferEnabled": true,
        "transferInterval": "weekly",
        "automaticAnticipationEnabled": false,
        "anticipatableVolumePercentage": "",
        "automaticAnticipationType": "",
        "automaticAnticipationDays": "",
        "automaticAnticipation1025Delay": ""
    }
}'

< HTTP/2 201
< content-type: application/json; charset=utf-8
{
    "id": "19d05a45-0e92-478e-8366-955231bcf3d6",
    "providers": {
        "providerType": "SANDBOX",
        "externalId": "1966811",
        "externalStatus": "active",
        "externalStatusReason": "ok",
        "status": "pending",
        "createdAt": "2022-12-21T23:10:13.498Z",
        "updatedAt": "2022-12-21T20:10:13.951Z"
    }
    "merchantId": "b1612460-0fef-447d-9590-97825cf60cf6",
    "clientid": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
    "metadata": null,
    "owner": {
       "id": "8231ba21-3758-4bd7-b664-5b5fdeda37a0",
        "updatedAt": "2023-07-11T23:02:51.581Z",
        "createdAt": "2023-07-11T23:02:51.581Z",
        "name": "Seller",
        "email": "seller@email.com",
        "phoneNumber": "85988350264",
        "birthdate": "1995-01-27T02:00:00.000Z",
         "address": {
            "country": "BR",
            "id": "883631f0-fea0-4682-ae1a-f4ac6349e0d9",
            "updatedAt": "2023-07-11T23:02:51.585Z",
            "createdAt": "2023-07-11T23:02:51.585Z",
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "casa 4",
            "zipCode": "61000-320",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        },
        "document": {
            "country": "BR",
            "id": "32543bbe-42c1-4000-9b68-d01a2735708e",
            "updatedAt": "2023-07-11T23:02:51.588Z",
            "createdAt": "2023-07-11T23:02:51.588Z",
            "type": "cpf",
            "number": "36243319067"
        },
    },
    "business": {
        "id": "607bb56a-974a-4d1d-9f56-cda865dfafbd",
        "updatedAt": "2023-07-11T23:02:51.571Z",
        "createdAt": "2023-07-11T23:02:51.571Z",
        "name": "Seller business",
        "phoneNumber": "85988350264",
        "email": "seller@email.com",
        "website": "www.sellerbusiness.com.br",
        "description": "Seller business",
        "facebook": "facebook Seller business",
        "twitter": "twitter Seller business",
        "openingDate": "1995-01-27",
         "address": {
            "country": "BR",
            "id": "b681dd2e-ebdb-4fad-8c8b-e703a17825ce",
            "updatedAt": "2023-07-11T23:02:51.574Z",
            "createdAt": "2023-07-11T23:02:51.574Z",
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "sala 100",
            "zipCode": "61000-320",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        },
        "document": {
            "country": "BR",
            "id": "b9380f2c-a657-4f47-a7da-21f3bf08182c",
            "updatedAt": "2023-07-11T23:02:51.578Z",
            "createdAt": "2023-07-11T23:02:51.578Z",
            "type": "cnpj",
            "number": "94938591000196"
        }
    },
    "bankAccount": {
        "id": "f7ac3221-8f69-4276-b88b-34ddbe5ec24a",
        "updatedAt": "2023-07-11T23:02:51.563Z",
        "createdAt": "2023-07-11T23:02:51.563Z",
        "holderName": "Seller name",
        "holderDocument": "36243319067",
        "bank": "077",
        "branchNumber": "492",
        "branchCheckDigit": "1",
        "accountNumber": "4929",
        "accountCheckDigit": "22",
        "type": "conta_corrente"
    },
     "transferPolicy": {
        "id": "a04bca24-f7d1-4cb5-acce-41f12680e5bf",
        "updatedAt": "2023-07-11T23:02:51.567Z",
        "createdAt": "2023-07-11T23:02:51.567Z",
        "transferDay": "5",
        "transferEnabled": true,
        "transferInterval": "weekly",
        "automaticAnticipationEnabled": false,
        "anticipatableVolumePercentage": "",
        "automaticAnticipationType": "",
        "automaticAnticipationDays": "",
        "automaticAnticipation1025Delay": ""
    },
    "mcc": 1,
     "status": "active"
}
```

Consulte a [tabela de códigos de banco para preenchimento de bank-bankAccount](/api#section/Tabela-código-dos-principais-bancos)

## Exemplos de busca de recebedor por ID

```bash
curl --location --request GET 'https://api.malga.io/v1/sellers/19d05a45-0e92-478e-8366-955231bcf3d6' \
--header 'X-Client-Id: <YOUR_SECRET_KEY>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json'

< HTTP/2 201
{
    "id": "19d05a45-0e92-478e-8366-955231bcf3d6",
    "providers": {
        "providerType": "SANDBOX",
        "externalId": "1966811",
        "externalStatus": "active",
        "externalStatusReason": "ok",
        "status": "pending",
        "createdAt": "2022-12-21T23:10:13.498Z",
        "updatedAt": "2022-12-21T20:10:13.951Z"
    }
    "merchantId": "b1612460-0fef-447d-9590-97825cf60cf6",
    "clientid": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
    "metadata": null,
    "owner": {
        "id": "4fece86f-dd81-4115-ac55-efe6a6825a35",
        "updatedAt": "2023-07-11T23:16:10.101Z",
        "createdAt": "2023-07-11T23:16:10.101Z",
        "name": "Seller",
        "email": "seller@email.com",
        "phoneNumber": "85988350264",
        "birthdate": "1995-01-27",
        "address": {
            "country": "BR",
            "id": "547a525b-cf08-4345-b4a5-4fca0f831a62",
            "updatedAt": "2023-07-11T23:16:10.103Z",
            "createdAt": "2023-07-11T23:16:10.103Z",
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "casa 4",
            "zipCode": "61000-320",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        },
        "document": {
            "country": "BR",
            "id": "babc1412-24c2-4c74-9791-3cd8b6926bcb",
            "updatedAt": "2023-07-11T23:16:10.105Z",
            "createdAt": "2023-07-11T23:16:10.105Z",
            "type": "cpf",
            "number": "36243319067"
        }
    },
    "business": {
        "id": "cdbeb363-0729-4271-b116-f4aee5282227",
        "updatedAt": "2023-07-11T23:16:10.094Z",
        "createdAt": "2023-07-11T23:16:10.094Z",
        "name": "Seller business",
        "phoneNumber": "85988350264",
        "email": "sellerbusiness@email.com",
        "website": "www.sellerbusiness.com.br",
        "description": "Seller business",
        "facebook": "facebook Seller business",
        "twitter": "twitter Seller business",
        "openingDate": "1995-01-27",
        "address": {
           "country": "BR",
            "id": "ee658a16-1b54-43ba-8ee1-1b84e3df5bd0",
            "updatedAt": "2023-07-11T23:16:10.096Z",
            "createdAt": "2023-07-11T23:16:10.096Z",
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "sala 100",
            "zipCode": "61000-320",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        },
        "document": {
            "country": "BR",
            "id": "f5ca5122-4479-44c1-9cad-08216df8c61e",
            "updatedAt": "2023-07-11T23:16:10.099Z",
            "createdAt": "2023-07-11T23:16:10.099Z",
            "type": "cnpj",
            "number": "94938591000196"
        }
    },
    "bankAccount": {
        "id": "a147a4fe-0e07-44e8-a58e-52806d5b72b3",
        "updatedAt": "2023-07-11T23:16:10.089Z",
        "createdAt": "2023-07-11T23:16:10.089Z",
        "holderName": "Seller name",
        "holderDocument": "36243319067",
        "bank": "077",
        "branchNumber": "492",
        "branchCheckDigit": "1",
        "accountNumber": "4929",
        "accountCheckDigit": "22",
        "type": "conta_corrente"
    },
    "transferPolicy": {
        "id": "eb896600-df76-4655-ab3f-8be651659e68",
        "updatedAt": "2023-07-11T23:16:10.092Z",
        "createdAt": "2023-07-11T23:16:10.092Z",
        "transferDay": "5",
        "transferEnabled": true,
        "transferInterval": "weekly",
        "automaticAnticipationEnabled": false,
        "anticipatableVolumePercentage": "",
        "automaticAnticipationType": "",
        "automaticAnticipationDays": "",
        "automaticAnticipation1025Delay": ""
    },
    "mcc": 1,
    "status": "active"
}
```

## Exemplo de alteração de um recebedor

É obrigatório a propriedade **bankAccount** (Dados da conta) e **owner**.

Se o parceiro for **PESSOA FÍSICA**, utiliza-se a propriedade **owner**.
Se o parceiro for **PESSOA JURÍDICA**, utiliza-se a propriedade **owner** e **business**.

```bash
curl --location --request PATCH 'https://api.malga.io/v1/sellers/b1612460-0fef-447d-9590-97825cf60cf6' \
--header 'X-Client-Id: <YOUR_SECRET_KEY>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "b1612460-0fef-447d-9590-97825cf60cf6",
    "owner": {
        "name": "Seller",
        "email": "seller@email.com",
        "phoneNumber": "85988350264",
        "birthdate": "1995-01-27",
        "document": {
            "type": "cpf",
            "number": "36243319067",
            "country": "BR"
        },
        "address": {
            "street": "Rua Nova Lua",
            "streetNumber": "30",
            "complement": "apto 208 bloco c",
            "zipCode": "61000-320",
            "country": "BR",
            "state": "CE",
            "city": "Maracanaú",
            "district": "AB"
        }
    },
    "mcc": 1,
    "bankAccount": {
        "holderName": "Seller name",
        "holderDocument": "36243319067",
        "bank": "077",
        "branchNumber": "492",
        "branchCheckDigit": "1",
        "accountNumber": "4929",
        "accountCheckDigit": "22",
        "type": "conta_corrente"
    },
    "metadata": {[
      "key": "768093",
      "value": "informação adicional"
    ]},
    "transferPolicy": {
        "transferDay": 5,
        "transferEnabled": true,
        "transferInterval": "weekly",
        "automaticAnticipationEnabled": false,
        "anticipatableVolumePercentage": "",
        "automaticAnticipationType": "",
        "automaticAnticipationDays": "",
        "automaticAnticipation1025Delay": ""
    }
}'
```

## Exemplo de exclusão de um recebedor

```bash
curl --location --request DELETE 'https://api.malga.io/v1/sellers/6fa25050-4813-4209-9faa-4170a75dd8d7' \
--header 'X-Client-Id: <YOUR_SECRET_KEY>' \
--header 'X-Api-Key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantId": "46b433bf-79aa-4cb1-9eaa-cdaea42cb955"
}'
```
