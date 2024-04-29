---
title: 11/03/24 - Pesquisa por NSU no Dashboard
---

## Grandes novidades no Dashboard 🤩

Adicionamos novos recursos ao [Dashboard Malga](https://dashboard.malga.io/app), evoluindo a experiência de gestão financeira através da nossa plataforma. Confira as mudanças:

- Pesquisa de cobranças por NSU;
- Informações de cobranças recusadas no Painel de Dados;
- Exibição de reversão de estorno no Histórico de Operações da cobrança.

### Pesquisa de cobranças por NSU

Agora é possível **buscar Cobranças pelo NSU** (número sequencial único), ID do pedido e ID da cobrança, em uma nova experiência no menu lateral. Cada cobrança possui um NSU referente à autorização no provedor **(authorizationNsu)** retornado para a Malga. 

Para utilizar a pesquisa, basta selecionar o **ícone de pesquisa na Listagem de Cobranças,** selecionando "NSU” em Buscar por e inserindo NSU no campo de busca. A cobrança, se encontrada, será exibida na listagem com a busca aplicada, refletindo o NSU pesquisado, conforme as imagens:

<img
  src="/images/release-notes/2024-03-11/1.png"
  alt="Imagem da Busca por cobranças"
/>

---

<img
  src="/images/release-notes/2024-03-11/2.png"
  alt="Imagem da Busca pelo NSU aplicada"
/>

---

<img
  src="/images/release-notes/2024-03-11/3.png"
  alt="Imagem da Listagem de Cobranças com opções de Busca, Filtros e Exportação"
/>

<Info>
Cada cobrança possui um NSU para identificação da transação realizada no provedor. O NSU <ins>não está presente</ins> em cobranças dos métodos Drip, Nupay e PIX, podendo ser buscadas pelo ID do pedido (se enviado) ou ID da cobrança na Malga.
</Info>

### Cobranças recusadas no Painel de dados

Ajustamos a visão geral de Cobranças no [Painel](https://dashboard.malga.io/app/insights), passando a exibir as **Cobranças Recusadas**, correspondentes ao status falha (failed), com o volume de cobranças e total financeiro de acordo com o período e moeda selecionados. 

No gráfico de **Principais motivos de recusa**, é possível conferir os principais motivos de recusa das cobranças falhadas (<code>declinedCode</code>), conforme as imagens:

<img
  src="/images/release-notes/2024-03-11/4.png"
  alt="Imagem das Cobranças recusadas no Painel de Dados"
/>

### Exibição da requisição de Reversão de Estorno

Adicionamos o evento de **Reversão do estorno** (<code>revert_void</code>) no Histórico de Operações, no detalhe das cobranças, com os detalhes de uma requisição de estorno revertida pelo provedor. No detalhe do Histórico, é possível conferir o provedor responsável pela atualização e os valores da cobrança alterada:

- **Valor atual da cobrança** (no momento da requisição);
- **Valor revertido;**
- **Valor da cobrança após a reversão do estorno pelo provedor.**

<Info>
Quando este cenário acontece, é necessário realizar um **novo estorno parcial ou total na cobrança**, utilizando o botão de **Estornar**, habilitado no detalhe da cobrança que sofreu reversão do estorno anterior;
</Info>

<img
  src="/images/release-notes/2024-03-11/5.png"
  alt="Imagem do Detalhe de uma Cobrança com reversão de estorno"
/>

---

<img
  src="/images/release-notes/2024-03-11/6.png"
  alt="Imagem do Histórico de Operações com reversão de estorno"
/>

Para mais informações sobre a **requisição de reversão de estorno**, confira o [release](https://docs.malga.io/blog/release-2024-03-05) e a [documentação](https://docs.malga.io/docs/payment-methods/credit-card#revers%C3%A3o-autom%C3%A1tica-de-um-estorno-revert_void);

<Tip>
Implementação de **nova tela de erro** por motivo inesperado;
Reorganização dos menus para **busca e filtro de cobranças**;
</Tip>

Em breve, mais novidades em nossa plataforma! 🙌