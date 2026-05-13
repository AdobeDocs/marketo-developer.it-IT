---
title: Contesto utente
feature: REST API
description: Scopri come abilitare e utilizzare l’API Contesto utente di Marketo RTP per impostare variabili personalizzate, leggere i dati degli utenti in più visite e tenere traccia delle campagne visualizzate e su cui è stato fatto clic.
exl-id: b8daace2-07a5-4621-aa3a-03fa9f66ea73
TQID: https://experienceleague.adobe.com/Ph0Tw-C9jzWaR4bYyUIXyzzoa2yjHQk2gt6tNA8H2mA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2:
  - id: a1d50dda-6d94-4e16-8c30-5eb7181c4650
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 296
ht-degree: 5%

---

# Contesto utente

L’API JavaScript per il contesto utente espone i dati a livello di utente e visitatore in più sessioni per abilitare la funzionalità di personalizzazione avanzata utilizzando dati e comportamenti storici degli utenti. L’API non si limita alla lettura dei dati, ma espone variabili personalizzate che consentono di inviare dati ed eventi significativi al backend RTP per scopi avanzati di segmentazione e personalizzazione. Funzionalità aggiuntive: [Triggers](../javascript-api/triggers.md), [Corrispondenza pattern](../javascript-api/pattern-match.md).

- Prima di utilizzare l&#39;API Contesto utente, è necessario diventare un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.
- L’API Contesto utente è una funzione che deve essere abilitata dal supporto Marketo su richiesta. Quando l’API è abilitata, viene esposto un oggetto userContext sotto l’oggetto globale RTP.

## Attributi contesto utente

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| `customVar[1-5]` | Stringa | Dati personalizzati salvati nel contesto utente. |
| `viewedCampaigns` | ID campagna come stringa separata da virgole | Campagne visualizzate nelle visite correnti o precedenti. |
| `clickedCampaigns` | ID campagna come stringa separata da virgole | Campagne selezionate nelle visite correnti o precedenti. |

## Imposta variabili personalizzate

Aggiunta di dati personalizzati a Contesto utente.

### Utilizzo

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| `'set'` | Obbligatorio | Stringa | Azione del metodo. |
| `customVar` | Obbligatorio | Stringa | Nome della variabile personalizzata. |
| `my_custom_value` | Obbligatorio | Stringa | Valore personalizzato da salvare sulla variabile personalizzata nell’indice 1-5. |

Nota: le variabili personalizzate vengono inviate all’RTP solo nella chiamata di visualizzazione, pertanto si consiglia di impostarle prima di chiamare la visualizzazione. In caso contrario, verrà inviato solo nella successiva chiamata di visualizzazione.

Restrizioni variabili personalizzate

- La lunghezza della variabile personalizzata non può superare i 100 caratteri.
- I dati della campagna sono limitati alle ultime dieci visite con dieci campagne per visita.

### Utilizzo

`rtp('set', 'customVar', 'A');`

```javascript
// Set and get customVars
rtp('set', 'customVar1', 'foo');

// Read location
if (rtp.userContext.location.state == 'CA')  {
    // Do something
}

// Check if user viewed campaign id 45:
// The campaign id is exposed in the RTP UI when hovering over a campaign name.
if (rtp.userContext.viewedCampaign('45')) {
    // Do something
}
```
