---
title: "Elenchi di account denominati"
feature: REST API
description: "Configurare gli elenchi di account denominati."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 2%

---


# Elenchi di account denominati

[Riferimento endpoint elenchi account denominati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Elenchi di account denominati](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/target-account-management/target/account-lists) in Marketo rappresentano insiemi di account denominati. Possono essere utilizzati per un’ampia varietà di casi, tra cui categorizzazione, arricchimento dei dati e filtro intelligente delle campagne. Le API dell’elenco di account denominati consentono la gestione remota di queste risorse dell’elenco e della loro appartenenza.
`Content`

## Autorizzazioni

Per eseguire una query sugli elenchi di account denominati, è necessario disporre dell&#39;autorizzazione Elenco account denominati di sola lettura o Elenco account denominati di lettura/scrittura. Per creare, aggiornare o eliminare elenchi, è necessaria l&#39;autorizzazione Lettura/scrittura elenco account denominati. La query dell&#39;appartenenza a un elenco richiede le autorizzazioni Account denominato di sola lettura o Account denominato di lettura-scrittura, mentre la gestione dell&#39;appartenenza richiede le autorizzazioni Account denominato di lettura-scrittura.

## Modello

Gli elenchi account denominati dispongono di un numero limitato di campi standard e non sono estensibili con i campi personalizzati.
`Named Account List Field`

| Nome | Tipo di dati | Aggiornabile | Note |
|---|---|---|---|
| marketoGUID | Stringa | Falso | Identificatore di stringa univoco dell’elenco degli account denominati. Questo campo è gestito dal sistema e non è consentito come campo durante la creazione di un record. Campo utilizzato da &quot;dedupeBy&quot;:&quot;idField&quot; durante l’esecuzione di una creazione o di un aggiornamento. |
| name | Stringa | Vero | Nome dell’elenco. Campo utilizzato da &quot;dedupeBy&quot;:&quot;dedupeFields&quot; durante l’esecuzione di una creazione o di un aggiornamento. |
| createdAt | Data e ora | Falso | Data e ora della creazione dell’elenco. Questo campo è gestito dal sistema e non è consentito come campo durante la creazione o l’aggiornamento di un record. |
| updatedAt | Data e ora | Falso | Datetime dell&#39;aggiornamento più recente dell&#39;elenco. Questo campo è gestito dal sistema e non è consentito come campo durante la creazione o l’aggiornamento di un record. |
| tipo | Stringa | Falso | Tipo dell’elenco. Può avere un valore &quot;default&quot; o &quot;external&quot;. Gli elenchi esterni sono quelli creati dalla Visualizzazione account CRM. |


## Query

La query degli elenchi di account è semplice. Attualmente, esistono solo due filterTypes validi per eseguire query sugli elenchi di account denominati: &quot;dedupeFields&quot; e &quot;idField&quot;. Il campo su cui filtrare è impostato in `filterType` e i valori sono impostati in `filterValues as` un elenco separato da virgole. Il `nextPageToken` e `batchSize` I filtri sono anche parametri facoltativi.

```
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

La creazione e l&#39;aggiornamento dei record dell&#39;elenco dei conti denominati seguono i modelli stabiliti per altre operazioni di creazione e aggiornamento del database lead. Tieni presente che gli elenchi di account denominati hanno un solo campo aggiornabile, `name`.

L’endpoint consente due tipi di azione standard: &quot;createOnly&quot; e &quot;updateOnly&quot;.  Il `action defaults` su &quot;createOnly&quot;.

L&#39;opzione `dedupeBy parameter` può essere specificato se l&#39;azione è `updateOnly`.  I valori consentiti sono &quot;dedupeFields&quot; (corrispondente a &quot;name&quot;) o &quot;idField&quot; (corrispondente a &quot;marketoGUID&quot;).  In entrata `createOnly` , è consentito solo &quot;name&quot; come `dedupeBy` campo. È possibile inviare fino a 300 record alla volta.

```
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

L’eliminazione degli elenchi di account denominati è semplice e può essere eseguita in base al `name`o `marketoGUID` dell&#39;elenco. Per selezionare la chiave da utilizzare, passare &quot;dedupeFields&quot; per il nome o &quot;idField&quot; per marketoGUID nel`deleteB` membro della tua richiesta. Se non viene impostato, per impostazione predefinita verranno utilizzati dedupeFields. È possibile eliminare fino a 300 record alla volta.

```
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

Nel caso in cui non sia possibile trovare un record per una determinata chiave, l’elemento risultante corrispondente avrà un`status` di &quot;ignorato&quot; e un motivo con un codice e un messaggio che descrivono l’errore, come mostrato nell’esempio precedente.

## Gestione dell’iscrizione

### Iscrizione alla query

La query dell’appartenenza a un elenco di account denominati è semplice e richiede solo`i` dell’elenco dei conti. I parametri facoltativi sono:

-`field` - un elenco separato da virgole di campi da includere nei record di risposta -`nextPageToke` - per scorrere il set di risultati -`batchSiz` - per specificare il numero di record da restituire

Se`field` non è impostato, quindi`marketoGUI`,`nam`, `createdA`, e`updatedA` verrà restituito. `batchSiz` ha un valore massimo e predefinito di 300.

```
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

Gli account denominati possono essere facilmente aggiunti a un elenco di account denominati. Gli account possono essere aggiunti solo utilizzando il relativo marketoGUID. È possibile aggiungere fino a 300 record alla volta.

```
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

La rimozione di record da un elenco account ha un percorso diverso, ma la stessa interfaccia richiede un`marketoGUI` per ogni record da eliminare. È possibile rimuovere fino a 300 record alla volta.

```
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

- Gli endpoint dell’elenco account denominati hanno un timeout di 30 secondi, a meno che non sia indicato di seguito
   - Elenchi account denominati di sincronizzazione: 60s 
   - Elimina elenchi account denominati: 60s 
   - Ottieni elenchi account denominati: anni 60 
   - Aggiungi membri elenco account denominati: 60s 
   - Rimuovi membri elenco account denominati: 60s 
   - Ottieni membri elenco account denominati: 60s
