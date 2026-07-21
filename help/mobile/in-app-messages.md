---
title: Messaggi in-app
feature: Mobile Marketing
description: Configura i messaggi in-app Marketo con Mobile SDK, configura i trigger di evento personalizzati, tieni traccia dell’attività di tocco e correggi i problemi di inizializzazione della prima app aperta.
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
TQID: https://experienceleague.adobe.com/RVkEUBaFb-PHd0gE9ngzYc5zOojINwSI7ic2TmcU7-8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 321
ht-degree: 2%

---

# Messaggi in-app

Per utilizzare la messaggistica in-app di Marketo, completa i passaggi seguenti:

1. Installa Marketo Mobile SDK come descritto in [Installazione di Mobile](installation.md).
1. Aggiungi la tua app mobile a Marketo come descritto in [Aggiungi un&#39;app mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Facoltativo: aggiungi codice alla tua app mobile per acquisire [azioni personalizzate](custom-actions.md).

Dopo aver installato Marketo Mobile SDK e aver aggiunto l’app a Marketo, puoi inviare messaggi in-app che vengono visualizzati quando un utente apre l’app.

Per impostazione predefinita, i messaggi in-app vengono attivati all’apertura dell’app. Per attivare un messaggio per un altro evento, ad esempio la visualizzazione di una pagina specifica o la selezione di un pulsante specifico, aggiungi un’azione personalizzata al codice. Vedi [Azioni personalizzate](custom-actions.md) per esempi di codice.

## Risoluzione dei problemi

**Messaggio in-app non visualizzato**

Marketo risponde ai trigger dell’app solo dopo che Marketo Mobile SDK è stato inizializzato con la piattaforma Marketo. L’inizializzazione si verifica al primo avvio dell’app.

Poiché l’inizializzazione viene eseguita dopo la prima apertura dell’app, l’evento &quot;App Open&quot; non viene attivato finché non apri l’app una seconda volta. Chiudi e riapri l’app. Sul dispositivo dovrebbe quindi essere visualizzato un messaggio attivato da App Open.

Gli eventi personalizzati vengono attivati dall’interazione dell’utente dopo l’apertura dell’app. Gli eventi personalizzati vengono riconosciuti da Marketo durante la prima sessione.

**Tracciamento attività tocco in-app**

Per tenere traccia delle attività di tocco e basare la frequenza di visualizzazione sul numero di tocchi, assegna a un pulsante primario o secondario un’azione diversa da &quot;ignora&quot;.

Per ulteriori informazioni, consulta [Messaggi in-app](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) nella documentazione del prodotto.
