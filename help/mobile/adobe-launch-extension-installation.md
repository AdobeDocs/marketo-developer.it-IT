---
title: Installazione dell'estensione [!DNL Adobe Launch]
feature: Mobile Marketing
description: Panoramica sull'installazione dell'estensione [!DNL Adobe Launch]
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 0%

---

# Installazione dell&#39;estensione [!DNL Adobe Launch]

Istruzioni di installazione per l&#39;estensione Marketo [!DNL Adobe Launch]. I passaggi seguenti sono necessari per inviare notifiche push e/o messaggi in-app.

## Prerequisiti

1. [Aggiungi un&#39;applicazione in Marketo Admin](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin)
1. [Configura la proprietà in [!DNL Adobe Launch] portale](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configurare la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin per la proprietà nel portale [!DNL Adobe Launch]
1. [Imposta notifiche push](push-notifications.md) (facoltativo)

## Installare l’estensione Marketo su iOS

### Imposta intestazione di bridging Swift

1. Vai a [!UICONTROL File] > [!UICONTROL New] > [!UICONTROL File] e seleziona **[!UICONTROL Header File]**.

1. Denomina il file &quot;&lt;_ProjectName_>-Bridging-Header&quot;.

1. Vai a [!UICONTROL Project] > [!UICONTROL Target] > [!UICONTROL Build Settings] > [!UICONTROL Swift Compiler] > [!UICONTROL Code Generation]. Aggiungi il seguente percorso all’intestazione &quot;Objective-Bridging&quot; (Ponderazione obiettivo):

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Inizializza estensione

>[!BEGINTABS]

>[!TAB Obiettivo C]

Aggiorna il metodo `applicationDidBecomeActive` come segue

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Aggiorna il metodo `applicationDidBecomeActive` come segue

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Dispositivi di prova iOS

1. Selezionare **[!UICONTROL Project]** > **[!UICONTROL Target]** > **[!UICONTROL Info]** > **[!UICONTROL URL Types]**.
1. Aggiungi identificatore: ${PRODUCT_NAME}
1. Imposta schemi URL: mkto-&lt;Chiave_segreto_>
1. Includi da `application:openURL:sourceApplication:annotation:` a `AppDelegate.m file` (Objective-C)

### Gestire il tipo di URL personalizzato in AppDelegate

>[!BEGINTABS]

>[!TAB Obiettivo C]

```
#ifdef __IPHONE_10_0
-(BOOL)application:(UIApplication *)application
           openURL:(NSURL *)url
           options:(NSDictionary *)options{
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
#endif

- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Installare Marketo SDK su Android

### Configurazione dell&#39;estensione Android

Segui le istruzioni nel portale [!DNL Adobe Launch]

### Configurare le autorizzazioni

Apri `AndroidManifest.xml` e aggiungi le seguenti autorizzazioni. L&#39;app deve richiedere le autorizzazioni &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se l&#39;app richiede già queste autorizzazioni, salta questo passaggio.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Inizializza estensione

Configurazione ProGuard (opzionale)

Se utilizzi ProGuard per l&#39;app, aggiungi le seguenti righe nel file `proguard.cfg`. Il file si trova nella cartella `project`. L’aggiunta di questo codice esclude Marketo SDK dal processo di offuscamento.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android  Test  Dispositivi

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

Il Software Development Kit (SDK) MME per Android è stato aggiornato a un framework più moderno, stabile e scalabile che contiene maggiore flessibilità e nuove funzioni di progettazione per lo sviluppatore di app Android.

Gli sviluppatori di app Android ora possono utilizzare direttamente [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) di Google con questo SDK.

### Aggiunta di FCM all’applicazione

1. Integra la versione più recente di Marketo Android SDK nell’app Android.  I passaggi sono disponibili in [GitHub](https://github.com/Marketo/android-sdk).
1. Configurare l’app Firebase nella console Firebase.
   1. Crea/Aggiungi un progetto nella console [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/)Firebase.
      1. Nella [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleziona **[!UICONTROL Add Project]**.
      1. Selezionare il progetto GCM dall&#39;elenco dei progetti Google Cloud esistenti e selezionare **[!UICONTROL Add Firebase]**.
      1. Nella schermata di benvenuto di Firebase, seleziona **[!UICONTROL Add Firebase to your Android App]**.
      1. Specificare il nome del pacchetto e SHA-1 e selezionare **[!UICONTROL Add App]**. È stato scaricato un nuovo file `google-services.json` per l&#39;app Firebase.
      1. Selezionare **[!UICONTROL Continue]** e seguire le istruzioni dettagliate per l&#39;aggiunta del plug-in Google Services in Android Studio.

   1. Passa a **[!UICONTROL Project Settings]** in [!UICONTROL Project Overview]
      1. Fare clic sulla scheda **[!UICONTROL General]**. Scarica il file `google-services.json`.
      1. Fare clic sulla scheda **[!UICONTROL Cloud Messaging]**. Copia [!UICONTROL Server Key] e [!UICONTROL Sender ID]. Fornisci questi [!UICONTROL Server Key] e [!UICONTROL Sender ID] a Marketo.
   1. Configurare le modifiche FCM nell’app Android
      1. Passa alla vista Progetto in Android Studio per visualizzare la directory principale del progetto
         1. Sposta il file `google-services.json` scaricato nella directory principale del modulo app Android
         1. Nel livello di progetto `build.gradle` aggiungere quanto segue:

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

         1. Infine, fai clic su **[!UICONTROL Sync now]** nella barra visualizzata nell&#39;ID
   1. Modifica il manifesto dell&#39;app Il SDK FCM aggiunge automaticamente tutte le autorizzazioni richieste e le funzionalità richieste per il ricevitore. Assicurati di rimuovere i seguenti elementi obsoleti (e potenzialmente dannosi, in quanto potrebbero causare la duplicazione dei messaggi) dal manifesto dell&#39;app:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```

### Domande frequenti su FCM

Domande frequenti sul supporto di Firebase Cloud Messaging.

**D: dove posso trovare le istruzioni per l&#39;aggiornamento alla versione più recente del SDK MME?Le istruzioni per** sono disponibili nel sito Marketo Developer [HERE](installation.md).

**D: per eseguire l&#39;aggiornamento alla versione più recente di SDK è necessario pubblicare una versione aggiornata dell&#39;applicazione Android nei miei utenti esistenti?N.**

**D: che impatto ha sui clienti MME esistenti che hanno pubblicato app Android integrate con Marketo Android SDK?** Gli utenti possono eseguire la migrazione di un&#39;app client GCM esistente da Android a Firebase Cloud Messaging (FCM) come segue:

1. Nella [console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), seleziona **[!UICONTROL Add Project]**.
1. Selezionare il progetto GCM dall&#39;elenco dei progetti Google Cloud esistenti e selezionare **[!UICONTROL Add Firebase]**.
1. Nella schermata di benvenuto di Firebase, seleziona **[!UICONTROL Add Firebase to your Android App]**.
1. Specificare il nome del pacchetto e SHA-1 e selezionare **[!UICONTROL Add App]**. Un nuovo file google-services.json per il tuo
1. L&#39;app Firebase viene scaricata.
1. Selezionare **[!UICONTROL Continue]** e seguire le istruzioni dettagliate per l&#39;aggiunta del plug-in Google Services in Android Studio.

**D: è possibile eseguire il targeting dei lead creati utilizzando il vecchio SDK Marketo che utilizzava l&#39;app GCM?** Sì. Tutti i lead creati con Marketo SDK possono essere oggetto di targeting per l’invio di notifiche push.
