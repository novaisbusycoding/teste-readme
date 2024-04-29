---
id: intro
title: Introdução
category: 662fa995369d3500194ad422
---

# Introdução

Conforme as empresas crescem a estrutura de pagamentos digitais também cresce e fica mais complexa. Em muitos modelos de negócio o uso de múltiplos provedores precisa ser feito de forma estratégica para otimizar a aprovação de cobranças com características específicas, assim como os custos de operação.

Os Fluxos Inteligentes colocam à disposição **Operadores Condicionais** que podem ser configurados para alterar o comportamento de suas cobranças, como modificar as prioridades de provedores ou ativar um provedor antifraude para melhorar a segurança, no momento de seu processamento.

Com essa funcionalidade, aqui na Malga, as cobranças seguem o comportamento definido no Fluxo Inteligente vinculado ao **Merchant** e **Método de Pagamento** que está sendo utilizado.

Caminhos únicos dentro de um Fluxo Inteligente são chamados de **Ramificações**. No momento, cada **Método de Pagamento** pode ter até um Fluxo Inteligente ativo com até um provedor antifraude e até três provedores de cobrança por ramificação.

# Operadores Condicionais

Os Operadores Condicionais são **separadores de fluxo** que podem ser configurados utilizando comparadores lógicos em conjunto com dados disponíveis nas cobranças para decidir quais provedores serão utilizados no processamento daquela cobrança. Os condicionais podem utilizar dados nativos das cobranças, como o valor, quantidade de parcelas e a bandeira do cartão selecionado ou **metadados** enviados no campo `metadata` das `charges`.

### Tabela de Operadores Suportados

```md
| Operador    | Descrição          | Tipo dos Operandos | Exemplo de Expressão                                         |
| ----------- | ------------------ | ------------------ | ------------------------------------------------------------ |
| **lt (<)**  | Menor que          | `number`           | `transaction.amount < 5000`                                  |
| **gt (>)**  | Maior que          | `number`           | `transaction.amount > 5000`                                  |
| **le (<=)** | Menor ou igual a   | `number`           | `transaction.amount <= 5000`                                 |
| **ge (>=)** | Maior ou igual a   | `number`           | `transaction.amount >= 5000`                                 |
| **eq (=)**  | Igual a            | `number`, `string` | `transaction.installments = 0`                               |
| **ne (!=)** | Diferente de       | `number`, `string` | `transaction.installments != 0`                              |
| **and**     | Operador lógico E  | `boolean`          | `transaction.installments = 0 and transaction.amount < 5000` |
| **or**      | Operador lógico OU | `boolean`          | `transaction.installments = 0 or transaction.amount < 5000`  |
```

## Propriedades de charge disponíveis

As propriedades das `charges` podem ser utilizadas para fazer a composição das regras de processamento de cobrança junto dos Operadores Condicionais. As propriedades atualmente mapeadas para uso nas regras de processamento se encontram na tabela a seguir.

| Propriedade      | Tipo     | Descrição                                                                                                                                                |
| ---------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **amount**       | `number` | Valor da cobrança em centavos                                                                                                                            |
| **currency**     | `string` | Identificador da moeda para processamento da cobrança, formato ISO 4217                                                                                  |
| **cardBin**      | `string` | Os seis primeiros dígitos de um cartão, conhecido como Bank Identification Number ou Issuer Identification Number                                        |
| **brand**        | `string` | Bandeira do cartão _(exclusivo para cartões)_                                                                                                            |
| **installments** | `number` | Quantidade de parcelas para cobrança _(exclusivo para transações do tipo crédito)_                                                                       |
| **metadata.\***  | -        | Propriedade para inserção de campos adicionais na cobrança. Podem ser adicionadas propriedades do tipo `number`, `string`, `boolean`, `object` ou `list` |

## Uso de metadados de transação

É possível enviar **metadados** da cobrança para uso em seus Fluxos Inteligentes. Esta propriedade é uma propriedade especial da cobrança que aceita quaisquer propriedades dentro dela, possibilitando que você processe regras de negócio de um ponto de vista de escolha dos provedores que devem processar aquela transação utilizando o motor da Malga.

```json
{
  [...],
  "paymentFlow": {
    "metadata": {
      "stringProperty": "value for property 1",
      "numericProperty": 10,
      "numericProperty2": 10.5,
      "listProperty": ["element1", "element2"],
      "objectProperty": {
        "subProp1": "value"
      }
      [...]
    }
  },
  [...]
}
```

### Exemplo de uso de metadata

Os dados adicionais de cobrança são extremamente flexíveis e servem para que você possa processar suas regras de negócio utilizando o motor interno da Malga. Por exemplo, caso queira utilizar propriedades que não existem normalmente em uma cobrança Malga, é possível adicionar estas propriedades utilizando o campo de `metadata`, como demonstrado no exemplo a seguir:

![Exemplo de Fluxo Inteligente](/img/payment-flow/payment-flow-example01-ptbr.png)

Neste cenário caso o valor de `metadata.daysToEvent` seja maior do que 60 não é feito o uso de provedores antifraude para proteger a cobrança. Assim como o envio da propriedade arbitrária `daysToEvent`, qualquer propriedade pode ser enviada com uma cobrança para processar regras condicionais.

| Cobrança                                          | Antifraude  | Provedor 1  | Provedor 2  | Provedor 3 |
| ------------------------------------------------- | ----------- | ----------- | ----------- | ---------- |
| daysToEvent = 61; amount = 550; installments = 2  | -           | PagSeguro 2 | PagSeguro 3 | Adyen      |
| daysToEvent = 45; amount = 300; installments = 6  | Clearsale 2 | PagSeguro 1 | PagSeguro 3 | Adyen      |
| daysToEvent = 70; amount = 1200; installments = 3 | Clearsale 1 | PagSeguro 2 | PagSeguro 3 | Adyen      |

:::info
Apesar de ser possível enviar metadados passando uma listas de elementos ou objetos aninhados em **metadata** para processamento ainda não existem operadores compatíveis com estes tipos de propriedades.
:::

## Distribuição de Carga

É possível fazer a distribuição de cobranças em ramificações do Fluxo Inteligente utilizando a propriedade `math/random`. Esta propriedade gera um número aleatório entre 0 e 1 e pode ser acessada em conjunto com operadores lógicos para criar regras de distribuição de carga, como no exemplo a seguir, onde 60% das transações são enviadas pela ramificação **true** e 40% são enviados pela ramificação **false**: `math/random < 0.6`

![Exemplo de Distribuição de Carga](/img/payment-flow/payment-flow-loadbalancing-ptbr.png)

## Comportamento de Provedores de Cobrança

Os Provedores de Cobrança são configurados em uma ordem de prioridade dentro dos Fluxos Inteligentes. Uma vez que uma cobrança não seja devidamente aprovada em um provedor, caso a causa do erro seja retentável, a cobrança segue para ser processada no próximo provedor configurado no Fluxo Inteligente. Caso não haja um novo provedor a ser tentado após o último erro retentável retornado, a cobrança é retornada como `failed`.

:::info
Podem ser configurados até três provedores de cobrança em uma única **ramificação** do seu Fluxo Inteligente.
:::

### Tabela de causas de falha

Causas comuns de falha podem ser vistas [aqui](/api#section/Tabela-de-codigo-de-negacao-para-declinedCode).

## Comportamento de Provedores Antifraude

Em um Fluxo Inteligente é possível configurar Provedores Antifraude para proteger suas transações e seu comportamento pode ser verificado em [**nossa documentação**](/docs/anti-fraud). Dentro de uma única **ramificação** do Fluxo Inteligente é possível configurar apenas um provedor antifraude, ou seja, não é possível processar múltiplos provedores antifraude para uma única cobrança.

Este tipo de configuração permite que você alterne entre múltiplos Provedores Antifraude, ou até desligue este tipo de proteção, utilizando estratégias diferentes para cobranças de características distintas. Estas regras são processadas cobrança a cobrança de acordo com as características configuradas no Fluxo Inteligente.

:::info
As retentativas baseadas nos provedores antifraude acontecem na etapa de **pré autorização**, portanto uma vez pré autorizada em um provedor a transação termina seu ciclo de vida neste provedor até o momento de captura ou falha.
:::
