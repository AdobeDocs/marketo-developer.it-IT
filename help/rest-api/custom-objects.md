---
title: Oggetti personalizzati
feature: REST API, Custom Objects
description: Scopri come creare e gestire oggetti personalizzati di Marketo tramite API REST, inclusi elenchi e descrizioni di endpoint, metadati, relazioni, campi e query.
exl-id: 88e8829b-f8f1-46d7-a753-5aa6e20e2c40
TQID: https://experienceleague.adobe.com/NWm9CjFVqQdVDJRrnE4nA299-Lg53-JR7xvY-82dUqY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: ea4e3ff5-e7b9-4b4c-a5a0-dc27cc3f4275
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2938
ht-degree: 0%

---

# Oggetti personalizzati

[**Riferimento all&#39;endpoint di oggetto personalizzato**](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

Gli oggetti personalizzati di Marketo possono essere correlati a oggetti standard di Marketo, ad esempio lead e società, o ad altri oggetti personalizzati di Marketo. Creare oggetti personalizzati di Marketo nell&#39;interfaccia utente di [Marketo](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects) o utilizzando l&#39;API metadati oggetti personalizzati descritta in questo documento.

L’accesso all’API dei metadati di oggetti personalizzati richiede un tipo di abbonamento Marketo appropriato. Per informazioni, contatta il tuo CSM.

## Elenco

Oltre alle chiamate standard Describe, Query, Update e Delete per gli oggetti del database lead, gli oggetti personalizzati forniscono una [chiamata elenco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET). L’endpoint restituisce gli oggetti personalizzati disponibili nell’istanza di destinazione e i metadati relativi a ciascun oggetto.

```http
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

La risposta elenca le relazioni per ciascun oggetto. Ogni relazione contiene:

- `field`: campo dell&#39;oggetto che contiene il valore di collegamento.
- `type`: se l&#39;oggetto correlato è un oggetto padre o figlio.
- `relatedTo`: nome dell&#39;oggetto correlato e relativo campo di collegamento.

## Descrivere

La chiamata [Descrivi](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) per gli oggetti personalizzati segue lo stesso pattern di Opportunità e Aziende, con due aggiunte:

- Il parametro percorso `apiName` specifica il nome API del tipo di oggetto personalizzato da descrivere.
- La risposta include un array `relationships` che elenca le relazioni disponibili per il tipo di oggetto personalizzato.

```http
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

[La query degli oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET) è leggermente diversa dalla query degli altri oggetti del database lead. Come in Describe, la richiesta accetta un parametro di percorso `apiName`.

Per un filterType normale, invia una richiesta GET con i parametri `filterType` e `filterValues` richiesti. È inoltre possibile includere i parametri facoltativi `**fields**`, `batchSize` e `nextPageToken`.

Quando si richiede un elenco di campi, un campo richiesto non restituito ha un valore implicito nullo.

```http
GET /rest/v1/customobjects/{apiName}.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
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

Quando si esegue una query con chiavi composte, l’API si comporta come l’API Ruoli opportunità e accetta una richiesta POST con un corpo JSON. Il corpo può contenere gli stessi membri di una query GET tranne `filterValues`.

Invece di filtrare i valori, fornire un array `input` di oggetti. Ogni oggetto contiene un membro per ogni campo del tipo di oggetto `dedupeFields`.

```http
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

Utilizzare l&#39;endpoint [Sincronizza oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) per creare o aggiornare oggetti personalizzati. Specificare l&#39;operazione con il parametro `action`. Ogni chiamata può creare o aggiornare fino a 300 record.

Basare i valori nell&#39;array `input` sulle informazioni restituite dall&#39;endpoint [Descrizione oggetti personalizzati](https://experienceleague.adobe.com/it/docs/marketo-developer/marketo/rest/endpoint-reference#!/Custom_Objects/describeUsingGET_1). Nell&#39;oggetto car di esempio, l&#39;unico campo di deduplicazione è `vin`. Quando si utilizza la modalità dedupeFields per creare o aggiornare record, includere almeno un campo `vin` in ogni oggetto nell&#39;array di input.

```http
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

Quando si aggiornano i record in modalità `idField`, `idField` è sempre `marketoGUID`. Includi un campo `marketoGUID` in ogni record.

Poiché questo campo è gestito dal sistema, `idField` è valido solo per il tipo di azione updateOnly. La matrice dei risultati include il **stato** di ogni record. Include anche un `marketoGUID` per un&#39;operazione riuscita o un array `reasons` per un&#39;operazione non riuscita.

## Elimina

Per [eliminare i record](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST), selezionare una modalità `deleteBy` di `idField` o `dedupeFields`. Includere i campi corrispondenti in ogni record nell&#39;array `input`. Ogni chiamata consente un massimo di 300 record.

```http
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

Come per gli aggiornamenti, il risultato contiene uno stato per ogni record. Include anche un `marketoGUID` per l&#39;eliminazione riuscita o un array `reasons` per l&#39;eliminazione non riuscita.

## Tipi di oggetti personalizzati

L’API dei metadati di oggetti personalizzati consente di gestire in remoto gli schemi di oggetti personalizzati. Utilizzarlo per creare un tipo di oggetto personalizzato o modificarne uno esistente. Dopo aver creato o modificato un tipo, approvalo prima dell’uso.

Per ulteriori informazioni, consulta la [documentazione del prodotto per oggetti personalizzati](https://experienceleague.adobe.com/it/docs/marketo/using/home).

- Non è possibile modificare i tipi di oggetto personalizzati creati dall’API nell’interfaccia utente di Marketo.
- Il numero massimo di tipi di oggetto personalizzati è 10.
- Il numero massimo di campi oggetto personalizzati è 50 per tipo.
- I nomi API e i nomi visualizzati del tipo di oggetto personalizzato possono contenere caratteri alfanumerici e il carattere di sottolineatura &quot;_&quot;.

### Tipo di query

Recuperare i metadati del tipo di oggetto personalizzato in uno dei modi seguenti:

- Descrivi tipo di oggetto personalizzato restituisce un record del tipo di oggetto personalizzato e supporta il filtro per stato di approvazione.
- Elenca tipi di oggetti personalizzati restituisce tutti i tipi di oggetti personalizzati nella sottoscrizione e supporta il filtro per nome e stato di approvazione.

### Descrivi tipo

L&#39;endpoint [Describe Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) restituisce metadati per un tipo di oggetto personalizzato. Il parametro di percorso `apiName` richiesto specifica il nome API del tipo da descrivere.

Se esiste una versione approvata, l’endpoint la restituisce. In caso contrario, restituisce la versione bozza. Utilizzare il parametro facoltativo `state` per richiedere `draft`, `approved` o `approvedWithDraft`.

```http
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

La risposta contiene:

- Metadati: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations.
- Campi standard: marketoGUID, createdAt, updatedAt.
- Campi personalizzati: leadId, vin, make, model, year.

### Tipi di elenco

L&#39;endpoint [List Custom Object Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/listCustomObjectTypesUsingGET) restituisce i metadati per tutti i tipi di oggetto personalizzati disponibili nell&#39;istanza di destinazione. È simile a [Elenca oggetti personalizzati](https://experienceleague.adobe.com/docs/marketo-developer/marketo/soap/custom-objects/custom-objects.html?lang=it), ma include metadati aggiuntivi quali stato, relazioni e campi.

Se esiste una versione approvata, l’endpoint la restituisce. In caso contrario, restituisce la versione bozza.

I parametri facoltativi sono:

- **state**: specifica la versione da restituire. I valori validi sono **draft**, **approval** e **approvalWithDraft**.
- **names**: specifica i tipi di oggetti personalizzati da restituire come elenco separato da virgole di nomi API.

```http
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
            "description": "No really, it is a car!",
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

Utilizzare l&#39;endpoint [Sync Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) per creare o aggiornare un tipo di oggetto personalizzato.

Gli attributi sono:

- **action**: attributo facoltativo che controlla l&#39;operazione di registrazione. I valori validi sono **createOnly**, **createOrUpdate** e **updateOnly**. Il valore predefinito è createOrUpdate.
- **displayName** e **apiName**: obbligatorio a meno che l&#39;azione non sia updateOnly. Entrambi devono essere univoci per evitare conflitti con i tipi con provisioning del cliente. I partner LaunchPoint devono anteporre uno spazio dei nomi rappresentativo. Per apiName, utilizza lettere minuscole o camelCase per distinguerlo dalle altre stringhe di testo.
- **pluralName**: attributo facoltativo che specifica la forma plurale di displayName.
- **description**: Attributo facoltativo che descrive il tipo di oggetto personalizzato.
- **showInLeadDetail**: attributo booleano facoltativo che abilita i dati oggetto personalizzati nella pagina Database lead dell&#39;interfaccia utente di Marketo. Il valore predefinito è false.

Scegliere attentamente i nomi degli oggetti personalizzati. Aggiungi a ogni nuovo nome di oggetto personalizzato un prefisso con una stringa che identifica l&#39;azienda. Il prefisso può contenere caratteri alfanumerici o trattini bassi. Questa convenzione semplifica la ricerca dell&#39;oggetto nell&#39;interfaccia utente di MLM e ne garantisce l&#39;univocità del nome.

Nell&#39;esempio seguente viene creato un tipo di oggetto personalizzato con il nome API &quot;transaction&quot;.

```http
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

La richiesta seguente descrive il tipo appena creato.

```http
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

La risposta contiene:

- Metadati: state, displayName, description, apiName, idField, createdAt, updatedAt, dedupeFields, searchableFields, relations.
- Campi standard: marketoGUID, createdAt, updatedAt.

#### Tipo di aggiornamento

L’esempio seguente aggiorna la Descrizione di un tipo esistente il cui Nome API è &quot;transaction&quot;. L&#39;attributo **apiName** è obbligatorio. Poiché il tipo esiste già, la richiesta utilizza updateOnly per l&#39;attributo facoltativo **action**.

Oltre a **apiName**, puoi aggiornare gli attributi disponibili durante la creazione.

```http
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

Approvare i tipi di oggetto personalizzati prima di utilizzarli. Quando si crea un tipo con l&#39;endpoint [Sincronizza tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectTypeUsingPOST), Marketo crea una versione bozza. Dopo aver aggiunto i campi personalizzati, approva la bozza. L’approvazione crea una versione approvata ed elimina la bozza.

Quando si modifica un tipo esistente con l&#39;endpoint Tipo di oggetto personalizzato di sincronizzazione o Aggiungi/aggiorna/elimina campo tipo di oggetto personalizzato, in Marketo viene creata una bozza. Le modifiche al tipo o ai relativi campi influiscono solo sulla versione bozza. Dopo aver apportato le modifiche, approva la bozza. L’approvazione sostituisce la versione approvata con la bozza ed elimina la bozza.

Per ulteriori informazioni, vedere la [documentazione sull&#39;approvazione degli oggetti personalizzati](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object).

Una volta approvato un tipo di oggetto personalizzato, non è possibile:

- Aggiorna `displayName` o `apiName`.
- Aggiungi o rimuovi un campo collegamento.
- Aggiungere o rimuovere un campo di deduplicazione.

Pianifica attentamente lo schema e la convenzione di denominazione prima di approvare il tipo.

### Approva tipo

Utilizza l&#39;endpoint [Approve Custom Object Type](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/approveCustomObjectTypeUsingPOST) per pubblicare una bozza come nuova versione approvata. L&#39;unico parametro obbligatorio è il parametro di percorso **apiName**.

È possibile approvare un tipo solo quando è in stato di bozza e soddisfa le [regole di convalida](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/approve-a-custom-object) documentate.

```http
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

Utilizza l&#39;endpoint [Elimina bozza tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/discardCustomObjectTypeUsingPOST) per eliminare una bozza di versione. L&#39;unico parametro obbligatorio è il parametro di percorso `apiName`.

È possibile eliminare solo un tipo in stato di bozza. Impossibile eliminare un tipo approvato.

```http
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

Utilizzare l&#39;endpoint [Elimina tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST) per eliminare una versione approvata. L&#39;unico parametro obbligatorio è il parametro di percorso `apiName`.

Operazione distruttiva. Impossibile annullarla. Prima di eliminare un tipo, rimuovine l’utilizzo da risorse quali trigger e filtri. Utilizza l’endpoint Assets Get Custom Object Dependent per recuperare le risorse dipendenti per un tipo.

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

- GUID Marketo: identificatore univoco per il tipo di oggetto personalizzato.
- Data creazione: Data e ora di creazione del tipo di oggetto personalizzato.
- Data/ora aggiornamento: data/ora dell&#39;ultimo aggiornamento del tipo di oggetto personalizzato.

Utilizza i seguenti endpoint per aggiungere, modificare o eliminare campi personalizzati.

- Il numero massimo di campi è 50.
- Dopo l’approvazione di un oggetto personalizzato, puoi aggiungervi al massimo 20 campi aggiuntivi.
- È necessario almeno un campo di deduplicazione. È consentito un massimo di tre campi di deduplicazione.
- I nomi API dei campi e i nomi visualizzati possono contenere caratteri alfanumerici e il carattere di sottolineatura &quot;_&quot;.

Per ulteriori informazioni, consulta la [documentazione sui campi oggetto personalizzati](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Aggiungi campi

Utilizza l&#39;endpoint [Aggiungi campi tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/addCustomObjectTypeFieldsUsingPOST) per aggiungere uno o più campi a un oggetto personalizzato. Il corpo della richiesta contiene un array `input` con uno o più elementi. Ogni elemento è un oggetto JSON con attributi che descrivono un campo.

Gli attributi del campo sono:

- `name`: obbligatorio. Il nome API del campo, che deve essere univoco per l’oggetto personalizzato. Utilizzare caratteri minuscoli o camelCase per distinguere il nome da altre stringhe di testo.
- `displayName`: obbligatorio. Il nome del campo leggibile, che deve essere univoco per l’oggetto personalizzato.
- `dataType`: obbligatorio. Tipo di dati del campo. Utilizza l&#39;endpoint [Get Custom Object Type Field Data Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) per recuperare i tipi di dati consentiti.
- `description`: facoltativo. Descrizione del campo.
- `isDedupeField`: Valore booleano facoltativo che specifica se il campo viene utilizzato per la deduplicazione durante le operazioni di aggiornamento degli oggetti personalizzati. Il valore predefinito è false. Per le relazioni uno-a-molti è necessario un campo di deduplicazione.
- `relatedTo`: oggetto facoltativo che specifica un campo di collegamento. Per una relazione uno-a-molti, `name` identifica l&#39;&quot;oggetto link&quot; o l&#39;oggetto padre e `field` identifica il &quot;campo link&quot; o il campo chiave nell&#39;oggetto padre.

Gli oggetti personalizzati possono contenere campi con il tipo di dati &quot;link&quot;. I campi di collegamento stabiliscono relazioni tra oggetti personalizzati e altri tipi di oggetti, ad esempio Lead e Azienda. Per informazioni dettagliate sui campi collegamento, consulta la [documentazione del campo oggetto personalizzato](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields). Utilizzare l&#39;endpoint [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) per recuperare gli oggetti di collegamento consentiti.

Un oggetto personalizzato non può essere collegato a un altro oggetto personalizzato con un campo di collegamento esistente. Per ulteriori informazioni, consulta la [documentazione sui campi di collegamento](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-fields).

### Relazione uno-a-molti

Per una struttura oggetto personalizzata uno-a-molti, utilizzare un campo di collegamento per collegare un oggetto personalizzato a un oggetto Lead o Company standard. Il flusso di lavoro seguente utilizza l&#39;[esempio proprietario auto](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure) per creare un oggetto personalizzato che memorizza le informazioni sull&#39;auto e si connette ai lead.

1. Crea un oggetto **Car**.
1. Aggiungi campi all&#39;oggetto **Car**: deduplica in **VIN** e collegamento a **Lead**&#x200B;**/ID lead**.
1. Approva l&#39;oggetto **Car**.

Innanzitutto, crea il tipo di oggetto personalizzato contenente informazioni specifiche per l’auto.

```http
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

Quindi, aggiungi i campi al tipo di oggetto personalizzato Auto. Utilizza un campo di collegamento per specificare sia l’oggetto che il campo da connettere. In questo esempio, l&#39;oggetto collegamento è Lead e il campo collegamento è ID.

Utilizza un campo stringa per la deduplicazione (VIN). Aggiungi altri tre campi per memorizzare gli attributi Make, Model e Year.

```http
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

```http
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

Una relazione molti-a-molti utilizza un oggetto personalizzato &quot;bridge&quot; tra un oggetto standard, ad esempio Lead o Company, e un oggetto personalizzato &quot;edge&quot;. L’oggetto edge è l’entità principale e contiene campi descrittivi.

L&#39;oggetto bridge risolve la relazione con due campi di collegamento. Un campo punta all&#39;oggetto standard padre, come in una relazione uno-a-molti. L&#39;altro punta all&#39;oggetto edge, che è un oggetto personalizzato senza collegamenti. L’oggetto ponte può contenere anche campi descrittivi.

Il seguente flusso di lavoro utilizza l&#39;[esempio di iscrizione al corso universitario](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/add-marketo-custom-object-link-fields#AddMarketoCustomObjectLinkFields-CreateaLinkFieldforaOne-to-ManyStructure). Crea un oggetto bordo Corso e un oggetto ponte di iscrizione che collega Corsi con Lead.

1. Crea un oggetto Edge **Course**.
1. Aggiungi campi al **corso:** deduplicazione in **ID corso**.
1. Approva **Corso**.
1. Crea un oggetto bridge **Iscrizione**.
1. Aggiungi campi alla **iscrizione:** deduplicazione in **ID iscrizione**, collegamento al campo **ID corso**&#x200B;**/ID corso** e collegamento a **ID lead**&#x200B;**/ID lead**.
1. Approva **iscrizione**.

Innanzitutto, crea il tipo di oggetto edge contenente informazioni specifiche per il corso:

```http
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

Quindi, aggiungi quattro campi personalizzati per modellare un corso universitario: ID corso, Istruttore corso, Posizione corso e Nome corso. Imposta ID corso come campo di deduplicazione perché è necessario almeno un campo di deduplicazione.

```http
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

```json
{
    "requestId": "cc36#16895b82a41",
    "result": [],
    "success": true
}
```

Approvare il tipo di oggetto edge in modo da potervi fare riferimento quando si esegue il collegamento al tipo di oggetto bridge. Un tipo di oggetto personalizzato deve essere approvato prima di poter essere selezionato come oggetto collegamento.

```http
POST /rest/v1/customobjects/schema/course/approve.json
```

```json
{
    "requestId": "460b#16896055fa3",
    "result": [],
    "success": true
}
```

Dopo aver completato l&#39;oggetto perimetrale, creare il tipo di oggetto ponte che contiene informazioni specifiche per l&#39;iscrizione.

```http
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

Aggiungere due campi di collegamento al tipo di oggetto ponte: uno che collega all&#39;oggetto Lead e uno che collega all&#39;oggetto Course. Utilizza il campo ID lead per collegarti al lead e il campo ID corso per collegarti al corso.

Aggiungi ID iscrizione come campo di deduplicazione perché è necessario almeno un campo di deduplicazione. Quindi aggiungi un campo Grado per tenere traccia delle prestazioni dello studente.

```http
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

```http
POST /rest/v1/customobjects/schema/enrollment/approve.json
```

```json
{
    "requestId": "9a76#16897b0e84b",
    "result": [],
    "success": true
}
```

Compilare i record oggetto personalizzati a livello di programmazione utilizzando [Sincronizza oggetto personalizzato](#create_and_update) o [Importazione oggetti personalizzati in blocco](https://experienceleague.adobe.com/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import.html?lang=it). In alternativa, utilizzare [Importa dati oggetto personalizzati](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-objects/import-custom-object-data) nell&#39;interfaccia utente di Marketo.

## Aggiorna campo

Utilizzare l&#39;endpoint [Aggiorna campo tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/updateCustomObjectTypeFieldUsingPOST) per aggiornare un campo in una bozza di oggetto personalizzato.

I parametri di percorso richiesti sono:

- `apiName`: nome API del tipo di oggetto personalizzato.
- `fieldAPIName`: nome API del campo del tipo di oggetto personalizzato.

Il corpo della richiesta contiene un oggetto JSON con coppie chiave/valore che specificano gli attributi del campo da aggiornare.

```http
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

Utilizza l&#39;endpoint [Elimina campi tipo di oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectTypeFieldsUsingPOST) per eliminare uno o più campi da un oggetto personalizzato. Il parametro di percorso `apiName` richiesto specifica il nome API del tipo di oggetto personalizzato.

Il corpo della richiesta contiene un oggetto JSON con una matrice `input` di uno o più elementi. Ogni elemento è un oggetto JSON il cui attributo `name` specifica il nome API di un campo da eliminare.

```http
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

L&#39;endpoint [Get Custom Object Type Field Data Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeFieldDataTypesUsingGET) restituisce tutti i tipi di dati dei campi consentiti. Utilizzare questo endpoint per identificare i tipi di dati dei campi personalizzati disponibili per la modellazione di un tipo di oggetto personalizzato.

```http
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

L&#39;endpoint [Get Custom Object Linkable Objects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeLinkableObjectsUsingGET) restituisce tutti gli oggetti link consentiti e i relativi campi di collegamento. La risposta include gli oggetti standard, ad esempio Lead e Società, e tutti gli oggetti personalizzati creati nell’istanza.

```http
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

L&#39;endpoint [Get Custom Object Dependent Assets](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectTypeDependentAssetsUsingGET) restituisce le risorse dipendenti di un tipo di oggetto personalizzato e le relative posizioni nell&#39;istanza. Utilizzalo quando rimuovi un’integrazione per identificare ovunque sia in uso un tipo di oggetto personalizzato.

```http
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

- Gli endpoint di oggetti personalizzati hanno un timeout di 30 secondi, a meno che non venga specificato diversamente.
- Timeout degli oggetti personalizzati di sincronizzazione: 120 secondi.
- Timeout dell’eliminazione di oggetti personalizzati: 60 secondi.
