---
id: webhooks
title: Webhooks
category: 662fa995369d3500194ad422
---

# Webhooks v1.1

A Malga utiliza o serviço de webhooks para notificar o seu sistema sobre os eventos ocorridos na nossa plataforma. Através de webhooks, você consegue atualizar seu sistema sempre que um evento importante acontece, como a atualização de status de uma cobrança para confirmar ou cancelar um determinado pagamento.

Procurando pela doc da versão 1.0? [Acesse aqui](/docs/webhooks-1.0)

## Fluxo básico para receber notificações via webhooks:

- Crie um serviço com um endpoint acessível dentro do seu sistema para receber as requisições de notificação dos eventos feitos pela Malga.
- Registre seu endpoint na Malga criando um webhook para receber notificações dos eventos desejados.
- A Malga enviará uma requisição HTTP para seu endpoint com os dados do objeto alterado sempre que o determinado evento registrado no seu webhook acontecer.

## Criação de um webhook

Realize a criação e gestão de webhooks usando o [Serviço de Webhooks](/api#operation/createWebhook).

```bash
curl --location --request POST 'https://api.malga.io/v1/webhooks' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "event": "transaction.authorized",
        "endpoint": "https://enuqkxq2lu8be0y.m.pipedream.net",
        "version": 1.1,
        "status": true
}'

< HTTP/2 201
{
    "id": "31c142ad-4c30-4964-ba24-2df0f2bbb745",
    "clientId": "cc0b1e41-2936-45c5-947f-93995ffcdc00",
    "event": "transaction.authorized",
    "endpoint": "https://enuqkxq2lu8be0y.m.pipedream.net",
    "publicKey": "-----BEGIN PUBLIC KEY-----\nMCowBQYDK2VwAyEAIda0HljovhG1yKez/Du7MUoKup/cbXqPgwyGOATOiJQ=\n-----END PUBLIC KEY-----\n"
    "version": 1.1,
    "status": true,
    "createdAt": "2021-07-06T21:03:36.590Z",
    "updatedAt": "2021-07-06T21:03:36.590Z"
}
```

## Evento de notificação enviado

Muitos dos eventos que ocorrem na sua integração com a Malga são síncronos e você recebe um retorno direto como resposta da sua requisição, como nos casos de criar um cliente, criar um cartão, etc.

Porém, em determinados casos, a resposta que você recebe após realizar uma requisição não contempla o status final daquele objeto, sendo necessário registrar um webhook para receber respostas assíncronas da API da Malga para manter seu sistema atualizado, isso ocorre principalmente nos casos de cobrança através de PIX e Boleto, notificação de suspeita de fraude, liquidação financeira de transações, entre outros.

Quando um determinado evento ocorre a Malga então cria um objeto do tipo `event` que é enviado através de uma requisição HTTP para o seu endpoint cadastrado. O evento é imutável dentro da estrutura de notificações da Malga, isso significa que o os dados do objeto que sofreu alteração são gravados junto com o evento, representando o estado do objeto imediatamente após o evento que o alterou.

## transaction

**Exemplo de requisição de notificação de evento de uma transação enviada pela Malga para seu endpoint:**

```bash
> POST <ENDPOINT_URL> HTTP/2
> Host: <ENDPOINT_HOST>
> User-Agent: axios/0.21.1
> Accept: application/json, text/plain, */*
> Content-Type: application/json
> X-Idempotency-Key: 5616b19e-4d99-4bd3-b415-4990e5cab4f4
> X-Plug-Date: 1660053072711
> X-Plug-Signature: 1f40fc92da241694750979ee6cf582f2d5d7d28e18335de05abc54d0560e0f5302860c652bf08d560252aa5e74210546f369fbbbce8c12cfc7957b2652fe9a75
> HTTP/2

{
  "id": "5616b19e-4d99-4bd3-b415-4990e5cab4f4",
  "apiVersion": "1.1",
  "object": "transaction",
  "event": "authorized",
  "createdAt": "2021-07-05T18:56:08.672Z"
  "data": {
      "id": "242b9be8-cd60-461d-af27-f31e3d6e3fb7",
      "updatedAt": "2021-07-05T18:56:08.247Z",
      "createdAt": "2021-07-05T18:56:08.247Z",
      "idempotencyKey": null,
      "requestId": null,
      "amount": 1500,
      "originalAmount": 1500,
      "installments": 1,
      "clientId": "cc0b1e41-2936-45c5-947f-93995ffcdc00",
      "description": null,
      "statementDescriptor": "Pedido #231 loja joão",
      "status": "authorized",
      "capture": true,
      "isDispute": false,
      "fee": null,
      "feeAmount": null,
      "transactionRequests": [{
          "id": "87c3973c-6a8d-40a5-8b3b-f6e89a8393ab",
          "updatedAt": "2021-07-05T18:56:08.648Z",
          "createdAt": "2021-07-05T18:56:08.255Z",
          "idempotencyKey": "fdd134fa-f79e-4164-b635-a6510908e1a2",
          "requestId": null,
          "providerId": "367b118f-a9be-421b-bb61-5df4261df634",
          "providerType": "STRIPE",
          "transactionId": "ch_3JYE7MHjGFBGEeiP0lfTD3Ob",
          "amount": 1500,
          "authorizationCode": "486677",
          "authorizationNsu": "d8d230ce-7222-4ca6-b08f-135289588bc8",
          "responseCode": "1",
          "requestStatus": "success",
          "requestType": "authorization",
          "responseTs": "377ms",
      }]
  }
}
```

## seller

Na propriedade data, você terá acesso a dois objetos aninhados: origin e seller.

Dentro do objeto seller, você poderá visualizar o estado atual do vendedor com informações de todos os provedores.

Já na propriedade origin, você receberá o nome do provedor que originou a emissão do evento. Por exemplo, se o valor de origin for SANDBOX, significa que somente o provedor SANDBOX sofreu modificação de status.

Confira o exemplo abaixo para entender melhor como funciona:

```json
{
  "id": "f2406fde-0c6b-4084-a23f-cec980ab72cc",
  "apiVersion": "1.1",
  "object": "seller",
  "event": "active",
  "data": {
    "origin": {
      "provider": "SANDBOX",
      "providerId": "af1f0562-150b-45df-93d3-ced36c6967f0",
      "status": "active"
    },
    "seller": {
      "bankAccount": {
        "accountCheckDigit": "22",
        "accountNumber": "3850",
        "bank": "077",
        "branchCheckDigit": "1",
        "branchNumber": "492",
        "createdAt": "2023-03-24T19:53:38.780Z",
        "holderDocument": "603933950",
        "holderName": "Vitória Julia Agatha Aparício",
        "id": "9c1adcfa-a4c2-41cf-b717-30af7807342a",
        "type": "conta_corrente",
        "updatedAt": "2023-03-24T19:53:38.780Z"
      },
      "business": {
        "address": {
          "city": "Maracan",
          "complement": null,
          "country": "BR",
          "createdAt": "2023-03-24T19:53:38.826Z",
          "district": "AB",
          "id": "59b67976-5d66-4870-921d-7eac4cf6d45a",
          "state": "CE",
          "street": "Rua Nova Lua",
          "streetNumber": "30",
          "updatedAt": "2023-03-24T19:53:38.826Z",
          "zipCode": "61000-320"
        },
        "createdAt": "2023-03-24T19:53:38.815Z",
        "description": "business description",
        "document": {
          "country": "BR",
          "createdAt": "2023-03-24T19:53:38.838Z",
          "id": "364d26de-98d3-4c44-b99c-4f7eec069350",
          "number": "88.627.023/0001-71",
          "type": "cnpj",
          "updatedAt": "2023-03-24T19:53:38.838Z"
        },
        "email": "email@email.com",
        "facebook": null,
        "id": "d4cfef03-fd11-4ce7-8240-5b197143795b",
        "name": "Business 1",
        "openingDate": "2022-01-10",
        "phoneNumber": "12345667",
        "twitter": null,
        "updatedAt": "2023-03-24T19:53:38.815Z",
        "website": null
      },
      "clientId": "e234eeb3-483d-4df2-87eb-1e2be5cdaccd",
      "id": "1705bde2-6707-49bb-8f72-63b7f91e9f38",
      "mcc": 4040,
      "merchantId": "37a9aed8-b1a9-4069-befa-71479f54e0b1",
      "metadata": null,
      "owner": null,
      "providers": [
        {
          "createdAt": "2023-03-24T19:53:38.939Z",
          "externalId": "4039535",
          "externalStatus": "active",
          "externalStatusReason": "ok",
          "providerType": "SANDBOX",
          "status": "pending",
          "updatedAt": "2023-03-24T19:58:00.452Z"
        }
      ],
      "transferPolicy": {
        "anticipatableVolumePercentage": null,
        "automaticAnticipation1025Delay": null,
        "automaticAnticipationDays": null,
        "automaticAnticipationEnabled": null,
        "automaticAnticipationType": null,
        "createdAt": "2023-03-24T19:53:38.800Z",
        "id": "837ea08e-f6f8-4148-bd22-48783751d7fb",
        "transferDay": "5",
        "transferEnabled": true,
        "transferInterval": "monthly",
        "updatedAt": "2023-03-24T19:53:38.800Z"
      }
    }
  },
  "createdAt": "2023-03-24T19:58:03.663Z"
}
```

## Boas práticas para receber notificações

**Tentativas e Retentativas de envio de notificações**

A Malga fará a tentativa de envio de uma determinada notificação para seu webhook, em caso de problemas no processamento do serviço passamos a realizar novas tentativas de entrega das notificações com um escalonamento de tempo entre as tentativas.

Para concluir o processamento da notificação o seu serviço deve retornar um HTTP STATUS 200 (OK) ou 201 (CREATED) dentro do tempo máximo de espera, caso contrário, será entendido que o endpoint não o recebeu corretamente e o evento será marcado para retentativa.

| Evento             | Prazo de intervalo do disparo | Tempo de espera para resposta |
| ------------------ | ----------------------------- | ----------------------------- |
| Criado             | Imediatamente                 | 30 segundos                   |
| Primeira tentativa | 5 minutos                     | 5 segundos                    |
| Segunda tentativa  | 45 minutos                    | 5 segundos                    |
| Terceira tentativa | 6 horas                       | 5 segundos                    |
| Quarta tentativa   | 1 dias                        | 5 segundos                    |
| Quinta tentativa   | 2 dias                        | 5 segundos                    |
| Sexta tentativa    | 4 dias                        | 5 segundos                    |

:::info
A Malga mantém o registro de todas as notificações feitas, bem como as informações da requisição (Request) e da resposta do servidor externo (Response). Após o número máximo de tentativas de entrega do evento exceder, marcamos a mensagem como perdida e ela fica armazenada para reprocessamento futuro mediante solicitação.
:::

A URL do seu webhook deve estar exposta (pública) para a internet, de forma que a plataforma da Malga a alcance e consiga enviar os eventos.

**Processando eventos, ordem e duplicidade**

Quando um determinado evento ocorre, a Malga então cria um objeto do tipo `event` que registra o objeto e o tipo do evento de atualização, bem como a data da ocorrência. Cada evento possui um identificador único que deve ser utilizado do lado do cliente para evitar duplicidade de processamento. O identificador é enviado no objeto `event` no corpo da requisição e também no header `x-idempotency-key` do request, sendo o mesmo valor.

Os eventos são enviados através de uma requisição HTTP para o seu endpoint exatamente na ordem em que eles ocorreram no sistema da Malga, porém recomendamos que seja utilizado a data de criação do evento, também enviado no objeto `event`, para garantir uma ordem cronológica no processamento dos eventos do lado do cliente. Caso você receba um evento com data de criação inferior a data de criação de um outro evento já processado pelo seu sistema, os dados do objeto enviado no evento estarão desatualizados, ficando a seu critério tomar ou não alguma ação com esse evento.

## Testando notificação via webhooks

Para testar sua integração com os webhooks da Malga, você pode desenvolver direto seu sistema ou utilizar algum serviço como request.bin ou pipedream.com para validar inicialmente os eventos enviados. Basta gerar um novo endpoint nestes serviços e cadastrar um webhook na Malga com o endpoint gerado que todos os eventos enviados ficarão registrados nestes serviços para consulta e debug.

Permitimos no ambiente de sandbox `sandbox-api.malga.io` a atualização manual de transações criadas para os status de authorized, voided e charged_back, dessa forma você consegue criar uma transação e simular o evento desejado.

**Requisição para atualizar manualmente uma transação em sandbox**

```bash
curl --location --request POST 'https://api.malga.io/v1/charges/<CHARGE_ID>' \
          --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
          --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
          --header 'Content-Type: application/json' \
          --data-raw '{
              "status": "charged_back"
          }'
```

## Segurança Webhook

A partir da versão 1.1 do webhook a Malga passou a assinar todos os eventos para garantir a segurança do recebimento do evento.

Utilizamos uma chave privada do tipo [Ed25519](https://ed25519.cr.yp.to/) para assinar os eventos. No momento do cadastro do webhook, você recebe a chave pública de mesmo tipo para verificar se a assinatura enviada bate com payload recebido.

Todo evento recebido deve ser verificado e caso a assinatura não seja reconhecida, por medidas de segurança, você deve descartar o evento.

### Como verificar a assinatura do evento?

Enviamos 2 headers:

- **X-Plug-Date** contendo a data que o evento foi gerado. O formato segue o padrão UTC Unix Timestamp.
- **X-Plug-Signature** contendo um hash em hexadecimal de 64 bits.

Para previnir um [ataque de reply](https://en.wikipedia.org/wiki/Replay_attack), verifique se a data do evento (X-Plug-Date) está dentro de seu critério de aceite. Recomendamos não aceitar eventos com mais de 5 minutos.

Agora você deve validar a assinatura (X-Plug-Signature). Para isso você precisará utilizar 4 dados, **X-Plug-Date**, **X-Plug-Signature**, **payload** e a **chave pública (pubKey)** (que é retornada na criação do webhook).

Primeiro você deve concatenar o date com o body encodado para utf8, criptografar com a chave pública do webhook criado, e validar o criptograma recebido no header signature com o criptograma gerado para verificar integridade.

O algoritmo para verificar a assinatura se parece com o declarado abaixo:

```bash
date = header['X-Plug-Date']
msg = "{date}\n{payload}"
sig_raw = header['X-Plug-Signature']
sig_byte = hex_to_bin(sig_raw)

EdDSA_signature_verify(msg, pubKey, sig_byte) --> bool

```

[Clique aqui e saiba mais sobre EdDSA e Ed25519](https://cryptobook.nakov.com/digital-signatures/eddsa-and-ed25519#eddsa-verify-signature)

### Exemplos de validação da signature

Os exemplos estão disponíveis nesse repositório github:

    https://github.com/plughacker/plug-sample-signature-verify/

Veja um trecho de cada linguagem:

import Tabs from "@theme/Tabs";
import TabItem from "@theme/TabItem";

<Tabs
defaultValue="nodejs"
values={[
{label: 'Node.js', value: 'nodejs'},
{label: 'Golang', value: 'golang'},
{label: 'Python', value: 'python'},
{label: 'C#', value: 'csharp'},
{label: 'Java', value: 'java'},
{label: 'PHP', value: 'php'},
{label: 'Ruby', value: 'ruby'},
]}>
<TabItem value="nodejs">

```javascript
const crypto = require("crypto");

module.exports = {
  /**
   * Example of a function that validates the payload signature
   * @param {string} publicKey       Key returned on webhook creation
   * @param {string} payload         Event sent by the plug. This information goes in the body http
   * @param {number} signatureTime   Time in unix timestamp that the event was subscribed to
   * @param {string} signature       Hash sha512
   * @returns {bool}
   */
  verify: function (publicKey, payload, signatureTime, signature) {
    const payloadUtf8 = Buffer.from(payload, "utf-8").toString();
    const signatureBuffer = Buffer.from(signature, "hex");
    const data = Buffer.from(`${signatureTime}\n${payloadUtf8}`);
    return crypto.verify(null, data, publicKey, signatureBuffer);
  },
};
```

  </TabItem>

  <TabItem value="golang">

```go
func Verify(publicKeyRaw string, payload string, signatureTime int64, sigHex string) (bool, error) {

	sig, err := hex.DecodeString(sigHex)

	if err != nil {
		return false, err
	}

	publicKey, err := getEd25519PublicKey(publicKeyRaw)

	if err != nil {
		return false, err
	}

	body := fmt.Sprintf("%d\n%s", signatureTime, payload)
	message := []byte(body)

	isValid := ed25519.Verify(publicKey, message, sig)

	return isValid, nil
}
```

  </TabItem>

  <TabItem value="python">

```py
signaturePayload = str.encode(httpSample['headers']['X-Plug-Date'] + "\n" + httpSample['body'])
signature = bytes.fromhex(httpSample['headers']['X-Plug-Signature'])

try:
  public_key.verify(signature, signaturePayload)
  print('Congrats ... signature is valid!!!')
except:
  print('Ops ... Signature is not valid!')

```

  </TabItem>

  <TabItem value="csharp">

```csharp
namespace PlugPagamentos;

using System.Text;
using NSec.Cryptography;

public class PlugSignature
{
    private static Ed25519 algorithm = SignatureAlgorithm.Ed25519;

    public static Boolean verify(String payload, long signatureDate, String signatureHex, string publicKeyRaw)
    {
        var signature = Convert.FromHexString(signatureHex);
        var payloadToVerify = Encoding.UTF8.GetBytes($"{signatureDate}\n{payload}");
        var publicKeyBytes = Encoding.UTF8.GetBytes(publicKeyRaw);

        var publicKey = PublicKey.Import(algorithm, publicKeyBytes, KeyBlobFormat.PkixPublicKeyText);
        return algorithm.Verify(publicKey, payloadToVerify, signature);
    }

}
```

  </TabItem>
  
  <TabItem value="java">

```java
package com.plugpagamentos;

import org.bouncycastle.crypto.params.Ed25519PublicKeyParameters;
import org.bouncycastle.crypto.signers.Ed25519Signer;
import org.bouncycastle.util.encoders.Hex;

public class PlugSignature {

    public static boolean verify(String publicKeyHex, String payload, String signatureTime, String signatureHex) {
        var publicKeyBytes = Hex.decode(publicKeyHex);

        var signatureBytes = Hex.decode(signatureHex);
        var signingDataBytes = (signatureTime+"\n"+payload).getBytes();

        var params = new Ed25519PublicKeyParameters(publicKeyBytes, 0);
        var verifier = new Ed25519Signer();

        verifier.init(false, params);
        verifier.update(signingDataBytes, 0, signingDataBytes.length);

        return verifier.verifySignature(signatureBytes);
    }

}
```

  </TabItem>

  <TabItem value="php">

```php
$data = "{$sampleHttp['headers']['X-Plug-Date']}\n{$sampleHttp['body']}";

// convert signature hex to bin
$signature = hex2bin($sampleHttp['headers']['X-Plug-Signature']);

if ($key->verify($data, $signature)) {
  echo 'Congrats ... signature is valid!!!';
} else {
  echo 'Ops ... Signature is not valid!';
}
```

  </TabItem>

  <TabItem value="ruby">

```ruby
sig_bytes = hex_to_bin(http_sample['headers']['X-Plug-Signature'])

message = http_sample['headers']['X-Plug-Date'] + "\n" + http_sample['body']

begin
  verify_key.verify(sig_bytes, message)
  puts 'Congrats ... signature is valid!!!'
rescue StandardError
  puts 'Ops ... Signature is not valid!'
end
```

  </TabItem>
</Tabs>

## Eventos suportados para notificação via webhooks

### Eventos Transaction

| Evento                         | Descrição                                                                                                                                                                  |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **transaction.pending**        | Evento enviado quando a cobrança é registrada e os dados para pagamento estão disponíveis                                                                                  |
| **transaction.pre_authorized** | Evento enviado quando é reconhecida a confirmação da pré-captura do pagamento da cobrança                                                                                  |
| **transaction.authorized**     | Evento enviado quando é reconhecida a confirmação da captura do pagamento da cobrança                                                                                      |
| **transaction.failed**         | Evento enviado quando a cobrança é negada pela instituição financeira antes de ter sido autorizada                                                                         |
| **transaction.canceled**       | Evento enviado quando a cobrança é cancelada após ter sido autorizada porém não capturada, sem estorno financeiro                                                          |
| **transaction.voided**         | Evento enviado quando a cobrança é cancelada após ter sido autorizada e capturada, produzindo um estorno financeiro                                                        |
| **transaction.charged_back**   | Evento enviado quando a cobrança é cancelada após ter sido contestada e/ou não reconhecida pelo portador do cartão                                                         |
| **transaction.dispute**        | Evento enviado quando uma disputa relacionada à transação é aberta                                                                                                         |
| **transaction.dispute_closed** | Evento enviado quando uma disputa é encerrada. Caso você receba charged_back ao invés de dispute_closed, significa que o cliente ganhou a disputa.                         |
| **transaction.refund_pending** | Evento enviado quando um estorno está pendente. Isso pode ocorrer em fluxos assíncronos como o PIX.                                                                        |
| **transaction.revert_void**    | Evento enviado quando precisamos fazer a reversão de um estorno bem sucedido. [Veja mais](/docs/payment-methods/credit-card#reversão-automática-de-um-estorno-revert_void) |

### Eventos Seller

| Evento              | Descrição                                                                                                                         |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **seller.active**   | Evento enviado quando um seller é aprovado no provedor. Isso indica que podemos gerar uma transação com split usando esse seller. |
| **seller.inactive** | Evento enviado quando o seller é inativado manualmente ou não aprovado na análise do provedor.                                    |
