---
title: Validação de Cartões
category: 662fa995369d3500194ad422
---

Uma transação de validação de cartões, também conhecida como zero dollar ou zero dólar, é uma funcionalidade usada para verificar se o cartão de crédito é válido sem gerar cobranças ao portador do cartão.

Para usar essa funcionalidade você precisa informar o número do cartão, do nome impressão no cartão, o CVV e a data de expiração.

## Validar um cartão via zero dollar

A funcionalidade de zero dollar funciona no momento de salvar um cartão de crédito.

Abaixo detalhamos o passo a passo:

### Passo 1 - Criar um merchant

Crie um merchant com pelo menos 1 provedor com suporte a validação de zero dollar.

:::info
Esse passo é importante por que o merchant criado será usado no passo 3 (validar e salvar o cartão)
:::

```bash
curl --location 'https://sandbox-api.malga.io/v1/merchants' \
--header 'x-api-key: <YOUR_SECRET_KEY>' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'Content-Type: application/json' \
--data '{
    "name": "zero dollar validator",
    "mcc": "4040",
    "providers": [{
        "priority": 1,
        "name": "SANDBOX",
        "credentials": {
            "type": "SANDBOX",
            "apiKey": "123"
        }
    }]
}'
```

Resultado:

```bash
{
    "id": "3fb1884c-f442-4044-be6f-5afe2e0629b5",
    "name": "zero dollar validator",
    "mcc": "4040",
    "clientId": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
    "status": false,
    "createdAt": "2023-05-30T13:16:06.979Z",
    "updatedAt": "2023-05-30T13:16:06.979Z",
    "providers": [
        {
            "id": "ead031bb-cd67-40e7-a5b2-aa3e5b08f934",
            "name": "SANDBOX",
            "priority": 1,
            "credentials": {
                "type": "SANDBOX",
                "apiKey": "******"
            },
            "options": null,
            "createdAt": "2023-05-30T13:16:06.979Z",
            "updatedAt": "2023-05-30T13:16:06.979Z"
        }
    ]
}
```

Nesse exemplo acima criamos um merchant usando nosso adapter de sandbox.

:::caution
É importante dizer que nosso adapter sandbox não faz uma verificação real do cartão. Usamos mocks para emular cenários.
:::

### Passo 2 - Tokenização

Agora precisamos tokenizar os dados do cartão para trafegar os dados de forma segura.

```bash
curl --location 'https://api.malga.io/v1/tokens' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "cardHolderName": "JOAO DA SILVA",
    "cardNumber": "4929564637987814",
    "cardCvv": "320",
    "cardExpirationDate": "12/2026"
}'
```

Resultado:

```bash
{
    "tokenId": "d3202b89-1ccd-4dea-95f2-05b3561bc055"
}
```

:::info
Se você estiver usando um merchant com nosso provedor SANDBOX de testes, você pode variar o CVV para testar um cenário de cartão válido e outro de cartão inválido.

Para testar um cenário válido basta que o CVV termine com 0 (zero), por exemplo, 320.

Para testar um cenário que o cartão não é válido basta que o CVV não termine com 0 (zero), por exemplo, 321
:::

Se tiver dúvidas sobre tokenização de cartão [clicando aqui](/docs/tokenization) você acessa nossa documentação sobre esse assunto.

### Passo 3 - Validar e salvar o cartão

O processo de verificação do cartão ocorre no momento que o cartão será salvo no cofre(vault) de cartões.

Para validar o cartão usando zero dollar precisamos informar 3 propriedades no payload:

- **merchantId:** Merchant contendo pelo menos 1 provedor com validação de zero dollar.
- **tokenId:** Token que contém os dados do cartão
- **cvvCheck:** `true` ou `false` para indicar o desejo de verificar ou não o cartão via zero dollar. Importante dizer que se você optar por não verificar o cartão, o mesmo será criado com status `pending`, permitindo que você tente validar esse cartão via uma transação.

```bash
curl --location 'https://api.malga.io/v1/cards' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "merchantId": "<MERCHANT_ID CRIADO NO PASSO 1>",
    "tokenId": "<TOKEN_ID CRIADO NO PASSO 2>",
    "cvvCheck": true
}'
```

Resultado:

```bash
{
    "id": "59085c6d-4d21-4fc0-a6da-dda38b51c2aa",
    "status": "active",
    "statusReason": null,
    "createdAt": "2023-05-30T16:18:11.045Z",
    "clientId": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
    "brand": "Visa",
    "cardHolderName": "JOAO DA SILVA",
    "cvvChecked": true,
    "fingerprint": "p5hgkYxaw5EXgim3GbJ7KzDUhdtxo1O2+qkLFj/Qyoo=",
    "first6digits": "492956",
    "last4digits": "7814",
    "expirationMonth": "12",
    "expirationYear": "2026",
    "transactionRequests": [
        {
            "id": "edd0d86a-76d0-4c2c-b924-1528510a5a32",
            "createdAt": "2023-09-25T18:09:59.001Z",
            "providerId": "5ce68ed3-2213-423b-8eaf-9d8c4b40df2b",
            "providerType": "SANDBOX",
            "requestStatus": "success",
            "requestType": "zero_dollar",
            "responseTs": "32ms"
        }
    ]
}
```

## Validar o cartão em uma transação de pequeno valor

Caso você não tenha configurado um provedor com suporte a zero dollar você tem a opção de validar um cartão através de uma transação de baixo valor, por exemplo, R$1,00.

Depois de validado você estorna a transação.

Para realizar esse teste você pode seguir os passos 1, 2 e 3 relatados acima. A única mudança a se fazer é no passo 3, basta não informar a propriedade `merchantId` e `cvvCheck` ou informe o cvvCheck como `false`.

```bash
curl --location 'https://api.malga.io/v1/cards' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "tokenId": "<TOKEN_ID CRIADO NO PASSO 2>",
}'
```

Usando `cvvCheck` com valor `false`

```bash
curl --location 'https://api.malga.io/v1/cards' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_SECRET_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "merchantId": "<MERCHANT_ID CRIADO NO PASSO 1>",
    "tokenId": "<TOKEN_ID CRIADO NO PASSO 2>",
    "cvvCheck": false
}'
```

O resultado da operação se parece com o json abaixo:

```bash
{
    "id": "03099be2-9412-4d6d-a0e5-f9adee45c6b1",
    "status": "pending",
    "statusReason": "cvv check was sent as false",
    "createdAt": "2023-05-30T16:28:32.506Z",
    "clientId": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
    "brand": "Visa",
    "cardHolderName": "JOAO DA SILVA",
    "cvvChecked": false,
    "fingerprint": "rjf45RCpvnKyTHB3iwsdfct4qjOgY8ppLtOboUUjAAM=",
    "first6digits": "492956",
    "last4digits": "7814",
    "expirationMonth": "10",
    "expirationYear": "2026"
}
```

Recebendo um cartão com status `pending` basta iniciar uma transação de baixo valor.

```bash
curl --location 'https://api.malga.io/v1/charges' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "merchantId": "<YOUT_MERCHANT_ID>",
    "amount": 100,
    "currency": "BRL",
    "statementDescriptor": "Validacao do cartao",
    "capture": true,
    "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
    },
    "paymentSource": {
        "sourceType": "card",
        "cardId": "<CARD_ID_TO_BE_VALIDATED>"
    }
}'
```

:::caution
CARD_ID_TO_BE_VALIDATED deve conter o id de um cartão com status pending
:::

Caso a transação acima receba sucesso (authorized) o cartão será ativado (active), em caso de falha o cartão será dado como inativo (inactive).

Para verificar o status do cartão acesse a api de cards

```bash
curl --location 'https://api.malga.io/v1/cards/<CARD_ID>' \
--header 'x-client-id: <YOUR_CLIENT_ID>' \
--header 'x-api-key: <YOUR_API_KEY>'
```

Resultado:

```bash
{
    "id": "d66e25e7-7f83-45f1-8e2b-ab9e2d1e9c8d",
    "status": "active",
    "createdAt": "2023-05-25T23:29:15.382Z",
    "clientId": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
    "customerId": null,
    "brand": "Mastercard",
    "cardHolderName": "JOAO DA SILVA",
    "cvvChecked": true,
    "fingerprint": "A4wxUVbW5AIiNYP+i/ATLZxB7VQKa5WwSlcBo12GdDg=",
    "first6digits": "527971",
    "last4digits": "3624",
    "expirationMonth": "12",
    "expirationYear": "2026"
}
```

## Provedores com suporte a zero dollar

| Provedor  | Detalhes                                                                               |
| --------- | -------------------------------------------------------------------------------------- |
| SANDBOX   | Somente para testes. Use para emular comportamentos de cartão válido / inválido        |
| CIELO     | Suporta somente bandeiras Visa, Master e Elo                                           |
| PagSeguro | https://dev.pagbank.uol.com.br/docs/validando-um-cart%C3%A3o                           |
| REDE      | https://developer.userede.com.br/e-rede#documentacao-zero-dollar                       |
| KLAP      | https://api.pasarela.multicaja.cl/docs/apitarjetas#operation/Demorado%20(Modelo%20PSP) |
