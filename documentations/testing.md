---
title: Testando sua integração
category: 662fa995369d3500194ad422
---

Os números de cartões a serem utilizados no teste podem ser gerados a partir de qualquer site gerador de cartões, devendo o comportamento seguir a seguinte regra:

| Cartão          | Autorizado?       | Status             |
| --------------- | ----------------- | ------------------ |
| Final 0, 1 ou 4 | SIM               | Aprovado           |
| Final 2         | NÃO               | Não autorizado     |
| Final 3         | NÃO               | Cartão expirado    |
| Final 5         | NÃO               | Cartão bloqueado   |
| Final 6         | NÃO               | Timeout            |
| Final 7         | NÃO               | Cartão cancelado   |
| Final 8         | Aleatório SIM/NÃO | Aprovado / Timeout |

:::info
Transações no ambiente de sandbox envolvendo tokenização funcionam normalmente com base em cartões de teste. Cada cartão salvo na tokenização é tratado como um cartão normal, podendo ser usado no processo de simulação.

A informação de Cód.Segurança (CVV) segue a seguinte regra de validação em sanbox, CVV final zero "0" o cartão é validado, para qualquer outro valor o status retorna como não validado.
:::

**Se você conseguiu inserir os dados de cartão na sua aplicação cliente, criar um token, enviar os dados para o seu servidor e criar uma cobrança, então sua integração está finalizada.**

## Captura e estorno

Caso queira testar o cenário de falha na captura ou estorno, o valor **991** deverá ser utilizado como valor para essas duas requisições.

## Antifraude

Caso queira testar cenários antifraude, o valor analisado em sandbox será o final do documento enviado no nó de antifraude (`fraudAnalysis`) na criação da transação. O status retornado para antifraude seguirá as regras dispostas na tabela abaixo:

| Documento            | Autorizado? | Status    | Declined Code                             |
| -------------------- | ----------- | --------- | ----------------------------------------- |
| Final 0              | NÃO         | Pendente  | Nulo                                      |
| Final 1              | SIM         | Aprovado  | Nulo                                      |
| Final 2              | NÃO         | Falha     | Aleatório `timeout` ou `processing_error` |
| Qualquer outro final | NÃO         | Reprovado | Nulo                                      |
