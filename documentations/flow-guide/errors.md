---
sidebar_position: 1
id: errors
category: 662fa995369d3500194ad422
title: teste7
---

# Tratamento de erros

Durante o **processo de publicação do fluxo de pagamentos**, realizamos a validação das configurações de cada ramificação para garantir que as transações sejam processadas corretamente. Caso algum erro seja identificado, será retornada a mensagem com a orientação da respectiva alteração necessária no fluxo, conforme abaixo:

| Erro identificado                                                                     | Mensagem                                                                                                                      | O que fazer                                                                |
| ------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Ramificação NÃO possui provedor de cobrança**                                       | _Cada caminho do seu fluxo deve ter pelo menos um provedor de cobrança._                                                      | Adicione um provedor de cobrança na ramificação                            |
| **Ramificação apenas com antifraude, SEM provedor de cobrança**                       | _Cada caminho do seu fluxo deve ter pelo menos um provedor de cobrança._                                                      | Adicione um provedor de cobrança na ramificação                            |
| **Ramificação com dois ou mais antifraudes ao mesmo tempo**                           | _Cada caminho do seu fluxo permite apenas um provedor antifraude atuando simultaneamente na transação._                       | Remova um dos antifraudes inseridos na ramificação                         |
| **Ramificação quando há um antifraude SEM provedor de cobrança depois do antifraude** | _Cada caminho do seu fluxo deve ter pelo menos um provedor de cobrança depois de um provedor antifraude._                     | Adicione um provedor de cobrança após o antifraude na ramificação          |
| **Provider utilizado no fluxo NÃO pertencem ao merchant sendo atualizado**            | _O provedor [nome do provedor] não pode ser usado por essa subconta. Por favor, contate nosso suporte para mais informações._ | Entre em contato com o nosso atendimento para realizar a correção no fluxo |
| **Valor de metadata está sendo usado no flow sem um valor/caminho default**           | _Cada metadado do seu fluxo deve ter um valor default. Por favor, contate nosso suporte para mais informações._               | Entre em contato com o nosso atendimento para realizar a correção no fluxo |
| **Fluxo inválido**                                                                    | _Houve um problema ao publicar o fluxo. Por favor, verifique e tente novamente._                                              | Entre em contato com o nosso atendimento para realizar a correção no fluxo |
