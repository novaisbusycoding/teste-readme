---
title: Tratamento de erros
category: 662fa995369d3500194ad422
---

A Malga disponibiliza uma lista de códigos de erro bem definida para facilitar a identificação dos diferentes tipos de problema que podem acontecer na integração.

Através desta especificação de erros é possível identificar os erros que acontecem quando uma requisição é processada com falha na autorização, ou quando ela é rejeitada por erro funcional. As categorias de erro HTTP possíveis são:

- (2xx) não autorizado;
- (4xx) problema no conteúdo da requisição;
- (5xx) erro interno;
- falha na comunicação ou timeout;

## Código de retorno

Para transações não autorizadas que são recusadas pelo emissor, a Malga fornece uma padronização dos código de retorno e uma recomendação de como você deve proceder com as principais falhas.

### Tabela de erros possíveis `error.type`

| Type                    | HTTP code | Descrição                                                | O que fazer                                    |
| ----------------------- | --------- | -------------------------------------------------------- | ---------------------------------------------- |
| _api_error_             | _500_     | Erro inesperado                                          | Entre em contato com o suporte da Malga        |
| _bad_request_           | _400_     | Erro na validação dos dados enviados no request          | Verifique o detalhe do erro                    |
| _invalid_request_error_ | _400_     | Erro no processamento com base nos dados enviado request | Verifique o detalhe do erro                    |
| _card_declined_         | _200_     | Transação não aprovada pelo provedor                     | Verifique o motivo de rejeição no declinedCode |

### Consulte a [tabela com os códigos de erro](/api#section/Tabela-de-codigo-de-negacao-para-declinedCode) para o `error.declinedCode`
