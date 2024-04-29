---
sidebar_position: 1
id: new
category: 662fa995369d3500194ad422
title: teste2
---

# Começando um novo fluxo

A partir da tela Fluxos Inteligentes, é possível **limpar o fluxo atual ativo e iniciar a configuração de um novo fluxo de pagamentos** para o método de pagamento selecionado. A criação do fluxo inicia através da inserção de um card, que pode ser um provedor de cobrança, antifraude ou uma condicional, seguindo o fluxo abaixo:

#### 1. Botão de iniciar novo fluxo

![Botão de iniciar novo fluxo](/img/flow-guide/new/new-button.png)

#### 2. Escolhendo parâmetros

![Escolhendo parâmetros](/img/flow-guide/new/add-param.png)

#### 3. Seleção do primeiro parâmetro a ser adicionado no fluxo

![Seleção do primeiro parâmetro a ser adicionado no fluxo](/img/flow-guide/new/select-param.png)

## Parâmetros disponíveis

Confira os **parâmetros disponíveis** para inserir no fluxo de pagamentos:

1. Provedores de cobrança;
2. Provedor antifraude;
3. Condicionais.

| Tipo de Parâmetro                                           | Opções disponíveis                                                                                                                                                                                                                                | Exemplo de card                                                                                                                                                                                             |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Provedor de cobrança <br />**                             | [Provedores disponíveis Malga](https://docs.malga.io/en/api#section/Provedores-e-meios-de-pagamentos-suportados)                                                                                                                                  | ![Card de provedor](/img/flow-guide/new/provider-card.png)                                                                                                                                                  |
| **Provedor antifraude <br /> [1 provedores de antifraude]** | Disponível para crédito <br /> &bull; Clearsale                                                                                                                                                                                                   | ![Card de antifraude](/img/flow-guide/new/antifraud-card.png)                                                                                                                                               |
| **Condicionais <br /> [7 condicionais + combinações]**      | Condicionais para crédito, pix e boleto <br /> &bull; Balanceador de Distribuição de carga <br /> &bull; Campo personalizado (metadata) <br /> &bull; Moeda <br /> &bull; Valor <br /> &bull; Combinações (diferentes condicionais no mesmo card) | ![Condicionais](/img/flow-guide/new/cond-card.png) <br /> Somente para crédito <br /> &bull; Bandeira - somente método cartão <br /> &bull; Número do cartão <br /> &bull; Parcelas - somente método cartão |

:::info
Os provedores de cobrança e antifraude disponíveis para adição no fluxo correspondem à conta previamente configurada e escolhida no menu de subcontas. Caso queira utilizar outros provedores, entre em contato com o nosso time de atendimento.
:::

#### Menu lateral para adição/edição de provedores de cobrança e antifraude do fluxo

![Menu lateral para adição/edição de provedores de cobrança e antifraude do fluxo](/img/flow-guide/new/provider-param.png)

#### Menu lateral para adição/edição de condicionais do fluxo

![Menu lateral para adição/edição de condicionais do fluxo](/img/flow-guide/new/conditional-param.png)
