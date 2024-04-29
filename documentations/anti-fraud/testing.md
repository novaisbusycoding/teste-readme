---
id: testing
slug: /anti-fraud/testing
title: Testando
category: 662fa995369d3500194ad422
---

# Testando em Sandbox

É possível utilizar o provedor de sandbox da Malga para o controle dos resultados de transações de teste com retornos de antifraude. Este controle é feito através do documento de identidade do `customer` enviado junto aos dados de `fraudAnalysis`.

O número da identidade pode ser gerado a partir de qualquer gerador de identidades, seguindo a seguinte regra:

| Documento            | Autorizado? | Status    | Declined Code                             |
| -------------------- | ----------- | --------- | ----------------------------------------- |
| Final 0              | NÃO         | Pendente  | Nulo                                      |
| Final 1              | SIM         | Aprovado  | Nulo                                      |
| Final 2              | NÃO         | Falha     | Aleatório `timeout` ou `processing_error` |
| Qualquer outro final | NÃO         | Reprovado | Nulo                                      |

:::info
Para simular o fluxo do antifraude assíncrono, use o **[endpoint de alteração de status de antifraude](/api#operation/changeAntifraudStatusTransaction)** e observe as várias alterações possíveis de status na transação, dependendo das `options` cadastradas para o provedor.
:::

Para configurar seu provedor antifraude de sandbox você precisa criar um **merchant** utilizando o provedor de transação `"SANDBOX"` e o provedor antifraude `"SANDBOX_ANTIFRAUD"` e determinar o comportamento automático do antifraude utilizando as flags no objeto de `options`.

# Exemplo de criação de merchant com sandbox de antifraude:

```bash
curl --location --request POST 'https://api.dev.malga.io/v1/merchants' \
--header 'X-Client-Id: <YOUR_CLIENT_ID>' \
--header 'X-Api-Key: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data-raw '
{
    "mcc": "4789",
    "providers": [
        {
            "name": "sandbox",
            "priority": 1,
            "credentials": {
                "type": "SANDBOX", // Sandbox transactions provider
                "apiKey": "<YOUR_API_KEY>"
            }
        },
        {
            "name": "sandbox_antifraud",
            "priority": 1,
            "credentials": {
                "type": "SANDBOX_ANTIFRAUD", // Sandbox anti-fraud provider
                "apiKey": "<YOUR_API_KEY>"
            },
            "options": {
                "type": "ANTIFRAUD",
                "captureOnError": false,
                "refundOnError": false,
                "captureOnApproved": true,
                "refundOnReproved": true
            }
        }
    ]
}'
```
