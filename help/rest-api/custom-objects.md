---
title: Oggetti personalizzati
feature: REST API, Custom Objects
description: Creazione e modifica di oggetti Marketo personalizzati.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '2909'
ht-degree: 0%

---

# Oggetti personalizzati

[**Riferimento endpoint oggetto personalizzato**](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects) Marketo consente agli utenti di definire oggetti personalizzati di Marketo correlati a oggetti standard di Marketo (lead, società) o ad altri oggetti personalizzati di Marketo.  Gli oggetti personalizzati di Marketo possono essere creati utilizzando l&#39;interfaccia utente di Marketo come descritto [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) oppure utilizzando l&#39;API metadati oggetti personalizzati come descritto di seguito.

Per accedere all’API dei metadati dell’oggetto personalizzato è necessario un tipo di abbonamento Marketo appropriato.  Per ulteriori informazioni, consulta il tuo CSM.

## Elenco

Oltre alle chiamate standard di descrizione, query, aggiornamento ed eliminazione disponibili per gli oggetti di database lead, per gli oggetti personalizzati è disponibile una [chiamata elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET).  La chiamata di questo endpoint restituisce una risposta con un elenco di oggetti personalizzati disponibili nell’istanza di destinazione, insieme a metadati aggiuntivi sugli oggetti.

```
GET /rest/v1/customobjects.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "relatedTo":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
         ]
      }
   ]
}
```

La risposta fornisce un elenco delle relazioni presenti su ciascun oggetto.  Una relazione avrà un membro `field` che indica il campo sull&#39;oggetto che contiene il valore del collegamento, un membro `type` che indica se la relazione è con un oggetto di tipo padre o figlio e un oggetto `relatedTo` che indica il nome dell&#39;oggetto correlato e il campo del collegamento su tale oggetto.

## Descrivere

La chiamata [description](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) per gli oggetti personalizzati segue lo stesso pattern di Opportunità e Aziende, con l&#39;aggiunta dell&#39;array `relationships` nella risposta e di un parametro di percorso `apiName` nell&#39;URI che accetta il nome API del tipo di oggetto personalizzato da descrivere.  Analogamente alla chiamata elenco, verranno elencate tutte le relazioni disponibili per questo tipo di oggetto personalizzato.

```
GET /rest/v1/customobjects/{apiName}/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"Car",
         "displayName":"Car",
         "description":"Car owner",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":["vin"],
         "searchableFields":[
            ["vin"],
            ["marketoGUID"],
            ["siebelId"]
         ],
         "relationships":[
            {
               "field":"siebelId",
               "type":"parent",
               "object":{
                  "name":"Lead",
                  "field":"siebelId"
               }
            }
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
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"vin",
               "displayName":"VIN",
               "description":"Vehicle Identification Number",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"siebelId",
               "displayName":"External Id",
               "description":"External Id",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"make",
               "displayName":"Make",
               "dataType":"string",
               "length":36,
               "updateable":true
            },
            {
               "name":"model",
               "displayName":"Model",
               "description":"Vehicle Model",
               "dataType":"string",
               "length":255,
               "updateable":true
            },
            {
               "name":"year",
               "displayName":"Year",
               "dataType":"integer",
               "updateable":true
            },
            {
               "name":"color",
               "displayName":"Color",
               "description":"Vehicle color",
               "dataType":"String",
               "length": 255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Query

[La query degli oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET) è leggermente diversa dalle altre API del database lead e accetta il parametro del percorso `apiName` come descritto.  Per i normali parametri filterType, la query è una semplice query di tipo GET per altri tipi di record e richiede `filterType` e `filterValues`.  Accetterà facoltativamente `**fields**`, `batchSize` e `nextPageToken` parametri.  Quando si richiede un elenco di campi, se un particolare campo viene richiesto ma non restituito, il valore deve essere nullo.

```
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "vin":"19UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "vin":"29UYA31581L000000",
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
   ]
}
```

In alternativa, quando si esegue una query con chiavi composte, l’API si comporta come l’API Ruoli opportunità, accettando un POST con un corpo JSON.  Il corpo JSON può avere gli stessi membri di una query GET, ad eccezione di `filterValues`.  Anziché filtrare i valori, è disponibile un array `input` che accetta gli oggetti che contengono un membro denominato per ogni `dedupeFields` del tipo di oggetto.

```
POST /rest/v1/customobjects/{apiName}.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "Bedrooms",
      "yearBuilt"
   ],
   "input":[
      {
         "mlsNum":"1962352",
         "houseOwnerId":"42645756"
      },
      {
         "mlsNum":"2962352",
         "houseOwnerId":"52645756"
      },
      {
         "mlsNum":"3962352",
         "houseOwnerId":"62645756"
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
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa",
         "Bedrooms":3,
         "yearBuilt":1948,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "Bedrooms":4,
         "yearBuilt":1956,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      },
      {
         "seq":2,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc",
         "Bedrooms":3,
         "yearBuilt":2001,
         "createdAt":"2015-02-23T18:21:53Z",
         "updatedAt":"2015-02-23T18:23:41Z"
      }
   ]
}
```

## Crea e aggiorna

Utilizzare l&#39;endpoint [sincronizza oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) per creare o aggiornare oggetti personalizzati. È possibile specificare l&#39;operazione utilizzando il parametro `action`.  È possibile creare o aggiornare fino a 300 record con una sola chiamata.  I valori utilizzati nell&#39;array `input` si basano in gran parte sulle informazioni restituite dall&#39;endpoint [Descrivi oggetti personalizzati](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/descriptionUsingGET_1). In un oggetto car di esempio è presente un solo campo di deduplicazione, `vin`.  Per aggiornare o creare record quando si utilizza la modalità dedupeFields, ogni record nell&#39;array di input deve includere almeno un campo `vin`.

```
POST /rest/v1/customobjects/{apiName}.json
```

```json
{
   "action":"updateOnly",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000",
         "siebelId":"f2676861b5fb",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"29UYA31581L000000",
         "siebelId":"f2676861b5fc",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
      },
      {
         "vin":"39UYA31581L000000",
         "siebelId":"f2676861b5fd",
         "make":"BMW",
         "model":"3-Series 330i",
         "year":2003
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
         "status": "updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status": "created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1004",
               "message":"Lead not found"
            }
         ]
      }
   ]
}
```

Quando si eseguono aggiornamenti tramite la modalità `idField`, `idField` sarà sempre `marketoGUID`, quindi ogni record richiederà sempre un campo `marketoGUID`.  Ricorda che `idField` è valido solo per il tipo di azione updateOnly, in quanto questo campo è sempre gestito dal sistema.  La risposta includerà lo **stato** di ogni singolo record nell&#39;array dei risultati e un array `marketoGUID` o `reasons` a seconda che l&#39;operazione sia stata eseguita correttamente per un singolo record.

## Elimina

[Eliminare i record](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) è molto semplice.  Seleziona la modalità `deleteBy`, `idField` o `dedupeFields`, e includi i campi corrispondenti in ciascuno dei record dell&#39;array `input`. È consentito un massimo di 300 record per chiamata.

```
POST /rest/v1/customobjects/{apiName}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}

{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

Come per l&#39;aggiornamento, il risultato conterrà uno stato per ogni singolo record e un array `marketoGUID` o `reasons` a seconda che l&#39;eliminazione sia avvenuta correttamente.

## Tipi di oggetti personalizzati

L’API dei metadati di oggetti personalizzati consente di gestire in remoto gli schemi di oggetti personalizzati.  L’API ti consente di creare un nuovo Tipo di oggetto personalizzato o di modificarne uno esistente.  Una volta creato o modificato, il tipo di oggetto personalizzato deve essere approvato per l&#39;uso.  Per ulteriori informazioni sugli oggetti personalizzati, consulta la documentazione del prodotto [qui](https://experienceleague.adobe.com/it/docs/marketo/using/home).

* I tipi di oggetto personalizzati creati dall’API non possono essere modificati utilizzando l’interfaccia utente di Marketo
* Il numero massimo di tipi di oggetti personalizzati consentiti è 10
* Il numero massimo di campi oggetto personalizzati consentiti è 50 per tipo
* I nomi API e i nomi visualizzati del tipo di oggetto personalizzato possono contenere caratteri alfanumerici e il carattere di sottolineatura &quot;_&quot;

### Tipo di query

Esistono due modi per recuperare i metadati del tipo di oggetto personalizzato: Descrivere il tipo di oggetto personalizzato, che restituisce  il record per un singolo tipo di oggetto personalizzato, filtrabile per stato di approvazione, e Elenca tipi di oggetto personalizzati, che restituisce un elenco di tutti i tipi di oggetto personalizzati nella sottoscrizione ed è filtrabile per nome e per stato di approvazione.

### Descrivi tipo

L&#39;endpoint [Describe Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) restituisce metadati per un singolo tipo di oggetto personalizzato. Il parametro di percorso `apiName` richiesto è il nome API del tipo di oggetto personalizzato descritto.  Se esiste una versione approvata, viene restituita.  In caso contrario, viene restituita la versione bozza.  Il parametro facoltativo `state` viene utilizzato per specificare la versione da restituire: `draft`, `approved` o `approvedWithDraft`.

```
GET /rest/v1/customobjects/schema/{apiName}/describe.json?state=approved
```

```json
{
    "requestId": "d9bf#16876fa84b9",
    "result": [
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "Automobile owned",
            "apiName": "car",
            "idField": "marketoGUID",
            "createdAt": "2019-01-22T19:12:18Z",
            "updatedAt": "2019-01-22T19:12:18Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Qui è possibile visualizzare i seguenti attributi:

* Metadati: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Campi standard: marketoGUID, createdAt, updatedAt
* Campi personalizzati leadId, vin, make,  modello, anno

### Tipi di elenco

L&#39;endpoint [List Custom Object Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) restituisce i metadati per tutti i tipi di oggetto personalizzati disponibili nell&#39;istanza di destinazione.  Questo endpoint è simile a [Elenca oggetti personalizzati](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=en), ma è più completo e include metadati aggiuntivi come stato, relazioni e campi. Se esiste una versione approvata, viene restituita.  In caso contrario, viene restituita la versione bozza.  Il parametro facoltativo **state** viene utilizzato per specificare la versione del tipo di oggetto personalizzato da restituire: **draft**, **approval** o **approvalWithDraft**.  Il parametro facoltativo **names** viene utilizzato per specificare nomi specifici di tipi di oggetti personalizzati da restituire; è strutturato come un elenco separato da virgole di nomi API.

```
GET /rest/v1/customobjects/schema.json?names=purchaseHistory
```

```json
{
    "requestId": "a181#167ebe94703",
    "result": [
        {
            "state": "approved",
            "displayName": "Purchases",
            "description": "Purchase data",
            "apiName": "purchaseHistory",
            "idField": "marketoGUID",
            "createdAt": "2014-09-12T16:13:37Z",
            "updatedAt": "2014-09-12T16:13:42Z",
            "dedupeFields": [
                "lead_id",
                "product_name"
            ],
            "searchableFields": [
                [
                    "lead_id",
                    "product_name"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "lead_id"
                ]
            ],
            "relationships": [
                {
                    "field": "lead_id",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "lead_id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "marketoGUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "amount",
                    "displayName": "Amount",
                    "dataType": "float",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "lead_id",
                    "displayName": "lead_id",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "product_name",
                    "displayName": "Product Name",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "purchase_date",
                    "displayName": "Transaction Date",
                    "dataType": "datetime",
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        },
        {
            "state": "approved",
            "version": "approved",
            "displayName": "Car",
            "description": "No really, it's a car!",
            "apiName": "car_c",
            "idField": "marketoGUID",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2018-12-11T23:52:56Z",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "year",
                    "displayName": "Year",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

### Crea e aggiorna tipo

#### Crea tipo

L&#39;endpoint [Sync Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) viene utilizzato per creare o aggiornare un tipo di oggetto personalizzato.  L&#39;operazione record da eseguire è controllata dall&#39;attributo facoltativo **action** che può essere **createOnly**, **createOrUpdate** o **updateOnly**.  L&#39;impostazione predefinita è createOrUpdate. Gli attributi **displayName** e **apiName** sono obbligatori, tranne quando si utilizza updateOnly come azione.   Entrambi devono essere univoci per evitare conflitti con i tipi con provisioning del cliente.  Se sei un partner LaunchPoint, devi anteporre a questi nomi uno spazio dei nomi rappresentativo.  Per apiName, la convenzione consiste nell’utilizzare lettere minuscole o camelCase per distinguere altre stringhe di testo. L&#39;attributo facoltativo **pluralName** specifica la forma plurale di displayName.  L&#39;attributo facoltativo **description** viene utilizzato per descrivere il tipo di oggetto personalizzato.  L&#39;attributo booleano facoltativo **showInLeadDetail** viene utilizzato per abilitare la visualizzazione dei dati oggetto personalizzati nella pagina del database lead dell&#39;interfaccia utente di Marketo.  L&#39;impostazione predefinita è false.

Presta attenzione quando denomini gli oggetti personalizzati. Durante la creazione di un nuovo oggetto personalizzato, si consiglia di anteporre al nome un prefisso con una stringa che indica il nome dell’azienda (alfanumerico o trattino basso consentito). Questo rende l&#39;oggetto personalizzato facilmente ricercabile nell&#39;interfaccia utente di MLM e aiuta anche a non essere sicuri che il nome sia univoco.

Esempio di creazione di un nuovo tipo di oggetto personalizzato con API  Nome &quot;transazione&quot;.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"createOnly",
  "displayName": "Transaction",
  "apiName": "transaction",
  "description": "Commerce happens"
}
```

```json
{
    "requestId": "fb9d#167f2879557",
    "result": [],
    "success": true
}
```

Chiamata successiva per descrivere il tipo appena creato.

```
GET /rest/v1/customobjects/schema/transaction/describe.json
```

```json
{
    "requestId": "cf9b#167f28db0a9",
    "result": [
        {
            "state": "draft",
            "displayName": "Transaction",
            "description": "Commerce happens",
            "apiName": "transaction",
            "idField": null,
            "createdAt": null,
            "updatedAt": null,
            "dedupeFields": [],
            "searchableFields": [
                []
            ],
            "relationships": [],
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Qui possiamo vedere i seguenti dati personalizzati relativi agli oggetti:

* Metadati: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations
* Campi standard: marketoGUID, createdAt, updatedAt

#### Tipo di aggiornamento

Di seguito è riportato un esempio di aggiornamento della Descrizione per un tipo esistente il cui Nome API è &quot;transaction&quot;.  L&#39;attributo **apiName** è obbligatorio.  In questo caso si presuppone che il tipo esista già e si utilizza updateOnly per l&#39;attributo facoltativo **action**.  Oltre a **apiName**, è possibile aggiornare gli attributi disponibili per la creazione.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
  "action":"updateOnly",
  "apiName": "transaction",
  "description":"No really, commerce happens!"
}
```

```json
{
    "requestId": "103c3#167f2223fd7",
    "result": [],
    "success": true
}
```

## Omologazione del tipo

I tipi di oggetto personalizzati devono essere approvati prima di poter essere utilizzati. Quando viene creato un nuovo tipo di oggetto personalizzato utilizzando l&#39;endpoint [Sincronizza tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST), viene creato come versione bozza. Al termine dell’aggiunta dei campi personalizzati, devi approvare la versione bozza. In questo modo viene creata una versione approvata ed eliminata la bozza di versione. Quando un tipo di oggetto personalizzato esistente viene modificato utilizzando l&#39;endpoint Sync Custom Object Type o uno degli endpoint Add/Update/Delete Custom Object Type Field, viene creata una bozza della versione. Tutte le modifiche al tipo o ai relativi campi influiscono solo sulla versione bozza. Al termine della modifica, dovete approvare la versione bozza. In questo modo la versione approvata viene sostituita con la versione bozza ed eliminata. Per ulteriori informazioni sull&#39;approvazione degli oggetti personalizzati, consulta la documentazione del prodotto [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Una volta approvato un tipo di oggetto personalizzato, non è possibile:

* Aggiorna `displayName` o `apiName`
* Aggiungere o rimuovere un campo collegamento
* Aggiungere o rimuovere un campo di deduplicazione

Per questi motivi, è importante considerare attentamente lo schema e la convenzione di denominazione che intendi utilizzare.

### Approva tipo

Utilizza l&#39;endpoint [Approve Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) per pubblicare una bozza di versione come nuova versione approvata.  **apiName** è l&#39;unico parametro obbligatorio come parametro di percorso.  Un tipo non può essere approvato a meno che non sia in stato di bozza e soddisfi un set di regole di convalida descritte [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

```
POST /rest/v1/customobjects/schema/{apiName}/approve.json
```

```json
{
    "requestId": "11d86#1685304a983",
    "result": [],
    "success": true
}
```

### Tipo di eliminazione

Utilizza l&#39;endpoint [Elimina bozza tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) per eliminare una bozza di versione. `apiName` è l&#39;unico parametro obbligatorio come parametro di percorso. Un tipo deve essere in stato di bozza per essere eliminato, ovvero un tipo approvato non può essere eliminato.

```
POST /rest/v1/customobjects/schema/{apiName}/discardDraft.json
```

```json
{
    "requestId": "5228#1684edde793",
    "result": [],
    "success": true
}
```

### Elimina tipo

Utilizzare l&#39;endpoint [Elimina tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) per eliminare una versione approvata.  `apiName` è l&#39;unico parametro obbligatorio come parametro di percorso.  Si tratta di un&#39;operazione distruttiva che non può essere annullata.  Un tipo può essere eliminato solo se è stato rimosso dall’utilizzo da parte di eventuali risorse, ad esempio trigger o filtri.  L’endpoint Assets Get Custom Object Dependent può essere utilizzato per recuperare un elenco di risorse dipendenti per un determinato tipo.

POST /rest/v1/customobjects/schema/{apiName}/delete.json

```json
{
    "requestId": "14e36#1684efc4227",
    "result": [],
    "success": true
}
```

## Campi oggetto personalizzati

Per impostazione predefinita, tutti i tipi di oggetto personalizzati contengono i seguenti campi standard:

* GUID Marketo: identificatore univoco per il tipo di oggetto personalizzato
* Creato in data - Data e ora di creazione del tipo di oggetto personalizzato
* Data/ora ultimo aggiornamento del tipo di oggetto personalizzato

Puoi aggiungere, modificare o eliminare campi personalizzati utilizzando gli endpoint descritti di seguito.

* Il numero massimo di campi consentiti è 50
* Dopo l&#39;approvazione di un oggetto personalizzato, è possibile aggiungere al massimo 20 campi aggiuntivi
* È necessario almeno un campo di deduplicazione, è consentito un massimo di 3
* I nomi API dei campi e i nomi visualizzati possono contenere caratteri alfanumerici e il carattere di sottolineatura &quot;_&quot;

Per ulteriori informazioni sui campi oggetto personalizzati, consulta la documentazione del prodotto [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Aggiungi campi

L&#39;endpoint [Aggiungi campi tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) consente di aggiungere uno o più campi all&#39;oggetto personalizzato.  Il corpo della richiesta contiene un array `input` con uno o più elementi.  Ogni elemento è un oggetto JSON con attributi che descrivono un campo. L&#39;attributo `name` richiesto è il nome API del campo e deve essere univoco per l&#39;oggetto personalizzato.   Per convenzione, utilizza lettere minuscole o CamelCase per distinguere altre stringhe di testo. L&#39;attributo `displayName` richiesto è il nome leggibile del campo e deve essere univoco per l&#39;oggetto personalizzato. L&#39;attributo `dataType` richiesto è il tipo di dati del campo.  A  è possibile ottenere un elenco dei tipi di dati consentiti chiamando l&#39;endpoint [Get Custom Object Type Field Data Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET).  Gli oggetti personalizzati possono contenere campi con tipo di dati &quot;link&quot;.  I campi di collegamento vengono utilizzati per stabilire relazioni tra oggetti personalizzati e altri tipi di oggetti nel sistema, ad esempio Lead, Società.  Ulteriori informazioni sui campi di collegamento sono disponibili [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). L&#39;attributo facoltativo `description` è la descrizione del campo. L&#39;attributo booleano facoltativo `isDedupeField` specifica se il campo viene utilizzato per la deduplicazione durante le operazioni di aggiornamento dell&#39;oggetto personalizzato.  L&#39;impostazione predefinita è false.  Per le relazioni uno-a-molti è necessario un campo di deduplicazione. L&#39;attributo facoltativo dell&#39;oggetto `relatedTo` specifica un campo di collegamento.  Per le relazioni uno-a-molti, questo oggetto contiene un attributo `name` che è l&#39;&quot;oggetto link&quot; o l&#39;oggetto padre a cui collegarsi e un attributo `field` che è il &quot;campo link&quot;  o il campo all&#39;interno dell&#39;oggetto padre da utilizzare come attributo chiave.  Chiamare l&#39;endpoint [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) per recuperare un elenco di oggetti di collegamento consentiti.  Per ulteriori informazioni sui campi di collegamento, consulta la documentazione del prodotto [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Un oggetto personalizzato non può essere collegato a un altro oggetto personalizzato con un campo di collegamento esistente.

### Relazione uno-a-molti

Per una struttura oggetto personalizzata uno-a-molti, utilizzare un campo di collegamento in un oggetto personalizzato per collegarlo a un oggetto standard: Lead o Company. Utilizzando l&#39;esempio del proprietario dell&#39;automobile dalla documentazione del prodotto Marketo [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), viene creato un oggetto personalizzato contenente informazioni relative all&#39;automobile per la connessione con i lead.

1. Crea un oggetto **Car**
1. Aggiungi campi all&#39;oggetto **Car**: deduplicazione in **VIN**, collegamento a **Lead**&#x200B;**/ID lead**
1. Approva oggetto **Car**

Innanzitutto, crea il tipo di oggetto personalizzato per contenere informazioni specifiche per l’auto.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Car",
    "pluralName": "Cars"
    "apiName": "car",
    "description": "Automobile owned",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "cbaa#16876dd3da6",
    "result": [],
    "success": true
}
```

Ora aggiungi i campi al tipo di oggetto personalizzato Auto. Utilizziamo un campo di collegamento per specificare sia l’oggetto che il campo a cui connettersi. In questo caso, l’oggetto collegamento è Lead e il campo collegamento è ID. Utilizza un campo stringa per la deduplicazione (VIN).  Aggiungeremo altri tre campi per memorizzare gli attributi Car aggiuntivi (Make, Model, Year).

```
POST /rest/v1/customobjects/schema/car/addField.json
```

```json
{
  "input": [
    {
      "displayName": "Lead ID",
      "description": "Link field to Lead object",
      "name": "leadID",
      "dataType": "link",
      "relatedTo": {
        "field": "id",
        "name": "lead"
      }
    },
    {
      "displayName": "VIN",
      "description": "Vehicle ID number",
      "name": "vin",
      "dataType": "string",
      "isDedupeField": true
    },
    {
      "displayName": "Make",
      "description": "Vehicle make",
      "name": "make",
      "dataType": "string"
    },
    {
      "displayName": "Model",
      "description": "Vehicle model",
      "name": "model",
      "dataType": "string"
    },
    {
      "displayName": "Year",
      "description": "Vehicle year",
      "name": "year",
      "dataType": "integer"
    }
  ]
}

{
    "requestId": "b359#16876f17996",
    "result": [],
    "success": true
}
```

Infine, approva il tipo di oggetto personalizzato.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

### Relazione molti-a-molti

Le relazioni molti-a-molti sono rappresentate utilizzando un &quot;ponte&quot;, o un oggetto personalizzato intermedio, tra un oggetto personalizzato standard, come Lead o Company, e un oggetto personalizzato &quot;edge&quot;. L’oggetto edge è l’entità principale contenente gli attributi descrittivi (campi). L&#39;oggetto bridge contiene i dati per risolvere le relazioni tra gli oggetti utilizzando 2 campi di collegamento.  Un campo di collegamento punta all&#39;oggetto standard principale come in un  configurazione di relazioni uno-a-molti.  L&#39;altro campo di collegamento punta all&#39;oggetto perimetrale, che è un oggetto personalizzato senza collegamenti.  L’oggetto bridge può contenere anche attributi descrittivi (campi). Utilizzando l&#39;esempio di iscrizione al corso universitario riportato nella documentazione del prodotto Marketo [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure), viene creato un oggetto personalizzato Edge contenente informazioni relative al corso e un oggetto ponte di iscrizione utilizzato per collegare i corsi ai lead. Di seguito sono riportati i passaggi:

1. Crea un oggetto Edge **Course**
1. Aggiungi campi a **Corso:** deduplicazione in **ID corso**
1. Approva **Corso**
1. Crea un oggetto bridge **Registrazione**
1. Aggiungi campi a **Iscrizione:** deduplicazione in **ID iscrizione**, collegamento al campo **ID corso**&#x200B;**/ID corso** e collegamento a **ID lead**&#x200B;**/ID lead**
1. Approva **iscrizione**

Innanzitutto, crea il tipo di oggetto edge per contenere informazioni specifiche del corso:

```
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action":"createOnly",
    "displayName": "Course",
    "pluralName": "Courses",
    "apiName": "course",
    "description": "Modeling a college course, an edge object in Marketo",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "4aec#168879ede00",
    "result": [],
    "success": true
}
```

Quindi, aggiungiamo campi personalizzati al tipo di oggetto edge.  In questo esempio aggiungeremo i seguenti quattro campi personalizzati per modellare un corso dell’università: ID corso, Istruttore corso, Posizione corso, Nome corso.  Tieni presente che stiamo designando l’ID corso come campo di deduplicazione, in quanto è necessario almeno un campo di deduplicazione.

```
POST /rest/v1/customobjects/schema/course/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Course ID",
            "name": "courseID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Course Instructor",
            "name": "courseInstructor",
            "dataType": "string"
        },
        {
            "displayName": "Course Location",
            "name": "courseLocation",
            "dataType": "string"
        },
        {
            "displayName": "Course Name",
            "name": "courseName",
            "dataType": "string"
        }
    ]
}
```

```
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Ora è necessario approvare il tipo di oggetto edge in modo che sia possibile farvi riferimento in un secondo momento durante il collegamento al tipo di oggetto ponte.  Tieni presente che i tipi di oggetto personalizzati devono essere approvati per essere selezionabili come oggetto collegamento.

```
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

L&#39;oggetto edge è terminato.  Passiamo ora alla creazione del tipo di oggetto ponte per contenere informazioni specifiche per l’iscrizione.

```
POST /rest/v1/customobjects/schema.json
```

```json
{
    "action": "createOnly",
    "displayName": "Enrollment",
    "pluralName": "Enrollments",
    "apiName": "enrollment",
    "description": "Bridge object for Course custom object",
    "showInLeadDetail": true
}
```

```json
{
    "requestId": "8fbb#168960f671b",
    "result": [],
    "success": true
}
```

Per aggiungere campi personalizzati al tipo di oggetto ponte, aggiungi due campi collegamento: uno che si collega all’oggetto Lead e un altro che si collega all’oggetto Course appena creato. Per creare un collegamento all&#39;oggetto Lead, utilizzare il campo ID lead. Per creare un collegamento all’oggetto Corso, utilizza il campo ID Corso.  Quindi, aggiungi un identificatore univoco dell’ID di iscrizione come campo di deduplicazione, in quanto è necessario almeno un campo di deduplicazione. Infine, aggiungi un campo Grado per monitorare come ha fatto lo studente.

```
POST /rest/v1/customobjects/schema/enrollment/addField.json
```

```json
{
    "input": [
        {
            "displayName": "Lead ID",
            "description": "Link field to Lead object",
            "name": "leadID",
            "dataType": "link",
            "relatedTo": {
                "field": "id",
                "name": "lead"
            }
        },
        {
            "displayName": "Course ID",
            "description": "Link field to Course object",
            "name": "courseID",
            "dataType": "link",
            "relatedTo": {
                "field": "courseID",
                "name": "course"
            }
        },
        {
            "displayName": "Enrollment ID",
            "description": "Unique ID for deduplication",
            "name": "enrollmentID",
            "dataType": "string",
            "isDedupeField": true
        },
        {
            "displayName": "Grade",
            "description": "Grade for the course",
            "name": "grade",
            "dataType": "string"
        }
    ]
}
```

```json
{
    "requestId": "7be5#168973f5052",
    "result": [],
    "success": true
}
```

Infine, approvare l&#39;oggetto ponte.

```
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

È possibile popolare i record oggetto personalizzati a livello di programmazione utilizzando [Sincronizza oggetto personalizzato](#create_and_update) o [Importazione oggetti personalizzati in blocco](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=en). In alternativa, è possibile utilizzare la funzionalità dell&#39;interfaccia utente di Marketo [Importa dati oggetto personalizzati](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data).

## Aggiorna campo

L&#39;endpoint [Aggiorna campo tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) consente di aggiornare un campo nella bozza dell&#39;oggetto personalizzato.  Il parametro obbligatorio del percorso `apiName` è il nome API del tipo di oggetto personalizzato.  Il parametro obbligatorio del percorso `fieldAPIName` è il nome API del campo del tipo di oggetto personalizzato.  Il corpo della richiesta contiene un oggetto JSON contenente coppie chiave/valore che specificano gli attributi del campo da aggiornare.

```
POST /rest/v1/customobjects/schema/{apiName}/{fieldApiName}/updateField.json
```

```json
{
  "displayName": "Very Long Title",
  "dataType": "text"
}
```

```json
{
    "requestId": "d523#1684f355db9",
    "result": [],
    "success": true
}
```

## Elimina campi

L&#39;endpoint [Elimina campi tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) consente di eliminare uno o più campi dall&#39;oggetto personalizzato.  Il parametro obbligatorio del percorso `apiName` è il nome API del tipo di oggetto personalizzato.  Il corpo della richiesta contiene un oggetto JSON con un array `input` con uno o più elementi.  Ogni elemento è un oggetto JSON con un attributo `name` che specifica il nome API del campo da eliminare.

```
POST /rest/v1/customobjects/schema/{apiName}/deleteField.json
```

```json
{
    "input":
    [
        {
            "name": "title"
        },
        {
            "name": "author"
        }
    ]
}
```

```json
{
"requestId": "b359#19934f17996",
"result": [],
"success": true
}
```

## Tipi di dati campo elenco

L&#39;endpoint [Get Custom Object Type Field Data Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) restituisce l&#39;elenco di tutti i tipi di dati di campo consentiti. Questa funzione consente di modellare il tipo di oggetto personalizzato per identificare i tipi di dati dei campi personalizzati supportati.

```
GET /rest/v1/customobjects/schema/fieldDataTypes.json
```

```json
{
    "requestId": "c405#167ed49e866",
    "result": [
        "string",
        "boolean",
        "integer",
        "float",
        "link",
        "email",
        "currency",
        "date",
        "datetime",
        "phone",
        "text"
    ],
    "success": true
}
```

## Elenca oggetti personalizzati collegabili

L&#39;endpoint [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) restituisce un elenco di tutti gli oggetti di collegamento consentiti e dei relativi campi di collegamento.  L’elenco contiene gli oggetti standard (lead, società) ed eventuali oggetti personalizzati creati nell’istanza.

```
GET /rest/v1/customobjects/schema/linkableObjects.json
```

```json
{
    "requestId": "11e62#167f1160e4e",
    "result": [
        {
            "name": "lead",
            "displayName": "Lead",
            "fields": [
                {
                    "name": "Account Balance",
                    "displayName": "Account Balance",
                    "dataType": "integer"
                },
                {
                    "name": "Email Address",
                    "displayName": "Email Address",
                    "dataType": "email"
                },
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Display Name",
                    "displayName": "Marketo Social Facebook Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Id",
                    "displayName": "Marketo Social Facebook Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Photo URL",
                    "displayName": "Marketo Social Facebook Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Profile URL",
                    "displayName": "Marketo Social Facebook Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Facebook Reach",
                    "displayName": "Marketo Social Facebook Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Enrollments",
                    "displayName": "Marketo Social Facebook Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Facebook Referred Visits",
                    "displayName": "Marketo Social Facebook Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Gender",
                    "displayName": "Marketo Social Gender",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Display Name",
                    "displayName": "Marketo Social LinkedIn Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Id",
                    "displayName": "Marketo Social LinkedIn Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Photo URL",
                    "displayName": "Marketo Social LinkedIn Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Profile URL",
                    "displayName": "Marketo Social LinkedIn Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social LinkedIn Reach",
                    "displayName": "Marketo Social LinkedIn Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Enrollments",
                    "displayName": "Marketo Social LinkedIn Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social LinkedIn Referred Visits",
                    "displayName": "Marketo Social LinkedIn Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Syndication Id",
                    "displayName": "Marketo Social Syndication Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Total Referred Enrollments",
                    "displayName": "Marketo Social Total Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Total Referred Visits",
                    "displayName": "Marketo Social Total Referred Visits",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Display Name",
                    "displayName": "Marketo Social Twitter Display Name",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Id",
                    "displayName": "Marketo Social Twitter Id",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Photo URL",
                    "displayName": "Marketo Social Twitter Photo URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Profile URL",
                    "displayName": "Marketo Social Twitter Profile URL",
                    "dataType": "string"
                },
                {
                    "name": "Marketo Social Twitter Reach",
                    "displayName": "Marketo Social Twitter Reach",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Enrollments",
                    "displayName": "Marketo Social Twitter Referred Enrollments",
                    "dataType": "integer"
                },
                {
                    "name": "Marketo Social Twitter Referred Visits",
                    "displayName": "Marketo Social Twitter Referred Visits",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "company",
            "displayName": "Company",
            "fields": [
                {
                    "name": "Id",
                    "displayName": "Id",
                    "dataType": "integer"
                }
            ]
        },
        {
            "name": "car_c",
            "displayName": "Car",
            "fields": [
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string"
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string"
                }
            ]
        }
    ],
    "success": true
}
```

## Ottieni Assets personalizzato dipendente da oggetti

L&#39;endpoint [Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) restituisce un elenco di risorse dipendenti di un tipo di oggetto personalizzato, inclusa la posizione nell&#39;istanza.  Questo è utile quando si rimuove un’integrazione ed è necessario identificare ovunque sia in uso un tipo di oggetto personalizzato.

```
GET /rest/v1/customobjects/schema/{apiName}/dependentAssets.json
```

```json
{
    "requestId": "71cf#16a21f30ed6",
    "result": [
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)"
        },
        {
            "assetType": "Smart Campaign",
            "assetId": 3773,
            "assetName": "CarTest.HasCar (Smart List)",
            "usedFields": [
                "leadID",
                "make",
                "model",
                "vin",
                "year"
            ]
        }
    ],
    "success": true
}
```

## Timeout

* Gli endpoint di oggetti personalizzati hanno un timeout di 30 secondi, a meno che non sia indicato di seguito
   * Sincronizza oggetti personalizzati: 120s
   * Elimina oggetti personalizzati: 60s
