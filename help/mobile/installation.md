---
title: Installazione
feature: Mobile Marketing
description: Guida all’installazione e all’inizializzazione di Marketo Mobile SDK su iOS e Android tramite CocoaPods, Swift Package Manager o Gradle, per abilitare i messaggi push e in-app.
exl-id: e0b79d85-3509-46d2-a77d-cee211c5ec7f
TQID: https://experienceleague.adobe.com/zYNoGPwJTQnqmP6CH0NDbmb-b8vAKRScMmms6vy0Sb4
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 765
ht-degree: 0%

---

# Installazione

Installa e inizializza Marketo Mobile SDK per inviare notifiche push, messaggi in-app o entrambi.

## Installare Marketo SDK su iOS

### Prerequisiti

1. [Aggiungi un&#39;applicazione in Marketo Admin](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin.
1. Facoltativo: [Configurare le notifiche push](push-notifications.md).

### Installare Framework tramite CocoaPods

1. Installare CocoaPods. `$ sudo gem install cocoapods`
1. Cambia la directory nella directory del progetto e crea un Podfile con impostazioni avanzate. `$ pod init`
1. Apri il Podfile. `$ open -a Xcode Podfile`
1. Aggiungi la seguente riga al tuo Podfile. `$ pod 'Marketo-iOS-SDK'`
1. Salva e chiudi il Podfile.
1. Scaricare e installare Marketo iOS SDK. `$ pod install`
1. Apri l’area di lavoro in Xcode. `$ open App.xcworkspace`

### Installare Framework con Gestione pacchetti Swift

1. Seleziona il progetto in Navigatore progetti. In &quot;Aggiungi dipendenza pacchetto&quot;, selezionare &quot;+&quot;.

   ![Aggiungi dipendenza](assets/dependency-manager-add.png)

1. Aggiungere il pacchetto Marketo da <https://github.com/Marketo/ios-sdk>.

   ![URL archivio](assets/dependency-manager-url.png)

1. Aggiungi il bundle di risorse. Individuare `MarketoFramework.XCframework` nel Navigatore progetti e aprirlo nel Finder. Trascina `MKTResources.bundle` per copiare le risorse del bundle.

### Imposta intestazione di bridging Swift

1. Selezionare File > Nuovo > File e quindi &quot;File di intestazione&quot;.

   ![Selezionare &quot;File di intestazione&quot;](assets/choose-header-file.png)

1. Denomina il file &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vai a Progetto > Target > Fasi build > Compilatore Swift > Generazione del codice. Aggiungi il seguente percorso a Objective-Bridging Header:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

   ![Fasi build](assets/build-phases.png)

## Inizializza SDK

Inizializza Marketo iOS SDK con il tuo ID account Munchkin e la chiave segreta dell&#39;app. Trova entrambi i valori in &quot;Mobile Apps and Devices&quot; (App e dispositivi mobili) in Amministrazione Marketo.

1. Apri il file AppDelegate.m per Objective-C o il file Bridging per Swift. Importa il file di intestazione Marketo.h.

   ```
   #import <MarketoFramework/MarketoFramework.h>
   ```

1. Incollare il codice seguente nella funzione `application:didFinishLaunchingWithOptions`:.

   Passa &quot;nativo&quot; come tipo di framework per le app native.

>[!BEGINTABS]

>[!TAB Obiettivo C]

```objectivec
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance initializeWithMunchkinID:@"munchkinAccountId" appSecret:@"secretKey" mobileFrameworkType:@"native" launchOptions:launchOptions];
```

>[!TAB Swift]

```swift
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.initialize(withMunchkinID: "munchkinAccountId", appSecret: "secretKey", mobileFrameworkType: "native", launchOptions: launchOptions)
```

>[!ENDTABS]

1. Sostituire `munkinAccountId` e `secretKey` con &quot;ID account Munchkin&quot; e &quot;Chiave segreta&quot; da Marketo **[!UICONTROL Admin]** > **[!UICONTROL Mobile Apps and Devices]**.

## Dispositivi di prova iOS

1. Seleziona Progetto > Target > Informazioni > Tipi di URL.
1. Aggiungi l&#39;identificatore ${PRODUCT_NAME}.
1. Impostare gli schemi URL su `mkto-<Secret Key_>`.
1. Aggiungi application:openURL:sourceApplication:annotation: al file AppDelegate.m per Objective-C.

## Gestire il tipo di URL personalizzato in AppDelegate

>[!BEGINTABS]

>[!TAB Obiettivo C]

```objectivec
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

>[!TAB Swift]

```swift
private func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
    {
        return Marketo.sharedInstance().application(app, open: url, options: options)
    }
```

>[!ENDTABS]

## Installare Marketo SDK su Android

### Prerequisiti

1. [Aggiungi un&#39;applicazione in Marketo Admin](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin.
1. Facoltativo: [Configurare le notifiche push](push-notifications.md#android_setup_push).
1. [Scarica Marketo SDK per Android](https://codeload.github.com/Marketo/android-sdk/zip/refs/heads/master)

### Installazione di Android SDK con Gradle

1. Nel file build.gradle a livello di applicazione, aggiungi la dipendenza nella sezione dipendenze.

   `implementation 'com.marketo:MarketoSDK:0.8.9'`

1. Aggiungere la configurazione seguente al file radice `build.gradle`.

   ```
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
   ```

1. Sincronizza il progetto con i file Gradle.

### Configurare le autorizzazioni

Apri `AndroidManifest.xml` e aggiungi le seguenti autorizzazioni. L&#39;app deve richiedere le autorizzazioni &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Ignora questo passaggio se l’app li richiede già.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Inizializza SDK

1. Aprire la classe Application o Activity. Importa Marketo SDK nell’attività prima di setContentView o nel contesto dell’applicazione.

   ```java
   // Initialize Marketo
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   marketoSdk.initializeSDK("native","munchkinAccountId","secretKey");
   ```

1. Configurazione ProGuard (opzionale)

   Se l&#39;app utilizza ProGuard, aggiungere le righe seguenti al file `proguard.cfg` nella cartella del progetto. Questa configurazione esclude Marketo SDK dall’offuscamento.

   ```
   -dontwarn com.marketo.*
   -dontnote com.marketo.*
   -keep class com.marketo.`{ *; }
   ```

## Dispositivi di prova Android

Aggiungi &quot;MarketoActivity&quot; a `AndroidManifest.xml` all&#39;interno del tag dell&#39;applicazione.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize" >
    <intent-filter android:label="MarketoActivity" >
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto" />
    </intent-filter>
</activity>
```

## Supporto di Firebase Cloud Messaging

MME SDK per Android supporta l&#39;uso diretto di [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) di Google.

### Aggiunta di FCM all’applicazione

1. Integra la versione più recente di Marketo Android SDK nell’app Android. Consulta i passaggi in [GitHub](https://github.com/Marketo/android-sdk).
1. Configura l’app Firebase nella console Firebase.
   1. Crea/Aggiungi un progetto nella console [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Firebase.
      1. Nella [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleziona `Add Project`.
      1. Selezionare il progetto GCM dall&#39;elenco dei progetti Google Cloud esistenti e selezionare `Add Firebase`.
      1. Nella schermata di benvenuto di Firebase, seleziona `Add Firebase to your Android App`.
      1. Specificare il nome del pacchetto e SHA-1 e selezionare `Add App`. È stato scaricato un nuovo file `google-services.json` per l&#39;app Firebase.
      1. Selezionare `Continue` e seguire le istruzioni dettagliate per l&#39;aggiunta del plug-in Google Services in Android Studio.

   1. Vai a &quot;Impostazioni progetto&quot; in Panoramica progetto
      1. Fare clic sulla scheda Generale. Scarica il file &quot;google-services.json&quot;.
      1. Fai clic sulla scheda &quot;Messaggistica cloud&quot;. Copia &quot;Chiave server&quot; e &quot;ID mittente&quot;. Fornisci questi &quot;Server Key&quot; e &quot;Sender ID&quot; a Marketo.
   1. Configura FCM nell’app Android.
      1. Passa alla vista Progetto in Android Studio per visualizzare la directory principale del progetto
         1. Sposta il file scaricato &quot;google-services.json&quot; nella directory principale del modulo app Android
         1. In build.gradle a livello di progetto, aggiungi quanto segue:

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. In build.gradle a livello di app, aggiungi quanto segue:

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            }
            // Add to the bottom of the file
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Infine, seleziona **[!UICONTROL Sync now]** nella barra visualizzata nell&#39;ID
   1. Modifica il manifesto dell’app. FCM SDK aggiunge automaticamente le autorizzazioni richieste e le funzionalità del ricevitore. Rimuovi i seguenti elementi obsoleti che potrebbero causare la duplicazione dei messaggi:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND"
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```
