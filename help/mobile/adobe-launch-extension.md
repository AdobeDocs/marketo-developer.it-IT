---
title: Estensione Marketo Mobile per  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installa e configura l’estensione Marketo Mobile SDK in Adobe Launch per iOS e Android, inclusa la configurazione per notifiche push e messaggi in-app.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
TQID: https://experienceleague.adobe.com/Bk5GTnQjm6NDosl5Iw6TS-NRjH8owNRUKoE0mZ-H3pY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 303
ht-degree: 0%

---

# Estensione Marketo Mobile per [!DNL Adobe Launch]

Installa l&#39;estensione Marketo Mobile SDK in [!DNL Adobe Launch] per inviare notifiche push, messaggi in-app o entrambi.

## Prerequisiti

- [Aggiungi un&#39;applicazione in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) e ottieni la chiave segreta dell&#39;applicazione e l&#39;ID Munchkin.
- Seguire le istruzioni di installazione nel portale [!DNL Adobe Launch].
- Facoltativo: [Configurare le notifiche push](push-notifications.md).

## iOS

### Imposta intestazione di bridging Swift

1. Selezionare File > Nuovo > File e quindi &quot;File di intestazione&quot;.
1. Denomina il file &quot;&lt;_ProjectName_>-Bridging-Header&quot;.
1. Vai a Progetto > Target > Fasi build > Compilatore Swift > Generazione del codice.
1. Aggiungi il seguente percorso a Objective-Bridging Header:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Per Swift, rimuovi la seguente istruzione di importazione perché i passaggi precedenti aggiungono l’intestazione di bridging.

`import Marketo/ALMarketo`

### Dispositivi di prova iOS

Segui le istruzioni riportate in [Aggiunta di dispositivi di prova iOS](installation.md#ios_test_devices).

### Gestire il tipo di URL personalizzato in AppDelegate

Segui le [istruzioni URL personalizzate](installation.md#ios_test_devices).

### Configurare le notifiche push su iOS

Segui le [istruzioni per la notifica push](push-notifications.md). Utilizza il nome di classe &quot;ALMarketo&quot; invece di &quot;Marketo&quot;.

## Android

### Configurare le autorizzazioni

Apri `AndroidManifest.xml` e aggiungi le seguenti autorizzazioni. L&#39;app deve richiedere le autorizzazioni &quot;INTERNET&quot; e &quot;ACCESS_NETWORK_STATE&quot;. Ignora questo passaggio se l’app li richiede già.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### Configurazione ProGuard (opzionale)

Se l&#39;app utilizza ProGuard, aggiungere le righe seguenti al file `proguard.cfg` nella cartella del progetto. Questa configurazione esclude Marketo SDK dall’offuscamento.

```text
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Dispositivi di prova Android

Segui le istruzioni in [Dispositivi di prova Android](installation.md#android_test_devices).

## Configurare le notifiche push su Android

Segui le [istruzioni di Android Firebase Cloud Messaging](installation.md#android_firebase_cloud_messaging_support). Utilizza il nome di classe &quot;ALMarketo&quot; invece di &quot;Marketo&quot;.

Per configurare i profili utente, seguire le [istruzioni del profilo utente](user-profiles.md). Per impostare azioni personalizzate, seguire le [istruzioni per azioni personalizzate](custom-actions.md#android_custom_action). In entrambi i set di istruzioni, utilizzare il nome di classe &quot;ALMarketo&quot; invece di &quot;Marketo&quot;.
