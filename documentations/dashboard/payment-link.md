---
id: payment-link
title: Link de Pagamento
category: 662fa995369d3500194ad422
---

# Link de Pagamento

O Link de Pagamento é uma forma facilitada para gerar cobranças de produtos e serviços. Através da nossa [**Dashboard**](https://dashboard.malga.io/) ou da integração com o [**Serviço de Sessões**](/docs/sessions), você poderá criar um link para pagamento para seu cliente com as opções de PIX, Boleto e Cartão de Crédito. O cliente final, pagador, irá efetuar o pagamento do Link através da experiência do [**Checkout da Malga**](/docs/intro-sdk), configurado com as suas cores e logo.

Durante a criação do Link, você define as configurações do pagamento, especificando a moeda, os provedores que quer utilizar para a cobrança e quais métodos quer disponibilizar para o seu cliente na página de pagamento, entre Pix, Boleto e/ou Cartão de Crédito.

## Link de Pagamento na Dashboard

Através da [**Dashboard**](https://dashboard.malga.io/), você pode:

- Criar um Link de Pagamento;
- Visualizar todos os Links já criados;
- Visualizar os detalhes de um Link;
- Desativar um Link de Pagamento;
- Cancelar um Link;
- Visualizar o status de pagamento de um Link.

![Listagem de Links](/img/payment-link/payment-link-list.png)

![Detalhes de um Link](/img/payment-link/payment-link-details.png)

## Link de Pagamento integrando com Sessões

Para criar um Link de Pagamento via _back-end_ você pode integrar com o [**Serviço de Sessões**](/docs/sessions). Ao criar uma sessão enviando o parâmetro `createLink` verdadeiro, a sessão possuirá um `paymentLink`, que retornará na criação e na rota de detalhes de um Link, esta será a URL em que o ambiente de pagamento estará disponível para você. Você pode ler mais detalhes sobre a criação de uma sessão na [**especificação da API**](/api#operation/createSession).

## Experiência de pagamento

O Checkout do Link de Pagamento utiliza o [**Malga Checkout Full**](/docs/intro-sdk), entregando uma experiência responsiva, customizável, segura e sem necessidade de integração com o _back-end_ da Malga.

:::info
A configuração da empresa ainda precisa ser feita de forma assistida pela equipe da Malga. Caso queira **alterar as suas configurações de cores e logo**, entre em contato com a equipe da Malga.
:::

![Checkout do Link](/img/sdks/malga-checkout-full.png)
