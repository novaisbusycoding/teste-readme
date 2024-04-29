---
id: intro
slug: /tokenization
title: Introdução
category: 662fa995369d3500194ad422
---

Malga criou este [Serviço de Tokenização](/api#tag/Tokens) para permitir que os dados sensíveis de cartão sejam processados de maneira segura.

Através do [Serviço de Tokenização](/api#tag/Tokens) é possível garantir que os dados sensíveis do cartão (holder name, pan, cvv) não passem pelo seu backend e possam ser enviados para os servidores da Malga diretamente da sua aplicação cliente.

Um token representa dados sensíveis de cartão armazenados de maneira segura na Malga, seguindo as melhores práticas do PCI, sendo os dados enviados direto do seu frontend para os servidores da Malga.

É extremamente recomendado que você realize o processo de tokenização diretamente no lado do cliente, coletando os dados sensíveis de cartão na sua interface e enviando diretamente para o Serviço de Tokenização, a partir de um client token criado para essa funcionalidade, enviando para seu backend o identificador do token gerado que não guarda dados sensíveis e pode ser trafegado normalmente pelos seus servidores.

:::caution

Os Tokens são invalidados após o primeiro uso, não sendo possível armazenar o token gerado para uso futuro em compras recorrentes. Caso queira salvar os dados de um cartão para uso futuro, você deve realizar a criação de um token e na sequência solicitar a criação de um cartão a partir do token criado, dessa forma o cartão ficará armazenado em definitivo no nosso _vault_, servidor de armazenamento seguro de cartões, bastando enviar somente o card id gerado nas transações futuras.

:::

![Fluxo de criação de token](/img/fluxotokennovo.png)

Você pode criar cartões a partir de tokens gerados, sendo possível associar múltiplos cartões a um mesmo comprador, de maneira a possibilitar cobrança futura.

:::caution
O tempo de expiração do token é de 2 horas
:::

## Criando um CardId

Uma vez criado um cartão é gerado um identificador único `cardId` que pode ser armazenado no seu sistema, já que não contém dados sensíveis de cartão, somente um identificador de um cartão salvo de maneira segura no vault da Malga.

A partir de um `cardId` gerado é possível realizar cobranças recorrentes, bastando enviar esse identificador no momento da criação do `charge`.

:::note
O código de segurança (cvv) do cartão, que foi enviado na tokenização, é validado através de um transação de valor zero junto ao emissor do cartão, dessa maneira a Malga consegue validar se os dados tokenizados do cartão são válidos sem que seja necessário realizar uma cobrança efetiva. Após a validação com sucesso dos dados do cartão, o mesmo fica com status:active e disponível para compras futuras, caso contrário, o cartão ficará com status:failed e invalidado, devendo ser gerado um novo token e um novo cartão.
:::

## Status de Cartão

1. O atributo `cvvchecked` identifica se o cartão teve ou não seus dados validados na tokenização. Algumas bandeiras como a AMEX não permitem validação zero dollar, nesse caso o cartão passa a ser criado como `status=active` e `cvvchecked=false`.
2. O status `active` continua identificando se o cartão está disponível para uso no fluxo transacional, porém não é garantia de que um cartão `status=active` retornar sucesso na cobrança, é possível retorno de falha na cobrança por parte dos bancos.
3. O status `failed` continua identificando um cartão que NÃO pode ser usado no fluxo transacional, é um token já identificado como inválido e não poderá ser utilizado.
4. O status `pending` passa a identificar os casos de impossibilidade de validação dos dados do cartão na sua criação. Enquanto o status estiver pending o cartão pode ser usado para criar transação _durante um intervalo de 1 hora_, garantindo maior resiliência do transacional. Um cartão `status=pending` _ao ser utilizado para uma cobrança_, tem seu status atualizado para `active` se os dados do cartão forem válidos, e atualizado para `failed` se os dados do cartão forem inválido. Após 1 hora um cartão criado como `pending` tem seu token expirado e é atualizado automaticamente para `failed` ficando invalidado para uso futuro.

Os status possíveis para um `card` na Malga são:

| Status      | Descrição                                                                                                                                                                                                                                                 |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **active**  | Caso os dados do cartão sejam validados, o status é retornado como active e cvvchecked true                                                                                                                                                               |
| **failed**  | Caso os dados do cartão não sejam validados, o status é retornado como failed e cvvchecked false                                                                                                                                                          |
| **pending** | Caso o Serviço de Validação dos dados do cartão esteja indisponível, o status é retornado como pending e cvvchecked: false; Enquanto o status estiver pending o cartão pode ser usado para criar transação, garantindo maior resiliência do transacional. |

:::tip
É possível enviar o `cvv` do cartão como opcional na requisição de cobrança, sendo útil nos cenários onde o cartão está com status pending ou active, sendo este `cvv` repassado para o provedor aumentando assim as chances de aprovação nos casos onde você pode solicitar o código de segurança no momento da cobrança.
:::

## Bandeiras aceitas

:::caution
**Importante:** Apesar do suporte à tokenização para o método de pagamento voucher, é importante observar limitações próprias em cartões desse método de pagamento, que impossibilitam transações sem CVV. Em decorrência disso, o cartão tokenizado só pode ser utilizado em uma única transação.
:::

As bandeiras aceitas para transações na plataforma da Malga são:

| Bandeira     | Crédito | Débito | Voucher |
| ------------ | ------- | ------ | ------- |
| **Visa**     | SIM     | NÃO    | SIM     |
| **Master**   | SIM     | NÃO    | NÃO     |
| **Amex**     | SIM     | NÃO    | NÃO     |
| **Elo**      | SIM     | NÃO    | SIM     |
| **Diners**   | SIM     | NÃO    | NÃO     |
| **Discover** | SIM     | NÃO    | NÃO     |
| **Jcb**      | SIM     | NÃO    | NÃO     |
| **Vr**       | NÃO     | NÃO    | SIM     |
| **Sodexo**   | NÃO     | NÃO    | SIM     |
