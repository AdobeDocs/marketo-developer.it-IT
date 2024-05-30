---
title: "PhoneGap"
feature: "Mobile Marketing"
description: "Utilizzo di PhoneGap con Marketo su dispositivi mobili"
source-git-commit: 2e4eb416846de3ad62ff0626f536630278e1c0cd
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# PhoneGap

Integrazione del plug-in PhoneGap di Marketo

## Prerequisiti

1. [Aggiungere un’applicazione in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (ottieni la chiave segreta dell’applicazione e l’ID Munchkin).
1. Imposta notifiche push ([iOS](push-notifications.md) | [Android](push-notifications.md)).
1. [Installazione di PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Istruzioni di installazione

1. Configurazione plug-in PhoneGap di Marketo

   Se Cordova CLI è installato, vai alla directory dell&#39;applicazione PhoneGap ed esegui il seguente comando per aggiungere il plug-in Marketo all&#39;applicazione:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Installare il plug-in FCM

   `$ cordova plugin add cordova-plugin-fcm`

   Per verificare che il plug-in sia stato aggiunto all’applicazione, esegui il seguente comando e verifica

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Migra a versione più recente (facoltativo)**

Per rimuovere un plug-in esistente, eseguire il comando seguente:

`$ cordova plugin remove com.marketo.plugin`

Per aggiungere di nuovo il plug-in, esegui il comando seguente:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova versione 8.0.0 (Cordova@Android7.0.0) e successive**

Una volta creata la piattaforma Cordova Android, apri l&#39;app con Android Studio e aggiorna la `dirs` valore del `Marketo.gradle` file trovato in `com.marketo.plugin` cartella.

```
repositories{    
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Aggiungi le piattaforme di destinazione per l’app `$cordova platform add android` `$ cordova platform add ios`

Controlla l’elenco delle piattaforme aggiunte `$cordova platform ls`

1. Supporto di Firebase Cloud Messaging

1. Configurare l’app Firebase nella console Firebase.
   1. Crea/Aggiungi un progetto su [](https://console.firebase.google.com/)Console Firebase.
      1. In [Console Firebase](https://console.firebase.google.com/), seleziona [!UICONTROL Add Project].
      1. Seleziona il progetto GCM dall’elenco dei progetti Google Cloud esistenti, quindi seleziona [!UICONTROL Add Firebase].
      1. Nella schermata di benvenuto di Firebase, seleziona &quot;Add Firebase to your Android App&quot; (Aggiungi Firebase all’app Android).
      1. Specifica il nome del pacchetto e SHA-1, quindi seleziona [!UICONTROL Add App]. Una nuova `google-services.json` per la tua app Firebase viene scaricato.
   1. Vai a &quot;Impostazioni progetto&quot; in Panoramica progetto
      1. Fare clic sulla scheda Generale. Scarica il file &quot;google-services.json&quot;.
      1. Fai clic sulla scheda &quot;Messaggistica cloud&quot;. Copia &quot;Chiave server&quot; e &quot;ID mittente&quot;. Fornisci questi &quot;Server Key&quot; e &quot;Sender ID&quot; a Marketo.
   1. Configurare le modifiche FCM nell’app Phonegap
      1. Sposta il file scaricato &#39;google-services.json&#39; nella directory principale del modulo app Phonegap
      1. Rimuovi il file &#39;MyFirebaseInstanceIDService&#39; dal percorso `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (Obsoleto)
      1. Modificare il file &#39;MyFirebaseMessagingService&#39; nel percorso `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` come segue:

         ```
         import com.marketo.Marketo;
         
         public class MyFirebaseMessagingService extends FirebaseMessagingService{
         
         @Override
         public void onNewToken(String s){
           super.onNewToken(s);
           MarketoExtension.setPushNotificaitonTokens(s);
           //Add your code here
         }
         
         @Override
         public void onMessageReceived(RemoteMessage remoteMessage) {
           MarketoExtension.showPushNotificaiton(remoteMessage);
           //Add your code here
         }
         }
         ```

         1. Modifica il file &quot;fcm_config_files_process.js&quot; nei plug-in di posizione/cordova-plugin-fcm/scripts come segue

            ```
            //change
            var strings = fs.readFileSync("platforms/android/res/values/strings.xml").toString();
            //to
            var strings = fs.readFileSync("platforms/android/app/src/main/res/values/strings.xml").toString();
            
            //AND change
            fs.writeFileSync("platforms/android/res/values/strings.xml", strings);
            //to
            fs.writeFileSync("platforms/android/app/src/main/res/values/strings.xml", strings);
            ```


### 3. Abilitare le notifiche push in xCode

Attiva la funzionalità di notifica push nel progetto xCode.

### 4. Tracciare le notifiche push

Incolla il seguente codice all&#39;interno del `application:didFinishLaunchingWithOptions:` funzione.

>[!BEGINTABS]

>[!TAB Obiettivo C]

Aggiornare il `applicationDidBecomeActive` metodo come sotto

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Aggiornare il `applicationDidBecomeActive` metodo come sotto

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### 5. Inizializzare Marketo Framework

Per garantire che il framework Marketo sia avviato all&#39;avvio dell&#39;app, aggiungi il seguente codice nella `onDeviceReady` nel file JavaScript principale.

Tieni presente che dobbiamo passare `phonegap` come tipo di framework per le app PhoneGap.

### Sintassi

```
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

### Parametri

- Callback riuscito : funzione da eseguire se il framework Marketo viene inizializzato correttamente.
- Callback errore : funzione da eseguire se il framework Marketo non è in grado di inizializzare.
- ID MUNCHKIN : ID Munchkin ricevuto da Marketo al momento della registrazione.
- CHIAVE SEGRETA : Chiave segreta ricevuta da Marketo al momento della registrazione.

### 6. Inizializzare la notifica push di Marketo

Per assicurarsi che la notifica push di Marketo venga avviata, aggiungi il seguente codice dopo la funzione di inizializzazione nel file JavaScript principale.

### Sintassi

```
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parametri

- Success Callback : funzione da eseguire se la notifica push di Marketo viene inizializzata correttamente.
- Failure Callback : funzione da eseguire se la notifica push di Marketo non viene inizializzata.
- GCM_PROJECT_ID : ID progetto GCM trovato in [Console per sviluppatori Google](https://console.developers.google.com/) dopo aver creato l’app.

Il token può anche essere annullato alla disconnessione.

```
marketo. uninitializeMarketoPush(
  function() { console.log("Marketo push successfully uninitialized."); } ,
  function(error) { console.log("an error occurred:" + error); }
);
```

## Associa lead

È possibile creare un lead di Marketo chiamando la funzione associateLead.

### Sintassi

```
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

```
// First create a lead as shown below
var lead = {};
lead[marketo.KEY_FIRST_NAME] = "Phone";
lead[marketo.KEY_LAST_NAME] = "Gap";
lead[marketo.KEY_EMAIL] = email;
lead[marketo.KEY_ADDRESS] = "demo address";
lead[marketo.KEY_CITY] = "city";
lead[marketo.KEY_STATE] = "state";
lead[marketo.KEY_COUNTRY] = "country";
lead[marketo.KEY_POSTAL_CODE] = "postalCode";
lead[marketo.KEY_GENDER] = "gender";
// To use lead custom field, use the REST API NAME as key
lead["REST API NAME"] = "value";

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

```
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

```
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

Associare i tipi di evento &quot;pausa&quot; e &quot;ripresa&quot; come mostrato di seguito per segnalare gli eventi di inizio e fine.  Viene utilizzato per tenere traccia del tempo trascorso nell’app mobile. Nota: questo è richiesto in Android.

```
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

A seconda del metodo utilizzato, un lead appena creato verrà riconosciuto da diversi trigger e filtri. I lead creati utilizzando l’SDK MME o l’API REST vengono visualizzati nei trigger e nei filtri &quot;Lead creato&quot;. I lead creati mediante l’invio di un modulo vengono visualizzati nei trigger e nei filtri &quot;Compila modulo&quot;.

La best practice prevede di rimanere coerente con il metodo utilizzato dall’app web durante la creazione di lead. Se disponi già di un’app web che utilizza l’invio di moduli come meccanismo per creare lead, utilizza lo stesso meccanismo quando crei lead nell’app ibrida. Se disponi già di un’app web che utilizza l’API REST come meccanismo per creare lead, utilizza lo stesso meccanismo quando crei lead nell’app ibrida. Nei casi in cui non utilizzi l’invio di moduli né API REST come meccanismo per creare lead nell’app web, puoi utilizzare l’SDK MME per creare lead in Marketo.
