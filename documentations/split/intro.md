---
id: intro
slug: /split/intro
sidebar_label: Introdução
category: 662fa995369d3500194ad422
title: aham3
---

# Introdução

O Split de pagamento é um serviço que divide recebíveis de maneira automática, entre duas ou mais partes envolvidas em uma transação,
usado em sua maioria por marketplaces, franquias, aplicativos de delivery, entre outros tipos de negócios que têm a necessidade de
fazer uma divisão de recebíveis.

## Metódos de pagamento suportado pelo Split

Os metódos de pagamento suportado pelo serviço de split são:

| Provedor  | Cartão | Pix | Boleto |
| --------- | ------ | --- | ------ |
| `BRASPAG` | SIM    | NÃO | NÃO    |
| `PAGARME` | SIM    | SIM | SIM    |
| `ZOOP`    | SIM    | NÃO | NÃO    |
| `SANDBOX` | SIM    | SIM | SIM    |

## Recebedores do Split

Para a utilização do serviço de split é necessário existir o seller (recebedor)
**[veja mais sobre o seller](/docs/split/seller)** e informar esse na transação.

O serviço de seller é responsável pela criação e gerenciamento dos parceiros que poderão ser beneficiados
por uma transação splitada, podendo definir o intervalo de recebimento e dados bancários.

## Fluxo de transação com Split

:::caution
É necessário informar pelo menos uma regra de split através do atributo: `splitRules`
e informar o `sellerId` correspondente.
:::

- Criar um seller (recebedor), **[veja mais sobre a criação do seller](/api#operation/postSeller)**.
- Criar uma nova transação (**[charge](/api#operation/charge)**), informando pelo menos uma regra do split por meio do atributo `splitRules`, nele deverá ser informado o `sellerId` do recebedor criado previamente, dessa maneira será utilizado os dados do comprador para gerar a cobrança e realizar a distribuição dos recebíveis automaticamente.
