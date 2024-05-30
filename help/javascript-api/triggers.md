---
title: "Triggers"
description: "Triggers"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 8%

---


# Triggers

Aggiunge la possibilità di attivare funzioni su determinati stati dell&#39;oggetto rtp globale.

Devi essere un cliente di personalizzazione web e disporre del [Tag RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito prima di utilizzare l’API Contesto utente.

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
