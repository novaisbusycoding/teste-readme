---
id: intro
slug: /3ds2/intro
sidebar_label: Introdução
category: 662fa995369d3500194ad422
title: aham
---

# 3D Secure 2

3D Secure 2 é um protocolo da indústria de pagamentos que permite prevenir e eliminar o risco de fraude em pagamentos online ao permitir ao pagador se autenticar durante o processo de compra.
O 3D Secure 2 visa também, gerar a melhor experiência para o usuário, possibilitando que o fluxo seja mais fluido para o usuário final.

# Fluxo de cobrança com 3D Secure 2

Para utilizar o 3D Secure 2 em suas transações basta estar credenciado em um provedor que suporte 3D secure 2
[**(Lista de provedores suportados)**](/docs/3ds2/intro#provedores-suportado). Configure um provedor em seu fluxo de pagamentos e adicione o objeto
`threeDSecure2` ao corpo da transação com os respectivos campos requeridos. Verifique nossa [**API**](/api#operation/charge) para detalhes.
Tanto o fluxo sem desafio quanto o fluxo com desafio utilizam redirecionamento para fazer a autenticação do portador do cartão.

<center>
    <small> Nos exemplos a seguir iremos nos referir a 3D Secure 2 como 3DS2 </small>
</center>

- Fluxo sem desafio

  - O portador preenche os dados do cartão no checkout do estabelecimento.
  - O estabelecimento aciona a solução 3DS2 da Malga via API `/charges` para criar a transação com 3DS2.
  - A solução 3DS2 da Malga envia ao provedor de autenticação os dados do comprador.
  - O provedor de autenticação se comunica com a bandeira para processar a autenticação.
  - A bandeira identifica o emissor do cartão, e envia os dados do comprador para análise.
  - A bandeira retorna os dados de autenticação silenciosa ao provedor.
  - A provedor retorna dados para autenticação silenciosa a Malga.
  - A Malga retorna uma transação com status `pending` ao estabelecimento com dados adicionais para autenticação.
  - O estabelecimento utiliza os dados para autenticação por redirecionamento.
  - Para transações autorizadas, o redirecionamento será executado automaticamente pelo browser retornando para o endereço informado em `redirectURL`, não precisando da interação do portador do cartão.
  - O provedor de autenticação retorna o resultado da autorização silenciosa à solução 3DS2 da Malga via webhook.
  - A Malga retorna o resultado da autorização ao estabelecimento via webhook.
  - O estabelecimento informa o resultado do pagamento.

- Fluxo com desafio
  - O portador preenche os dados do cartão no checkout do estabelecimento.
  - O estabelecimento aciona a solução 3DS2 da Malga via API `/charges` para criar a transação com 3DS2.
  - A solução 3DS2 da Malga envia ao provedor de autenticação os dados do comprador.
  - O provedor de autenticação se comunica com a bandeira para processar a autenticação.
  - A bandeira identifica o emissor do cartão, e envia os dados do comprador para análise.
  - O emissor identifica o portador como um possível risco, desta forma, exige o desafio e retorna os dados de Autenticação.
  - A bandeira retorna os dados de autenticação ao provedor.
  - O provedor de autenticação retorna os dados de autenticação à solução 3DS2 da Malga.
  - A Malga retorna uma transação com status `pending` ao estabelecimento com dados adicionais para autenticação.
  - O estabelecimento apresenta a tela da autenticação via lightbox.
  - O portador resolve o desafio na tela do emissor.
  - O emissor retorna o resultado da autenticação à bandeira.
  - A bandeira retorna o resultado da autenticação ao provedor de autenticação.
  - O provedor de autenticação retorna o resultado da autenticação à solução 3DS2 da Malga via webhook.
  - A Malga retorna o resultado da autorização ao estabelecimento via webhook.
  - O estabelecimento informa o resultado do pagamento.

## Diagrama de sequência

![Diagrama 3DS2](/img/3ds2_diagrama.jpg)

## Diagrama de fluxo

![Fluxo 3DS2](/img/3DSecure2.jpg)

### Provedores suportados

Veja os provedores que suportam cobrança com 3DS na nossa [tabela de provedores e meios de pagamento suportados](/api#section/Provedores-e-meios-de-pagamentos-suportados).
