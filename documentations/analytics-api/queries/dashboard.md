---
id: dashboard
title: Dashboard
category: 662fa995369d3500194ad422
---

# Dashboard

O grupo de consultas <b>Dashboard</b> disponibiliza informações para o <a href="https://dashboard.malga.io">Painel de Dados</a> da Malga, que também estão disponíveis para consulta pela Analytics API:

<table>
    <tbody>
        <tr>
            <td><code>providersResume</code></td>
            <td>
                Retorna a taxa de autorização por provedor de pagamento
            </td>
        </tr>
        <tr>
            <td><code>chargeCurrencies</code></td>
            <td>
                Retorna as moedas em que a empresa processa pagamentos
            </td>
        </tr>
    </tbody>
</table>

---

## providersResume

Retorna a taxa de autorização por provedor de pagamento

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>providersResume: ProvidersResume</strong></td>
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

## chargeCurrencies

Retorna as moedas em que a empresa processa pagamentos

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>chargeCurrencies: ChargeCurrencies</strong></td>
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
            <td>currency: <b>String!</b></td>
            <td>Moeda da transação</td>
        </tr>
    </tbody>
</table>

## chargeCurrencies

Retorna as subcontas (merchants) cadastradas

<table>
    <thead>
        <tr>
            <td><strong>QUERY : OBJETO DE RETORNO</strong></td>
            <td><strong>merchants: Merchants</strong></td>
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
            <td>environment: <b>[EnvironmentEnum!]</b></td>
            <td>Ambiente das subcontas, podendo ser <code>SANDBOX</code> (retorna todas as subcontas com provedores de Sandbox), <code>PRODUCTION</code> (retorna todas com provedor de produção) e, não enviando nenhum, todas disponíveis são retornadas</td>
        </tr>
    </tbody>
</table>
