---
title: PhoneGap
feature: Mobile Marketing
description: Configura il plug-in Marketo PhoneGap con Cordova, configura Firebase Cloud Messaging, abilita iOS e Android push, tieni traccia delle notifiche e inizializza SDK.
exl-id: 99f14c76-9438-4942-9309-643bca434d07
TQID: https://experienceleague.adobe.com/eFAwR7r5IE6vKigsEWrJdCmC3VrfB-nl0h8x7Vgt1VY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 758
ht-degree: 2%

---

# PhoneGap

Integra il plug-in PhoneGap di Marketo con un’app Cordova.

## Prerequisiti

1. [Aggiungi un&#39;applicazione in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin.
1. Configura le notifiche push per [iOS](push-notifications.md) o [Android](push-notifications.md).
1. [Installa PhoneGap/Cordova CLI](https://cordova.apache.org/docs/en/latest/guide/cli/).

## Istruzioni di installazione

1. Configura il plug-in PhoneGap di Marketo.

   Vai alla directory dell&#39;applicazione PhoneGap ed esegui il seguente comando per aggiungere il plug-in Marketo:

   `$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

1. Installare il plug-in FCM.

   `$ cordova plugin add cordova-plugin-fcm`

   Esegui il comando seguente per confermare che il plug-in è stato aggiunto:

   `$ cordova plugin ls com.marketo.plugin 0.X.0 "MarketoPlugin" cordova-plugin-fcm 2.1.2 "FCMPlugin"`

**Esegui migrazione alla versione più recente (facoltativo)**

Per rimuovere un plug-in esistente, eseguire il comando seguente:

`$ cordova plugin remove com.marketo.plugin`

Per aggiungere di nuovo il plug-in, esegui il comando seguente:

`$ cordova plugin add https://github.com/Marketo/PhoneGapPlugin.git --variable APPLICATION_SECRET_KEY="YOUR_APPLICATION_SECRET"`

**Cordova versione 8.0.0 (Cordova@Android7.0.0) e successive**

Dopo aver generato la piattaforma Android Cordova, apri l’app in Android Studio. Aggiornare il valore `dirs` nel file `Marketo.gradle` nella cartella `com.marketo.plugin`.

```groovy
repositories{
  jcenter()
  flatDir{
      dirs '../app/src/main/aar'
   }
}
```

Aggiungere le piattaforme di destinazione per l&#39;app: `$cordova platform add android` `$ cordova platform add ios`

Controlla le piattaforme aggiunte: `$cordova platform ls`

1. Supporto di Firebase Cloud Messaging

1. Configura l’app Firebase nella console Firebase.
   1. Crea o aggiungi un progetto in [&#128279;](https://console.firebase.google.com/)Firebase Console.
      1. Nella [console Firebase](https://console.firebase.google.com/), seleziona **[!UICONTROL Add Project]**.
      1. Selezionare il progetto GCM dall&#39;elenco dei progetti Google Cloud esistenti e selezionare **[!UICONTROL Add Firebase]**.
      1. Nella schermata di benvenuto di Firebase, seleziona &quot;Add Firebase to your Android App&quot; (Aggiungi Firebase alla tua app).
      1. Specificare il nome del pacchetto e SHA-1 e selezionare **[!UICONTROL Add App]**. È stato scaricato un nuovo file `google-services.json` per l&#39;app Firebase.
   1. Vai a **[!UICONTROL Project Settings]** in [!UICONTROL Project Overview].
      1. Selezionare la scheda **[!UICONTROL General]** e scaricare il file &#39;google-services.json&#39;.
      1. Seleziona la scheda **[!UICONTROL Cloud Messaging]**. Copia [!UICONTROL Server Key] e [!UICONTROL Sender ID] e forniscili a Marketo.
   1. Configura FCM nell’app PhoneGap.
      1. Sposta il file scaricato &quot;google-services.json&quot; nella directory principale del modulo app PhoneGap.
      1. Rimuovi il file &#39;MyFirebaseInstanceIDService&#39; dal percorso `platforms/android/app/src/main/java/com/gae/scaffolder/plugin` (obsoleto)
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

### &#x200B;3. Abilitare le notifiche push in xCode

Attiva la funzionalità di notifica push nel progetto xCode.

### &#x200B;4. Tracciare le notifiche push

Incollare il codice seguente nella funzione `application:didFinishLaunchingWithOptions:`.

>[!BEGINTABS]

>[!TAB Obiettivo C]

Aggiornare il metodo `applicationDidBecomeActive` come segue.

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance trackPushNotification:launchOptions];
```

>[!TAB Swift]

Aggiornare il metodo `applicationDidBecomeActive` come segue.

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.trackPushNotification(launchOptions)
```

>[!ENDTABS]

### &#x200B;5. Inizializza framework Marketo

Per inizializzare il framework Marketo all&#39;avvio dell&#39;app, aggiungere il codice seguente nella funzione `onDeviceReady` nel file JavaScript principale.

Passa `phonegap` come tipo di framework per le app PhoneGap.

### Sintassi

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

### Parametri

- Callback riuscito: funzione da eseguire se il framework Marketo viene inizializzato correttamente.
- Callback errore: funzione da eseguire se l&#39;inizializzazione del framework Marketo non riesce.
- MUNCHKIN ID: Munchkin ID ricevuto da Marketo durante la registrazione.
- CHIAVE SEGRETA: chiave segreta ricevuta da Marketo durante la registrazione.

### &#x200B;6. Inizializza notifica push Marketo

Per inizializzare le notifiche push di Marketo, aggiungi il seguente codice dopo la funzione di inizializzazione nel file JavaScript principale.

### Sintassi

```javascript
// This function will Enable user notifications (prompts the user to accept push notifications in iOS)
marketo.initializeMarketoPush(
    function() { console.log("Marketo push successfully initialized."); },
    function(error) { console.log("an error occurred:" + error); },
    'YOUR_GCM_PROJECT_ID' // This is required for Android and will be ignored in iOS
);
```

### Parametri

- Callback riuscito: funzione da eseguire se la notifica push di Marketo viene inizializzata correttamente.
- Callback di errore: funzione da eseguire se la notifica push Marketo non viene inizializzata.
- GCM_PROJECT_ID: ID progetto GCM trovato in [Console sviluppatori Google](https://console.developers.google.com/) dopo la creazione dell&#39;app.

È inoltre possibile annullare la registrazione del token alla disconnessione.

```javascript
marketo. uninitializeMarketoPush(
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
