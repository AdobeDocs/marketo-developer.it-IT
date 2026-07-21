---
title: Eventi di dati personalizzati
description: Invia eventi personalizzati con l’API JavaScript RTP per Web Personalization, con parametri, dati stringa o array fino a quattro elementi e trigger basati su clic.
feature: Javascript
exl-id: ef7cab9c-3bd0-450e-9247-9324b1e6f9ab
TQID: https://experienceleague.adobe.com/oWDmtMF94xG5HYXeTwkx5zF9PWo98bpwoVB6kAKLYDo
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 241
ht-degree: 3%

---

# Eventi di dati personalizzati

Utilizza questo metodo per inviare eventi personalizzati per il tracciamento e la personalizzazione in tempo reale. Puoi inviare dati di terze parti o attivare un evento personalizzato in base al comportamento del visitatore.

Ogni evento di dati personalizzato viene conteggiato una volta durante la sessione di un visitatore.

Prima di utilizzare l&#39;API Contesto utente, è necessario essere un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
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

L’array di dati personalizzato può contenere fino a quattro elementi. Per inviare più di quattro elementi, chiama ripetutamente l’API Send Event con non più di quattro elementi in ogni chiamata.

```javascript
var customData = {value: ['MyEvent', 'download - example whitepaper']};
rtp('send', 'event', customData);
```

### Invia evento in base al clic del pulsante

Questo esempio invia un evento di dati personalizzato quando un visitatore seleziona il pulsante per scaricare un white paper specifico. RTP può utilizzare l’evento per segmentare tali visitatori in tempo reale.

Il sito web può quindi visualizzare una campagna personalizzata dopo altri due clic. Ad esempio, la campagna può presentare un altro contenuto relativo al white paper scaricato.

```html
<button id="download-whitepaper" onclick="rtp('send', 'event', {value :'download - example whitepaper'})">Download</button>
```
