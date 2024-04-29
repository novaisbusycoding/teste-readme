---
id: transaction-request
title: TransactionRequests
category: 662fa995369d3500194ad422
---

# TransactionRequests

Lista de informações do histórico de transações:

<table>
    <thead>
        <tr>
            <td><strong>CAMPO: OBJETO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>amount: <b>Int</b></td>
            <td>Valor da transação enviada para processamento do provider, em casos de estorno ou captura parcial o valor pode ser diferente do amount original da transação</td>
        </tr>
        <tr>
            <td>authorizationCode: <b>String</b></td>
            <td>Código de autorização retornado pelo provedor</td>
        </tr>
        <tr>
            <td>authorizationNsu: <b>String</b></td>
            <td>Número de autorização retornado pelo provedor</td>
        </tr>
        <tr>
            <td>boleto <a href="/docs/analytics-api/objects/boleto">[Boleto]</a></td>
            <td>Informações sobre a forma de pagamento por Boleto</td>
        </tr>
        <tr>
            <td>createdAt: <b>Datetime</b></td>
            <td>Data de criação da requisição da transação</td>
        </tr>
        <tr>
            <td>id: <b>String</b></td>
            <td>ID de requisição da transação</td>
        </tr>
        <tr>
            <td>idempotencyKey: <b>String</b></td>
            <td>Chave única de referência gerada pela Malga para cada requisição</td>
        </tr>        
        <tr>
            <td>nupay <a href="/docs/analytics-api/objects/nupay">[Nupay]</a></td>
            <td>Informações sobre a forma de pagamento por Nupay</td>
        </tr>
        <tr>
            <td>pix <a href="/docs/analytics-api/objects/pix">[Pix]</a></td>
            <td>Informações sobre a forma de pagamento por Pix</td>
        </tr>
        <tr>
            <td>providerAuthorization <a href="/docs/analytics-api/objects/provider-authorization">[ProviderAuthorization]</a></td>
            <td>Dados adicionais do retorno da autorização do provider no processamento da transação</td>
        </tr>
        <tr>
            <td>providerError <a href="/docs/analytics-api/objects/provider-error">[ProviderError]</a></td>
            <td>Detalhes do erro em caso de falha no processamento da transação</td>
        </tr>
        <tr>
            <td>providerId: <b>String</b></td>
            <td>Identificador do provedor relacionado ao merchant ID</td>
        </tr>
        <tr>
            <td>providerType: <b>String</b></td>
            <td>Nome do provedor</td>
        </tr>
        <tr>
            <td>requestStatus: <b>String</b></td>
            <td>Status of request processing</td>
        </tr>
        <tr>
            <td>requestType: <b>String</b></td>
            <td>Tipo de requisição no provedor</td>
        </tr>
        <tr>
            <td>responseTs: <b>String</b></td>
            <td>Tempo de processamento da requisição</td>
        </tr>
        <tr>
            <td>transactionId: <b>String</b></td>
            <td>Identificador único da transação retornado pelo provider, txId, pode ser usado para recuperar a transação nas APIs ou dashboard do provedor</td>
        </tr>
        <tr>
            <td>updatedAt: <b>Datetime</b></td>
            <td>Data de atualização da requisição da transação</td>
        </tr>
    </tbody>
</table>
