---
title: Azioni personalizzate
feature: Mobile Marketing
description: Scopri come inviare e segnalare azioni personalizzate con Marketo Mobile SDK per iOS e Android, mettere in coda offline, attivare campagne intelligenti e soddisfare i requisiti dei 20 caratteri...
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Azioni personalizzate

Puoi tenere traccia dell’interazione dell’utente inviando azioni personalizzate. Quando l’app mobile effettua una chiamata al SDK di Marketo per inviare un’azione personalizzata, quest’ultima viene inizialmente salvata sul dispositivo. Marketo SDK verifica quindi se è disponibile una connettività Internet adeguata prima di inviare l’azione personalizzata. Di conseguenza, può verificarsi un ritardo tra il momento in cui l’azione personalizzata viene inviata e quello in cui viene ricevuta da Marketo.

Le azioni personalizzate possono essere utilizzate come attivatori e filtri nelle campagne avanzate. Per ulteriori informazioni, vedere [Attività app mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

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

La configurazione delle azioni personalizzate per dispositivi mobili è semplice, ma esistono restrizioni relative al numero di caratteri che è possibile inviare dal SDK mobile a Marketo. Assicurati che tutte le azioni personalizzate che inviano rapporti a Marketo tramite il SDK mobile abbiano una lunghezza inferiore a 20 caratteri.

**Nota sui casi d&#39;uso multiutente su un dispositivo condiviso:** Quando un utente accede a un&#39;app mobile integrata con Marketo SDK, viene effettuata la prima chiamata per associare il lead all&#39;installazione dell&#39;app. Al termine della chiamata, ulteriori attività dell&#39;utente nell&#39;app sono visibili nel registro attività del lead. Nota: poiché si tratta di una chiamata asincrona, se sono presenti azioni personalizzate registrate immediatamente dopo l’accesso queste possono essere associate all’utente che aveva precedentemente eseguito l’accesso finché la chiamata di associazione non ha esito positivo.
