---
title: "Contesto utente"
feature: REST API
description: "Panoramica sul contesto utente e descrizioni API"
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 5%

---


# Contesto utente

L’API JavaScript per Contesto utente espone i dati a livello di utente e visitatore in più sessioni per abilitare la funzionalità di personalizzazione avanzata utilizzando dati e comportamenti storici degli utenti. L’API non si limita alla lettura dei dati, ma espone variabili personalizzate che consentono di inviare dati ed eventi significativi al backend RTP per scopi avanzati di segmentazione e personalizzazione. Funzionalità aggiuntive: [Triggers](../javascript-api/triggers.md), [Corrispondenza pattern](../javascript-api/pattern-match.md).

- Devi diventare un cliente di personalizzazione web e disporre del [Tag RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito prima di utilizzare l’API Contesto utente.
- L’API Contesto utente è una funzione che deve essere abilitata dal supporto Marketo su richiesta. Quando l’API è abilitata, viene esposto un oggetto userContext sotto l’oggetto globale RTP.

## Attributi contesto utente

| Nome | Tipo | Descrizione |
|------------------|-------------|------|
| customVar[1-5] | Stringa | Dati personalizzati salvati nel contesto utente. |
| viewedCampaigns | ID campagna come stringa separata da virgole | Campagne visualizzate nelle visite correnti o precedenti. |
| clickedCampaigns | ID campagna come stringa separata da virgole | Campagne selezionate nelle visite correnti o precedenti. |

## Imposta variabili personalizzate

Aggiunta di dati personalizzati a Contesto utente.

### Utilizzo

`rtp('set', 'customVar'[1-5], my_custom_value);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|-----------------|-------------------|--------|-----------------|
| &#39;set&#39; | Obbligatorio | Stringa | Azione del metodo. |
| customVar | Obbligatorio | Stringa | Nome della variabile personalizzata. |
| my_custom_value | Obbligatorio | Stringa | Valore personalizzato da salvare sulla variabile personalizzata nell’indice 1-5. |

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
