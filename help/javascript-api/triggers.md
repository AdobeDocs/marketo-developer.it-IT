---
title: Trigger
description: Utilizza i trigger RTP in Web Personalization per eseguire funzioni sullo stato rtp, incluso userContextReady, con sintassi, parametri e un esempio di posizione.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
TQID: https://experienceleague.adobe.com/yTz9i4bnD4I0PDAmpnjdD1okYJzd40wriA-2ZzO5OMM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 117
ht-degree: 8%

---

# Trigger

Attiva l&#39;esecuzione delle funzioni quando l&#39;oggetto `rtp` globale raggiunge uno stato specificato.

Prima di utilizzare l&#39;API Contesto utente, è necessario essere un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

## Utilizzo

`rtp('triggerName', function_to_trigger);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| &#39;triggerName&#39; | Obbligatorio | Stringa | Nome del metodo. |
| function_to_trigger | Obbligatorio | Funzione | Funzione di attivazione. |

### Trigger pronto contesto utente

Il trigger `userContextReady` chiama una funzione quando l&#39;oggetto `rtpUserContext` globale è pronto. L’esempio che segue imposta una variabile personalizzata in base alla posizione dell’utente.

```javascript
rtp('userContextReady', function() {
    if (rtpUserContext.location.state == 'CA') {
        rtp('set', 'custom1', 'productA');
    }
});
```
