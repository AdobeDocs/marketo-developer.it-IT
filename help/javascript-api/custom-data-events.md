---
title: Eventi di dati personalizzati
description: Invia eventi personalizzati con l’API JavaScript RTP per Web Personalization, con parametri, dati stringa o array fino a quattro elementi e trigger basati su clic.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 3%

---

# Eventi di dati personalizzati

Questo metodo invia eventi personalizzati per il tracciamento e la personalizzazione in tempo reale. Può essere utilizzato per inviare dati di terze parti o per attivare un evento personalizzato in base al comportamento del visitatore. Gli eventi di dati personalizzati vengono conteggiati una volta nella sessione di un visitatore.

Prima di utilizzare l&#39;API Contesto utente, è necessario diventare un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| `send` | Obbligatorio | Stringa | Azione del metodo. |
| `event` | Obbligatorio | Stringa | Nome del metodo. |
| `customData` | Obbligatorio | Stringa o array | Dati personalizzati. |

## Esempi

### Invia evento utilizzando stringa per dati personalizzati

```javascript
var customData = {value: 'MyEvent'};
rtp('send', 'event', customData);
```

### Invia evento tramite array di stringhe per dati personalizzati

L’array di dati personalizzato può contenere un massimo di quattro elementi.  Se devi inviare più di quattro elementi, chiama ripetutamente l’API Send Event (con un massimo di quattro elementi) fino a quando tutti gli elementi non vengono inviati.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Invia evento in base al clic del pulsante

Marketo personalizza i contenuti del proprio sito web per i visitatori che scaricano un white paper specifico. A tale scopo, acquisiscono il clic del visitatore sul pulsante di download del white paper, che invia un evento di dati personalizzato. RTP segmenta in tempo reale tutti i visitatori che hanno fatto clic sul pulsante Scarica white paper, mostrando a ogni visitatore una campagna personalizzata con un’offerta di 2 clic in un secondo momento. Ciò si ottiene visualizzando un altro contenuto relativo al white paper scaricato.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
