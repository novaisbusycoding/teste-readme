---
sidebar_position: 1
id: examples
category: 662fa995369d3500194ad422
title: teste6
---

# Exemplos de uso

Abaixo descrevemos **três operações através da orquestração do fluxo inteligente** para otimizar os resultados de pagamentos:

## 1. Modificando a ordem dos provedores

Através do Dashboard, é muito simples ajustar a **ordem dos provedores no fluxo de pagamentos**, visando otimizar os custos de adquirência, cumprir acordos negociados e manter a taxa de aprovação. Por exemplo, caso haja um acordo de processamento de volume de vendas mínimo com um determinado provedor [1] visando uma taxa negociada, e sendo necessária a realização de novas tentativas em outros provedores [2 e 3, por exemplo] para garantir a aprovação das transações em caso de erro/recusa na 1a tentativa, é possível ordenar o fluxo com o provedor 1 como prioritário, seguido dos provedores 2 e 3 para novas tentativas. Veja o fluxo de exemplo abaixo:

#### Modificando a ordem dos provedores

![Modificando a ordem dos provedores](/img/flow-guide/examples/examples-providers.png)

## 2. Utilizando condicionais no fluxo de crédito

Através da inserção de **condicionais no fluxo de crédito** (bandeira, valor e parcelas), é possível otimizar os custos de adquirência e aumentar a taxa de aprovação.

Por exemplo, caso você possua uma taxa mais vantajosa para antecipar vendas parceladas em um determinado provedor, é possível utilizar o **condicional de parcelas** para indicar qual provedor deverá processar as transações de acordo com o número de parcelas. Também é possível combinar os **condicionais de parcela e bandeira**, indicando quais provedores devem processar quais transações, conforme abaixo:

#### Utilizando condicionais no fluxo de crédito

![Utilizando condicionais no fluxo de crédito](/img/flow-guide/examples/examples-conditional.png)

## 3. Uso da metadata como condicional para antifraude

Outra forma de otimizar custos através do fluxo inteligente, é reduzindo o volume de transações analisadas pelo antifraude através do envio de campos personalizados (metadata) para serem utilizados como condicionais no processamento da transação.

Por exemplo, se você possui um determinado produto ou evento que não deverá ser analisado pelo antifraude, você pode inserir a identificação correspondente e depois aplicá-la nas configurações.

Também é possível indicar que essa transação corresponde a um cliente final que já realizou outras compras em sua loja e já teve seu perfil validado, não sendo necessária nova avaliação do antifraude. Neste cenário, você pode enviar o código identificador do cliente e indicar que a transação deve seguir pelo fluxo sem antifraude, conforme exemplo abaixo:

#### Uso da metadata como condicional para antifraude

![Uso da metadata como condicional para antifraude](/img/flow-guide/examples/examples-antifraud.png)

Confira mais informações sobre o [uso da metadata em nossa documentação](https://docs.malga.io/docs/flow-guide/intro#exemplo-de-uso-de-metadata).
