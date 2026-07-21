---
title: Azioni personalizzate
feature: Mobile Marketing
description: Scopri come inviare e segnalare azioni personalizzate con Marketo Mobile SDK per iOS e Android, mettere in coda offline, attivare campagne intelligenti e soddisfare i requisiti dei 20 caratteri...
exl-id: 8c2698ce-4e39-4b2b-9d36-0864c55be17a
TQID: https://experienceleague.adobe.com/yZKzdm-dH0cYPGGKE-Z-4KcbhGIwyFl0Z9vEqcv1QXI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 259
ht-degree: 2%

---

# Azioni personalizzate

Le azioni personalizzate tengono traccia delle interazioni degli utenti nell’app mobile. Quando l’app chiama il SDK Marketo per inviare un’azione personalizzata, SDK salva prima l’azione sul dispositivo. SDK invia l’azione dopo aver rilevato l’adeguata connettività Internet, pertanto Marketo potrebbe riceverla dopo un ritardo.

Le azioni personalizzate possono essere utilizzate come attivatori e filtri nelle campagne avanzate. Per ulteriori informazioni, vedere [Attività app mobile](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/triggers-and-filters-for-mobile-smart-campaigns).

## Invio di azioni personalizzate su iOS

Invia un’azione personalizzata.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];
[sharedInstance reportAction:@"Login" withMetaData:nil];
```

>[!TAB Swift]

```swift
sharedInstance.reportAction("Login", withMetaData:nil);
```

>[!ENDTABS]

Invia un’azione personalizzata con i metadati.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```objectivec
MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
[meta setType:@"Shopping"];
[meta setDetails:@"RedShirt"];
[meta setLength:20];
[meta setMetric:30];

[sharedInstance reportAction:@"Bought Shirt" withMetaData:meta];
```

>[!TAB Swift]

```swift
let meta = MarketoActionMetaData()
meta.setType("Shopping");
meta.setDetails("RedShirt");
meta.setLength(20);
meta.setMetric(30);

sharedInstance.reportAction("Bought Shirt", withMetaData:meta);
```

>[!ENDTABS]

Segnala immediatamente tutte le azioni salvate.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```objectivec
[sharedInstance reportAll];
```

>[!TAB Swift]

```swift
sharedInstance.reportAll();
```

>[!ENDTABS]

## Invio di azioni personalizzate su Android

1. Invia un’azione personalizzata.

   ```
   Marketo.reportAction("Login", null);
   ```

1. Invia un’azione personalizzata con i metadati.

   ```
   MarketoActionMetaData meta = new MarketoActionMetaData();
   meta.setActionType("Shopping");
   meta.setActionDetails("RedShirt");
   meta.setActionLength("20");
   meta.setActionMetric("30");
   
   Marketo.reportAction("Bought Shirt", meta);
   ```

1. Segnala immediatamente tutte le azioni personalizzate salvate.

   ```
   Marketo.reportAll();
   ```

## Risolvere i problemi relativi alle azioni personalizzate

I nomi delle azioni personalizzate inviate da Mobile SDK a Marketo devono contenere meno di 20 caratteri.

**Casi d&#39;uso multiutente su un dispositivo condiviso:** Quando un utente accede a un&#39;app mobile che utilizza Marketo SDK, la prima chiamata associa il lead all&#39;installazione dell&#39;app. Una volta completata la chiamata, le attività successive dell’utente vengono visualizzate nel registro delle attività del lead.

La chiamata di associazione è asincrona. Le azioni personalizzate registrate immediatamente dopo l’accesso possono essere associate all’utente connesso in precedenza fino a quando la chiamata non riesce.
