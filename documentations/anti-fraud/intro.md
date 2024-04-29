---
id: intro
slug: /anti-fraud/intro
sidebar_label: Introdução
category: 662fa995369d3500194ad422
title: aham1
---

# Antifraude no Cartão de Crédito

Nossa API está preparada para fazer análise de risco em provedores de antifraude, sem a necessidade de integrações adicionais e dentro do próprio fluxo de cobranças do cartão de crédito. Esta avaliação de risco pode ser feita de forma síncrona ou assíncrona e requer algumas **[informações adicionais](/api#operation/charge)** no payload de cobrança para que ela possa ser feita. Os provedores suportados podem ser consultados na **[tabela de provedores antifraude](/api#section/Provedores-de-AntiFraude)**.

## Configurando um provedor Antifraude

Além da configuração no fluxo inteligente, é possível configurar o antifraude da Malga para processar algumas automações de acordo com o retorno da avaliação de risco.

Quando for adicionar um provedor antifraude na Malga é importante estar atento ao modelo de retorno do serviço configurado. Estão disponíveis para orquestração na Malga antifraudes com retorno síncrono e retorno assíncrono. Em alguns casos provedores Antifraude podem exibir comportamentos tanto de retornos síncronos quanto assíncronos, sendo chamados de híbridos. Quando estiver integrando um antifraude de modelo híbrido, você deve considerar os dois ciclos de vida.

### Ciclo de Vida Síncrono

As transações são avaliadas no momento de sua criação e são resolvidas automaticamente. O resultado da avaliação e da transação é determinado na resposta da requisição de criação.

### Ciclo de Vida Assíncrono

O provedor de avaliação de risco retorna um status aguardando uma validação e a transação só é finalizada após a conclusão dos eventos de webhook. Em geral, produtos com ciclo de vida assíncrono também podem ser híbridos (síncrono e assíncrono ao mesmo tempo).

### Opções de configuração de provedores antifraude

| Option             | Descrição                                                                                                                                                          |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `runBeforeCharge`  | Determina se o provedor antifraude é executado antes do provedor de cobrança. Não pode ser habilitado em provedores assíncronos ou híbridos.                       |
| `captureOnApprove` | Determina se a transação deve ser capturada automaticamente caso o provedor Antifraude aprove a transação.                                                         |
| `refundOnReprove`  | Determina se a transação deve ser estornada ou cancelada automaticamente caso o provedor Antifraude reprove a transação.                                           |
| `captureOnError`   | Determina se a transação deve ser capturada caso o provedor antifraude retorne um erro ao ser chamado.                                                             |
| `refundOnError`    | Determina se a transação deve ser estornada caso o provedor antifraude retorne um erro ao ser chamado.                                                             |
| `productType`      | Determina o modo esperado de funcionamento do provedor antifraude. Pode assumir o valor `SYNC` ou `ASYNC`. Para provedores com resposta híbrida, utilizar `ASYNC`. |

Além das configurações de automação, ao configurar um provedor antifraude em um merchant na Malga é possível escolher se você deseja enviar as transações não avaliadas pelo antifraude para o provedor com o objetivo de melhorar a calibragem do mesmo. Esta configuração também pode ser utilizada para calibrar um novo antifraude antes de utilizá-lo em sua orquestração.

### Informações de aparelhos e comportamentos de uso - Fingerprints

É comum que provedores antifraude solicitem informações sobre o aparelho que está sendo utilizado para originar a transação. Recomendamos que você utilize o SDK do provedor configurado em seu fluxo de pagamentos e encaminhe o fingerprint nas requisições de cobrança, com o intuito de manter a qualidade da avaliação de risco e, assim, aumentar sua taxa de aprovação.

:::caution
Alguns provedores antifraude obrigam o uso de Fingerprints em sua integração.
:::

| Provedor  | Fingerprint obrigatório? |
| --------- | ------------------------ |
| Clearsale | SIM                      |

## Retornos possíveis

Quando uma transação é avaliada por um provedor de antifraude é criado uma `transactionRequest` do tipo `anti_fraud`, que pode assumir os status a seguir.

| Status Fraud Analysis | Descrição                                                                              |
| --------------------- | -------------------------------------------------------------------------------------- |
| `pending`             | Sua transação foi enviada para ser avaliada e terá um retorno de avaliação assíncrono. |
| `approved`            | Sua transação foi aprovada pelo provedor antifraude.                                   |
| `reproved`            | Sua transação foi reprovada pelo provedor antifraude.                                  |
