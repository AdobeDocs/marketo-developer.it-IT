---
title: Estensione Marketo Mobile per  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installa e configura l’estensione Marketo Mobile SDK in Adobe Launch per iOS e Android, inclusa la configurazione per notifiche push e messaggi in-app.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Estensione Marketo Mobile per [!DNL Adobe Launch]

Istruzioni di installazione per l&#39;estensione Marketo Mobile SDK in [!DNL Adobe Launch]. I passaggi seguenti sono necessari per inviare notifiche push e/o messaggi in-app.

## Prerequisiti

- [Aggiungi un&#39;applicazione in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin)
- Seguire le istruzioni fornite nel portale [!DNL Adobe Launch] per l&#39;installazione
- [Imposta notifiche push](push-notifications.md) (facoltativo)

## iOS

### Imposta intestazione di bridging Swift

1. Selezionare File > Nuovo > File e selezionare &quot;Header File&quot;.
1. Denomina il file &quot;&lt;_ProjectName_>-Bridging-Header&quot;.
1. Vai a Progetto > Target > Fasi build > Compilatore Swift > Generazione del codice. Aggiungi il seguente percorso a Objective-Bridging Header:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Per gli utenti Swift: rimuovi la seguente istruzione di importazione, poiché l’intestazione di bridging viene aggiunta nei passaggi precedenti.

`import Marketo/ALMarketo`

### Dispositivi di prova iOS

Segui le istruzioni in [Aggiunta di dispositivi di prova iOS](installation.md#ios_test_devices)

### Gestire il tipo di URL personalizzato in AppDelegate

Segui le istruzioni [qui](installation.md#ios_test_devices)

### Configurare le notifiche push su iOS

Segui le istruzioni [qui](push-notifications.md) e utilizza il nome di classe &quot;ALMarketo&quot; invece di &quot;Marketo&quot;

## Android

### Configurare le autorizzazioni

Apri `AndroidManifest.xml` e aggiungi le seguenti autorizzazioni. L&#39;app deve richiedere le autorizzazioni &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Se l&#39;app richiede già queste autorizzazioni, salta questo passaggio.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configurazione ProGuard (opzionale)

Se utilizzi ProGuard per l&#39;app, aggiungi le seguenti righe nel file `proguard.cfg`. Il file si trova all’interno della cartella del progetto. L’aggiunta di questo codice esclude Marketo SDK dal processo di offuscamento.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Dispositivi di prova Android

Segui le istruzioni [qui](installation.md#android_test_devices)

## Configurare le notifiche push su Android

Segui le istruzioni [qui](installation.md#android_firebase_cloud_messaging_support) e utilizza il nome di classe &quot;ALMarketo&quot; invece di &quot;Marketo&quot;

Per la configurazione dei profili utente seguire le istruzioni [qui](user-profiles.md) e per le azioni personalizzate seguire le istruzioni [qui](custom-actions.md#android_custom_action). Nelle istruzioni seguenti, utilizza il nome di classe &quot;ALMarketo&quot; invece di &quot;Marketo&quot;
