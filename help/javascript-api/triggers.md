---
title: Trigger
description: Utilizza i trigger RTP in Web Personalization per eseguire funzioni sullo stato rtp, incluso userContextReady, con sintassi, parametri e un esempio di posizione.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
TQID: https://experienceleague.adobe.com/yTz9i4bnD4I0PDAmpnjdD1okYJzd40wriA-2ZzO5OMM
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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 116
ht-degree: 8%

---

# Trigger

Aggiunge la possibilità di attivare funzioni su determinati stati dell&#39;oggetto rtp globale.

Prima di utilizzare l&#39;API Contesto utente, è necessario essere un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

## Utilizzo

`rtp('triggerName', function_to_trigger);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
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
