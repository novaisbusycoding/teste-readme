---
title: Idempotência
category: 662fa995369d3500194ad422
---

A API da Malga suporta chaves de idempotência para evitar duplicidade no processamento de requisições no caso de retentativas de uma mesma operação. Este recurso é útil quando uma requisição falha por erro na comunicação e você não recebeu uma resposta conclusiva. Se você receber um erro de conexão em uma determinada chamada você pode simplesmente retentar uma nova requisição usando a mesma chave de idempotência, dessa forma nossa API consegue garantir que a segunda chamada não será processada em duplicidade.

Para realizar chamadas com idempotência basta enviar um header adicional `X-Idempotency-Key`.

```bash
  curl --location --request POST 'https://api.malga.io/v1/charges' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_PUBLIC_KEY>' \
    --header 'Content-Type: application/json' \
    --header 'X-Idempotency-Key: <IDEMPOTENCY_KEY>'
```

Nosso serviço salva a chave de `Idempotency-Key` e o `Body` enviado no seu request dado um request, independente de falha ou sucesso no processamento. Salvamos o resultado do processamento, e quando uma segunda requisição é feita com a mesma chave de idempotência nosso serviço retorna o mesmo conteúdo retornado na primeira chamada, incluindo os erros.

:::caution
A chave de idempotência precisa ser única, gerada no lado do cliente, sendo necessário garantir do seu lado que as chaves não colidam. Sugerimos usar V4 UUID ou outro algoritmo similar que garanta hashes únicos.
:::

O resultado só é armazenado no lado da Malga, caso tenha sido iniciado o processamento do primeiro request. Se existir alguma falha na validação dos campos, ou se outra chave estiver em processamento simultaneamente, nenhuma resposta de idempotência é dada.

Todas as chamadas de POST aceitam chave de idempotência.

## Condição de corrida

Caso uma transação seja enviada mais de uma vez ao mesmo tempo, usando a mesma chave de idempotência, o processo entra em condição de corrida (race condition). Caso isso ocorra você poderá receber 1 transação com status de sucesso e outra com erro informando essa condição. Os erros retornados em caso de condição de corrida são:

- Http 409 - Transaction is processing
- Http 400 - There is a transaction with the same idempotency key in transaction at that time

Para evitar a condição de corrida recomendamos que você adicione um tempo mínimo de intervalo entre duas chamadas com mesma chave de idempotência, e trate os erros recebidos de 409 e 400 para fazer retentativa. Recomendamos pelo menos 3 tentativas, no máximo 5, com intervalo mínimo de 10 segundos entre elas.

Veja um exemplo:

import Tabs from "@theme/Tabs";
import TabItem from "@theme/TabItem";

<Tabs
defaultValue="nodejs"
values={[
{label: 'Node.js', value: 'nodejs'},
]}>
<TabItem value="nodejs">

Para rodar o exemplo instale o axios `npm i axios axios-retry` então rode executando `node index.js`

```javascript
const axios = require("axios");
const axiosRetry = require("axios-retry");

const MERCHANT_ID = "<YOUR_DATA_HERE>";
const CARD_ID = "<YOUR_DATA_HERE>";
const CLIENT_ID = "<YOUR_DATA_HERE>";
const API_KEY = "<YOUR_DATA_HERE>";
const IDEMPOTENCY_KEY = "<USE_RANDOM_UUID_V4>";

axiosRetry(axios, {
  retries: 3,
  retryDelay: () => {
    return 10_000;
  },
  retryCondition: (error) => {
    if (axiosRetry.isNetworkOrIdempotentRequestError(error)) {
      return true;
    }

    const isUsingIdempotencyKey =
      error.request.getHeader("x-idempotency-key") !== undefined;
    const isMethodPost = error.request.method === "POST";
    if (isUsingIdempotencyKey && isMethodPost) {
      return true;
    }

    return false;
  },
  onRetry: (retryCount) => {
    console.debug(`retried ${retryCount}`);
  },
});

async function main() {
  const data = JSON.stringify({
    merchantId: MERCHANT_ID,
    amount: 150,
    currency: "BRL",
    statementDescriptor: "Pedido #231 loja joão",
    capture: true,
    paymentMethod: {
      paymentType: "credit",
      installments: 1,
    },
    paymentSource: {
      sourceType: "card",
      cardId: CARD_ID,
    },
  });

  const headers = { "x-idempotency-key": IDEMPOTENCY_KEY };
  const result = await createCharge(data, headers);

  console.log(result?.data);
}

/**
 * Create a transaction without using idempotency key
 */
async function createCharge(data, headers = {}) {
  const config = {
    method: "post",
    url: "https://api.malga.io/v1/charges",
    headers: {
      "x-client-id": CLIENT_ID,
      "x-api-key": API_KEY,
      "Content-Type": "application/json",
      ...headers,
    },
    data: data,
  };

  try {
    const response = await axios(config);
    return response;
  } catch (e) {
    return e.response;
  }
}

main();
```

  </TabItem>
</Tabs>
