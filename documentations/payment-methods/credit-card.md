---
id: credit-card
slug: /payment-methods/credit-card
title: Cartão Crédito
sidebar_label: Cartão de crédito
category: 662fa995369d3500194ad422
---

# Cartão de crédito

A Malga realiza o papel de hub de pagamentos, porém não realiza a liquidação financeira sendo necessário conta em uma instituição financeira integrante do sistema de pagamentos. Dessa forma, recebemos os dados enviados pelo cliente no [Serviço de Charges](https://docs.malga.io/api#tag/Charges) e fazemos a comunicação com os adquirentes e PSPs.
Confira abaixo a lista de provedores suportados para cobranças com Cartão de Crédito.

## Fluxo de pagamento

- Para criar uma transação com Cartão basta o cliente informar o meio de pagamento _credit_ na criação de uma cobrança e os dados que identifiquem o cartão;
- Uma cobrança é feita no cartão através da instuição de pagamentos escolhida;
- Após a confirmação do pagamento, o provedor de pagamento irá realizar a transferência de fundos para a conta do recebedor;
- Posteriormente o cliente recebedor poderá ser notificado de que o pagamento foi cancelado ou contestado por suspeita de fraude diretamente pelo pagador junto ao seu banco emissor.

## Fluxo de pagamento associado ao customer

- Criar um [customer](https://docs.malga.io/api#operation/createCustomer);
- Criar a [tokenização do cartão](https://docs.malga.io/api#operation/create_token);
- Criar um [card](https://docs.malga.io/api#operation/saveCard) através do token gerado;
- Realizar a [associação](https://docs.malga.io/api#operation/linkCard) do _card_ com o _customer_;
- Para verificar a associação do cartão ao customer, basta realizar a consulta por meio [Serviço de card](https://docs.malga.io/api#operation/getCards)

## Provedores suportados

Veja os provedores que suportam cobrança com cartão de crédito na nossa [tabela de provedores e meios de pagamento suportados](/api#section/Provedores-e-meios-de-pagamentos-suportados).

Quando criada na Malga, a cobrança do tipo CARTÃO credit:

- É registrada com status `authorized`, `pre_authorized` ou `failed`;
- É possível realizar o estorno total ou parcial da cobrança;
- É possível realizar a pré-autorização e captura da cobrança.

## Notificação de alteração de status

| objeto        | evento           | descrição                                                                                                                  |
| ------------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------- |
| _transaction_ | _pending_        | Evento enviado quando a cobrança é criada na Malga                                                                         |
| _transaction_ | _pre_authorized_ | Evento enviado quando é pre-autorizado o pagamento, com reserva de saldo                                                   |
| _transaction_ | _authorized_     | Evento enviado quando é autorizado o pagamento                                                                             |
| _transaction_ | _failed_         | Evento enviado quando a cobrança é negada pela instituição financeira antes de ter sido autorizada, sem estorno financeiro |
| _transaction_ | _canceled_       | Evento enviado quando a cobrança é cancelada antes ter sido capturada, não produzindo um estorno financeiro                |
| _transaction_ | _voided_         | Evento enviado quando a cobrança é cancelada após ter sido autorizada, produzindo um estorno financeiro                    |
| _transaction_ | _charged_back_   | Evento enviado quando a cobrança é contestada por fraude                                                                   |

## Exemplo de cobrança

Realize uma cobrança a partir dos dados do cartão de crédito sem tokenizar usando o [Serviço de Charges](/api#operation/charge).

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
      "capture": false,
      "paymentMethod": {
        "paymentType": "credit",
        "installments": 1
      },
      "paymentSource": {
        "sourceType": "card",
        "card": {
          "cardNumber": "4929564637987814",
          "cardCvv": "320",
          "cardExpirationDate": "06/2028",
          "cardHolderName": "JOAO DA SILVA"
        }
      }
    }'

  < HTTP/2 201
  < content-type: application/json; charset=utf-8
  {
    "id": "10ad53ec-76bb-41d2-a0fe-7513f8f50a8f",
    "merchantId": "7f8870a2-71c9-4ef0-a531-82000e00b7e1",
    "clientId": "cc0b1e41-2936-45c5-947f-93995ffcdc00",
    "createdAt": "2021-03-23T15:12:38.379Z",
    "amount": 150,
    "originalAmount": 150,
    "currency": "BRL",
    "statementDescriptor": "Pedido #231 loja joão",
    "capture": false,
    "status": "pre_authorized",
    "paymentMethod": {
      "paymentType": "credit",
      "installments": 1
    },
    "paymentSource": {
      "sourceType": "card",
      "cardId": "148d5db0-f1c3-439f-902d-f1f268086e1d"
    },
    "transactionRequests": [{
        "id": "5e506984-318a-4390-b87a-a3bd9b91d357",
        "updatedAt": "2021-03-23T15:12:42.799Z",
        "createdAt": "2021-03-23T15:12:38.392Z",
        "idempotencyKey": "4271e96e-8c2f-4857-a122-279613bc4747",
        "requestId": null,
        "providerId": "72cc1ff1-5f6e-4eb2-9cc5-6a3a85525e4b",
        "providerType": "STRIPE",
        "amount": 150,
        "authorizationCode": "",
        "authorizationNsu": "1616512362092",
        "responseCode": "20000",
        "requestStatus": "success",
        "requestType": "pre_authorization",
        "responseTs": null
    }]
}
```

## Pré-autorização e captura

No fluxo de pagamentos por cartão é possível realizar uma autorização que reserva o saldo e fica pendente de uma captura posterior, ou você pode já autorizar o pagamento de maneira definitiva. Alguns lojistas optam por não capturar imediatamente a transação para poderem rodar um anti-fraude próprio após reservar o saldo no cartão, e dependendo do resultado da análise confirmar ou não a transação. A vantagem de estornar uma transação que ainda não tenha sido "capturada" é que o estorno é feito imediatamente na fatura do comprador, em alguns casos quando você tenta estornar uma transação já capturada pode levar até 30 dias para constar o estorno na fatura, por isso muitos lojistas optam por não capturar automaticamente e fazer isso manualmente depois.

O flag de `capture` serve para indicar se a transação deve ou não ser capturada.

Na nossa API caso você envie o atributo `capture: false` iremos retornar a transação autorizada (saldo reservado no cartão) com status `pre_authorized` sendo necessário que você faça um request no [Serviço de Captura](/api#operation/captureCharge) para forçar manualmente a captura da transação e liberar para liquidação na conta do provedor. Se a transação não for capturada em 7 dias o adquirente pode desfazer automaticamente a transação.

Caso você envie o atributo `capture: true` iremos retornar a transação autorizada com status `authorized`, não sendo necessário mais nenhuma ação, a transação já fica liberada para liquidação no adquirente e não precisa chamar o Serviço de Captura.

Em nosso dashboard, também é possível _capturar_.

## Estorno Total ou Parcial

No fluxo de pagamento por cartão, é possível realizar um estorno no valor total ou parcial da cobrança. No caso de estorno parcial, o lojista opta por realizar um estorno com valor inferior ao valor da venda, sendo feito o reembolso do valor parcial estornado para o comprador, permanecendo a cobrança como autorizada para liquidação do valor restante para o lojista.

Para realizar um estorno parcial, basta enviar na [requisição de void](/api#operation/refundCharge) um `amount` inferior ao valor original da transação, sendo este `amount` do estorno o valor a ser estornado. Uma vez aprovado a solicitação de estorno parcial junto ao emissor do cartão, o `amount` da transação é então atualizado para o valor residual da transação após o estorno (valor da cobrança - valor estornado),

:::caution
O atributo `originalAmount` do objeto `charge` se mantém como o valor originalmente autorizado na transação, é o valor inicial da transação quando aprovada. Já o atributo `amount` do objeto `charge` é alterado a cada solicitação de estorno parcial, mantendo o saldo residual a receber pelo lojista.
:::

Importante ressaltar que podem ser realizadas diversas requisições de estorno parcial para uma mesma transação, sendo que a cada requisição nosso sistema verifica se o valor a ser estornado, (o valor enviado na requisição de `void`), é menor ou igual ao valor residual da transação, não sendo possível estornar um valor superior. O histórico de requisições de estorno pode ser encontrado na listagem de `transactionRequests` do objeto `charge`.

O status da transação permanecerá como `authorized` ou `pre_authorized` enquanto restar valor a ser pago ao lojista, sendo alterado para `voided` somente quando o estorno zera todo o valor a receber do lojista.

## Reversão automática de um estorno (revert_void)

Os estornos recusados são eventos relativamente raros, mas podem ocorrer em circunstâncias específicas. A causa principal desses casos está muitas vezes relacionada com a abertura de chargebacks pouco tempo antes do estorno ser solicitado. Isso pode resultar em processamentos assíncronos, especialmente em sistemas legados de emissores, nos quais as transações não são tratadas de forma síncrona.

Um dos provedores que implementa essa funcionalidade de reversão de estorno é a ADYEN.

:::info
Embora raro, um reembolso pode falhar após você ter recebido um webhook de REEMBOLSO com success: true. Um webhook de REEMBOLSO bem-sucedido significa que nossas validações foram bem-sucedidas e enviamos a solicitação de reembolso para o esquema do cartão. No entanto, o esquema do cartão ainda pode rejeitar o reembolso. Isso pode acontecer mesmo alguns dias após você enviar a solicitação de reembolso.

Na maioria das vezes, a Adyen pode corrigir o problema, para que o comprador eventualmente receba os fundos. Às vezes, no entanto, você precisa tomar medidas por conta própria. Para saber por que um reembolso pode falhar e o que, se necessário, você precisa fazer em cada caso, consulte Reembolsos falhados.
:::
Veja mais em https://docs.adyen.com/online-payments/refund/#refund-failed

### Como a Malga trata os eventos de reversão de estorno?

Quando a Malga recebe um evento de reversão de estorno por parte de algum provedor, criamos um novo evento requestType `revert_void` dentro da `transactionRequets` da charge. Esse evento é disparado via webhook através do evento `transaction.revert_void`.

Consulte todos [eventos de webhook disponíveis](/docs/webhooks#eventos-suportados-para-notificação-via-webhooks)

Exemplo estorno total

| #   | Tipo de operação                       | Valor da operação | Valor atual da charge (amount) | Status     |
| --- | -------------------------------------- | ----------------- | ------------------------------ | ---------- |
| 1   | Autorização/Captura                    | 100               | 100                            | authorized |
| 2   | Estorno Parcial                        | 10                | 90                             | authorized |
| 3   | Estorno Total                          | 90                | 0                              | voided     |
| 4   | Provedor solicitou reversão do estorno | 10                | 10                             | authorized |

Exemplo estorno parcial

| #   | Tipo de operação                       | Valor da operação | Valor atual da charge (amount) | Status     |
| --- | -------------------------------------- | ----------------- | ------------------------------ | ---------- |
| 1   | Autorização/Captura                    | 100               | 100                            | authorized |
| 2   | Estorno Parcial                        | 10                | 90                             | authorized |
| 3   | Estorno Parcial                        | 20                | 70                             | authorized |
| 4   | Provedor solicitou reversão do estorno | 10                | 80                             | authorized |

## Tratando erros na captura e estorno

As modificações feitas em uma cobrança para fins de estorno total, estorno parcial, ou captura podem retornar com erro no provedor. A fim de garantir o correto tratamento dos casos de exceção retornado pelos provedores nestas operações, nossa API, ao receber erro na captura/estorno, retorna HTTP Status Code 201, sendo criado um novo transactionRequest com o status `failed`, porém não é modificado o status original da transação. **É recomendado que você verifique o status do objeto charge retornado, ou o status do primeiro objeto da lista de transactionRequests para se certificar que a operação foi realizada com sucesso.**

## Cobrando em outras Moedas

A Malga suporta realização de cobranças em diferentes moedas, possibilitando você processar o pagamento na moeda de preferência do comprador.

Caso a moeda informada na cobrança seja diferente da moeda de origem do cartão do comprador, o mesmo pode ter que pagar taxa adicional no seu banco para compras no exterior. O comprador pode ainda ser cobrado uma taxa adicional pelo banco caso o cartão e o lojista estejam em paises diferentes, mesmo a moeda sendo a mesma.

A moeda deve ser enviada no campo `currency` do objeto `charge`, sendo a moeda `BRL` o valor default caso não seja informado pelo solicitante. O código da moeda deve ser enviado no padrão <a href="https://en.wikipedia.org/wiki/ISO_4217" target="blank">ISO 4217</a>, com todas as letras em maiúsculo.

:::info
Importante, a aceitação da moeda é condicionado ao provedor de pagamentos escolhido, somente alguns provedores suportam cobrança em diferentes moedas. Consulte o nosso suporte para saber se seu provedor suporta pagamentos em outras moedas.
:::

Consulte a [tabela de moedas aceitas](/api#section/Tabela-tipos-de-moedas-aceitas)

## Split

Consulte como criar uma transaçao utilizando [split clicando aqui](/docs/split/intro)
