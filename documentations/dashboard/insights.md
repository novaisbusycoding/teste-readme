---
id: insights
title: Painel de Dados
category: 662fa995369d3500194ad422
---

# Painel de Dados Malga

O **Painel de Dados** do [Dashboard](https://dashboard.malga.io/app/insights) Malga apresenta as informações de vendas e performance dos pagamentos realizados na sua loja, simplificando a gestão e análise das operações.

Através do painel, é possível obter a **visão consolidada das cobranças autorizadas e recusadas** por período, com dados organizados por **status**, divisão por **provedores de pagamento**, **taxa de aprovação** geral e diária, além da listagem dos principais motivos de recusa de transação.

Acesse o **Painel de Dados** [neste link](https://dashboard.malga.io/app/insights).

![Painel de Dados Malga](/img/insights/insights-pt-br.png)

## Filtros por moeda, subconta e período

Através dos filtros de moeda, subconta (merchant) e período, é possível filtrar a visualização dos dados do Painel, conforme descrito abaixo:

- **Moeda**: em casos de vendas em uma única moeda, ela será automaticamente aplicada no painel, e quando houver cobranças em diferentes moedas, é necessário selecionar a moeda desejada usando o atalho na barra superior ou no menu lateral;
- **Subconta**: é possível selecionar uma subconta (Merchant) para visualizar somente as cobranças processadas por aquela conta na Malga, facilitando a visão de operações e filiais;

![Barra superior com atalhos](/img/insights/insights-header-filter-pt-br.png)

![Menu lateral com filtros](/img/insights/insights-sidebar-pt-br.png)

:::info Filtro por período

- O filtro por período corresponde à <ins>data de criação da cobrança</ins> na loja (created_at), estando disponíveis cobranças no ambiente online (produção) e no sandbox (ambiente de testes) do Dashboard;
- É possível <ins>filtrar as cobranças em um intervalo de até 31 dias</ins>, a partir do dia anterior (d-1), e também com os atalhos pré-estabelecidos (este mês, esta semana, mês anterior);
- Estão disponíveis <ins>todos os dados desde a primeira cobrança</ins> processada na loja utilizando a Malga;
- Os dados apresentados correspondem ao <ins>padrão d-1</ins>, ou seja, trazem as informações das cobranças criadas até o dia anterior. Também é possível consultar e extrair os dados em arquivo .csv no menu de [Cobranças](https://dashboard.malga.io/app/charges) e da [Analytics API](https://docs.malga.io/docs/intro-analytics).
  :::

## Visão geral das cobranças

Na parte superior do painel, apresentamos a **Visão geral das cobranças** no período pela sua data de criação (<code>created_at</code>) e moeda (<code>currency</code>), com as seguintes métricas:

- **Total das cobranças** - valor total financeiro e quantidade de cobranças realizadas no período selecionado, correspondendo ao total de vendas ou pedidos da loja;
- **Cobranças autorizadas** - valor total financeiro e quantidade de cobranças pré-autorizadas e autorizadas no período selecionado;
- **Cobranças recusadas** - valor total financeiro e quantidade de cobranças recusadas no período selecionado, contemplando o status falha (failed);
- **Taxa de aprovação geral** - taxa percentual de aprovação das cobranças pré-autorizadas e autorizadas sobre o total de cobranças criadas no período;
- **Taxa de aprovação por dia** - apresentada no gráfico de linha do tempo, representa a taxa de aprovação por dia no período selecionado, contemplando todos os métodos e provedores de pagamento somados. Em cada ponto do gráfico, é possível visualizar a taxa de aprovação, quantidade de cobranças autorizadas, recusadas e o total de cada dia;
- **Cobranças por status** - apresentando o volume % por status das cobranças realizadas no período selecionado. Em cada status, é possível visualizar a quantidade de cobranças e o total financeiro correspondente;

![Visão geral das cobranças no Painel de Dados Malga](/img/insights/insights-charges-resume-pt-br.png)

## Detalhes de cobranças autorizadas

No bloco de **Detalhes de cobranças autorizadas**, é possível conferir os principais métodos e provedores de pagamento responsáveis pelas cobranças da sua loja, conforme descrito abaixo:

### Principais métodos de pagamento

Apresenta as **cobranças autorizadas** (com os status de pré-autorizada e autorizada) por método de pagamento, ordenadas de forma decrescente pelo volume de cobranças nos 6 (seis) principais métodos de pagamento. As métricas representam:

- **Volume %** por método de pagamento sobre o total de cobranças autorizadas;
- **Quantidade** de cobranças autorizadas no método;
- **Total** financeiro do valor monetário autorizado por método;

### Principais provedores

Apresenta as **cobranças autorizadas** (com os status de pré-autorizada e autorizada) por provedor de pagamento, ordenadas de forma decrescente pelo volume dos 6 (seis) principais provedores da sua loja. As métricas representam:

- **Volume %** por provedor de pagamento sobre o total de cobranças autorizadas;
- **Aprovação** representa a taxa geral percentual de autorização de cobranças naquele provedor, correspondendo à soma das cobranças pré-autorizadas e autorizadas sobre o total de cobranças processadas pelo provedor no período selecionado (soma das cobranças pré-autorizadas, autorizadas e com falha);
- **Quantidade** de cobranças autorizadas no provedor;
- **Total** financeiro do valor monetário autorizado por provedor;

![Visão de cobranças autorizadas por método e por provedor de pagamento](/img/insights/insights-providers-list-pt-br.png)

:::info PROVEDORES DE PAGAMENTO EXIBIDOS NO PAINEL
No Painel de Dados do **ambiente de produção** (ambiente online) do Dashboard, são exibidas as cobranças dos provedores de pagamento do fluxo, excluindo o provedor sandbox e antifraude. No **ambiente de testes** (sandbox), são exibidos os dados de cobranças de provedores e sandbox, excluindo o provedor antifraude.
[Confira os **métodos e provedores de pagamento suportados** pela Malga.](/api#section/Provedores-e-meios-de-pagamentos-suportados)
:::

## Detalhes de cobranças recusadas

No bloco de **Detalhes de cobranças recusadas**, são apresentados os principais motivos de recusa das cobranças do método crédito com status recusada (<code>failed</code>), correspondendo ao <code>declinedCode</code> retornado pelo provedor de pagamento e tratado pela Malga.

Os dados são ordenados de forma decrescente pelo motivo de perda com maior volume de cobranças. Ao lado de cada motivo, na listagem de **Principais motivos de recusa**, são exibidas a quantidade de cobranças e o total financeiro monetário das cobranças recusadas por aquele motivo.

![Principais motivos de recusa das cobranças perdidas](/img/insights/insights-denied-reason-list-pt-br.png)

## Tratamento dos motivos de recusa

Para facilitar a análise dos **motivos de recusa**, tratamos a listagem de <code>declinedCode</code> recebidos dos provedores para o Painel, correspondendo às mensagens da tabela abaixo.

[Saiba mais sobre os **motivos de recusa** em nossa documentação.](/api#section/Tabela-de-codigo-de-negacao-para-declinedCode)

| **Motivo de recusa do Painel de Dados**                | **Provider error (Declined code)**                |
| ------------------------------------------------------ | ------------------------------------------------- |
| Cartão bloqueado                                       | blocked_card                                      |
| Cartão cancelado                                       | canceled_card                                     |
| Cartão com a segurança comprometida                    | security_violation                                |
| Cartão com código de segurança inválido                | invalid_cvv / invalid_security_code               |
| Cartão com credenciais inválidas                       | invalid_credential / invalid_credentials          |
| Cartão com data inválida                               | invalid_data                                      |
| Cartão com limite insuficiente                         | insufficient_funds                                |
| Cartão com número inválido                             | invalid_card_number / invalid_number              |
| Cartão com restrição                                   | restricted_card                                   |
| Cartão com restrições identificadas                    | pick_up_card / pickup_card                        |
| Cartão com senha inválida                              | invalid_pin                                       |
| Cartão expirado                                        | expired_card                                      |
| Cartão inválido para o tipo da cobrança realizada      | transaction_not_allowed                           |
| Cartão reportado como perdido                          | lost_card / card_reported_lost                    |
| Cartão reportado como roubado                          | stolen_card / card_reported_stolen                |
| Cobrança com fraude confirmada                         | fraud_confirmed                                   |
| Cobrança com suspeita de fraude                        | fraud_suspect                                     |
| Cobrança internacional não permitida pelo cartão       | service_not_allowed                               |
| Cobrança não permitida pelo cartão                     | not_permitted                                     |
| Cobrança recusada por merchant inválido no provedor    | invalid_merchant                                  |
| Cobrança recusada por quantidade de parcelas inválida  | invalid_installment                               |
| Erro no provedor por falha de contrato                 | processing_error / bad_request                    |
| Erro genérico no provedor                              | generic / try_again                               |
| Erro interno na Malga                                  | internal_error / requestStatus.internal_error     |
| Falha de contato com o emissor do cartão               | issuer_not_available                              |
| Função do cartão incompatível com a cobrança realizada | card_not_supported / card_cannot_make_this_charge |
| Limite máximo atingido nas tentativas da senha         | pin_retry_exceeded / pin_try_exceeded             |
| Motivo de recusa não transmitido pelo provedor         | denied_reason_not_available                       |
| Valor da cobrança não permitido pelo provedor          | invalid_charge_amount / invalid_amount            |

:::info INFORMAÇÕES GERAIS DO PAINEL DE DADOS MALGA

- **Acesso pelo Dashboard:** O painel está disponível no ambiente online (produção) e no ambiente de testes (sandbox), nos idiomas português (pt-br) e inglês (eng-us);
- **Painel exclusivo para usuários administradores:** O painel de dados possui acesso restrito aos administradores da conta da sua empresa, listados no menu Usuários do Dashboard. Avise o nosso time de atendimento caso precise realizar alguma alteração nos administradores e usuários da sua conta;
- **Indisponibilidade do Painel:** Realizamos a atualização diária dos dados entre 1h e 4h (UTC-3), período no qual o Painel de Dados <ins>permanece indisponível para acesso</ins>;
- Através da [Analytics API](https://docs.malga.io/docs/intro-analytics), é possível realizar consultas na base de cobranças e obter as métricas do Painel e o retorno completo sobre o processamentos dos provedores. Saiba mais sobre os cálculos disponíveis na API [neste release](/blog/release-2023-10-06).
  :::
