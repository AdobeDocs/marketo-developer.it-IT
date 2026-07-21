---
title: Account denominati
feature: REST API
description: Guida REST di Marketo a CRUD sugli account denominati per ABM, con descrizione, query, esempi di aggiornamento, campi ricercabili, regole di deduplicazione e nessun collegamento di lead.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
TQID: https://experienceleague.adobe.com/iY3UYVelm3aKuuDBCTxaVCbkXfwnJzDjV3Kvn9rcNbA
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 590
ht-degree: 1%

---

# Account denominati

[Riferimento endpoint account denominati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts)

Marketo fornisce API per l’esecuzione di operazioni CRUD su account denominati da utilizzare con Marketo ABM. Queste API seguono il pattern di interfaccia standard del database lead e forniscono le opzioni Describe, Create/Update, Delete e Query.

Attualmente, le API di Marketo supportano solo operazioni CRUD per gli account denominati. Non è possibile collegare i lead a account denominati tramite le API.

## Descrivere

Describe Named Accounts restituisce i metadati per l’utilizzo di account denominati tramite le API di Marketo. La risposta include campi validi in cui è possibile eseguire ricerche e tutti i campi disponibili per l’API.

Il `idField` di un account denominato è sempre `marketoGUID`. Il campo `name` dell&#39;oggetto è l&#39;unico `dedupeField` disponibile e la sola chiave di creazione.

```http
GET /rest/v1/namedaccounts/describe.json
```

```json
{
   "requestId":"d65e#156c27ac57d",
   "result":[
      {
         "name":"Named Account",
         "description":"Marketo standard account attribute map",
         "createdAt":"2016-08-18T20:16:41Z",
         "updatedAt":"2016-08-18T20:16:41Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "name"
         ],
         "searchableFields":[
            [
               "marketoGUID",
            ],
            [
               "annualRevenue"
            ],
            [
               "city"
            ],
            [
               "country"
            ],
            [
               "domainName"
            ],
            [
               "industry"
            ],
            [
               "logoUrl"
            ],
            [
               "membershipCount"
            ],
            [
               "name"
            ],
            [
               "numberOfEmployees"
            ],
            [
               "opptyAmount"
            ],
            [
               "opptyCount"
            ],
            [
               "score1"
            ],
            [
               "score2"
            ],
            [
               "score3"
            ],
            [
               "score4"
            ],
            [
               "score5"
            ],
            [
               "sicCode"
            ],
            [
               "state"
            ]
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"annualRevenue",
               "displayName":"annualRevenue",
               "dataType":"currency",
               "updateable":true
            },
            {
               "name":"city",
               "displayName":"city",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"country",
               "displayName":"country",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ],
   "success":true
}
```

### Query

Eseguire query sugli account denominati utilizzando un filterType e un massimo di 300 filterValues separati da virgola. Il filterType può essere un singolo campo restituito nel membro `searchableFields` della risposta Describe. Ogni voce filterValues deve essere un valore valido per il tipo di dati del campo.

Per restituire campi specifici, passa un parametro di campi con un elenco di campi separato da virgole. Una pagina query contiene un massimo di 300 record. Per recuperare record aggiuntivi, utilizza il valore nextPageToken restituito dalla chiamata.

```http
GET /rest/v1/namedaccounts.json?filterType=name&filterValues=Google,Yahoo
```

```json
{
    "requestId": "6dac#157d4ddc9d7",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "16efafdd-0148-4ea7-8782-f451d7c6345d",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Google",
            "updatedAt": "2016-10-17T22:49:04Z"
        },
        {
            "seq": 1,
            "marketoGUID": "44d62353-7f9d-4d43-b9cc-7ef0f7a09137",
            "createdAt": "2016-10-17T22:49:04Z",
            "name": "Yahoo",
            "updatedAt": "2016-10-17T22:49:04Z"
        }
    ],
    "success": true
}
```

### Crea e aggiorna

Creare e aggiornare conti denominati utilizzando il modello standard di database lead. Passa i record nel membro di input del corpo JSON di una richiesta POST. È possibile includere fino a 300 record.

I membri della richiesta sono:

- `input`: l&#39;unico membro richiesto.
- `action`: membro facoltativo che accetta createOnly, updateOnly o createOrUpdate. Il valore predefinito è createOrUpdate.
- `dedupeBy`: membro facoltativo disponibile solo quando action è updateOnly. Accetta dedupeFields o idField, che corrispondono rispettivamente ai campi name e marketoGUID.

```http
POST /rest/v1/namedaccounts.json
```

```text
Content-Type: application/json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "name":"Google",
         "domainName":"www.google.com"
      },
      {
         "name":"Yahoo",
         "domainName":"www.yahoo.com"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

### Campi

L’oggetto account denominato contiene campi definiti da attributi quali nome visualizzato, nome API e dataType. Insieme, questi attributi sono denominati metadati.

I seguenti campi di query degli endpoint sull’oggetto aziendale. L’utente API deve avere un ruolo con l’autorizzazione Campo standard dello schema di lettura-scrittura, l’autorizzazione Campo personalizzato dello schema di lettura-scrittura o entrambi.

### Campi query

Esegui la query di un campo account denominato per nome API o recupera tutti i campi aziendali.

#### Per nome

L&#39;endpoint [Get Named Account Field by Name](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera i metadati per un campo nell&#39;oggetto account denominato. Il parametro obbligatorio percorso fieldApiName specifica il nome API del campo.

La risposta è simile alla risposta Describe Named Account ma include metadati aggiuntivi. L&#39;attributo isCustom, ad esempio, indica se il campo è personalizzato.

```http
GET /rest/v1/namedaccounts/schema/fields/annualRevenue.json
```

```json
{
    "requestId": "371c#17e979c5d1f",
    "result": [
        {
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Sfogliare

L&#39;endpoint [Recupera campi account denominati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera i metadati per tutti i campi nell&#39;oggetto account denominato. Per impostazione predefinita, restituisce un massimo di 300 record. Utilizzare il parametro di query batchSize per ridurre questo numero.

Se l&#39;attributo moreResult è true, saranno disponibili più risultati. Continua a chiamare l’endpoint con il valore nextPageToken restituito fino a quando moreResult non è false.

```http
GET /rest/v1/namedaccounts/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "f287#17e995bd0c5",
    "result": [
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Domain Name",
            "name": "domainName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Industry",
            "name": "industry",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "SIC Code",
            "name": "sicCode",
            "description": null,
            "dataType": "string",
            "length": 40,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "City",
            "name": "city",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "N42LHXWEULHZ3N2I77DKOJUVOY======",
    "moreResult": true
}
```

### Elimina

Elimina gli account denominati inviando una richiesta POST con un corpo JSON. La richiesta include un membro di input obbligatorio e un membro deleteBy facoltativo.

Il membro deleteBy accetta &quot;dedupeFields&quot; o &quot;idField&quot;, che corrispondono rispettivamente a name e marketoGUID. Se non viene impostato, il valore predefinito è dedupeFields. Il membro di input accetta un massimo di 300 record. Ogni record contiene il nome o marketoGUID, a seconda dell&#39;impostazione deleteBy.

```http
POST /rest/v1/namedaccounts/delete.json
```

```text
Content-Type: application/json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "name":"Google"
      },
      {
         "name":"Yahoo"
      },
      {
         "name":"Marketo"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      },
      {
         "seq":1,
         "id":"dff23271-f996-47d7-984f-f2676861b5fc",
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Timeout

- Gli endpoint dell’account denominato hanno un timeout di 30 secondi, a meno che non venga indicato diversamente.
- Il timeout degli account denominati di sincronizzazione è di 120 secondi.
- L’eliminazione degli account denominati ha un timeout di 60 secondi.
