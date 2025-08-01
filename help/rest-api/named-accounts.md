---
title: Account denominati
feature: REST API
description: Manipolare account denominati tramite l’API.
exl-id: 2aa1d2a0-9e54-4a9a-abb1-0d0479ed3558
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 0%

---

# Account denominati

[Riferimento endpoint account denominati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts)

Marketo offre un set di API per l’esecuzione di operazioni CRUD su account denominati da utilizzare con Marketo ABM. Queste API seguono il pattern di interfaccia standard per le API del database principale e forniscono le opzioni Describe, Create/Update, Delete e Query.

Attualmente, le uniche funzioni relative ad ABM disponibili tramite le API di Marketo sono le operazioni CRUD per gli account denominati; i lead non possono essere collegati ad account denominati tramite alcuna API.

## Descrivere

La descrizione degli account denominati restituisce i metadati relativi all’utilizzo degli account denominati tramite le API di Marketo, incluso un elenco di campi ricercabili validi durante la query e un elenco di tutti i campi disponibili per l’utilizzo API. Il `idField` di un account denominato è sempre `marketoGUID` e l&#39;unico `dedupeField` disponibile e chiave per la creazione è il campo `name` dell&#39;oggetto.

```
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

La query per gli account denominati si basa sull&#39;utilizzo di un filterType e di un set di un massimo di 300 filterValues separati da virgola. `filterType` può essere un singolo campo restituito nel membro `searchableFields` del risultato descritto per gli account denominati, mentre filterValues può essere un qualsiasi input valido per il tipo di dati del campo. Per restituire un set specifico di campi da, è necessario trasmettere un parametro fields, dove il valore è un elenco separato da virgole di campi da restituire nella risposta. Come altre opzioni di query, il numero massimo di record per una singola pagina di query è 300 e devono essere richiesti record aggiuntivi nel set con l’utilizzo del nextPageToken restituito dalla chiamata.

```
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

La creazione e l&#39;aggiornamento dei conti denominati seguono il modello standard del database dei lead. I record devono essere passati nel membro di input di un corpo JSON in una richiesta POST. `input` è l&#39;unico membro obbligatorio, con `action` e `dedupeBy` come membri facoltativi. Possono essere inclusi fino a 300 record. L&#39;azione può essere createOnly, updateOnly o createOrUpdate. Se non specificato, l&#39;impostazione predefinita è createOrUpdate. dedupeBy può essere specificato solo quando action è updateOnly e accetta solo uno dei campi dedupeFields o idField, che corrispondono rispettivamente ai campi name e marketoGUID.

```
POST /rest/v1/namedaccounts.json
```

```
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

L&#39;oggetto account denominato contiene un set di campi. Ogni definizione di campo è composta da un insieme di attributi che descrivono il campo. Esempi di attributi sono nome visualizzato, nome API e dataType. Questi attributi sono noti collettivamente come metadati.

I seguenti endpoint consentono di eseguire query sui campi dell’oggetto aziendale. Queste API richiedono che l’utente API proprietario abbia un ruolo con una o entrambe le autorizzazioni Campo standard schema di lettura-scrittura o Campo personalizzato schema di lettura-scrittura.

### Campi query

La query dei campi account denominati è semplice. È possibile eseguire una query su un singolo campo account denominato per nome API oppure sul set di tutti i campi aziendali.

#### Per nome

L&#39;endpoint [Get Named Account Field by Name](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera i metadati per un singolo campo nell&#39;oggetto account denominato. Il parametro di percorso fieldApiName obbligatorio specifica il nome API del campo. La risposta è simile all’endpoint Describe Named Account, ma contiene metadati aggiuntivi, come l’attributo isCustom, che indica se il campo è un campo personalizzato.

```
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

L&#39;endpoint [Recupera campi account denominati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Accounts/operation/getNamedAccountFieldByNameUsingGET) recupera i metadati per tutti i campi nell&#39;oggetto account denominato. Per impostazione predefinita, vengono restituiti al massimo 300 record. È possibile utilizzare il parametro di query batchSize per ridurre questo numero. Se l&#39;attributo moreResult è true, saranno disponibili più risultati. Continua a chiamare questo endpoint fino a quando l’attributo moreResult restituisce false, il che significa che non sono disponibili risultati. Il nextPageToken restituito da questa API deve sempre essere riutilizzato per la successiva iterazione di questa chiamata.

```
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

Le eliminazioni vengono eseguite tramite una richiesta POST JSON e hanno un membro di input obbligatorio e un membro deleteBy facoltativo. deleteBy può essere uno dei valori &quot;dedupeFields&quot; o &quot;idField&quot;, corrispondenti rispettivamente a name o marketoGUID e, se non impostato, verrà impostato automaticamente su dedupeFields. Il membro di input accetta un array di un massimo di 300 record, contenenti un membro ciascuno, nome o marketoGUID a seconda dell&#39;impostazione di deleteBy.

```
POST /rest/v1/namedaccounts/delete.json
```

```
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

- Gli endpoint denominati dell’account hanno un timeout di 30 secondi, a meno che non sia indicato di seguito
   - Sincronizza account denominati: 120s 
   - Elimina account denominati: 60s
