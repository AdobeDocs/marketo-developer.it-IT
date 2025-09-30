---
title: Ottieni dati visitatore
description: Ottieni l’identificazione dei visitatori in tempo reale utilizzando l’API Contesto utente RTP con parametri, esempi di callback e risposte di esempio per segmenti, ABM e posizione.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 4%

---

# Ottieni dati visitatore

Questo metodo viene utilizzato per ottenere dati di identificazione dei visitatori in tempo reale.

- Prima di utilizzare l&#39;API Contesto utente, è necessario diventare un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.
- RTP non supporta gli elenchi di account denominati Account Based Marketing (Marketing basato su account). Gli elenchi e il codice ABM si riferiscono solo agli elenchi di account caricati (file CSV) gestiti all’interno di RTP.

Se si verifica un errore, viene visualizzato un messaggio di errore come parte della risposta JSON. Se viene restituito un codice 500, contatta l’assistenza per la richiesta effettuata.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| `get` | Obbligatorio | Stringa | Azione del metodo. |
| `visitor` | Obbligatorio | Stringa | Nome del metodo. |
| `callback` | Obbligatorio | Funzione | Funzione di callback da attivare per ogni campagna restituita. |

## Esempi

Ottenere i dati di identificazione del visitatore:

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Risposta con corrispondenza segmento:

Di seguito è riportato un esempio di risposta che viene restituita nel caso in cui il visitatore corrisponda ai segmenti in tempo reale prima della chiamata API Get Visitor Data.

```json
{
    "status": 200,
    "results": {
        "matchedSegments": [
            {
                "name": "first click",
                "id": 177
            }
        ],
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```

Risposta senza corrispondenza segmento:

Di seguito è riportato un esempio di risposta che viene restituita nel caso in cui il visitatore non corrisponda ad alcun segmento in tempo reale prima della chiamata API Get Visitor Data.

```json
{
    "status": 200,
    "results": {
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```
