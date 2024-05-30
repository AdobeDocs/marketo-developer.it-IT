---
title: "Abilitazione dei collegamenti profondi"
feature: "Mobile Marketing"
description: "Istruzioni per abilitare i collegamenti profondi"
source-git-commit: cb000968c78e062b3c17be7d0faa6236c73e7358
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---


# Abilitazione dei collegamenti profondi

I collegamenti profondi consentono di reindirizzare le persone a contenuti specifici (risorse) all’interno dell’app. Ad esempio, quando una persona fa clic su un messaggio push mobile che pubblicizza una t-shirt viola, puoi aprire l’app direttamente sul contenuto della t-shirt viola (anziché sulla pagina Home).

Il processo funziona in questo modo:

1. L’utente di Marketo inserisce un URI personalizzato nel tocco Azione per il proprio messaggio push.
1. Quando una persona tocca il messaggio push sul proprio dispositivo, l’SDK di Marketo MME attiva un evento con l’URI personalizzato.
1. L’app elabora quindi l’evento e reindirizza la persona al contenuto appropriato all’interno dell’app.

A tal fine è necessario definire una struttura URI personalizzata per l&#39;app, registrare lo schema nel manifesto dell&#39;app e quindi aggiungere il codice per elaborare gli eventi di collegamento profondo e indirizzarli alla posizione corretta nell&#39;app.

Per iOS, consulta la documentazione di Apple su [Definizione di uno schema URL personalizzato per l’app](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Per Android, consulta la documentazione di Google su [Abilitazione dei collegamenti profondi per il contenuto dell’app](https://developer.android.com/training/app-links/deep-linking).

Per le app PhoneGap, i collegamenti profondi non sono diretti come nelle app native per iOS o Android, ma esistono plug-in che consentono all&#39;app ibrida di rispondere agli schemi URL personalizzati dei collegamenti profondi e ai collegamenti universali/alle app sia su iOS che su Android. Considerare [questi plug-in](https://cordova.apache.org/plugins/?q=deeplink).

Dopo aver abilitato i collegamenti profondi nell&#39;app, condividi gli URI personalizzati con gli utenti di Marketo in modo che possano inserirli nell&#39;azione Tocca per i messaggi push.

Marketo utilizza una struttura URI predefinita per la configurazione dei dispositivi di test. Consultare la sezione &quot;Dispositivi di prova&quot; del [Guida all’installazione](installation.md) per ulteriori informazioni.

## Tecniche consigliate per la definizione di una struttura URI

Se il brand ha un sito mobile esistente, la best practice prevede di seguire la struttura URL anche per l’URI del collegamento profondo. Ad esempio, se `https://myappname.com/products/purple-shirt` è l’indirizzo del tuo sito web per il prodotto in questione, `myappname://products/purple-shirt` sarebbe una buona struttura URI per i collegamenti profondi da utilizzare nell’app.

In genere, i tuoi schemi devono essere specifici per il tuo marchio. Sebbene attualmente non esistano normative che rendano unici i sistemi in tutto il mondo, uno dei modi per garantire che i tuoi schemi siano univoci consiste nell’annullare il nome di dominio (ad esempio, `org.companyname`).
