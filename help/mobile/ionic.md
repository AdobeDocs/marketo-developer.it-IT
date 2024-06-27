---
title: '[!DNL Ionic]'
feature: Mobile Marketing
description: Utilizzo di [!DNL Ionic] con Marketo per dispositivi mobili
exl-id: 204e5fb4-c9d6-43a6-9d77-0b2a67ddbed3
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---

# Ionico

Questo argomento descrive come integrare il plug-in Marketo Cordova. [!DNL Ionic] il condensatore non è attualmente supportato.

## Prerequisiti

1. [Aggiungere un’applicazione in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (ottieni la chiave segreta dell’applicazione e l’ID Munchkin).
1. Imposta notifiche push ([iOS](push-notifications.md) | [Android](push-notifications.md) ).
1. Installa [[!DNL Ionic]](https://ionicframework.com/getting-started/) E [Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Istruzioni di installazione

### Configura Marketo [!DNL Ionic] Plug-in

1. Se Cordova CLI è installato, passare alla [!DNL Ionic] ed eseguire il comando seguente per aggiungere il plug-in Marketo all&#39;applicazione:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Per confermare che il plug-in è stato aggiunto all’applicazione, esegui il seguente comando:

   `$ ionic plugin list com.marketo.plugin 0.X.0 "MarketoPlugin"`

### Migra a versione più recente (facoltativo)

1. Per rimuovere un plug-in esistente, eseguire il comando seguente:

   `$ ionic plugin remove com.marketo.plugin`

1. Per leggere il plug-in, esegui il comando seguente:

   `$ ionic plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

### Abilitare le notifiche push in xCode

1. Attiva la funzionalità di notifica push nel progetto xCode.![Funzionalità di notifica](assets/notification-capability.png)

### Tracciare le notifiche push

Incolla il seguente codice all&#39;interno del `application:didFinishLaunchingWithOptions:` funzione.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotfication(launchOptions)
```

>[!ENDTABS]

### Inizializza framework Marketo

Per garantire che il framework Marketo sia avviato all&#39;avvio dell&#39;app, aggiungi il seguente codice nella `onDeviceReady` nel file JavaScript principale.

Devi superare `ionicCordova` come tipo di framework per [!DNL Ionic] App Cordova.

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

- Callback riuscito : funzione da eseguire se il framework Marketo viene inizializzato correttamente.
- Callback errore : funzione da eseguire se il framework Marketo non è in grado di inizializzare.
- ID MUNCHKIN : ID Munchkin ricevuto da Marketo al momento della registrazione.
- CHIAVE SEGRETA : Chiave segreta ricevuta da Marketo al momento della registrazione.

### Inizializza notifica push Marketo

Per assicurarsi che la notifica push di Marketo venga avviata, aggiungi il seguente codice dopo la funzione inizializzata nel file JavaScript principale.

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

- Success Callback : funzione da eseguire se la notifica push di Marketo viene inizializzata correttamente.
- Failure Callback : funzione da eseguire se la notifica push di Marketo non viene inizializzata.
- GCM_PROJECT_ID : ID progetto GCM trovato in [Console per sviluppatori Google](https://accounts.google.com/ServiceLogin?service=cloudconsole&amp;passive=1209600&amp;osid=1&amp;continue=https://console.cloud.google.com/apis/dashboard&amp;followup=https://console.cloud.google.com/apis/dashboard) dopo aver creato l’app.

Il token può anche essere annullato alla disconnessione.

```javascript
marketo.uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associa lead

È possibile creare un lead di Marketo chiamando la funzione associateLead.

### Sintassi

```javascript
marketo.associateLead(
  function(){ console.log("MarketoSDK : Lead Added"); },
  function(error){ console.log("an error occurred:" + error); },
  'Lead_Data_JSON_String'
);
```

### Parametri

- Success Callback : funzione da eseguire se il framework Marketo associa il lead correttamente.
- Callback errore : funzione da eseguire se il framework Marketo non associa il lead.
- Dati lead : dati lead in formato stringa JSON.

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

Puoi segnalare qualsiasi azione eseguita dall&#39;utente chiamando il `reportaction` funzione.

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

- Callback riuscito : funzione da eseguire se il framework di Marketo riporta correttamente l&#39;azione.
- Callback di errore : funzione da eseguire se il framework Marketo non riesce a segnalare l&#39;azione.
- Nome azione : nome azione.
- Dati azione : dati azione in formato stringa JSON.

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

Associare i tipi di evento &quot;pausa&quot; e &quot;ripresa&quot; come mostrato di seguito per segnalare gli eventi di inizio e fine. Viene utilizzato per tenere traccia del tempo trascorso nell’app mobile. Nota: questo è richiesto in Android.

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

1. SDK MARKETO MME
1. API REST di Marketo
1. Invio modulo

A seconda del metodo utilizzato, un lead appena creato viene riconosciuto da diversi trigger e filtri. I lead creati utilizzando l’SDK MME o l’API REST vengono visualizzati nei trigger e nei filtri &quot;Lead creato&quot;. I lead creati mediante l’invio di un modulo vengono visualizzati nei trigger e nei filtri &quot;Compila modulo&quot;.

La best practice prevede di rimanere coerente con il metodo utilizzato dall’app web durante la creazione di lead. Se disponi già di un’app web che utilizza l’invio di moduli come meccanismo per creare lead, utilizza lo stesso meccanismo quando crei lead nell’app ibrida. Se disponi già di un’app web che utilizza l’API REST come meccanismo per creare lead, utilizza lo stesso meccanismo quando crei lead nell’app ibrida. Nei casi in cui non utilizzi l’invio di moduli né API REST come meccanismo per creare lead nell’app web, puoi utilizzare l’SDK MME per creare lead in Marketo.
