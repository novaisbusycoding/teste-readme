---
id: export-data
title: Exportando dados
category: 662fa995369d3500194ad422
---

# Exportando dados

Através do **Export Data**, é possível extrair um arquivo em formato .csv com as suas transações, que pode ser configurado para ter os campos do arquivo a ser gerado de acordo com a sua necessidade, e receber o arquivo através de um email.

Através da [**Dashboard**](https://dashboard.malga.io/), você pode:

- Extrair as transações com autonomia, podendo posteriormente importar os dados em alguma ferramenta de planilhas;
- Aplicar filtros e escolher as colunas do arquivo conforme a sua necessidade, como status da transação, provedor, Subconta (merchant), entre outros filtros detalhados.

## Exportando os dados pela Dashboard

1. Entre na sua conta, na área de cobranças, clique no botão "exportar":

![](/img/export-data/charges-list-pt-br.png)

2. Através do menu lateral, você selecionará:

- Período das cobranças a serem exportadas
- Método de pagamento
- Status do pagamento
- Subconta
- Seleção dos dados de Cobrança, Pagamento e Cliente

|               Filtro por status               |                   Filtro por método                   |                Filtro por subconta                |               Seleção dos dados               |
| :-------------------------------------------: | :---------------------------------------------------: | :-----------------------------------------------: | :-------------------------------------------: |
| ![](/img/export-data/status-filter-pt-br.png) | ![](/img/export-data/payment-method-filter-pt-br.png) | ![](/img/export-data/subaccount-filter-pt-br.png) | ![](/img/export-data/row-selection-pt-br.png) |

:::info
Caso o usuário clique no botão de Exportar Cobranças, com filtros já aplicados na listagem de Cobranças, o menu lateral irá exibir os filtros aplicados e dar a opção ao usuário de limpar os filtros ou exportar conforme a visão já filtrada
:::

3. Após a aplicação dos filtros e seleção dos campos, você pode clicar em "Exportar Resultados" e os dados serão enviados para o seu e-mail (o e-mail de destino será o cadastrado na conta que está logada na Dashboard).

## Exportando dados pela API

É possível exportar dados através da API de `reports` também, você pode encontrar detalhes das requisições na [documentação do serviço.](/api#operation/postExportData) Segue um exemplo de uma requisição feita para a API de `reports`:

```bash
curl --location --request POST 'https://api.malga.io/v1/reports' \
--header 'X-Client-Id: <your_x_client_id>' \
--header 'X-Api-key: <your_x_api_key>' \
--header 'accept-language: pt-BR' \
--header 'X-User-Timezone: America/Sao_Paulo' \
--header 'Content-Type: application/json' \
--data-raw '{
  "fields": [
    "card_brand__brand",
    "card__number",
    "card__holder_name",
    "customer__client_id",
    "customer__name",
    "customer__email",
    "customer__phone_number",
    "customer__document_number",
    "customer__customer_adress_id",
    "customer_address__street",
    "customer_address__street_number",
    "customer_address__complement",
    "customer_address__zip_code",
    "customer_address__state",
    "customer_address__city",
    "customer_address__district",
    "customer_address__country",
    "transaction__created_at",
    "transaction__id",
    "transaction__order_id",
    "transaction__merchant_id",
    "transaction__description",
    "transaction__currency",
    "transaction__original_amount",
    "transaction__amount",
    "transaction__installments",
    "transaction__status",
    "transaction__statement_descriptor",
    "transaction__client_id",
    "transaction_request__created_at",
    "transaction_request__payment_method",
    "transaction_request__provider_id",
    "transaction_request__authorization_code",
    "transaction_request__authorization_nsu",
    "transaction_request__network_transaction_id",
    "transaction_request__idempotency_key",
    "nupay__payment_type",
    "session__id",
    "provider__name",
    "merchant__name"
  ],
  "sendTo": "test@test.com",
  "type": "transactions",
  "filters": {
    "transactionRequestPaymentMethod": ["pix"],
    "transactionCreatedAt": {
      "gte": "2022-08-09T03:00:00.000Z",
      "lte": "2023-02-14T02:50:00.000Z"
    }
  }
}'
```

#### Ciclo de vida de uma exportação

![](/img/export-data/state-machine.png)
