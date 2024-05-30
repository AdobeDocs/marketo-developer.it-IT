---
title: "Modalità di accesso sicurezza avanzata"
feature: "Mobile Marketing"
description: "Dettagli sulla modalità di accesso di sicurezza avanzata"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---


# Modalità di accesso protezione avanzata

L’SDK di Marketo espone i metodi per impostare e rimuovere la firma di sicurezza. Esiste anche un metodo di utilità per recuperare l’ID dispositivo. Al momento dell’accesso, l’ID dispositivo deve essere trasmesso insieme all’e-mail al server clienti per essere utilizzato nel calcolo della firma di sicurezza. L’SDK deve individuare il nuovo endpoint, che punta all’algoritmo elencato sopra, per recuperare i campi necessari per creare un’istanza dell’oggetto firma. L&#39;impostazione di questa firma nell&#39;SDK è un passaggio necessario se la modalità di accesso alla sicurezza è stata abilitata in Marketo Mobile Admin.

## Impostazione modalità accesso protetto

Questa configurazione deve essere implementata prima che la modalità Accesso sicuro sia stata abilitata tramite la pagina Marketo Admin > Mobile Apps &amp; Devices (Amministrazione > App e dispositivi mobili). Di seguito sono riportati ulteriori passaggi che descrivono il processo necessario per completare il processo di convalida della sicurezza.

La modalità di accesso sicuro richiede l’implementazione dell’algoritmo di firma sul lato server del cliente, che fornirà un endpoint per recuperare la chiave di accesso, la firma calcolata, la marca temporale di scadenza e l’e-mail. Questo algoritmo richiede la chiave di accesso utente, il segreto di accesso, l’e-mail, la marca temporale e l’ID dispositivo per eseguire il calcolo. Il cliente è responsabile della configurazione dell’endpoint, dell’implementazione dell’algoritmo per eseguire i calcoli della firma e per mantenere aggiornata la marca temporale della scadenza.

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

L’SDK di Marketo espone nuovi metodi per impostare e rimuovere la firma di sicurezza. Esiste anche un metodo di utilità per recuperare l’ID dispositivo. Al momento dell’accesso, l’ID dispositivo deve essere trasmesso insieme all’e-mail al server clienti per essere utilizzato nel calcolo della firma di sicurezza. L’SDK deve individuare il nuovo endpoint, che punta all’algoritmo elencato sopra, per recuperare i campi necessari per creare un’istanza dell’oggetto firma. L&#39;impostazione di questa firma nell&#39;SDK è un passaggio necessario se la modalità di accesso alla sicurezza è stata abilitata in Marketo Mobile Admin.

### iOS

```
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

```
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

```
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
