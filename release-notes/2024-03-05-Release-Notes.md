---
title: 05/03/24 - Reversão de uma operação de estorno bem sucedido
---

**Tratar eventos de reversão de um estorno que foi bem sucedido**

Adicionamos um novo tipo de evento que pode ocorrer em transações que tiveram uma solicitação de estorno bem sucedido: a reversão do estorno. Embora seja pouco comum, alguns provedores têm a capacidade de reverter uma transação de estorno bem-sucedida, como é o caso do provedor ADYEN. Conforme indicado pela ADYEN, tais ocorrências não são frequentes, porém são possíveis. Portanto, é necessário que lidemos com essa eventualidade de forma adequada.

Na plataforma Malga, esse evento é tratado como **revert_void**. Se ocorrer uma operação desse tipo, você receberá um webhook com o evento **transaction.revert_void**. Além disso, essa informação estará disponível nas transactionRequests com um novo requestType do tipo **revert_void**.

Para maiores informações sobre essa funcionalidade [acesse aqui](/docs/payment-methods/credit-card#reversão-automática-de-um-estorno-revert_void)

Siga acompanhando as novidades da Malga 🤓🎉