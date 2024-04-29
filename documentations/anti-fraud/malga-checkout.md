---
id: malga-checkout
slug: /anti-fraud/malga-checkout
sidebar_label: Malga Checkout
category: 662fa995369d3500194ad422
title: Malga Checkout
---

# Utilizando o Malga Checkout com antifraude

É possível utilizar o Malga Checkout e enviar os parâmetros de antifraude para o provedor, além disso o nosso checkout pode ser combinado com o SDK de um provedor de antifraude uma solução mais completa.

## Utilizando o Malga Checkout com a Clearsale

Para adicionar o antifraude da Clearsale nos pagamentos feitos através do Malga Checkout é necessário adicionar o `Behavior Analytics SDK`, da própria Clearsale, na sua aplicação `front-end` e utilizar `sessionId` provido por ele nas configurações do Malga Checkout.

- [Adicionando o Behavior Analytics SDK na sua aplicação;](https://api.clearsale.com.br/docs/behavior-analytics/sdk/browser)

Depois, já com o `Behavior Analytics SDK` devidamente implementado, basta enviar o `sessionId` no campo `browserFingerprint`, como mostra o exemplo abaixo:

:::info
O sessionId pode ser um UUID gerado randomicamente a cada nova sessão de usuário e enviado para a Clearsale ou ser gerado pelo próprio SDK dela. Confirma mais detalhes sobre o sessionId no FAQ do [`Behavior Analytics SDK`](https://api.clearsale.com.br/docs/behavior-analytics/faq)
:::

```md
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=5.0"
  />
  <script
    type="module"
    src="https://unpkg.com/@malga-checkout/core@latest/dist/malga-checkout/malga-checkout.esm.js"
  ></script>
  <title>Malga Checkout Components</title>
</head>
<body>
<main>
  <malga-checkout
    sandbox="false"
    public-key="<YOUR_PUBLIC_KEY>"
    client-id="<YOUR_CLIENT_ID>"
    merchant-id="<YOUR_MERCHANT_ID>"
  ></malga-checkout>
</main>

<script>
  const malgaCheckout = document.querySelector('malga-checkout')
  const fingerprint = '<SEU-SESSION-ID>'

  (function (a, b, c, d, e, f, g) {
  a['CsdpObject'] = e; a[e] = a[e] || function () {
  (a[e].q = a[e].q || []).push(arguments)
  }, a[e].l = 1 * new Date(); f = b.createElement(c),
  g = b.getElementsByTagName(c)[0]; f.async = 1; f.src = d; g.parentNode.insertBefore(f, g)
  })(window, document, 'script', '//device.clearsale.com.br/p/fp.js', 'csdp');
  csdp('app', 'seu_app');
  csdp('sessionId', fingerprint);

  malgaCheckout.paymentMethods = {
    pix: {
      expiresIn: 600
    },
    credit: {
      installments: {
        quantity: 1,
        show: true,
      },
      showCreditCard: true
    },
    boleto: {
      expiresDate: "2022-12-31",
      instructions: "Instruções para pagamento do boleto",
      interest: {
        days: 1,
        amount: 100,
      },
      fine: {
        days: 2,
        amount: 200,
      }
    }
  }

  malgaCheckout.transactionConfig = {
    statementDescriptor: '#1 Demonstration Malga Checkout',
    amount: 100,
    description: '',
    orderId: '',
    customerId: '',
    currency: 'BRL',
    capture: false,
    customerId: '6a9d59e7-afb4-4e17-a02e-36d82ed1c11c',
    // Configurando o objeto para antifraude
    fraudAnalysis: {
      browserFingerprint: fingerprint,
      cart: [
        {
          name: "",
          quantity: 0,
          sku: "",
          unitPrice: 0,
          risk: "Low"
        }
      ],
    }
  }

  malgaCheckout.dialogConfig={
    show: true,
    actionButtonLabel: 'Continuar',
    errorActionButtonLabel: 'Tentar novamente',
    successActionButtonLabel: 'Continuar',
    successRedirectUrl: 'https://www.malga.io/',
    pixFilledProgressBarColor: "#2FAC9B",
    pixEmptyProgressBarColor: "#D8DFF0",
  }

  malgaCheckout.addEventListener('paymentSuccess', (data) => {
    // Your specifications here
  })

  malgaCheckout.addEventListener('paymentFailed', (error) => {
    // Your specifications here
  })
</script>
</body>
</html>
```
