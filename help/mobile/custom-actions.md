---
title: "Azioni personalizzate"
feature: "Mobile Marketing"
description: "Panoramica delle azioni personalizzate"
source-git-commit: c51e1b84efdf444c13714c1a08ecc4cac677f483
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Azioni personalizzate

Puoi tenere traccia dell’interazione dell’utente inviando azioni personalizzate. Quando la tua app mobile chiama nell’SDK di Marketo per inviare un’azione personalizzata, quest’ultima viene inizialmente salvata sul dispositivo. Prima di inviare l’azione personalizzata, l’SDK per Marketo controlla quindi se è disponibile una connettività Internet adeguata. Di conseguenza, può verificarsi un ritardo tra il momento in cui l’azione personalizzata viene inviata e quello in cui viene ricevuta da Marketo.

Le azioni personalizzate possono essere utilizzate come attivatori e filtri nelle campagne avanzate. Per ulteriori informazioni, consulta [Attività app mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Invio di azioni personalizzate su iOS

Invia azione personalizzata.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Invia azione personalizzata con metadati.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Segnala immediatamente tutte le azioni (invia tutte le azioni salvate).

>[!BEGINTABS]

>[!TAB Obiettivo C]

```
[sharedInstance reportAll];
```

>[!TAB Swift]

```
sharedInstance.reportAll();
```

>[!ENDTABS]

## Invio di azioni personalizzate su Android

1. Invia azione personalizzata.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Invia azione personalizzata con metadati.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Segnala immediatamente tutte le azioni personalizzate (invia tutte le azioni salvate).

   ```
   Marketo.reportAll();
   ```

## Risoluzione dei problemi relativi alle azioni personalizzate

La configurazione delle azioni personalizzate per dispositivi mobili è semplice, ma esistono limitazioni relative al numero di caratteri che puoi inviare dall’SDK di Mobile a Marketo. Assicurati che tutte le azioni personalizzate che inviano rapporti a Marketo tramite l&#39;SDK mobile abbiano una lunghezza inferiore a 20 caratteri.

**Nota sui casi di utilizzo multiutente su un dispositivo condiviso:** Quando un utente accede a un’app mobile integrata con l’SDK di Marketo, viene effettuata la prima chiamata per associare il lead all’installazione dell’app. Al termine della chiamata, ulteriori attività dell&#39;utente nell&#39;app sono visibili nel registro attività del lead. Nota: poiché si tratta di una chiamata asincrona, se sono presenti azioni personalizzate registrate immediatamente dopo l’accesso queste possono essere associate all’utente che aveva precedentemente eseguito l’accesso finché la chiamata di associazione non ha esito positivo.
