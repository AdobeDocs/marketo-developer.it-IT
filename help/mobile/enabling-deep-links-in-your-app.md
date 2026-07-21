---
title: Abilitazione dei collegamenti profondi
feature: Mobile Marketing
description: Scopri come abilitare i collegamenti profondi nell’app per i messaggi push di Marketo utilizzando schemi URI personalizzati, con le linee guida e le best practice di iOS, Android e PhoneGap.
exl-id: c3647416-d81d-4f15-b660-bcb3e54cb9bc
TQID: https://experienceleague.adobe.com/UswOvHXGlfTrTUqr4Gsf3j2Z7Xpv2FF2luXeygT4qE0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 1%

---

# Abilitazione dei collegamenti profondi

I collegamenti profondi indirizzano le persone a contenuti specifici nell&#39;app. Ad esempio, quando una persona seleziona un messaggio push per dispositivi mobili che annuncia una t-shirt viola, l’app può aprire il contenuto della t-shirt viola invece che la pagina Home.

Il processo funziona in questo modo:

1. Un utente di Marketo inserisce un URI personalizzato nell’azione Tocca relativa a un messaggio push.
1. Quando una persona tocca il messaggio push sul proprio dispositivo, Marketo MME SDK attiva un evento con l’URI personalizzato.
1. L’app elabora l’evento e indirizza l’utente al contenuto corrispondente.

Per attivare questo processo:

1. Definisci una struttura URI personalizzata per l’app.
1. Registra lo schema nel manifesto dell’app.
1. Aggiungi codice che elabora gli eventi di collegamento profondo e indirizza le persone al contenuto corrispondente.

Per iOS, consulta la documentazione di Apple su [Definizione di uno schema URL personalizzato per la tua app](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app).

Per Android, consulta la documentazione di Google in [Abilitazione dei collegamenti profondi per il contenuto dell&#39;app](https://developer.android.com/training/app-links/deep-linking).

Per le app PhoneGap, utilizza un plug-in per consentire all’app ibrida di rispondere a schemi URL personalizzati e collegamenti universali/alle app su iOS e Android. Consulta i [plug-in per collegamenti profondi](https://cordova.apache.org/plugins/?q=deeplink) disponibili.

Dopo aver abilitato i collegamenti profondi nell&#39;app, condividi gli URI personalizzati con gli utenti di Marketo in modo che possano inserirli nell&#39;azione Tocca per i messaggi push.

Marketo utilizza una struttura URI predefinita per la configurazione dei dispositivi di test. Per ulteriori informazioni, vedere &quot;Test dispositivi&quot; nella [Guida all&#39;installazione](installation.md).

## Tecniche consigliate per la definizione di una struttura URI

Se il brand dispone di un sito mobile, segui la struttura URL quando definisci l’URI del collegamento profondo. Ad esempio, se l&#39;URL del prodotto è `https://myappname.com/products/purple-shirt`, utilizza `myappname://products/purple-shirt` come URI del collegamento profondo corrispondente.

Utilizza uno schema univoco per il tuo marchio. Sebbene nessuna normativa richieda schemi univoci a livello globale, è possibile creare uno schema univoco invertendo il nome di dominio, ad esempio `org.companyname`.
