---
sidebar_position: 1
id: validating
category: 662fa995369d3500194ad422
title: teste1
---

# Validação e publicação

## Publicando o fluxo de pagamentos

Após a edição, é necessário realizar a **publicação do fluxo** para torná-lo ativo e iniciar o processamento das transações através dele.

Esta ação acontece através do botão **Publicar**, localizado no canto superior direito da tela.

Durante o processamento da publicação, as seguintes ações acontecem:

1. **Nomear fluxo**: É possível atribuir um nome personalizado ao fluxo;

2. **Validação do fluxo**: são validadas as configurações realizadas pelo usuário, recebendo a mensagem de sucesso ou feedback do que precisa ser ajustado para aprovação do fluxo;

3. **Publicação do fluxo**: caso o fluxo seja aprovado na validação, ele é automaticamente publicado e passa a ser o fluxo ativo para o processamento das transações;

:::info
A **publicação de um novo fluxo** em produção não impacta as transações em andamento pois acontece após a atualização do fluxo anterior ativo. Se houver uma transação em andamento no momento da requisição de mudança, ela será concluída, e a seguinte será processada pelo novo fluxo publicado.
:::

#### Modal para nomear o fluxo e confirmar a publicação

![Modal para nomear o fluxo e confirmar a publicação](/img/flow-guide/validation/publish-modal.png)

#### Validação do fluxo em andamento

![Validação do fluxo em andamento](/img/flow-guide/validation/validation.png)

#### Publicação do fluxo com sucesso (ativo e no ar 🚀)

![Publicação do fluxo com sucesso (ativo e no ar 🚀)](/img/flow-guide/validation/success.png)

#### Feedback de erro na validação

![Feedback de erro na validação](/img/flow-guide/validation/error.png)

:::info
**O que é validado para publicação do fluxo de pagamentos?**

- Se toda ramificação possui pelo menos um provedor de cobrança;
- Se a ramificação com antifraude possui pelo menos um provedor de cobrança depois do antifraude;
- Se nenhuma ramificação não contém dois ou mais cards de antifraude;
- Se todos os provedores inseridos no fluxo estão ativos naquele merchant no momento da publicação.
  :::

## Descartando alterações realizadas

Ao fazer alterações no fluxo, você deve publicá-lo para que as novas regras passem a processar transações. Caso queira sair durante a edição de um fluxo que ainda não está publicado, será exibida uma mensagem **confirmando o descarte das alterações realizadas**. Caso confirme, as alterações realizadas são perdidas e o fluxo ativo permanece inalterado.

#### Confirmação de saída de um fluxo em edição com alterações não publicadas

![Confirmação de saída de um fluxo em edição com alterações não publicadas](/img/flow-guide/validation/exit-modal.png)
