---
title: Modalità di accesso protezione avanzata
feature: Mobile Marketing
description: Scopri la modalità di accesso avanzata con sicurezza per Marketo Mobile SDK, con la generazione della firma HMAC, la configurazione dell’endpoint del server, l’utilizzo dell’ID dispositivo ed esempi di iOS e Android.
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
TQID: https://experienceleague.adobe.com/F6lH1aGbCakK-E6IU4wLwYw58BG2-CRE-Ras2bMHeO8
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 217
ht-degree: 1%

---

# Modalità di accesso protezione avanzata

La modalità di accesso avanzata per la protezione richiede che Marketo SDK recuperi e imposti una firma di protezione. SDK fornisce metodi per impostare e rimuovere la firma e un metodo di utilità per recuperare l&#39;ID dispositivo.

Al momento dell’accesso, invia l’ID dispositivo e l’indirizzo e-mail al server clienti per calcolare la firma di sicurezza. SDK chiama quindi l’endpoint cliente per recuperare i campi necessari per creare un’istanza dell’oggetto firma. Se la modalità di accesso Sicurezza è abilitata in Marketo Mobile Admin, è necessario impostare questa firma in SDK.

## Impostazione modalità accesso protetto

Implementa questa configurazione prima di abilitare la modalità Accesso sicuro nella pagina Amministrazione di Marketo > App e dispositivi mobili.

La modalità Accesso sicuro richiede un algoritmo di firma lato server e un endpoint cliente. L’endpoint restituisce i seguenti valori:

- Chiave di accesso
- Firma calcolata
- Timestamp di scadenza
- Indirizzo e-mail

L’algoritmo utilizza la chiave di accesso dell’utente, il segreto di accesso, l’indirizzo e-mail, la marca temporale e l’ID dispositivo. Il cliente deve impostare l’endpoint, implementare il calcolo della firma e mantenere corrente il timestamp di scadenza.

```python
import argparse
import datetime
import hashlib
import hmac


ACCESS_KEY = 'Your Access Key'
ACCESS_SECRET = 'Your access secret'

# Key should not be unicode
def get_signing_key(timestamp):
    return 'MKTO' + ACCESS_SECRET + str(timestamp)

def get_string_to_sign(email, uuid):
    return email + uuid

def get_hmac(key, string_to_sign):
    return hmac.new(key, string_to_sign.encode('utf-8'), hashlib.sha256).hexdigest()

def get_epoch_plus_day():
    epoch = datetime.datetime.utcfromtimestamp(0)
    valid_until_dt = datetime.datetime.utcnow() + datetime.timedelta(days=1)
    return long((valid_until_dt - epoch).total_seconds())

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--email", required=True, help="email address")
    parser.add_argument("-u", "--uuid", required=True, help="Device install id")
    parser.add_argument("-t", "--timestamp", type=int, help="Valid until timestamp")
    args = parser.parse_args()
    string_to_sign = get_string_to_sign(args.email, args.uuid)
    if not args.timestamp:
        valid_until = get_epoch_plus_day()
    else:
        valid_until = args.timestamp
    signing_key = get_signing_key(valid_until)
    hmac_string = get_hmac(signing_key, string_to_sign)
    print 'HMAC is ', hmac_string
```

Utilizza i metodi specifici della piattaforma per impostare o rimuovere la firma di sicurezza e recuperare l’ID dispositivo.

### iOS

```objectivec
Marketo * sharedInstance =[Marketo sharedInstance];

// set secure signature
MKTSecuritySignature *signature =
[[MKTSecuritySignature alloc] initWithAccessKey:<ACCESS_KEY> signature:<SIGNATURE_TOKEN> timestamp:<EXPIRY_TIMESTAMP> email:<EMAIL>];
[sharedInstance setSecureSignature:signature];

// remove signature
[sharedInstance removeSecureSignature];

// get device id
[sharedInstance getDeviceId];
```

```swift
let sharedInstance = Marketo.sharedInstance()

 // set secure signature
let signature = MKTSecuritySignature(accessKey: <ACCESS_KEY>, signature: <SIGNATURE_TOKEN> , timestamp: <EXPIRY_TIMESTAMP>, email: <EMAIL>)
sharedInstance.setSecureSignature(signature)

// remove signature
[sharedInstance removeSecureSignature];

// get device id
sharedInstance.getDeviceId()
```

### Android

```java
Marketo sdk = Marketo.getInstance(getApplicationContext());

// set signature
MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
secureMode.setAccessKey(<ACCESS_KEY>);
secureMode.setEmail(<EMAIL_ADDRESS>);
secureMode.setSignature(<SIGNATURE_TOKEN>);
secureMode.setTimestamp(<EXPIRY_DATE>);
if (secureMode.isValid()) {
  sdk.setSecureSignature(secureMode);
}

// remove signature
sdk.removeSecureSignature();

// get device id
sdk.getDeviceId();
```
