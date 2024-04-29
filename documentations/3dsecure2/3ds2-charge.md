---
id: 3DS2-charge
slug: /3dsecure2/cobranca-com-desafio
title: Cobrança com 3DS2
category: 662fa995369d3500194ad422
---

## Como funciona o 3D Secure 2

Quando uma cobrança é enviada com dados de 3DS2, o provedor irá solicitar uma analise ao banco emissor.  
O banco emissor pode exigir que o cliente faça uma autenticação. Esse processo é conhecido como desafio.  
Uma cobrança pode ser autorizada pelo banco emissor sem exigir desafio.  
A cobrança com desafio exigirá uma ação por parte do usuário para autenticar a transação.
Nesse caso o usuário será redirecionado ao banco para autenticar a transação.  
O resultado final da transação será enviado via [**webhook**](/docs/webhooks)

## Cobrança com 3D Secure 2

Para iniciar uma cobrança com 3DS2, primeiro certifique-se de que o provedor está na [**lista de provedores suportados**](/docs/3ds2/intro#provedores-suportado).  
Inclua o objeto [**threeDSecure2**](/api#operation/charge) na cobrança.

### Principais informações

- **redirectURL**: A URL que o provedor irá utilizar para retornar ao site de origem após o redirecionamento
- **requestorURL**: A URL do site do portador do cartão
- **browser**: Dados do navegador do portador do cartão
- **cardHolder**: Dados do portador do cartão
- **billingAddress**: Endereço de cobrança
- **shippingAddress**: Endereço para envio

### Exemplo de cobrança com 3D Secure 2

```bash
curl --location --request POST 'https://api.malga.io/v1/charges' \
--header 'X-Client-Id: <YOUR_CLIENT_ID>' \
--header 'X-Api-Key: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
	"merchantId": "469ed9e8-97d1-433a-a586-2fd282b7d9a4",
    "amount": 100,
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
            "cardCvv": "123",
            "cardExpirationDate": "12/2026"
        }
	},
    "threeDSecure2": {
        "redirectURL": "http://your-company/receive",
        "requestorURL": "http://your-company",
        "browser": {
            "acceptHeader": "*/*",
            "colorDepth": 24,
            "javaEnabled": true,
            "javaScriptEnabled": true,
            "language": "pt-BR",
            "screenHeight": 1080,
            "screenWidth": 1920,
            "timeZoneOffset": "180",
            "userAgent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36",
            "ip": "0.0.0.0"
        },
        "cardHolder": {
            "email": "cardHolder@email.com",
            "mobilePhone": "11 99329899"
        },
        "billingAddress": {
            "city": "Rio de Janeiro",
            "country": "Brasil",
            "streetNumber": "45",
            "zipCode": "2547896",
            "state": "RJ",
            "street": "Av. Brasil"
        },
        "shippingAddress": {
            "city": "São Paulo",
            "country": "Brasil",
            "streetNumber": "45",
            "zipCode": "2957896",
            "state": "SP",
            "street": "Rua das Flores"
        }
    }
 }'
```

### Exemplo de resposta com 3D Secure 2

```json
{
  "clientId": "8827fe85-7a47-42c6-b331-769c243a5893",
  "merchantId": "469ed9e8-97d1-433a-a586-2fd282b7d9a4",
  "description": null,
  "orderId": null,
  "createdAt": "2023-02-24T21:46:36.392Z",
  "amount": 100,
  "originalAmount": 100,
  "currency": "BRL",
  "statementDescriptor": "Pedido #231 loja joão",
  "capture": true,
  "isDispute": false,
  "status": "pending",
  "paymentMethod": {
    "installments": 1,
    "paymentType": "credit"
  },
  "paymentSource": {
    "sourceType": "card",
    "cardId": "8747b602-02d5-4246-9e3a-2be879c126f7"
  },
  "transactionRequests": [
    {
      "idempotencyKey": "00f9729c-6cd3-4529-adec-74b3efd96263",
      "providerId": "617fe0b0-f0ff-4e73-b6f1-1c765e59f1f5",
      "providerType": "ADYEN",
      "transactionId": null,
      "amount": 100,
      "authorizationCode": null,
      "authorizationNsu": null,
      "requestStatus": "processing",
      "requestType": "authorization",
      "responseTs": "971ms",
      "providerAuthorization": {
        "networkAuthorizationCode": null,
        "networkResponseCode": "RedirectShopper"
      }
    }
  ],
  "threeDSecure2": {
    "redirectURL": "http://your-company/receive",
    "requestorURL": "http://your-company",
    "browser": {
      "acceptHeader": "*/*",
      "colorDepth": 24,
      "javaEnabled": true,
      "javaScriptEnabled": true,
      "language": "pt-BR",
      "screenHeight": 1080,
      "screenWidth": 1920,
      "timeZoneOffset": "180",
      "userAgent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36",
      "ip": "127.0.0.1"
    },
    "billingAddress": null,
    "shippingAddress": null,
    "cardHolder": null,
    "authData": {
      "action": "REDIRECT",
      "providerType": "ADYEN",
      "responseType": "AUTHENTICATION",
      "response": {
        "md": "M2RzMi5lZTBiZTgzZTdmZDBlZDk...AkHGAAAA",
        "url": "https://checkoutshopper-test.adyen.com/checkoutshopper/threeDS2.shtml?pspReference=8636772643981111",
        "paReq": "BQABAgB-MNl4GsRJSf5EmZE8guLfFQX7Q5Q...L_dRprriZL72QXGCtIPK-Hd",
        "termUrl": "https://checkoutshopper-test.adyen.com/checkoutshopper/threeDS/return/H4sIAAAAAAAA_..."
      }
    }
  }
}
```

A resposta da transação irá incluir o objeto **authData** dentro do objeto **threeDSecure2**, com dados para
a ação de autenticação.

### Examinando o objeto **authData**

- O atributo **action** representa que tipo de ação foi solicitada para autenticação.
- O atributo **providerType** identifica em qual provedor o 3DS2 foi processado.
- O atributo **responseType** mostra qual é a etapa do desafio, podendo ser uma etapa de autenticação ou autorização.
- O atributo **response** é um objeto dinâmico com a resposta do provedor com dados para autenticação.

Os provedores implementam o desafio de formas diferentes, exigindo tratamentos específicos.  
Na próxima sessão iremos mostrar como lidar com o fluxo com desafio.
