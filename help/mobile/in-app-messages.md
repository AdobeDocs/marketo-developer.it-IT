---
title: Messaggi in-app
feature: Mobile Marketing
description: Panoramica dei messaggi in-app
exl-id: 73c9f862-d154-4b37-94ce-92311aa756e8
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# Messaggi in-app

Per utilizzare le funzionalità di messaggistica in-app di Marketo, è necessario attenersi ai seguenti passaggi:

1. Installa l&#39;SDK di Marketo Mobile come descritto in [Installazione di Mobile](installation.md).
1. Aggiungi la tua app mobile a Marketo come descritto in [Aggiungi un&#39;app mobile](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Facoltativamente, aggiungi codice alla tua app mobile per acquisire [azioni personalizzate](custom-actions.md).

Dopo aver installato l’SDK di Marketo Mobile e completato l’aggiunta della tua app in Marketo, puoi inviare messaggi in-app che vengono visualizzati quando un utente apre la tua app.

Per impostazione predefinita, i messaggi in-app vengono attivati all’apertura dell’app. Se desideri attivare i messaggi in-app per altri eventi, ad esempio quando viene visualizzata una pagina specifica o quando viene premuto un pulsante specifico, devi aggiungere azioni personalizzate al codice. Per esempi di codice, consulta la sezione [Azioni personalizzate](custom-actions.md).

## Risoluzione dei problemi

**Messaggio in-app non visualizzato**

Marketo risponde ai trigger dalle app solo dopo che l’SDK di Marketo Mobile è stato inizializzato con la piattaforma Marketo. Il processo di inizializzazione si verifica quando si installa e si apre l&#39;app per la prima volta. Poiché l’inizializzazione viene eseguita dopo la prima apertura dell’app, l’evento &quot;App Open&quot; non viene attivato fino alla seconda apertura dell’app. Chiudi l’app e riaprila; sul dispositivo verrà visualizzato un messaggio attivato da App Open.

Gli eventi personalizzati vengono attivati dall’interazione dell’utente dopo l’apertura dell’app. Gli eventi personalizzati vengono riconosciuti da Marketo durante la prima sessione.

**Tracciamento attività tocco in-app**

Assicurati di assegnare un’azione oltre a &quot;ignora&quot; a uno dei pulsanti primari o secondari per tenere traccia delle attività di tocco e utilizzare le frequenze di visualizzazione di base in base al numero di tocchi.

Per ulteriori informazioni, consulta la sezione [Messaggi in-app](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) nella documentazione del prodotto.
