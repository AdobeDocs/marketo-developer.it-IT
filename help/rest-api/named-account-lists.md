---
title: Elenchi di account denominati
feature: REST API
description: Scopri come gestire gli elenchi di account denominati Marketo con l’API REST, che includono autorizzazioni, campi, filtri ed endpoint per eseguire query, creare, aggiornare ed eliminare.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
TQID: https://experienceleague.adobe.com/18lMhheW21Gz1-3TMHwleHhmLTOqJsZSQ5aqkbbchhM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 686
ht-degree: 2%

---

# Elenchi di account denominati

[Riferimento endpoint elenchi account denominati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Account-Lists)

[Gli elenchi di account denominati](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) sono insiemi di account denominati in Marketo. Utilizzali per la categorizzazione, l’arricchimento dei dati e il filtraggio intelligente delle campagne.

Le API per l’elenco degli account denominati consentono di gestire in remoto le risorse dell’elenco e la relativa appartenenza.
`Content`

## Autorizzazioni

L’autorizzazione richiesta dipende dall’operazione:

- Query Named Account Lists: elenco di account denominati di sola lettura o elenco di account denominati di lettura-scrittura.
- Creazione, aggiornamento o eliminazione di elenchi: Elenco account denominati in lettura e scrittura.
- Appartenenza all&#39;elenco di query: account denominato di sola lettura o account denominato di lettura-scrittura.
- Gestisci appartenenza a elenco: account denominato di lettura-scrittura.

## Modello

Gli elenchi account denominati dispongono di un set limitato di campi standard e non supportano i campi personalizzati.
`Named Account List Field`

| Nome | Tipo di dati | Aggiornabile | Note |
| --- | --- | --- | --- |
| marketoGUID | Stringa | False | Identificatore di stringa univoco dell’elenco degli account denominati. Questo campo è gestito dal sistema e non è consentito come campo durante la creazione di un record. Campo utilizzato da &quot;dedupeBy&quot;:&quot;idField&quot; durante l’esecuzione di una creazione o di un aggiornamento. |
| name | Stringa | True | Nome dell’elenco. Campo utilizzato da &quot;dedupeBy&quot;:&quot;dedupeFields&quot; durante l’esecuzione di una creazione o di un aggiornamento. |
| createdAt | Data e ora | False | Data e ora della creazione dell’elenco. Questo campo è gestito dal sistema e non è consentito come campo durante la creazione o l’aggiornamento di un record. |
| updatedAt | Data e ora | False | Datetime dell&#39;aggiornamento più recente dell&#39;elenco. Questo campo è gestito dal sistema e non è consentito come campo durante la creazione o l’aggiornamento di un record. |
| tipo | Stringa | False | Tipo dell’elenco. Può avere un valore &quot;default&quot; o &quot;external&quot;. Gli elenchi esterni sono quelli creati dalla Visualizzazione account CRM. |

## Query

Le query dell’elenco degli account denominati supportano due filterTypes: &quot;dedupeFields&quot; e &quot;idField&quot;. Imposta il campo nel parametro di query `filterType` e fornisci i valori in `filterValues as` in un elenco separato da virgole.

I filtri `nextPageToken` e `batchSize` sono facoltativi.

```http
GET /rest/v1/namedAccountLists.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fb,dff23271-f996-47d7-984f-f2676861b5fc
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      }
   ]
}
```

## Crea e aggiorna

Crea e aggiorna i record dell’elenco di conti denominati utilizzando il modello standard di database lead. Gli elenchi di account denominati dispongono di un solo campo aggiornabile: `name`.

L&#39;endpoint supporta due tipi di azioni standard: &quot;createOnly&quot; e &quot;updateOnly&quot;. `action defaults` a &quot;createOnly&quot;.

È possibile specificare l&#39;opzione `dedupeBy parameter` quando l&#39;azione è `updateOnly`. I valori consentiti sono &quot;dedupeFields&quot;, che corrisponde a &quot;name&quot;, e &quot;idField&quot;, che corrisponde a &quot;marketoGUID&quot;.

In modalità `createOnly`, solo &quot;name&quot; è consentito come campo `dedupeBy`. È possibile inviare fino a 300 record alla volta.

```http
POST /rest/v1/namedAccountLists.json
```

```json
{
   "action": "createOnly",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "name": "SAAS List"
      },
      {
         "name": "Manufacturing (Domestic)"
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

## Elimina

Eliminare gli elenchi di account denominati utilizzando `name` o `marketoGUID` dell&#39;elenco. Per selezionare la chiave, passare &quot;dedupeFields&quot; per il nome o &quot;idField&quot; per marketoGUID nel membro `deleteB` della richiesta.

Se non viene impostato, il valore predefinito è dedupeFields. È possibile eliminare fino a 300 record alla volta.

```http
POST /rest/v1/namedAccountLists/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
         "name": "Saas List"
      },
      {
         "name": "B2C List"
      },
      {
         "name": "Launchpoint Partner List"
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq": 1,
         "id": "dff23271-f996-47d7-984f-f2676861b5fc",
         "status": "deleted"
      },
      {
         "seq": 2,
         "status": "skipped",
         "reasons": [
            {
               "code": "1013",
               "message": "Record not found"
            }
         ]
      }
   ]
}
```

Se non è possibile trovare un record per una chiave, l&#39;elemento risultato corrispondente ha un `status` di &quot;ignorato&quot;. Include anche un motivo con un codice e un messaggio che descrivono l’errore.

## Gestione dell’iscrizione

### Iscrizione alla query

Eseguire una query sull&#39;appartenenza all&#39;elenco di account denominato fornendo `i` dell&#39;elenco di account. I parametri facoltativi sono:

-`field` - elenco di campi separati da virgole da includere nei record di risposta
-`nextPageToke` - per il paging attraverso il set di risultati
-`batchSiz` - per specificare il numero di record da restituire

Se `field` non è impostato, verranno restituiti `marketoGUI`, `nam`, `createdA` e `updatedA`. `batchSiz` ha un valore massimo e predefinito di 300.

```http
GET /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      }
   ]
}
```

### Aggiungi membri

Aggiungere account denominati a un elenco di account denominati utilizzando il relativo marketoGUID. È possibile aggiungere fino a 300 record alla volta.

```http
POST /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true,
}
```

### Rimuovi membri

La rimozione di record da un elenco di account utilizza un percorso diverso ma la stessa interfaccia. Fornisci un `marketoGUI` per ogni record da rimuovere. È possibile rimuovere fino a 300 record alla volta.

```http
POST /rest/v1/namedAccountList/{id}/namedAccounts/remove.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true
}
```

## Timeout

- Gli endpoint dell’elenco account denominati hanno un timeout di 30 secondi, a meno che non venga indicato diversamente.
- Timeout degli elenchi degli account denominati di Sync su 60 secondi.
- Il timeout per l’eliminazione degli elenchi di account denominati è di 60 secondi.
- Il timeout di Ottieni elenchi account denominati è di 60 secondi.
- Il timeout per Aggiungi membri dell’elenco account denominati è di 60 secondi.
- Timeout di rimozione membri dell’elenco account denominati su 60 secondi.
- Il timeout di Ottieni membri dell’elenco account denominati è di 60 secondi.
