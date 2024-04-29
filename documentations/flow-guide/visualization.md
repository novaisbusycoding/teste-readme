---
sidebar_position: 1
id: view
category: 662fa995369d3500194ad422
title: visualiation
---

# Visualizando o fluxo

Acessando o menu de [Fluxos Inteligentes](https://dashboard.malga.io/app/smart-flows), deverá ser selecionada a subconta correspondente na listagem (`merchantId` ou `name`) e o método de pagamento para o qual deseja ver o fluxo.

**Cada método de pagamento possui apenas um fluxo de pagamentos ativo por subconta**, estando disponíveis crédito, pix e boleto para configuração do fluxo inteligente.

Para conhecer as funcionalidades de orquestração, [consulte a documentação](https://docs.malga.io/docs/flow-guide/intro).

Para consultar **quais provedores estão ativos** para o seu fluxo, acesse a área de [Subcontas](https://dashboard.malga.io/app/merchants) no Dashboard e confira as contas configuradas para a sua empresa (vinculados ao seu client-ID). Caso queira inserir ou ajustar os provedores na sua subconta, [consulte aqui](https://docs.malga.io/api#section/Provedores-e-meios-de-pagamentos-suportados) os provedores e métodos de pagamento suportados pela Malga, e caso já possua as chaves de acesso ao provedor, utilize a [API de merchants](https://docs.malga.io/api#tag/Merchants) para alterar a sua subconta.

## Visualizando o fluxo de pagamentos

Ao selecionar a subconta e o método de pagamento, é possível visualizar o fluxo ativo, exibido na ordem dos provedores em que uma transação será processada e as regras configuradas para cada ramificação.

Na tela de Fluxos Inteligentes, através dos botões no canto inferior esquerdo, você pode:

#### Funcionalidades disponíveis para navegação

| Botões            | Função                                   | Identificador                                             |
| ----------------- | ---------------------------------------- | --------------------------------------------------------- |
| **Zoom in**       | Aplicar zoom para ampliar a vizualização | ![Zoom in](/img/flow-guide/visualization/zoom_in.png)     |
| **Zoom out**      | Aplicar zoom para reduzir a vizualização | ![Zoom in](/img/flow-guide/visualization/zoom_out.png)    |
| **Centralização** | Centralizar a vizualização               | ![Zoom in](/img/flow-guide/visualization/centralizar.png) |

No cabeçalho do fluxo, está disponível o **nome do fluxo de pagamentos**, que pode ser alterado no momento da publicação, facilitando a identificação futura do seu fluxo no histórico de versões e nas transações processadas através dele.

## Ações disponíveis na visualização

Durante a visualização e edição do fluxo, estão disponíveis as seguintes ações:

| Botões                  | Função                                            | Identificador                                                                       |
| ----------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Criar um novo fluxo** | Botão que inicia a criação de um novo fluxo vazio | <div align="center"><img src="/img/flow-guide/visualization/new.png" /> </div>      |
| **Histórico**           | Consultar histórico de fluxos e restaurar fluxos  | <div align="center"><img src="/img/flow-guide/visualization/historic.png" /> </div> |
| **Publicar fluxo**      | Validar e ativar fluxo configurado                | <div align="center"><img src="/img/flow-guide/visualization/publish.png" /> </div>  |

#### Seleção de subconta e método de pagamento | botões de criar, histórico e publicar

![Seleção de subconta e método de pagamento | botões de criar, histórico e publicar](/img/flow-guide/visualization/menu.png)

## Status do fluxo de pagamentos

Ao visualizar o fluxo, é exibido o seu status, conforme indicado no canto superior da tela.

**Existem quatro status disponíveis do fluxo no Dashboard**:

| Status              | Conceito                                                                                                    | Identificador                                                                              |
| ------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Ativo**           | Fluxo de pagamentos ativo e processando para aquele método de pagamento e subconta                          | <div align="center"><img  src="/img/flow-guide/visualization/status-active.png" /> </div>  |
| **Novo**            | Fluxo de pagamentos criado e ainda sem configurações                                                        | <div align="center"><img  src="/img/flow-guide/visualization/status-new.png" /> </div>     |
| **Em edição**       | Fluxo de pagamentos ativo que possui alterações ainda não publicadas pelo usuário                           | <div align="center"><img  src="/img/flow-guide/visualization/status-edit.png" /> </div>    |
| **Em visualização** | Fluxo de pagamentos aberto a partir do histórico de fluxos e que ainda não foi editado e não está publicado | <div align="center"><img  src="/img/flow-guide/visualization/status-viewing.png" /> </div> |

## Identificando as regras

Na tela de visualização e em cada ramificação exibida do fluxo de pagamentos, estão contidas informações visuais para identificação das regras de processamento da transação.

#### Elementos visuais para identificação das regras de processamento do fluxo

![Elementos visuais para identificação das regras de processamento do fluxo](/img/flow-guide/visualization/viewing.png)

**Confira o que significa cada elemento do fluxo:**

| Nome                                    | Conceito                                                                                                                                                                                                                                                                                                                                     | Identificador                                                                 |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Card**                                | Corresponde a cada elemento do fluxo, podendo ser um card de provedor de cobrança, provedor antifraude ou condicional.                                                                                                                                                                                                                       | ![Card](/img/flow-guide/visualization/card.png)                               |
| **Retentativa**                         | É a ordem na qual a transação será em caso de reprovação ou falha por motivo passível de retentativa. [Confira os motivos de falha ou recusa de uma transação](https://docs.malga.io/docs/flow-guide/intro#tabela-de-causas-de-falha).                                                                                                       | ![Retentativa](/img/flow-guide/visualization/retry.png)                       |
| **Aprovação**                           | Indica a ligação entre o antifraude e o provedor de cobrança, indicando onde a transação será processada após a aprovação do antifraude. **Transações reprovadas no antifraude não são processadas**. Confira [como utilizar o antifraude](https://docs.malga.io/docs/flow-guide/intro#comportamento-de-provedores-antifraude) no seu fluxo. | ![Aprovação](/img/flow-guide/visualization/approve.png)                       |
| **Distribuição de carga [condicional]** | Através do [condicional de distribuição de carga](https://docs.malga.io/docs/flow-guide/intro#distribui%C3%A7%C3%A3o-de-carga), é possível dividir o processamento das transações entre dois provedores de cobrança. O símbolo % indica qual a quantidade de transações será direcionado para cada ramificação.                              | ![Distribuição de carg](/img/flow-guide/visualization/charge-conditional.png) |
| **Operadores Condicionais**             | Após a inserção de um [operador condicional](https://docs.malga.io/docs/flow-guide/intro#tabela-de-operadores-suportados) no fluxo, as ramificações de "Sim” ou "Não” indicam o próximo passo do fluxo para o resultado daquela condicional.                                                                                                 | ![Operadores Condicionais](/img/flow-guide/visualization/operators.png)       |
| **Adição de elementos**                 | Através do :heavy_plus_sign:, é possível adicionar um novo provedor de cobrança, antifraude ou condicional, entre ou após os elementos daquela ramificação.                                                                                                                                                                                  | ![Adição de elementos](/img/flow-guide/visualization/add.png)                 |
