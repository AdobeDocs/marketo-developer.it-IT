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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 273
ht-degree: 5%

---

# Contesto utente

L’API JavaScript per il contesto utente espone dati a livello di utente e di visitatore in più sessioni. Utilizza i comportamenti e i dati storici per creare una personalizzazione avanzata.

L’API fornisce anche variabili personalizzate per l’invio di dati ed eventi al backend RTP per la segmentazione e la personalizzazione. Consulta le funzionalità [Triggers](../javascript-api/triggers.md) e [Corrispondenza pattern](../javascript-api/pattern-match.md) correlate.

- Devi essere un cliente di Web Personalization e avere il tag [RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul tuo sito.
- Per abilitare l’API Contesto utente, devi chiedere al supporto Marketo. Dopo l&#39;abilitazione, un oggetto userContext viene esposto sotto l&#39;oggetto globale RTP.

## Attributi contesto utente

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| `customVar[1-5]` | Stringa | Dati personalizzati salvati nel contesto utente. |
| `viewedCampaigns` | ID campagna come stringa separata da virgole | Campagne visualizzate nelle visite correnti o precedenti. |
| `clickedCampaigns` | ID campagna come stringa separata da virgole | Campagne selezionate nelle visite correnti o precedenti. |

## Imposta variabili personalizzate

Imposta le variabili personalizzate per aggiungere dati a Contesto utente.

### Utilizzo

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| `'set'` | Obbligatorio | Stringa | Azione del metodo. |
| `customVar` | Obbligatorio | Stringa | Nome della variabile personalizzata. |
| `my_custom_value` | Obbligatorio | Stringa | Valore personalizzato da salvare sulla variabile personalizzata nell’indice 1-5. |

Le variabili personalizzate vengono inviate a RTP solo in una chiamata di visualizzazione. Imposta le variabili personalizzate prima della chiamata di visualizzazione. In caso contrario, le variabili vengono inviate nella successiva chiamata di visualizzazione.

Le variabili personalizzate presentano le seguenti restrizioni:

- Una variabile personalizzata non può superare i 100 caratteri.
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
