---
id: errors-treatment
title: Tratamento de Erros
category: 662fa995369d3500194ad422
---

# Tratamento de Erros

## Status 200 - OK

O GraphQL não retorna códigos de status da mesma forma que as APIs REST.

Por exemplo, o GraphQL pode retornar um 200 OK nos casos em que uma API REST retornaria um código de status 5xx ou 4xx.

Devido a esse fato, o GraphQL pode retornar um 200 OK mesmo quando uma ação no nível do aplicativo falha.

## Tratamento de Erros

Para verificar se há erros, você precisa verificar o objeto 'errors' na resposta, que contém informações adicionais sobre o que causou o problema.

Algumas queries também têm um booleano "Sucesso" como campo de retorno, que retornará verdadeiro ou falso, dependendo do sucesso ou falha da ação.

Para verificar os campos de retorno de todas as mutações e consultas, verifique nossa <a href="https://docs.malga.io/docs/error">documentação da API.</a>

## 4XX and 5XX Status Codes

Conforme mencionado acima, na maioria dos casos, o GraphQL retornará uma resposta 200 OK quando uma API REST retornaria um código de status 4xx ou 5xx.

Mas em alguns casos específicos, um código de status 4xx ou 5xx pode ser retornado.

Veja abaixo esses casos e uma explicação deles.

| Status Code | Explicação                                                                       |
| ----------- | -------------------------------------------------------------------------------- |
| 401         | Não autorizado. Um token válido não foi transmitido no cabeçalho da solicitação. |
| 400         | Solicitação inválida (request_without_body)                                      |
| 403         | Proibido.                                                                        |
| 404         | Não encontrado.                                                                  |
| 415         | Tipo de mídia não compatível.                                                    |
| 405         | Método não permitido.                                                            |
| 422         | X-Client-Id é obrigatório.                                                       |

## Cenários de Erros

### Método não permitido

Fazer uma request utilizando o método GET na rota do `/v1/graphql`, onde só é permitido o método POST.

**ROTA**: `/v1/graphql`

| Status Code | Explicação |
| ----------- | ---------- |
| 404         | Not Found  |

### Request sem header

Fazer uma request sem o Header preenchido.

**ROTA**: `/v1/graphql`

| Status Code | Explicação |
| ----------- | ---------- |
| 403         | Forbidden  |

### Request sem Body ou JSON null

Fazer uma request sem o Body preenchido.

**ROTA**: `/v1/graphql`

| Status Code | Explicação             |
| ----------- | ---------------------- |
| 415         | Unsupported Media Type |

### Request com GraphQL Query vazio

Fazer uma request com o JSON não válido ou nulo.

**ROTA**: `/v1/graphql`

| Status Code | Explicação                            |
| ----------- | ------------------------------------- |
| 400         | No GraphQL query found in the request |

### Query incorreto ou inexistente

Fazer uma request com o nome da Query incorreto ou inexistente.

**ROTA**: `/v1/graphql`

| Status Code | Explicação                                                                                                                                                              |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400         | `{"data": null,"errors": [{"message": "Cannot query field 'allCharge' on type 'Query'. Did you mean 'charge' or 'charges'?","locations": [{"line": 2,"column": 3 }]}]}` |

### Query com alguma sintaxe errada

Fazer uma request com alguma sintaxe errada,seja falta de fechar um parente ou colchete.

**ROTA**: `/v1/graphql`

| Status Code | Explicação                                                                                                                     |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------ |
| 400         | `{"data": null,"errors": [{"message": "Syntax Error: Expected Name, found <EOF>.","locations": [{"line": 52,"column": 2 }]}]}` |

### Query sem os campos (fields)

Fazer uma request sem os campos (fields) no Body.

**ROTA**: `/v1/graphql`

| Status Code | Explicação                                                                                                                   |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 200         | `{"data": null,"errors": [{"message": "Syntax Error: Expected Name, found '}'.","locations": [{"line": 10,"column": 7 }]}]}` |

### Query com campo (field) incorreto

Fazer uma request sem os campos (fields) no Body.

**ROTA**: `/v1/graphql`

| Status Code | Explicação                                                                                                                                                  |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200         | `{"data": null,"errors": [{"message": "Cannot query field 'client' on type 'Charge'. Did you mean 'clientId'?","locations": [{"line": 10,"column": 9 }]}]}` |

### Query com campo (field) inexistente

Fazer uma request com algum campo (field) inexistente.
g
**ROTA**: `/v1/graphql`

| Status Code | Explicação                                                                                                                    |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 200         | `{"data": null,"errors": [{"message": "Syntax Error: Expected Name, found <EOF>.","locations": [{"line": 19,"column": 2}]}]}` |
