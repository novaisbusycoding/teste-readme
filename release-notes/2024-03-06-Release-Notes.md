---
title: 06/03/24 - Novas visões no Painel de Dados
---

## Novas visões no Painel de Dados 🔢

Estamos felizes em apresentar as novas visões no [Painel de Dados](https://dashboard.malga.io/app/insights) no Dashboard Malga, confira!

Adicionamos a visão de **Cobranças por status**, com a participação % de cada status de cobrança, pelo maior volume de transações, no período selecionado para exibição no Painel. Passando o mouse em cada status, são exibidas as informações detalhadas de **volume**, **quantidade de cobranças** e **total financeiro** daquele status; 

<img
  src="/images/release-notes/2024-03-06/1.png"
  alt="Imagem do Painel de Dados"
/>

<Info>
Os status exibidos no gráfico de **Cobranças por Status** correspondem à soma das cobranças realizadas nos métodos de pagamento da loja. No gráfico seguinte de **Principais métodos de pagamento**, é exibido o detalhamento de cobranças autorizadas por método de pagamento. Saiba mais sobre os [status das cobranças](https://docs.malga.io/api#operation/getCharges) na Malga em nossa documentação
</Info>

### Novidades nas Cobranças do Dashboard 🤘

Realizamos ajustes nas **nomenclaturas da listagem e detalhes de Cobranças** em nosso Dashboard:

- Data de criação (refere-se ao `created_at` da cobrança);
- ID da cobrança (charge ID);
- Valor original;
- Valor atual.

<img
  src="/images/release-notes/2024-03-06/2.png"
  alt="Imagem da Listagem de Cobranças"
/>

<img
  src="/images/release-notes/2024-03-06/3.png"
  alt="Imagem da Detalhe de Cobrança"
/>

<Note>
- **Adição das parcelas Nupay** no relatório .csv de exportação de cobranças, agora retornando o número de parcelas (installments) das cobranças do método Nupay;
- Correção na exibição das **informações da Transaction Request** no Histórico de Operações dentro do detalhe da cobrança;
- Correção em **traduções** para eng-us, carregamento de ilustrações e melhoria de performance.
</Note>

Siga acompanhando as melhorias e novidades da Malga! Até mais 🤓


