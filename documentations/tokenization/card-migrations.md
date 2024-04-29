---
id: card-migrations
title: Migração de cartões
slug: /tokenization/card-migrations
category: 662fa995369d3500194ad422
---

A Malga oferece a possibilidade de transferir a base de dados sensíveis de cartão de crédito de forma segura entre os Vaults de provedores de pagamentos.

## Possíveis cenários para migração dos dados

### Importando cartões de um provedor existente

Passos para importar a base de cartões de crédito para Malga.

1. Entre em contato com o time de CS (suporte@malga.io) da Malga para obter a última versão do nosso Attestation of Compliance (AoC) do PCI DSS.
2. Contate a solução do provedor (Vault/Gateway) existente e faça um pedido de exportação da base de cartões. Por favor, envie junto o nosso Attestation of Compliance (AoC) do PCI DSS, e a nossa [chave PGP](#chave-pgp). Uma boa mensagem para o provedor seria:

   > Olá [Provedor], Eu gostaria de exportar toda a minha base de cartões de crédito para o Vault da Malga que é certificado no PCI DSS Level 1. Segue em anexo o AoC da Malga.
   >
   > Quando a exportação estiver concluída, por favor envie as informações de como acessar os dados para o email security@malga.io passando o nome do Merchant. O time de Customer Success da Malga pode ajudar com eventuais dúvidas. A chave PGP da Malga se encontra nesta página: https://docs.malga.io/docs/tokenization/card-migrations

3. Passe a informação para o nosso time de CS com o clientId que os dados serão importados.
4. A transferência dos dados depende do processo do gateway e de como eles fazem a exportação.

- Se o Gateway prefere configurar o SFTP do lado deles para transferir, então eles precisam enviar as credenciais para o nosso time (security@malga.io) usando a nossa PGP Key.
- Se o Gateway prefere acessar nosso servidor SFTP, então eles precisam passar a chave pública SSH deles para o nosso time (security@malga.io) configurar um usuário no nosso SFTP, e em seguida passamos essa informação para o Gateway.

Se necessário, nós iremos trabalhar diretamente com o Gateway ou serviço de Vault para ajudar no processo de importação. Nossos clientes são responsáveis por autorizar a exportação dos dados para nosso Vault.

### Exportando dados da Malga para outro provedor

1. Contate o time de Customer Success (CS) da Malga pedindo os dados que vão ser exportados, e passando junto o AoC do PCI DSS do provedor de pagamentos que a base de dados será enviada, a chave PGP que precisa constar no site do provedor de pagamentos, e o endereço de email para envio das credenciais.
2. Assim que nosso time exportar a base de dados, vamos enviar as credenciais do nosso servidor SFTP para o endereço de email informado com as credenciais criptografadas com a chave PGP enviada pelo provedor.
3. Os dados ficam disponíveis no servidor SFTP por 30 dias.

## Formato dos dados

A Malga pode enviar em dois formatos (CSV, JSON), o resultado da importação para os clientes fazerem o de/para com os dados importados do Vault do Gateway para o Vault da Malga.

Exemplo do formato gerado via JSON:

```bash
{
    "malgaCard": {
      "id": "8c0d3c9a-ca87-4bd3-af0d-4eee47cc5a8a",
      "holderName": "NOME DE TESTE",
      "expirationDate": "10/2030",
      "first6Digits": "526607",
      "last4Digits": "8080",
      "status": "active"
    },
    "provider": {
      "reference": "card_12345",
      ... # podemos adicionar campos extras do gateway para eventual comparação
    }
  }
```

Exemplo do formato gerado pelo CSV:

```bash
provider_reference,malga_cardId,holderName,expirationDate,first6Digits,last4Digits,status
card_12345,8c0d3c9a-ca87-4bd3-af0d-4eee47cc5a8a,NOME DE TESTE,10/2030,526607,8080,active
```

A Malga permite incluir novos campos que possa ajudar a fazer a comparação na base do cliente, desde que esteja no arquivo enviado pelo provedor.

A Malga permite incluir/excluir quaisquer dos campos abaixo:

- id
- holderName
- expirationDate
- first6Digits
- last4Digits
- status

Além disso, existe a opção para não exportar os cartões de crédito já expirados.

## Chave PGP

A nossa chave pública PGP usada para transferência é essa encontrada abaixo. Ela se encontra disponível no [Keybase](https://keybase.io/plugpagamentos) e pode ser feita o download via esse [link](../../static/keys/pgp_keys.asc)

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Comment: https://keybase.io/download
Version: Keybase Go 6.0.4 (darwin)

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
KcqyqT/CGTL0boLI+l/4hb+wTMrnkbJ1ZOOwbH4=
=qoYZ
-----END PGP PUBLIC KEY BLOCK-----
```
