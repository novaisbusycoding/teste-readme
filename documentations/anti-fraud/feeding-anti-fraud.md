---
id: feeding-anti-fraud
slug: anti-fraud/feeding
title: Feeding
category: 662fa995369d3500194ad422
---

## Fluxo de Feeding

Com objetivo de **melhorar a taxa de aprovação dos antifraudes de nossos clientes e a perda de vendas legítimas**, a Malga é capaz de fornecer aos provedores de antifraude as informações das transações que **não serão avaliadas**, para que possam fazer composição de base e chegarem a um modelo melhor para avaliação das transações.

O processo começa com a criação de uma transação, que pode ser efetuada por meio de qualquer método de pagamento disponível - transações de **Pix, Boleto, Wallets, BNPL e Voucher, aprovadas ou não, transações de cartão de crédito que não foram avaliadas pelo antifraude configurado (devido, por exemplo, a um fluxo inteligente configurado) aprovadas ou não**.

O processo apenas ocorrerá quando o provedor antifraude estiver configurado na subconta [`merchant`] com a propriedade `options.sendAllCharges` com valor `true`.

Uma vez criada uma transação, se seu status final for `Authorized`, `Failed`, `Canceled` ou `Voided`, os dados relativos à sua criação serão encaminhados ao provedor de antifraude.

O próprio pagamento **não será avaliado**, e sua aprovação na Malga e em outros provedores não estará condicionada ao aceite ou recusa do provedor de antifraude.

Portanto, esse fluxo permite **otimizar a análise de fraudes**, uma vez que o provedor de antifraude, usando os dados de transações com os mais diversos métodos de pagamento, terá um maior volume de dados para usar como referencia e criar um perfil dos seus clientes para uma avaliação mais precisa.

## Como habilitar

Exemplo de payload de criação de provedor de antifraude com `options.sendAllCharges` assumindo valor `true`.

```bash
  curl --location --request POST 'https://sandbox-api.malga.io/v1/charges' \
    --header 'X-Client-Id: <YOUR_CLIENT_ID>' \
    --header 'X-Api-Key: <YOUR_SECRET_KEY>' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "mcc": "4040",
            "status": true,
            "providers": [
                {
                    "name": "SANDBOX",
                    "priority": 1,
                    "credentials": {
                        "type": "SANDBOX",
                        "apiKey": "apiKey_1044838358"
                    }
                },
                {
                    "name": "SANDBOX_ANTIFRAUD",
                    "priority": 1,
                    "credentials": {
                        "type": "SANDBOX_ANTIFRAUD",
                        "apiKey": "apiKey_1044838358"
                    },
                    "options": {
                        "type": "ANTIFRAUD",
                        "sendAllCharges": true,
                        "captureOnApproved": true,
                        "refundOnReproved": true
                    }
                }
            ]
        }'
```
