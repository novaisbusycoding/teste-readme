---
sidebar_position: 1
id: flow-dash
category: 662fa995369d3500194ad422
title: teste5
---

# Fluxos Inteligentes no Dashboard Malga

Através do menu de Fluxos Inteligentes no [Dashboard Malga](https://dashboard.malga.io/app/smart-flows), é possível visualizar e fazer alterações em tempo real no fluxo de pagamentos com **autonomia e flexibilidade**, modificando os provedores de **cobrança, antifraude e as regras de orquestração** através do uso de condicionais.

:::info
A página para gerenciamento tem **acesso exclusivo para usuários administradores** da conta. Caso precise realizar alterações nos usuários da sua empresa, contate o nosso time de atendimento.
:::

A seguir, explicaremos o **passo a passo** para visualizar, criar, editar, restaurar e publicar o fluxo de pagamentos através do Dashboard.

#### Fluxo inteligente no Dashboard

![Fluxo inteligente no Dashboard](/img/flow-guide/intro/introducao.png)

### Como funcionam os fluxos inteligentes?

O fluxo inteligente de pagamentos Malga pode ser configurado através através do Dashboard e consultado pela API Flows.

Está disponível para os métodos de cartão de crédito, pix e boleto. Ao acessar o fluxo pela primeira vez no Dashboard, caso você ainda não possua um fluxo inteligente configurado, irá visualizar os provedores e a ordem de prioridade estabelecidos na subconta [`merchant`] com o nome de “Fluxo Padrão”. A partir da criação e publicação de um fluxo inteligente, a ordem de prioridade dos provedores seguirá as regras configuradas. Para visualizar as suas subcontas, provedores e métodos ativos, acesse o [menu Subcontas](https://dashboard.plugpagamentos.com/app/merchants) no Dashboard.

O menu de Fluxos Inteligentes está disponível **somente no ambiente online do Dashboard Malga**.

Cada método de pagamento (crédito, pix e boleto) possui o seu respectivo fluxo inteligente de pagamentos, separados por subconta (`merchant`).

Os fluxos inteligentes permitem a combinação de provedores e condicionais, sendo somente possível adicionar provedor de antifraude ao método crédito.

Você possui a opção de criar uma subconta para testes utilizando o provedor sandbox Malga.

Encontre também mais informações [aqui](https://docs.malga.io/docs/flow-guide).
