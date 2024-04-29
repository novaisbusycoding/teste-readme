---
sidebar_position: 1
id: edit
category: 662fa995369d3500194ad422
title: teste8
---

# Editando o fluxo

É possível editar os cards do fluxo, através do ícone de 3 pontos localizado no canto superior direito do card. **Ações disponíveis para cada tipo de card**:

- **Editando um card do tipo provedor**: o card do tipo provedor sempre contém somente 1 provedor, sendo possível:

  - **Substituir** um provedor de cobrança ou antifraude;
  - **Excluir** um provedor de cobrança ou antifraude

- **Editando um card do tipo condicional**: o card do tipo condicional pode conter 1 ou mais condicionais, sendo possível:
  - **Editar** o condicional configurado dentro do menu lateral de edição, removendo ou adicionando novos condicionais, tornando-o um condicional do tipo Combinações;
  - **Excluir** o condicional configurado

#### Opções de edição do card de provedor de cobrança ou antifraude

![Opções de edição do card de provedor de cobrança ou antifraude](/img/flow-guide/editing/edit-provider.png)

#### Opções de remoção do card de provedor de cobrança ou antifraude

![Opções de remoção do card de provedor de cobrança ou antifraude](/img/flow-guide/editing/remove-provider.png)

## Adicionando novos parâmetros

Através do botão :heavy_plus_sign: em cada ramificação, é aberto o menu lateral para inserir novos cards ao fluxo.

A seguir, explicaremos as **regras de utilização de cada parâmetro**:

- **Adicionando provedores de cobrança**

  - No menu lateral, serão exibidos **somente os provedores configurados e ativos** da subconta selecionada, correspondente ao fluxo que está sendo editado;
  - Só é possível adicionar **1 (um) provedor** em cada card no fluxo;
  - É recomendado inserir **até 3 (três) provedores na mesma ramificação** para retentativas;
  - Ao **adicionar um novo provedor no meio de uma ramificação**, os demais elementos são deslocados para a direita, dando prosseguimento ao fluxo conforme as regras configuradas;
  - Ao adicionar **dois provedores de cobrança em sequência**, o fluxo entenderá que o segundo provedor configurado na ramificação será responsável pela retentativa da cobrança, em caso de falha ou reprovação no primeiro provedor, conforme imagem abaixo:

  #### Provedores de cobrança em sequência. Exemplo: Adyen > retentativa > Klap, onde a cobrança sofrerá retentativa na Klap caso haja falha na cobrança pela Adyen

  ![Provedores de cobrança em sequência. Exemplo: Adyen > retentativa > Klap, onde a cobrança sofrerá retentativa na Klap caso haja falha na cobrança pela Adyen](/img/flow-guide/editing/charge-providers.webp)

:::caution
Ao configurar o fluxo com as regras de processamento nos provedores e retentativas, a prioridade até então contida no merchant, passa a considerar **somente** a prioridade do fluxo inteligente.
:::

- **Adicionando antifraude**

  - O provedor antifraude só pode ser adicionado ao fluxo no **método de pagamento de crédito**;
  - Sempre deve haver um **provedor de cobrança após o provedor antifraude** no fluxo, ou seja, o card de antifraude **não pode estar na última posição** de uma ramificação;
  - **Não podem existir dois cards de antifraude** na mesma ramificação;
  - Se a transação for reprovada pela análise de antifraude, o fluxo de cobrança é encerrado.

- **Adicionando condicionais**
  - Podem ser adicionadas **regras condicionais a partir dos dados da cobrança ou de metadados** enviado na cobrança. Confira as regras de utilização de cada condicional no fluxo inteligente aqui [https://docs.malga.io/docs/flow-guide/condicionais]

## Excluindo parâmetros

Ao excluir um card do fluxo inteligente de pagamentos, temos os seguintes comportamentos:

- Quando um **card do tipo condicional** é excluído, todos os cards de ramificações após o card excluído também serão removidas;
- Quando um **card do tipo de provedor de cobrança ou antifraude** é excluído, somente o seu card é removido e o fluxo é movido para a esquerda, preservando os demais cards e a ordem da ramificação.

#### Confirmando a exclusão de um card condicional no fluxo

![Confirmando a exclusão de um card condicional no fluxo](/img/flow-guide/editing/exclude-card.png)
