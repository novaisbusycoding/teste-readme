---
id: charge
title: Charge
category: 662fa995369d3500194ad422
---

# Charge

Lista de informações de cobrança:

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
            <td>Valor da transação</td>
        </tr>
        <tr>
            <td>capture: <b>Boolean</b></td>
            <td>Se a transação deve ser capturada automaticamente</td>
        </tr>
        <tr>
            <td>clientId: <b>Datetime</b></td>
            <td>Identificação do cliente na Malga</td>
        </tr>
        <tr>
            <td>createdAt: <b>Datetime</b></td>
            <td>Data de criação da cobrança</td>
        </tr>
        <tr>
            <td>currency: <b>String</b></td>
            <td>Moeda da transação</td>
        </tr>
        <tr>
            <td>description: <b>String</b></td>
            <td>Descrição da transação para conciliação futura</td>
        </tr>
        <tr>
            <td>fraudAnalysisMetadata <a href="/docs/analytics-api/objects/fraud-analysis-metadata">[FraudAnalysisMetadata]</a></td>
            <td>Informações sobre os metadados de análise de fraude</td>
        </tr>
        <tr>
            <td>id: <b>Int</b></td>
            <td>Charge ID</td>
        </tr>
        <tr>
            <td>isDispute: <b>Boolean</b></td>
            <td>Se a transação está em disputao</td>
        </tr>
        <tr>
            <td>merchantId: <b>String</b></td>
            <td>ID da Subconta</td>
        </tr>
        <tr>
            <td>orderId: <b>String</b></td>
            <td>Identificador único do cliente para conciliação futura</td>
        </tr>
        <tr>
            <td>originalAmount: <b>Int</b></td>
            <td>Valor original da transação enviada ao provedor</td>
        </tr>
        <tr>
            <td>paymentMethod <a href="/docs/analytics-api/objects/payment-method">[PaymentMethod]</a></td>
            <td>Informações sobre o método de pagamento da cobrança</td>
        </tr>
        <tr>
            <td>paymentSource <a href="/docs/analytics-api/objects/payment-source">[PaymentSource]</a></td>
            <td>Informações sobre a fonte de pagamento</td>
        </tr>
        <tr>
            <td>statementDescriptor: <b>String</b></td>
            <td>Descrição a ser exibida no extrato bancário do comprador</td>
        </tr>
        <tr>
            <td>status: <b>Boolean</b></td>
            <td>Status da transação</td>
        </tr>
        <tr>
            <td>transactionRequests <a href="/docs/analytics-api/objects/transaction-request">[TransactionRequests]</a></td>
            <td>Informações sobre o histórico de transações</td>
        </tr>
    </tbody>
</table>
