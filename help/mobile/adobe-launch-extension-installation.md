---
title: " Installazione dell'estensione [!DNLAdobe Launch]"
feature: "Mobile Marketing"
description: "[!DNL Adobe Launch] panoramica sull’installazione dell’estensione"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---


# [!DNL Adobe Launch] Installazione dell’estensione

Istruzioni di installazione per [!DNL Adobe Launch] Estensione Marketo. I passaggi seguenti sono necessari per inviare notifiche push e/o messaggi in-app.

## Prerequisiti

1. [Aggiungere un’applicazione in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin)
1. [Configurare la proprietà in [!DNL Adobe Launch] portale](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configurare la chiave segreta dell’applicazione e l’ID Munchkin per la proprietà in [!DNL Adobe Launch] portale
1. [Configurare le notifiche push](push-notifications.md) (facoltativo)

## Installare l’estensione Marketo su iOS

### Imposta intestazione di bridging Swift

1. Selezionare File > Nuovo > File e selezionare &quot;Header File&quot;.

1. Denomina il file &quot;&lt;_NomeProgetto_>-Bridging-Header&quot;

1. Vai a Progetto > Target > Impostazioni build > Compilatore Swift > Generazione del codice. Aggiungi il seguente percorso all’intestazione &quot;Objective-Bridging&quot; (Ponderazione obiettivo):

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Inizializza estensione

>[!BEGINTABS]

>[!TAB Obiettivo C]

Aggiornare il `applicationDidBecomeActive` metodo come sotto

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Aggiornare il `applicationDidBecomeActive` metodo come sotto

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## Dispositivi di prova iOS

1. Seleziona [!UICONTROL Project] > [!UICONTROL Target] > [!UICONTROL Info] > Tipi di URL.
1. Aggiungi identificatore: ${PRODUCT_NAME}
1. Impostare gli schemi URL: mkto-&lt;s_ecret key_=&quot;&quot;>
1. Includi `application:openURL:sourceApplication:annotation:` a `AppDelegate.m file` (Obiettivo C)

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

Segui le istruzioni in [!DNL Adobe Launch] portale

### Configurare le autorizzazioni

Apri `AndroidManifest.xml` e aggiungi le seguenti autorizzazioni. L&#39;app deve richiedere le autorizzazioni &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se l&#39;app richiede già queste autorizzazioni, salta questo passaggio.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Inizializza estensione

Configurazione ProGuard (opzionale)

Se utilizzi ProGuard per la tua app, aggiungi le seguenti righe nella `proguard.cfg` file. Il file si trova all’interno del tuo `project` cartella. L’aggiunta di questo codice esclude l’SDK Marketo dal processo di offuscamento.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Dispositivi di prova Android

Aggiungi &quot;MarketoActivity&quot; a `AndroidManifest.xml` all’interno del tag dell’applicazione.

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

Google Gli sviluppatori di app Android ora possono utilizzare direttamente i [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) con questo SDK.

### Aggiunta di FCM all’applicazione

1. Integra l’SDK Marketo Android più recente nell’app Android.  I passaggi sono disponibili all’indirizzo [GitHub](https://github.com/Marketo/android-sdk).
1. Configurare l’app Firebase nella console Firebase.
   1. Crea/Aggiungi un progetto su [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)Console Firebase.
      1. In [Console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), seleziona [!UICONTROL Add Project].
      1. Seleziona il progetto GCM dall’elenco dei progetti Google Cloud esistenti, quindi seleziona [!UICONTROL Add Firebase].
      1. Nella schermata di benvenuto di Firebase, seleziona &quot;Add Firebase to your Android App&quot; (Aggiungi Firebase all’app Android).
      1. Specifica il nome del pacchetto e SHA-1, quindi seleziona [!UICONTROL Add App]. Una nuova `google-services.json` per la tua app Firebase viene scaricato.
      1. Seleziona [!UICONTROL Continue] e segui le istruzioni dettagliate per l’aggiunta del plug-in Google Services in Android Studio.

   1. Vai a &quot;Impostazioni progetto&quot; in Panoramica progetto
      1. Fare clic sulla scheda Generale. Scarica il file `google-services.json` file.
      1. Fai clic sulla scheda &quot;Messaggistica cloud&quot;. Copia &quot;Chiave server&quot; e &quot;ID mittente&quot;. Fornisci questi &quot;Server Key&quot; e &quot;Sender ID&quot; a Marketo.
   1. Configurare le modifiche FCM nell’app Android
      1. Passa alla vista Progetto in Android Studio per visualizzare la directory principale del progetto
         1. Sposta il download `google-services.json` file nella directory principale del modulo app Android
         1. A livello di progetto `build.gradle` aggiungi quanto segue:

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

         1. Infine, fai clic su &quot;[!UICONTROL Sync now]&quot; nella barra visualizzata nell’ID
   1. Modifica il manifesto dell’app L’SDK FCM aggiunge automaticamente tutte le autorizzazioni richieste e la funzionalità di ricezione richiesta. Assicurati di rimuovere i seguenti elementi obsoleti (e potenzialmente dannosi, in quanto potrebbero causare la duplicazione dei messaggi) dal manifesto dell&#39;app:

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

**D: Dove posso trovare le istruzioni per l’aggiornamento alla versione più recente dell’SDK di MME?** Le istruzioni sono disponibili sul sito Marketo Developer [QUI](installation.md).

**D: L’aggiornamento alla versione più recente dell’SDK richiederà la pubblicazione di una versione aggiornata della mia applicazione Android per i miei utenti esistenti?** No.

**D: Che impatto ha sui clienti MME esistenti che hanno pubblicato app Android integrate con l’SDK Marketo per Android?** Possono migrare un’app client GCM esistente su Android a Firebase Cloud Messaging (FCM) come segue:

1. In [Console Firebase](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), seleziona [!UICONTROL Add Project].
1. Seleziona il progetto GCM dall’elenco dei progetti Google Cloud esistenti, quindi seleziona **[!UICONTROL Add Firebase]**.
1. Nella schermata di benvenuto di Firebase, seleziona **[!UICONTROL Add Firebase to your Android App]**.
1. Specifica il nome del pacchetto e SHA-1, quindi seleziona **[!UICONTROL Add App]**. Un nuovo file google-services.json per il tuo
1. L&#39;app Firebase viene scaricata.
1. Seleziona **[!UICONTROL Continue]** e segui le istruzioni dettagliate per l’aggiunta del plug-in Google Services in Android Studio.

**D: È possibile eseguire il targeting dei lead creati utilizzando il vecchio SDK di Marketo che utilizzava l’app GCM?** Sì. Tutti i lead creati con l’SDK di Marketo possono essere oggetto di targeting per l’invio di notifiche push.
