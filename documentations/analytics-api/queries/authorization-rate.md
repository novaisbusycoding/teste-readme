---
id: authorization-rate
title: Authorization Rate
category: 662fa995369d3500194ad422
---

# Authorization Rate

A taxa de autorização <code>authorizationRate</code> corresponde ao percentual de cobranças aprovadas em relação ao total de cobranças processadas (somando pré-autorizadas, autorizadas e com falha). Esse indicador representa a conversão bem-sucedida de autorização das cobranças e pode ser filtrado por período, método de pagamento e provedor de cobrança.

No grupo <b>Authorization Rate</b> disponibilizamos as seguintes consultas:

<table>
    <tbody>
        <tr>
            <td><code>totalAuthorizationRate</code></td>
            <td>
                Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed)
            </td>
        </tr>
        <tr>
            <td><code>byInterval</code></td>
            <td>
                Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed) no intervalo selecionado
            </td>
        </tr>
        <tr>
            <td><code>byProviderType</code></td>
            <td>
                Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed) por provedor
            </td>
        </tr>
        <tr>
            <td><code>byPaymentMethod</code></td>
            <td>
                Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed) por método de pagamento
            </td>
        </tr>                
    </tbody>
</table>

---

## totalAuthorizationRate

Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed)

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>totalAuthorizationRate: AuthorizationRate</strong></td>
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

## byInterval

Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed) no intervalo selecionado

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>byInterval: AuthorizationRate</strong></td>
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
            <td>merchantId: <b>[String!]</b></td>
            <td>Lista de subcontas (merchantId's) das transações</td>
        </tr>      
    </tbody>
</table>

## byProviderType

Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed) por provedor

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>byProviderType: AuthorizationRate</strong></td>
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

## byPaymentMethod

Retorna o percentual de cobranças aprovadas (pre_authorized / authorized) dividido pela soma das cobranças aprovadas e recusadas (pre_authorized / authorized / failed) por método de pagamento

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>byPaymentMethod: AuthorizationRate</strong></td>
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
