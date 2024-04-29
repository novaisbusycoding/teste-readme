---
id: intro
slug: /payment-methods/intro
sidebar_label: Introdução
category: 662fa995369d3500194ad422
title: aham2
---

# Introdução

As cobranças podem ser feitas através de diferentes meios de pagamento, devendo ser informado o meio desejado no objeto `paymentSource` da cobrança. Cada meio de pagamento utiliza um conjunto específico de parâmetros condicionais que são descritos a seguir:

Um `paymentMethod` representa o meio de cobrança escolhido para processar a transação, os métodos suportados são:

| PaymentMethod | Descrição                                                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **credit**    | Cobrança feita a partir de um cartão de crédito, suporta como sourceType: `card`, `token` ou `customer`                         |
| **pix**       | Cobrança feita a partir de QRCode Dinâmico no formato PIX, suporta como sourceType: `customer`                                  |
| **boleto**    | Cobrança feita a partir de Boleto registrado, suporta como sourceType: `customer`                                               |
| **nupay**     | Cobrança feita a partir de Fluxo Transparente da Nupay for Business API, suporta como sourceType: `customer`                    |
| **drip**      | Cobrança feita a partir de PIX parcelando utilizando a Drip como provedor de processamento. Suporta como sourceType: `customer` |

Um `sourceType` representa os dados do pagador que serão utilizados para processar a transação, podendo ser um cartão, um token de cartão temporário, ou dados de um comprador:

| SourceType   | Descrição                                                                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **card**     | Cobrança feita a partir de um cartão tokenizado usando o `cardId` ou com os dados abertos do cartão (cardNumber, cardHolderName, cardCvv, cardExpirationDate) |
| **token**    | Cobrança feita a partir de um token de cartão para cobrança sem recorrência usando `tokenId`                                                                  |
| **customer** | Cobrança feita a partir de comprador cadastrado usando o `customerId`                                                                                         |

## Status da Transação

> Os status possíveis para uma transação na Malga são:

| Status             | Descrição                                                                                    |
| ------------------ | -------------------------------------------------------------------------------------------- |
| **pending**        | Transação criada porém não concluiu processamento                                            |
| **pre_authorized** | Transação pré autorizada com sucesso pendente a captura                                      |
| **authorized**     | Transação autorizada e capturada com sucesso                                                 |
| **failed**         | Transação não autorizada, verifique o erro para identificar o motivo                         |
| **canceled**       | Transação estornada após aprovada porém não capturada                                        |
| **voided**         | Transação estornada após aprovada e capturada                                                |
| **charged_back**   | Transação foi contestada por fraude, não reconhecimento da compra ou devolução da mercadoria |

## Rastreabilidade de cobranças

O atributo `appInfo`, representa informações referentes a rastreabilidade sobre cobrança, tais como
transações do produto, do dispositivo e sobre o sistema operacional,
auxiliando na identificação, mapeamento e rastreabilidade dos recursos gerados nas transações.

:::caution
A passagem desse atributo é opcional. [Saiba mais no serviço de Charges](/api#operation/charge).
:::

| Atributo     | Descrição                                                                     |
| ------------ | ----------------------------------------------------------------------------- |
| **Platform** | Informações sobre produto das transações (checkout-sdk, vtex, magento, etc..) |
| **Device**   | Informações sobre o dispositivo (ios, android, windows, linux)                |
| **System**   | Informações sobre o sistema proprietário de captura do merchant               |
