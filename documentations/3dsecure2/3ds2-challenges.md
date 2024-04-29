---
id: 3DS2-challenge
slug: /3dsecure2/fluxos-com-desafios
title: Fluxos com desafio
category: 662fa995369d3500194ad422
---

## Provedor Adyen

### Fluxo de redirecionamento

A resposta do provedor Adyen contém as propriedades `md`, `url`, `paReq` e `termUrl`. Essas propriedades serão utilizadas
no formulário de redirecionamento para autenticação. Esse formulário deve ser exibido ao portador do cartão no checkout no momento do pagamento.

### Utilizando a resposta de autenticação

```json
"authData": {
           "action": "REDIRECT",
           "providerType": "ADYEN",
           "responseType": "AUTHENTICATION",
           "response": {
               "md": "M2RzMi5lZTBiZTgzZTdmZDBlZDk...AkHGAAAA",
               "url": "https://checkoutshopper-test.adyen.com/checkoutshopper/threeDS2.shtml?pspReference=8636772643981111",
               "paReq": "BQABAgB-MNl4GsRJSf5EmZE8guLfFQX7Q5Q...L_dRprriZL72QXGCtIPK-Hd",
               "termUrl": "https://checkoutshopper-test.adyen.com/checkoutshopper/threeDS/return/H4sIAAAAAAAA_..."
           }
       }
```

- Coloque o valor de `url` na propriedade `action` da tag `form`
- Preencha os valores dos inputs `MD`, `PaReq` e `TermUrl` conforme o exemplo
- Quando o formulário for renderizado na tela do cliente, ele irá fazer os redirecionamentos automaticamente.

### Exemplo de lightbox para redirect

```html
<div id="lightbox">
  <form
    id="pageform"
    action="https://checkoutshopper-test.adyen.com/checkoutshopper/threeDS2.shtml?pspReference=8636772643981111"
    method="post"
  >
    <input
      type="hidden"
      name="MD"
      value="M2RzMi5lZTBiZTgzZTdmZDBlZDk...AkHGAAAA"
    />
    <input
      type="hidden"
      name="PaReq"
      value="BQABAgB-MNl4GsRJSf5EmZE8guLfFQX7Q5Q...L_dRprriZL72QXGCtIPK-Hd"
    />
    <input
      type="hidden"
      name="TermUrl"
      value="https://checkoutshopper-test.adyen.com/checkoutshopper/threeDS/return/H4sIAAAAAAAA_y3P2ZJrQAAA0L_JW6qsCQ95QNtGmiFmRL8oxNL2QYf29XNv1Zw_OBDcIBccEIsdClWMwupA4atHQO0QaDnY5NQLc96L_N0F_g4b_UCRzrmRz0BQUdi4bXz4mxfGFPb6BkOFR43aIzNm4v67vjc2sYTFVv4kO-c-NqL56sNIhBl6_VYgkpJaRYH8o9Bx4UZjj3qINkTTe8JjTsDSpwzI66ozCC-dR7ilc_jWylgcZiBoAwNr4_zk9TUx1tmdtJ95ZI2qsyRLE3keJiOmbYxHjOuCMubZuRPuGfUPMSzLyfE1X-He4vtjcDPr67tLbONVotwMcTYIHg4A6crOqq6qCdXHOXgWg0s3i3kL3tmlXc4KOlHxtoL-LcmsLdrAzCSYDNq-UaW1zP_nEx7WoprTFY_DLZ1wkpK1Pk3LFBRlMRdDXtykC3-5XrmLwMsS-89pwdXNPe7eh9vWYiCRdjlkH1bq4dSN49NPQeTDSDqmfhpZVNi_YAmF1MYBAAA"
    />
  </form>
  <script type="text/javascript">
    window.onload = function () {
      document.getElementById("pageform").submit();
    };
  </script>
</div>
```

### Fluxo após o redirecionamento

Ao enviar os dados pelo formulário, se for autenticado pelo emissor, o provedor redirecionará o
mesmo automaticamente para o endereço configurado em `redirectURL`.  
Se a bandeira exigir autenticação, o portador do cartão será redirecionado para o banco do emissor do cartão.
Após a autenticação no banco do emissor, o portador do cartão será redirecionado para o endereço configurado em `redirectURL`.  
O status da transação será enviado via [**webhook**](/docs/webhooks) pela Malga.
Aguarde o resultado do webhook no checkout para exibir o status final da transação.
