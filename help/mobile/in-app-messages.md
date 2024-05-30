---
title: "Messaggi in-app"
feature: "Mobile Marketing"
description: "Panoramica dei messaggi in-app"
source-git-commit: e8bb45a7b3bee71c3d0ab6117296a75c8959d72e
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# Messaggi in-app

Per utilizzare le funzionalità di messaggistica in-app di Marketo, è necessario attenersi ai seguenti passaggi:

1. Installare l’SDK di Marketo Mobile come descritto nella [Installazione mobile](installation.md).
1. Aggiungi la tua app mobile a Marketo come descritto in [Aggiungere un’app mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app).
1. Facoltativamente, aggiungi il codice alla tua app mobile per acquisire [Azioni personalizzate](custom-actions.md).

Dopo aver installato l’SDK di Marketo Mobile e completato l’aggiunta della tua app in Marketo, puoi inviare messaggi in-app che vengono visualizzati quando un utente apre la tua app.

Per impostazione predefinita, i messaggi in-app vengono attivati all’apertura dell’app. Se desideri attivare i messaggi in-app per altri eventi, ad esempio quando viene visualizzata una pagina specifica o quando viene premuto un pulsante specifico, devi aggiungere azioni personalizzate al codice. Consulta [Azioni personalizzate](custom-actions.md) sezione per esempi di codice.

## Risoluzione dei problemi

**Il messaggio in-app non viene visualizzato**

Marketo risponde ai trigger dalle app solo dopo che l’SDK di Marketo Mobile è stato inizializzato con la piattaforma Marketo. Il processo di inizializzazione si verifica quando si installa e si apre l&#39;app per la prima volta. Poiché l’inizializzazione viene eseguita dopo la prima apertura dell’app, l’evento &quot;App Open&quot; non viene attivato fino alla seconda apertura dell’app. Chiudi l’app e riaprila; sul dispositivo verrà visualizzato un messaggio attivato da App Open.

Gli eventi personalizzati vengono attivati dall’interazione dell’utente dopo l’apertura dell’app. Gli eventi personalizzati vengono riconosciuti da Marketo durante la prima sessione.

**Tracciamento attività tocco in-app**

Assicurati di assegnare un’azione oltre a &quot;ignora&quot; a uno dei pulsanti primari o secondari per tenere traccia delle attività di tocco e utilizzare le frequenze di visualizzazione di base in base al numero di tocchi.

Per ulteriori informazioni, vedere [Messaggi in-app](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/in-app-messages/creating-in-app-messages/create-an-in-app-message) nella documentazione del prodotto.
