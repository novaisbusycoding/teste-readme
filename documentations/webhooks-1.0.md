---
id: webhooks-1.0
title: Webhooks 1.0 (obsoleto veja versão 1.1)
sidebar_label: v1.0 (obsoleto)
category: 662fa995369d3500194ad422
---

# Webhooks 1.0 (obsoleto)

Acesse a versão 1.1 [clicando aqui](/docs/webhooks)

A Malga utiliza o Serviço de Webhooks para notificar o seu sistema sobre os eventos ocorridos na nossa plataforma. Através de webhooks você consegue atualizar seu sistema sempre que um evento importante acontece, como a atualização de status de uma cobrança para confirmar ou cancelar um determinado pagamento.

## Fluxo básico para receber notificações via webhooks:

- Crie um serviço com um endpoint acessível dentro do seu sistema para receber as requisições de notificação dos eventos feitas pela Malga.
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
        "version": 1,
        "status": true
}'

< HTTP/2 201
{
    "id": "31c142ad-4c30-4964-ba24-2df0f2bbb745",
    "clientId": "cc0b1e41-2936-45c5-947f-93995ffcdc00",
    "event": "transaction.authorized",
    "endpoint": "https://enuqkxq2lu8be0y.m.pipedream.net",
    "version": 1,
    "status": true,
    "createdAt": "2021-07-06T21:03:36.590Z",
    "updatedAt": "2021-07-06T21:03:36.590Z"
}
```

## Evento de notificação enviado

Muitos dos eventos que ocorrem na sua integração com a Malga são síncronos e você recebe um retorno direto como resposta da sua requisição, como nos casos de criar um cliente, criar um cartão, etc.

Porém, em determinados casos, a resposta que você recebe após realizar uma requisição não contempla o status final daquele objeto, sendo necessário registrar um webhook para receber respostas assíncronas da API da Malga, mantendo seu sistema atualizado, isso ocorre principalmente nos casos de cobrança através de PIX e Boleto, notificação de suspeita de fraude, liquidação financeira de transações, entre outros.

Quando um determinado evento ocorre a Malga então cria um objeto do tipo `event` que é enviado através de uma requisição HTTP para o seu endpoint cadastrado. O evento é imutável dentro da estrutura de notificações da Malga, isso significa que o os dados do objeto que sofreu alteração são gravados junto com o evento, representando o estado do objeto imediatamente após o evento que o alterou.

**Exemplo de requisição de notificação de evento enviada pela Malga para seu endpoint:**

```bash
> POST <ENDPOINT_URL> HTTP/2
> Host: <ENDPOINT_HOST>
> User-Agent: axios/0.21.1
> Accept: application/json, text/plain, */*
> Content-Type: application/json
> x-idempotency-key: 5616b19e-4d99-4bd3-b415-4990e5cab4f4
> HTTP/2

{
  "id": "421b0e9d-ab3f-4d29-b626-d832e89f3a3b",
  "apiVersion": "1",
  "object": "transaction",
  "event": "authorized",
  "data": {
    "status": "authorized",
    "id": "ca18308b-6846-43c4-aa92-bc7685e1f979",
    "updatedAt": "2022-08-01T21:27:54.759Z",
    "createdAt": "2022-08-01T21:27:52.088Z",
    "idempotencyKey": null,
    "requestId": null,
    "orderId": "17",
    "amount": 200,
    "originalAmount": 200,
    "currency": "BRL",
    "installments": 1,
    "clientId": "cc0b1e41-2936-45c5-947f-93995ffcdc00",
    "statementDescriptor": "W✔️C-",
    "merchantId": "5002bdcd-e816-4741-871c-6de4f31ea965",
    "capture": false,
    "isDispute": false,
    "isProcessing": false,
    "customerId": null,
    "description": null,
    "fee": null,
    "feeAmount": null,
    "transactionRequests": [
      {
        "id": "50c7da2f-6283-4e4d-93f9-3f5df50808ca",
        "updatedAt": "2022-08-01T21:27:53.719Z",
        "createdAt": "2022-08-01T21:27:53.317Z",
        "idempotencyKey": "d108207f-3189-4166-b774-34e2c535ece5",
        "requestId": null,
        "providerId": "e78f2c8a-2c13-4e2b-ba67-efe528452c61",
        "providerType": "STRIPE",
        "networkTransactionId": "ch_3JYE7MHjGFBGEeiP0lfTD3Ob",
        "amount": 200,
        "authorizationCode": null,
        "authorizationNsu": null,
        "requestStatus": "success",
        "requestType": "authorization",
        "responseTs": "395ms",
        "pix": null,
        "boleto": null,
        "providerError": null,
        "providerAuthorization": null
      }
    ],
    "transactionError": null
  },
  "createdAt": "2022-08-31T11:41:51.583Z"
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
| Quarta tentativa   | 2 dias                        | 5 segundos                    |
| Quinta tentativa   | 4 dias                        | 5 segundos                    |

:::info
A Malga mantém o registro de todas as notificações feitas, bem como as informações da requisição (Request) e da resposta do servidor externo (Response). Após o número máximo de tentativas de entrega do evento exceder, marcamos a mensagem como perdida e ela fica armazenada para reprocessamento futuro mediante solicitação.
:::

A URL do seu webhook deve estar exposta (pública) para a internet, de forma que a plataforma da Malga a alcance e consiga enviar os eventos.

**Processando eventos, ordem e duplicidade**

Quando um determinado evento ocorre, a Malga então cria um objeto do tipo `event` que registra o objeto e o tipo do evento de atualização, bem como a data da ocorrência. Cada evento possui um identificador único que deve ser utilizado do lado do cliente para evitar duplicidade de processamento. O identificador é enviado no objeto `event` no corpo da requisição e também no header `x-idempotency-key` do request, sendo o mesmo valor.

Os eventos são enviados através de uma requisição HTTP para o seu endpoint exatamente na ordem em que eles ocorreram no sistema da Malga, porém recomendamos que seja utilizado a data de criação do evento, também enviado no objeto `event`, para garantir uma ordem cronológica no processamento dos eventos do lado do cliente. Caso você receba um evento com data de criação inferior a data de criação de um outro evento já processado pelo seu sistema, os dados do objeto enviado no evento estarão desatualizados, ficando a seu critério tomar ou não alguma ação com esse evento.

## Testando notificação via webhooks

Para testar sua integração com os webhooks da Malga, você pode desenvolver direto seu sistema ou utilizar algum serviço como request.bin ou pipedream.com para validar inicialmente os eventos enviado. Basta gerar um novo endpoint nestes serviços e cadastrar um webhook na Malga com o endpoint gerado que todos os eventos enviados ficarão registrados nestes serviços para consulta e debug.

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

## Eventos suportados para notificação via webhooks

| Evento                         | Descrição                                                                                                                                                                  |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **transaction.pending**        | Evento enviado quando a cobrança é registrada e os dados para pagamento estão disponíveis                                                                                  |
| **transaction.pre_authorized** | Evento enviado quando é reconhecida a confirmação do pagamento da cobrança                                                                                                 |
| **transaction.authorized**     | Evento enviado quando é reconhecida a confirmação da captura do pagamento da cobrança                                                                                      |
| **transaction.failed**         | Evento enviado quando a cobrança é negada pela instituição financeira antes de ter sido autorizada                                                                         |
| **transaction.canceled**       | Evento enviado quando a cobrança é cancelada após ter sido autorizada porém não capturada, sem estorno financeiro                                                          |
| **transaction.voided**         | Evento enviado quando a cobrança é cancelada após ter sido autorizada e capturada, produzindo um estorno financeiro                                                        |
| **transaction.charged_back**   | Evento enviado quando a cobrança é cancelada após ter sido contestada e/ou não reconhecida pelo portador do cartão                                                         |
| **transaction.dispute**        | Evento enviado quando uma disputa relacionada à transação é aberta                                                                                                         |
| **transaction.dispute_closed** | Evento enviado quando uma disputa é encerrada. Caso você receba charged_back ao invés de dispute_closed, significa que o cliente ganhou a disputa.                         |
| **transaction.revert_void**    | Evento enviado quando precisamos fazer a reversão de um estorno bem sucedido. [Veja mais](/docs/payment-methods/credit-card#reversão-automática-de-um-estorno-revert_void) |
