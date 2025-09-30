---
title: Trigger
description: Utilizza i trigger RTP in Web Personalization per eseguire funzioni sullo stato rtp, incluso userContextReady, con sintassi, parametri e un esempio di posizione.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 10%

---

# Trigger

Aggiunge la possibilità di attivare funzioni su determinati stati dell&#39;oggetto rtp globale.

Prima di utilizzare l&#39;API Contesto utente, è necessario essere un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

## Utilizzo

`rtp('triggerName', function_to_trigger);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---------------------|-------------------|----------|----------------------|
| &#39;triggerName&#39; | Obbligatorio | Stringa | Nome del metodo. |
| function_to_trigger | Obbligatorio | Funzione | Funzione di attivazione. |

### Trigger pronto contesto utente

Imposta una variabile personalizzata in base alla posizione utente. Questa funzione viene chiamata quando l’oggetto globale &quot;rtpUserContext&quot; è pronto.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
