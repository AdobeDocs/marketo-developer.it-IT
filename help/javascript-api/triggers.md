---
title: Trigger
description: Utilizza i trigger RTP in Web Personalization per eseguire funzioni sullo stato rtp, incluso userContextReady, con sintassi, parametri e un esempio di posizione.
feature: Javascript
exl-id: 588836fa-1e4d-41f3-aec5-5cd17eb16071
TQID: 'https://experienceleague.adobe.com/yTz9i4bnD4I0PDAmpnjdD1okYJzd40wriA-2ZzO5OMM'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
    internal-label: Personalization
  - id: dda1332a-c3e0-583f-9d9b-15f1934e0ad3
    internal-label: Javascript
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 8%
---
# Trigger

Attiva l&#39;esecuzione delle funzioni quando l&#39;oggetto `rtp` globale raggiunge uno stato specificato.

Prima di utilizzare l&#39;API Contesto utente, è necessario essere un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

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
