---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Guida dettagliata per integrare il plug-in Marketo Cordova con Ionic, abilitare le notifiche push, inizializzare SDK, tenere traccia delle sessioni e associare i lead.
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
TQID: https://experienceleague.adobe.com/UTNWd69NliR896RcO-XM2GG35liuLeNNhTXo9GRtB4o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 581
ht-degree: 2%

---

# Ionico

Integrare il plug-in Marketo Cordova con un&#39;app [!DNL Ionic]. [!DNL Ionic] Il condensatore non è attualmente supportato.

## Prerequisiti

1. [Aggiungi un&#39;applicazione in Marketo Admin](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin.
1. Configura le notifiche push per [iOS](push-notifications.md) o [Android](push-notifications.md).
1. Installa [[!DNL Ionic]](https://ionicframework.com/getting-started/) e [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Istruzioni di installazione

### Configura plug-in di Marketo [!DNL Ionic]

1. Andare alla directory dell&#39;applicazione [!DNL Ionic] ed eseguire il comando seguente per aggiungere il plug-in di Marketo:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Esegui il comando seguente per confermare che il plug-in è stato aggiunto:

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migra a versione più recente (facoltativo)

1. Per rimuovere un plug-in esistente, eseguire il comando seguente:

   `$ ionic plugin remove com.marketo.plugin`

1. Per aggiungere di nuovo il plug-in, esegui il comando seguente:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Abilitare le notifiche push in xCode

1. Attiva la funzionalità di notifica push nel progetto xCode.![Funzionalità di notifica](assets/notification-capability.png)

### Tracciare le notifiche push

Incollare il codice seguente nella funzione `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Inizializza framework Marketo

Per inizializzare il framework Marketo all&#39;avvio dell&#39;app, aggiungere il codice seguente nella funzione `onDeviceReady` nel file JavaScript principale.

Passa `ionicCordova` come tipo di framework per [!DNL Ionic] app Cordova.

#### Sintassi

```javascript
// This method will Initialize the Marketo Framework using Your MunchkinId and Secret Key
marketo.initialize(
  function() { console.log("MarketoSDK Init done."); },
  function(error) { console.log("an error occurred:" + error); },
  'YOUR_MUNCHKIN_ID',
  'YOUR_SECRET_KEY',
  'FRAMEWORK_TYPE'
);

// For session tracking, add following.
marketo.onStart(
  function(){ console.log("onStart."); },
  function(error){ console.log("Failed to report onStart." + error); }
);
```

#### Parametri

- Callback riuscito: funzione da eseguire se il framework Marketo viene inizializzato correttamente.
- Callback errore: funzione da eseguire se l&#39;inizializzazione del framework Marketo non riesce.
- MUNCHKIN ID: Munchkin ID ricevuto da Marketo durante la registrazione.
- CHIAVE SEGRETA: chiave segreta ricevuta da Marketo durante la registrazione.

### Inizializza notifica push Marketo

Per inizializzare le notifiche push di Marketo, aggiungi il seguente codice dopo la funzione di inizializzazione nel file JavaScript principale.

#### Sintassi

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

#### Parametri

- Callback riuscito: funzione da eseguire se la notifica push di Marketo viene inizializzata correttamente.
- Callback di errore: funzione da eseguire se la notifica push Marketo non viene inizializzata.
- GCM_PROJECT_ID: ID progetto GCM trovato in [Console sviluppatori Google](https://accounts.google.com/ServiceLogin?service=cloudconsole&passive=1209600&osid=1&continue=https://console.cloud.google.com/apis/dashboard&followup=https://console.cloud.google.com/apis/dashboard) dopo la creazione dell&#39;app.

È inoltre possibile annullare la registrazione del token alla disconnessione.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associa lead

Chiamare la funzione associataLead per creare un lead Marketo.

### Sintassi

```javascript
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parametri

- Success Callback: Funzione da eseguire se il framework Marketo associa correttamente il lead.
- Callback errore: funzione da eseguire se il framework Marketo non associa il lead.
- Dati lead: dati lead in formato stringa JSON.

### Esempio

```javascript
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Ionic";
lead[marketo.KEY_LAST_NAME] = "App";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";

// Use associateLead function to associate it.
marketo.associateLead(
  function() { console.log("MarketoSDK : Lead Associated"); },
  function(error) { console.log("an error occurred:" + error); },
  JSON.stringify(lead)
);
```

## Azione report

Chiamare la funzione `reportaction` per segnalare un&#39;azione dell&#39;utente.

### Sintassi

```javascript
marketo.reportaction(
  function(){ console.log("MarketoSDK : New event sent "); },
  function(error){ console.log("an error occurred:" + error); },
  'Action_Name',
  'Action_Data_JSON_String'
);
```

### Parametri

- Success Callback: Funzione da eseguire se il framework Marketo riporta l&#39;azione correttamente.
- Callback di errore: funzione da eseguire se il framework Marketo non riesce a segnalare l&#39;azione.
- Nome azione: nome dell’azione.
- Dati azione: dati azione in formato stringa JSON.

### Esempio

```javascript
// First create an event as below
var event = {
    "Action Type":"Add To Cart",
    "Action Details":"Adding Product in cart",
    "Action Metric":"10",
    "Action Length":"1"
}

marketo.reportaction(
    function(){ console.log("Reported action successfully."); },
    function(error){ console.log("Failed to report action." + error); },
    "Add To Cart",
    JSON.stringify(event)
);
```

## Reporting sulle sessioni

Associare i tipi di evento &quot;pausa&quot; e &quot;ripresa&quot; per segnalare gli eventi di inizio e fine. Questi eventi tengono traccia del tempo trascorso nell’app mobile e sono necessari per Android.

```javascript
//Add the following code in your www/js/index.js

bindEvents: function() {
   document.addEventListener('pause', this.onStop, false);
   document.addEventListener('resume', this.onStart, false);
},
onStop: function() {
   marketo.onStop(
       function(){ console.log("onStop"); },
       function(error){ console.log("Failed to report onStop." + error); }
   );
},
onStart: function() {
   marketo.onStart(
       function(){ console.log("onStart."); },
       function(error){console.log( "Failed to report onStart." + error); }
   );
},
```

## Creazione di lead

Esistono tre modi per creare lead da un’app ibrida:

1. MARKETO MME SDK
1. API REST di Marketo
1. Invio modulo

I trigger e i filtri che riconoscono un nuovo lead dipendono dal metodo di creazione:

- I lead creati con MME SDK o REST API vengono visualizzati nei trigger e nei filtri &quot;Lead Created&quot;.
- I lead creati dall’invio del modulo vengono visualizzati nei trigger e nei filtri &quot;Compila modulo&quot;.

Utilizza lo stesso metodo di creazione di lead nell’app ibrida e nell’app web. Se l’app web utilizza l’invio di moduli o l’API REST, utilizza tale metodo nell’app ibrida. Se l’app web non utilizza nessuno dei due metodi, è consigliabile utilizzare MME SDK per creare lead in Marketo.
