---
id: boleto
slug: /payment-methods/boleto
title: Boleto
sidebar_label: Boleto
category: 662fa995369d3500194ad422
---

# Boleto

Boleto é um instrumento de cobrança bastante utilizado no Brasil para operações de comercialização de bens e serviços entre empresas e consumidores.

A Malga realiza o papel de intermediadora de pagamentos, porém não realiza a liquidação financeira, sendo necessário conta em uma instituição financeira integrante do sistema financeiro. Dessa forma, recebemos os dados enviados pelo cliente na api de charges e fazemos a comunicação com os emissores e fintechs.

## Os participantes do Boleto

- O recebedor, cliente da Malga, deve possuir uma conta em instituição financeira parceira para receber pagamentos via Boleto;
- Provedor de pagamento, instituição financeira onde o cliente possui conta, é responsável pela emissão e registro de um Boleto com os dados bancários do recebedor e do comprador;
- O pagador, um comprador qualquer que deverá realizar a leitura do código de barras em instituição financeira de sua escolha para realizar o pagamento.

## Fluxo de pagamento

- Para criar uma transação com Boleto, basta o cliente informar o meio de pagamento Boleto na criação de uma cobrança, sua data de expiração e dados que identifiquem o comprador;
- Uma cobrança é registrada no sistema de boletos do Banco Central e fica disponível para pagamento, os dados são retornados no formato de imagem, que pode ser escaneado pelo pagador, e o código, que pode ser copiado pelo pagador;
- O Cliente deve apresentar os dados da cobrança ao pagador que deve efetuar o pagamento dentro do tempo de validade definido na cobrança pelo recebedor;
- O pagador deve realizar o pagamento do Boleto em instituição financeira de sua escolha;
- Após confirmação do pagamento, o provedor de pagamento irá realizar a transferência de fundos para a conta do recebedor;
- Posteriormente o cliente recebedor será notificado de que o pagamento foi efetuado e a cobrança finalizada com sucesso.

## Fluxo de pagamento associado ao customer

- Criar um [customer](https://docs.malga.io/api#operation/createCustomer);
- Criar um novo [charge](https://docs.malga.io/api#operation/charge) informando como `paymentSource` o customer criado previamente, dessa forma iremos utilizar os dados do comprador para geração da cobrança.

## Provedores suportados para cobrança via Boleto

Veja os provedores que suportam cobrança via boleto na nossa [tabela de provedores e meios de pagamento suportados](/api#section/Provedores-e-meios-de-pagamentos-suportados).

A cobrança tipo Boleto quando criada na Malga é registrada com status `pending`, sendo atualizada automaticamente para o status de `authorized` quando somos notificados pela instituição financeira da confirmação do pagamento, esse tempo pode variar de provedor para provedor, não podendo exceder o tempo de expiração definido na criação da cobrança. Caso a confirmação do pagamento não seja reconhecida até o vencimento, a Malga cancela automaticamente a cobrança, atualizando seu status para `failed`.

:::caution
Não é possível realizar estorno para pagamentos do tipo Boleto, também não existe pré-autorização ou captura de cobranças deste tipo.
:::

## Notificação de alteração de status

| objeto        | evento       | descrição                                                                                                                  |
| ------------- | ------------ | -------------------------------------------------------------------------------------------------------------------------- |
| _transaction_ | _pending_    | Evento enviado quando a cobrança é registrada e os dados para pagamento estão disponíveis                                  |
| _transaction_ | _authorized_ | Evento enviado quando é reconhecido a confirmação do pagamento da cobrança                                                 |
| _transaction_ | _failed_     | Evento enviado quando a cobrança é negada pela instituição financeira antes de ter sido autorizada, sem estorno financeiro |

:::caution
É possível que o valor de pagamento de um boleto com status _authorized_ seja diferente do valor original de emissão do mesmo, como em caso de juros ou multa sendo aplicados. Sempre verifique o campo `amount` informado nas atualizações de transações para confirmar o valor pago.
:::

## Testando recebimento de notificação de boleto pago

Para testar sua integração com os webhooks da Malga, você pode desenvolver direto o seu sistema ou utilizar algum serviço como [request.bin](https://requestbin.com/) ou [pipedream.com](https://pipedream.com/) para validar inicialmente os eventos enviados. Basta gerar um novo endpoint nestes serviços e cadastrar um webhook na Malga com o endpoint gerado que todos os eventos enviados ficarão registrados nestes serviços para consulta e debug.

Permitimos no ambiente de sandobx `sandbox-api.malga.io` a atualização manual de transações criadas para os status de authorized, voided e charged_back, dessa forma você consegue criar uma transação e simular o evento desejado.

**Requisição para atualizar manualmente um boleto como pago em sandbox**

```bash
curl --location --request POST 'https://sandbox-api.malga.io/v1/charges/<CHARGE_ID>' \
          --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
          --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
          --header 'Content-Type: application/json' \
          --data-raw '{
              "status": "authorized"
          }'
```

## Exemplo de cobrança

Realize uma cobrança BOLETO a partir dos dados do comprador usando o [Serviço de Charges](/api#operation/charge).

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
      "capture": true,
      "paymentMethod": {
        "paymentType": "boleto",
        "expiresDate": "2022-12-31",
        "instructions": "Instruções para pagamento do boleto",
        "interest": {
          "days": 1,
          "amount": 100
        },
        "fine": {
          "days": 2,
          "amount": 200
        }
      },
      "paymentSource": {
        "sourceType": "customer",
        "customer": {
          "name": "Jose Bonifacio Da Silveira",
          "phoneNumber": "21 98889999099",
          "email": "jose@gmail.com",
          "document": {
              "number": "72912053013",
              "type": "cpf"
          }
        }
      }
}'

< HTTP/2 201
{
  "id": "148d5db0-f1c3-439f-902d-f1f268086e1d",
  "clientId": "cc0b1e41-2936-45c5-947f-93995ffcdc00",
  "createdAt": "2012-06-30 23:59:59 +0000",
  "amount": 150,
  "originalAmount": 150,
  "currency": "BRL",
  "statementDescriptor": "Pedido #231 loja joão",
  "capture": true,
  "status": "pending",
  "paymentMethod": {
    "paymentType": "boleto",
    "expiresDate": "2021-12-31",
    "barcodeData": "412343241324321431241341",
    "barcodeImageUrl": "https://...."
  },
  "paymentSource": {
    "sourceType": "customer",
    "customerId": "1cdcf0c9-eb04-4e43-b9b2-b7a4acdead1f"
  }
}
```
