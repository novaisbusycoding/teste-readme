---
id: charges
title: Charges
category: 662fa995369d3500194ad422
---

# Charges

Uma cobrança <code>charge</code> representa um pagamento realizado através da Malga, que pode englobar diversas transações em provedores de cobrança e de antifraude, contendo as informações sobre o seu processamento.

No grupo <b>Charges</b> disponibilizamos as seguintes consultas:

<table>
    <tbody>
        <tr>
            <td><code>allCharges</code></td>
            <td>
                Retorna todas as cobranças
            </td>
        </tr>
        <tr>    
            <td><code>charge</code></td>
            <td>
                Retorna informações de uma cobrança pelo seu identificador único
            </td>
        </tr>
        <tr>    
            <td><code>chargesByDeclinedCode</code></td>
            <td>
                Retorna as cobranças com status failed pelo declined code (motivos de perda)
            </td>
        </tr>
        <tr>    
            <td><code>totalCharges</code></td>
            <td>
                Retorna o número de cobranças e o total financeiro de um período
            </td>
        </tr>
        <tr>    
            <td><code>totalChargesByInterval</code></td>
            <td>
                Retorna o número de cobranças e o total financeiro por um intervalo selecionado
            </td>
        </tr>
        <tr>    
            <td><code>totalChargesByPaymentMethod</code></td>
            <td>
                Retorna o número de cobranças e o total financeiro processados por método de pagamento
            </td>
        </tr>  
        <tr>    
            <td><code>totalChargesByProviderType</code></td>
            <td>
                Retorna o número de cobranças e o total financeiro processados por provedor de pagamento
            </td>
        </tr>                                        
    </tbody>
</table>

---

## allCharges

Retorna todas as cobranças

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>allCharges: ChargeConnection</strong></td>
        </tr>
    </thead>
    <thead>
        <tr>
            <td><strong>ARGUMENTO : TIPO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>first: <b>Int</b></td>
            <td>Retorna os primeiros _n_ elementos da lista. O padrão é 100</td>
        </tr>
        <tr>
            <td>after: <b>String</b></td>
            <td>Retorna os elementos da lista que vêm após o ID global especificado.</td>
        </tr>
        <tr>
            <td>startDate: <b>Datetime</b></td>
            <td>startDate (intervalo de datas, iniciando o range no formato yyyy-MM-dd HH:mm:ss.SSSS)</td>
        </tr>
        <tr>
            <td>endDate: <b>Datetime</b></td>
            <td>endDate (intervalo de datas, finalizando o range no formato yyyy-MM-dd HH:mm:ss.SSSS)</td>
        </tr>
        <tr>
            <td>status: <b>[String!]</b></td>
            <td>Status da transação</td>
        </tr>
        <tr>
            <td>providerType: <b>[String!]</b></td>
            <td>Nome do provedor</td>
        </tr>
        <tr>
            <td>paymentMethod: <b>[String!]</b></td>
            <td>Método de pagamento</td>
        </tr>
        <tr>
            <td>merchantId: <b>[String!]</b></td>
            <td>ID da Subconta</td>
        </tr>
        <tr>
            <td>orderId: <b>[String!]</b></td>
            <td>Identificador único do cliente para conciliação futura</td>
        </tr>
        <tr>
            <td>amount: <b>Int</b></td>
            <td>Valor da transação</td>
        </tr>
        <tr>
            <td>originalAmount: <b>Int</b></td>
            <td>Valor original da transação enviada ao provedor</td>
        </tr>                                
    </tbody>
</table>

## charge

Retorna informações de uma cobrança pelo seu identificador único

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>charge: ChargeConnection</strong></td>
        </tr>
    </thead>
    <thead>
        <tr>
            <td><strong>ARGUMENTO : OBJETO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>id: <b>String</b></td>
            <td>ID da cobrança</td>
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
            <td>idempotencyKey: <b>String</b></td>
            <td>Chave única de referência gerada pela Malga para cada requisição</td>
        </tr>                        
    </tbody>
</table>

## chargesByDeclinedCode

Retorna as cobranças com status failed pelo declined code (motivos de perda)

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>chargesByDeclinedCode: DeclinedCode</strong></td>
        </tr>
    </thead>
    <thead>
        <tr>
            <td><strong>ARGUMENTO : OBJETO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>startDate: <b>Datetime!</b></td>
            <td>startDate (intervalo de datas, iniciando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>endDate: <b>Datetime!</b></td>
            <td>endDate (intervalo de datas, finalizando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>currency: <b>String!</b></td>
            <td>Moeda da transação</td>
        </tr>
        <tr>
            <td>merchantId: <b>[String!]</b></td>
            <td>Lista de subcontas (merchantId's) das transações</td>
        </tr>                 
    </tbody>
</table>

## totalCharges

Retorna o número de cobranças e o total financeiro de um período

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>totalCharges: TotalCharges</strong></td>
        </tr>
    </thead>
    <thead>
        <tr>
            <td><strong>ARGUMENTO : OBJETO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>startDate: <b>Datetime!</b></td>
            <td>startDate (intervalo de datas, iniciando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>endDate: <b>Datetime!</b></td>
            <td>endDate (intervalo de datas, finalizando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>currency: <b>String!</b></td>
            <td>Moeda da transação</td>
        </tr>
        <tr>
            <td>status: <b>[String!]</b></td>
            <td>Status da transação</td>
        </tr>
        <tr>
            <td>merchantId: <b>[String!]</b></td>
            <td>Lista de subcontas (merchantId's) das transações</td>
        </tr>   
    </tbody>
</table>

## totalChargesByInterval

Retorna o número de cobranças e o total financeiro por um intervalo selecionado

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>totalChargesByInterval: TotalCharges</strong></td>
        </tr>
    </thead>
    <thead>
        <tr>
            <td><strong>ARGUMENTO : OBJETO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>startDate: <b>Datetime!</b></td>
            <td>startDate (intervalo de datas, iniciando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>endDate: <b>Datetime!</b></td>
            <td>endDate (intervalo de datas, finalizando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>interval: <b>String!</b></td>
            <td>Intervalo de agrupamento [DAY, WEEK, MONTH, YEAR]</td>
        </tr>
        <tr>
            <td>currency: <b>String!</b></td>
            <td>Moeda da transação</td>
        </tr>
        <tr>
            <td>status: <b>[String!]</b></td>
            <td>Status da transação</td>
        </tr>
        <tr>
            <td>merchantId: <b>[String!]</b></td>
            <td>Lista de subcontas (merchantId's) das transações</td>
        </tr>   
    </tbody>
</table>

## totalChargesByPaymentMethod

Retorna o número de cobranças e o total financeiro processados por método de pagamento

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>totalChargesByPaymentMethod: TotalCharges</strong></td>
        </tr>
    </thead>
    <thead>
        <tr>
            <td><strong>ARGUMENTO : OBJETO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>startDate: <b>Datetime!</b></td>
            <td>startDate (intervalo de datas, iniciando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>endDate: <b>Datetime!</b></td>
            <td>endDate (intervalo de datas, finalizando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>currency: <b>String!</b></td>
            <td>Moeda da transação</td>
        </tr>
        <tr>
            <td>status: <b>[String!]</b></td>
            <td>Status da transação</td>
        </tr>
        <tr>
            <td>merchantId: <b>[String!]</b></td>
            <td>Lista de subcontas (merchantId's) das transações</td>
        </tr>   
    </tbody>
</table>

## totalChargesByProviderType

Retorna o número de cobranças e o total financeiro processados por provedor de pagamento

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>totalChargesByProviderType: TotalCharges</strong></td>
        </tr>
    </thead>
    <thead>
        <tr>
            <td><strong>ARGUMENTO : OBJETO</strong></td>
            <td><strong>DESCRIÇÃO</strong></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>startDate: <b>Datetime!</b></td>
            <td>startDate (intervalo de datas, iniciando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>endDate: <b>Datetime!</b></td>
            <td>endDate (intervalo de datas, finalizando o range no formato yyyy-MM-dd)</td>
        </tr>
        <tr>
            <td>currency: <b>String!</b></td>
            <td>Moeda da transação</td>
        </tr>
        <tr>
            <td>status: <b>[String!]</b></td>
            <td>Status da transação</td>
        </tr>
        <tr>
            <td>merchantId: <b>[String!]</b></td>
            <td>Lista de subcontas (merchantId's) das transações</td>
        </tr>   
    </tbody>
</table>
