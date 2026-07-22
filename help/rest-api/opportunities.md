---
title: Opportunità
feature: REST API
description: API REST di Marketo per descrivere, eseguire query, creare e aggiornare opportunità, deduplicare e cercare campi, limiti e comportamenti di sola lettura con la sincronizzazione di SFDC o Dynamics.
exl-id: 46451285-4125-4857-890a-575069a68288
TQID: https://experienceleague.adobe.com/rBDJcXWQrN5qyKRWHyzVC-sc9BH2mQFLm7fKUk-NUn8
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
source-wordcount: 708
ht-degree: 0%

---

# Opportunità

[Riferimento endpoint opportunità](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

Marketo fornisce API per la lettura, la scrittura, la creazione e l’aggiornamento dei record di opportunità. In Marketo, l&#39;oggetto Ruolo opportunità intermedio collega i record opportunità ai record lead e contatto. Un’opportunità può quindi essere collegata a più lead singoli.

L’API espone entrambi i tipi di oggetto. Come per la maggior parte dei tipi di oggetti del database lead, a ciascuno di essi corrisponde una chiamata Describe che restituisce i metadati dell&#39;oggetto.

Le API dell&#39;opportunità forniscono l&#39;accesso in sola lettura per le sottoscrizioni che hanno [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=it) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=it) abilitato.

## Descrivere

Descrivere i record Opportunità utilizzando il modello standard per gli oggetti del database lead.

```http
GET /rest/v1/opportunities/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunity",
         "displayName":"Opportunity",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId"
         ],
         "searchableFields":[
            [
               "externalOpportunityId"
            ],
            [
               "marketoGUID"
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            }
         ]
      }
   ]
}
```

I campi di risposta chiave sono:

- `idField`: identifica la chiave primaria dell&#39;opportunità, marketoGUID. Questa chiave generata dal sistema supporta le operazioni di lettura e aggiornamento, ma non gli inserimenti.
- `dedupeFields`: identifica chiavi valide per le operazioni di inserimento. Per le opportunità, l’unica chiave è externalOpportunityId.
- `searchableFields`: identifica i campi validi per le query. Questi campi sono externalOpportunityId e marketoGUID.

## Query

Il modello per [eseguire query sulle opportunità](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunitiesUsingGET) segue attentamente l&#39;API Leads. Tuttavia, il parametro `filterType` accetta solo i campi elencati nell&#39;array `searchableFields` della risposta Describe o dedupeFields corrispondente.

Per i campi opportunità personalizzati, nella matrice searchableFields vengono visualizzati solo i campi di tipo String o Integer.

```http
GET /rest/v1/opportunities.json?filterType=marketoGUID&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

Puoi includere i seguenti parametri di query facoltativi:

- `fields`: restituisce campi opportunità aggiuntivi.
- `nextPageToken`: pagine tramite set di risultati più grandi della dimensione del batch.
- `batchSize`: specifica la dimensione del batch. Il valore predefinito e massimo è 300.

Quando si richiede un elenco di `fields`, un campo richiesto non restituito ha un valore implicito nullo.

## Crea e aggiorna

Le opportunità seguono il pattern API Lead con alcune restrizioni. I valori `action` sono createOnly, createOrUpdate e updateOnly.

- Per la modalità createOnly o createOrUpdate, includere il campo externalOpportunityId in ogni record.
- Per la modalità updateOnly, utilizzare marketoGUID o externalOpportunityId.
- Se non viene specificato, per impostazione predefinita la modalità viene impostata su createOrUpdate.

Il parametro `lookupField` dall&#39;API dei lead non è disponibile. Il parametro dedupeBy lo sostituisce ed è valido solo quando action è updateOnly.

I valori dedupeBy sono &quot;dedupeFields&quot; e &quot;idField&quot;, che la risposta Describe identifica rispettivamente come externalOpportunityId e marketoGUID. Se dedupeBy non è specificato, per impostazione predefinita viene utilizzata la modalità dedupeFields. Il campo &quot;name&quot; non può essere nullo.

È possibile inviare fino a 300 record alla volta.

```http
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
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
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

La risposta include i seguenti valori per ogni record:

- `marketoGUID`: identificatore del record.
- `status`: successo o errore del singolo record.
- `seq`: indice del record inviato, che mette in correlazione il record della richiesta con l&#39;ordine di risposta.

### Campi

L’oggetto company contiene campi definiti da attributi quali nome visualizzato, nome API e dataType. Insieme, questi attributi sono denominati metadati.

I seguenti campi di query degli endpoint sull’oggetto aziendale. L&#39;utente API deve disporre di un ruolo con l&#39;autorizzazione `Read-Write Schema Standard Field`, l&#39;autorizzazione `Read-Write Schema Custom Field` o entrambe.

### Campi query

Esegui la query di un campo società per nome API o recupera tutti i campi società.

#### Per nome

L&#39;endpoint [Ottieni campo opportunità per nome](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) recupera i metadati per un campo nell&#39;oggetto aziendale. Il parametro di percorso `fieldApiName` richiesto specifica il nome API del campo.

La risposta è simile alla risposta Descrivi opportunità, ma include metadati aggiuntivi. Ad esempio, l&#39;attributo `isCustom` indica se il campo è personalizzato.

```http
GET /rest/v1/opportunities/schema/fields/externalOpportunityId.json
```

```json
{
    "requestId": "12331#17e9779cb4b",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
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

#### Sfoglia

L&#39;endpoint [Get Opportunity Fields](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityFieldsUsingGET) recupera i metadati per tutti i campi nell&#39;oggetto società. Per impostazione predefinita, restituisce un massimo di 300 record. Utilizzare il parametro di query `batchSize` per ridurre questo numero.

Se l&#39;attributo `moreResult` è true, sono disponibili altri risultati. Continuare a chiamare l&#39;endpoint con `nextPageToken` restituito finché moreResult non è false.

```http
GET /rest/v1/opportunities/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b4a#17e995b31da",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
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
            "displayName": "Description",
            "name": "description",
            "description": null,
            "dataType": "string",
            "length": 2000,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Type",
            "name": "type",
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
            "displayName": "Stage",
            "name": "stage",
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
    "nextPageToken": "E5ZONGE4SAHALYYW6FS25KB5BM======",
    "moreResult": true
}
```

#### Elimina

Elimina le opportunità tramite campi di deduplicazione o campo ID. Impostare il parametro `deleteBy` su dedupeFields o idField. Il valore predefinito è dedupeFields.

Il corpo della richiesta contiene un array `input` di opportunità da eliminare. Ogni chiamata consente un massimo di 300 opportunità.

```http
POST /rest/v1/opportunities/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000"
      },
      {
         "externalOpportunityId":"29UYA31581L000000"
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
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      }
   ]
}
```

## Timeout

- Gli endpoint di opportunità hanno un timeout di 30 secondi, a meno che non venga indicato diversamente.
- Opportunità di sincronizzazione ha un timeout di 60 secondi.
- Il timeout di Elimina opportunità è di 60 secondi.
