---
sidebar_position: 1
id: conditional
category: 662fa995369d3500194ad422
title: testea9
---

# Utilizando condicionais

Através do fluxo inteligente, é possível adicionar regras condicionais para o processamento das cobranças. Os condicionais podem utilizar **dados das cobranças**, como o valor, parcelas, bandeira do cartão selecionado, ou **metadados** enviados no campo `metadata` das `charges`.

Através do botão :heavy_plus_sign: nas ramificações do fluxo, é possível adicionar novos condicionais, utilizando do menu na lateral direita, conforme imagem:

#### Menu lateral para adição de condicionais

![Menu lateral para adição de condicionais](/img/flow-guide/conditional/cond-menu.png)

Confira os parâmetros de condicional disponíveis para inserir no fluxo de pagamentos:

- **Balanceador de Distribuição de carga**
- **Bandeira do cartão**
- **Campo personalizado (parâmetro de metadata enviado na cobrança)**
- **Moeda**
- **Número do cartão**
- **Parcelas**
- **Valor**
- **Combinações (diferentes condicionais no mesmo card)**

:::caution
Após cada condicional inserida no fluxo, serão exibidas as **saídas Sim/Não para configuração do que deve acontecer após a aplicação do condicional na cobrança**, podendo ser outra condicional ou provedor de antifraude e cobrança.
:::

## Como configurar cada tipo de condicional

### 1. Balanceador de Distribuição de carga

**Conceito**: Divide o processamento das transações de forma randômica entre duas ramificações, considerando os valores % estabelecidos na configuração (exemplo 40% ramificação 1, 60% ramificação 2);

**Como utilizar**:

- A soma dos valores deve somar 100% da carga
- Aceita até duas ramificações para divisão da carga
- Devem ser adicionados provedores após o condicional para processar cada carga estabelecida
- Podem ser adicionados condicionais após o balanceador
- Não podem ser inseridos valores negativos na distribuição da carga (exemplo - 10%)

#### Configuração do condicional de Balanceador, indicando a distribuição de carga

![Configuração do condicional de Balanceador, indicando a distribuição de carga](/img/flow-guide/conditional/balancer.png)

#### Card de distribuição de carga

![Card de distribuição de carga](/img/flow-guide/conditional/balancer-card.png)

### 2. Bandeira do cartão

**Conceito**: Utiliza a bandeira do cartão [`brand`] como condicional para o processamento da transação.

**Como utilizar**:

- Operadores: Igual a / Diferente de
- Selecionar o operador e a bandeira da condicional

#### Menu lateral para adicionar condicional de Bandeira

![Menu lateral para adicionar condicional de Bandeira](/img/flow-guide/conditional/brand.png)

#### Visualização do condicional de bandeira no fluxo

![Visualização do condicional de bandeira no fluxo](/img/flow-guide/conditional/brand-card.png)

:::info
Caso queira criar um condicional para um **conjunto de bandeiras**, realize a edição do card adicionando mais condicionais com cada bandeira e utilize o conector **OU** entre cada parâmetro.

**Exemplo**: Para criar uma condicional para transações de VISA e MASTERCARD, utilize a configuração “Se bandeira = MASTERCARD **OU** Se bandeira = VISA”.
:::

### 3. Campo personalizado [metadata]

**Conceito**: Utiliza a informação de metadata enviada na cobrança [`paymentFlow.metadata.*`] para uso como condicional para o processamento da transação;

**Como utilizar**:

- Indicar o **nome do atributo** (exatamente igual a `paymentFlow.metadata.nome_do_atributo` enviada na cobrança);
- Indicar o **operador** que quer utilizar na condicional, conforme o tipo de atributo:
  - **Número** [number] - operadores: menor/maior que, menor ou igual/maior ou igual a, igual a/diferente de;
  - **Booleano** [boolean] - operadores: true/false;
  - **Texto** [string] - operadores: igual a/diferente de.
- Indicar em **qual ramificação** o fluxo deve seguir **caso a informação de metadata não seja enviada na cobrança**, se o fluxo seguirá o caminho do condicional "Verdadeiro” ou "Falso"

#### **Como configurar um condicional de campo personalizado:**

#### Campo personalizado - **Tipo número **

![Configuração de condicional de campo personalizado do tipo "Número”](/img/flow-guide/conditional/metadata.png)
![Visualização do card condicional de campo personalizado tipo "Número”](/img/flow-guide/conditional/metadata-card.png)

#### Campo personalizado - Tipo **texto**

![Configuração de condicional de campo personalizado do tipo "Texto"](/img/flow-guide/conditional/metadata-string.png)
![Visualização do card condicional de campo personalizado tipo "Texto](/img/flow-guide/conditional/metadata-string-card.png)

#### Campo personalizado - Tipo **booleano**

![Configuração de condicional de campo personalizado do tipo "Booleano"](/img/flow-guide/conditional/metadata-boolean.png)
![Visualização do card condicional de campo personalizado tipo "Booleano"](/img/flow-guide/conditional/metadata-boolean-card.png)

### 4. Moeda

**Conceito**: Utilizar a moeda da cobrança [`currency`] como condicional para o processamento da transação.

**Como utilizar**:

- Operadores: Igual a / Diferente de
- Selecionar moeda na listagem

#### Configuração de condicional de Moeda

![Configuração de condicional de Moeda](/img/flow-guide/conditional/currency.png)

#### Configuração de condicional de Moeda

![Card condicional de Moeda](/img/flow-guide/conditional/currency-card.png)

### 5. Número do cartão

**Conceito**: Utilizar o número do cartão [`cardBin`] como condicional para o processamento da transação;

**Como utilizar**:

- Operadores: Igual a / Diferente de
- Preencher dado com os seis primeiros dígitos do cartão.

#### Configuração de condicional de Moeda

![Configuração de condicional de Número de cartão](/img/flow-guide/conditional/bin.png)

#### Card condicional de Moeda

![Card condicional de Número de cartão](/img/flow-guide/conditional/bin-card.png)

### 6. Parcelas

**Conceito**: Utilizar o número da parcelas da cobrança [`installments`] como condicional para o processamento da transação.

**Como utilizar**:

- Operadores: menor/maior que, menor ou igual/maior ou igual a, igual a/diferente de;
- Selecionar quantidade de parcelas até 12 vezes

#### Configuração de condicional de Parcelas

![Configuração de condicional de Parcelas](/img/flow-guide/conditional/installments.png)

#### Card condicional de Parcelas

![Card condicional de Parcelas](/img/flow-guide/conditional/installments-card.png)

### 7. Valor

**Conceito**: Utilizar o valor da cobrança [`amount`] como condicional para o processamento da transação;

**Como utilizar**:

- Operadores: menor/maior que, menor ou igual/maior ou igual a, igual a/diferente de;
- Preencher o valor estabelecido como condicional.

#### Configuração de condicional de Valor

![Configuração de condicional de Valor](/img/flow-guide/conditional/amount.png)

#### Card condicional de Valor

![Card condicional de Valor](/img/flow-guide/conditional/amount-card.png)

### 8. Combinações

Através do condicional de Combinações, é possível adicionar mais de uma regra condicional em um card do fluxo inteligente, utilizando o conector E/OU para combinar condicionais.

**Conceito**: Utilizar duas ou mais condicionais no mesmo card do fluxo, como regra para processamento da transação;

**Exemplo de combinação de condicionais**: Se a Bandeira do Cartão é "Mastercard” E Parcelas “Maior que 7 vezes” > processar no provedor x;

**Como utilizar**: Podem ser combinados condicionais de diferentes tipos, com limitação de dois condicionais por operação;

**Não podem ser combinados:**

- Balanceador de distribuição de carga não pode ser combinado com outro condicional no mesmo card;
- Provedores de cobrança e antifraude não podem ser combinados, estando sempre em cards independentes;
- Condicionais utilizando o operador = (igual a) e o conector E com a mesma propriedade;
  - **Exemplo**: bandeira do cartão é igual a (=) MASTERCARD E bandeira do cartão é igual a (=) VISA;
- Condicionais numéricos com operador > (maior) e < (menor) conflitantes;
  **Exemplo**: valor da cobrança é maior (>) que 500 E valor da cobrança é menor (\<) que 100;

#### Configuração do condicional de Combinações - Operador lógico E

![Configuração do condicional de Combinações - Operador lógico E](/img/flow-guide/conditional/and-operator.png)

#### Configuração do condicional de Combinações - Operador lógico OU

![Configuração do condicional de Combinações - Operador lógico OU](/img/flow-guide/conditional/or-operator.png)

#### Exibição do card combinações no fluxo

![Exibição do card combinações no fluxo](/img/flow-guide/conditional/or-operator-card.png)

#### Feedback em caso de inserção inválida - Condicionais iguais

![Feedback em caso de inserção inválida - Condicionais iguais](/img/flow-guide/conditional/operator-error.png)

#### Combinando condicionais de campo personalizado

Você pode realizar a **combinação de mais de um campo personalizado** e nesse cenário, deve se atentar que, caso o metadata não seja enviado na transação, o caminho que o fluxo seguirá é o resultado dos valores default que foram combinados.

**Utilizando o operador lógico** `OU`, o valor que será predominante é o **verdadeiro**. As combinações possíveis são:

| Primeiro valor default | Segundo valor default | Resultado  |
| ---------------------- | --------------------- | ---------- |
| falso                  | falso                 | falso      |
| falso                  | verdadeiro            | verdadeiro |
| verdadeiro             | falso                 | verdadeiro |
| verdadeiro             | verdadeiro            | verdadeiro |

**Utilizando o operador lógico** `E`, o valor que será predominante é o falso. As combinações possíveis são:

| Primeiro valor default | Segundo valor default | Resultado  |
| ---------------------- | --------------------- | ---------- |
| falso                  | falso                 | falso      |
| falso                  | verdadeiro            | falso      |
| verdadeiro             | falso                 | falso      |
| verdadeiro             | verdadeiro            | verdadeiro |

:::info
O **valor default** indica qual caminho o fluxo deve seguir caso o `metadata` (campo personalizado) não seja enviado na transação, podendo ser verdadeiro ou falso.
:::
