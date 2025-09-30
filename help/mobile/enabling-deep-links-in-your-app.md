---
title: Abilitazione dei collegamenti profondi
feature: Mobile Marketing
description: Scopri come abilitare i collegamenti profondi nell’app per i messaggi push di Marketo utilizzando schemi URI personalizzati, con le linee guida e le best practice di iOS, Android e PhoneGap.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---

# Abilitazione dei collegamenti profondi

I collegamenti profondi consentono di reindirizzare le persone a contenuti specifici (risorse) all’interno dell’app. Ad esempio, quando una persona fa clic su un messaggio push mobile che pubblicizza una t-shirt viola, puoi aprire l’app direttamente sul contenuto della t-shirt viola (anziché sulla pagina Home).

Il processo funziona in questo modo:

1. L’utente di Marketo inserisce un URI personalizzato nel tocco Azione per il proprio messaggio push.
1. Quando una persona tocca il messaggio push sul proprio dispositivo, Marketo MME SDK attiva un evento con l’URI personalizzato.
1. L’app elabora quindi l’evento e reindirizza la persona al contenuto appropriato all’interno dell’app.

A tal fine è necessario definire una struttura URI personalizzata per l&#39;app, registrare lo schema nel manifesto dell&#39;app e quindi aggiungere il codice per elaborare gli eventi di collegamento profondo e indirizzarli alla posizione corretta nell&#39;app.

Per iOS, consulta la documentazione di Apple su [Definizione di uno schema URL personalizzato per la tua app](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Per Android, consulta la documentazione di Google in [Abilitazione dei collegamenti profondi per il contenuto dell&#39;app](https://developer.android.com/training/app-links/deep-linking).

Per le app PhoneGap, i collegamenti profondi non sono diretti come nelle app native di iOS o Android, ma esistono plug-in che consentono all&#39;app ibrida di rispondere agli schemi URL personalizzati dei collegamenti profondi e ai collegamenti universali/alle app sia su iOS che su Android. Considera [questi plug-in](https://cordova.apache.org/plugins/?q=deeplink).

Dopo aver abilitato i collegamenti profondi nell&#39;app, condividi gli URI personalizzati con gli utenti di Marketo in modo che possano inserirli nell&#39;azione Tocca per i messaggi push.

Marketo utilizza una struttura URI predefinita per la configurazione dei dispositivi di test. Per ulteriori informazioni, consultare la sezione &quot;Dispositivi di test&quot; della [Guida all&#39;installazione](installation.md).

## Tecniche consigliate per la definizione di una struttura URI

Se il brand ha un sito mobile esistente, la best practice prevede di seguire la struttura URL anche per l’URI del collegamento profondo. Ad esempio, se `https://myappname.com/products/purple-shirt` è l&#39;indirizzo del sito Web per il prodotto in questione, `myappname://products/purple-shirt` sarebbe una buona struttura di URI di collegamento profondo da utilizzare nell&#39;app.

In genere, i tuoi schemi devono essere specifici per il tuo marchio. Sebbene attualmente non siano previste normative per rendere univoci gli schemi in tutto il mondo, uno dei modi per garantire che gli schemi siano univoci consiste nell&#39;invertire il nome di dominio (ad esempio, `org.companyname`).
