---
title: Ruoli opportunità
feature: REST API
description: Gestisci i ruoli opportunità di Marketo tramite API REST, incluse descrizioni, query con campi di deduplicazione composti, eliminazione di aggiornamenti di creazione, timeout e nessuna sincronizzazione CRM.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
TQID: https://experienceleague.adobe.com/aE27mBhsrn-0SO41M-pV5NFjoMq--1Lp-L2TQGL7-8Y
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 254
ht-degree: 0%

---

# Ruoli opportunità

[Riferimento endpoint ruoli opportunità](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/getOpportunityRolesUsingGET)

I collegamenti oggetto `opportunityRole` intermedi generano opportunità.

Le API per il ruolo opportunità sono disponibili solo per le sottoscrizioni per le quali non è abilitata la sincronizzazione CRM nativa.

## Descrivere

Come per le opportunità, l’API fornisce una chiamata Describe e operazioni CRUD per i ruoli di opportunità.

```http
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
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
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

## Query

I valori `dedupeFields` e `searchableFields` sono diversi dalle opportunità. `dedupeFields` fornisce una chiave composta che richiede `externalOpportunityId`, `leadId` e `role`. Affinché la creazione dei record venga eseguita correttamente, è necessario che l’opportunità e il lead a cui fanno riferimento i campi ID siano presenti nell’istanza di destinazione.

I valori `searchableFields` `marketoGUID`, `leadId` e `externalOpportunityId` sono validi per singole query che utilizzano lo stesso pattern di Opportunità. È inoltre possibile eseguire query in base alla chiave composta. Questa query richiede un oggetto JSON inviato tramite POST con il parametro di query `_method=GET`.

```http
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType": "dedupeFields",
   "fields": [
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input": [
      {
        "externalOpportunityId": "Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId": "Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId": "Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

Questa richiesta produce lo stesso tipo di risposta di una query GET standard, ma utilizza un’interfaccia di richiesta diversa.

## Crea e aggiorna

Crea e aggiorna i ruoli opportunità utilizzando la stessa interfaccia delle opportunità.

```http
POST /rest/v1/opportunities/roles.json
```

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456783,
         "role": "Technical Buyer",
         "isPrimary": false
      },
      {
         "externalOpportunityId": "19UYA31581L000000",
         "leadId": 456784,
         "role": "Technical Buyer",
         "isPrimary": false
      }
   ]
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result":[
      {
         "seq": 0,
         "status": "updated",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

## Elimina

Elimina i ruoli opportunità tramite campi di deduplicazione o campo ID. Impostare il parametro deleteBy su dedupeFields o idField. Il valore predefinito è dedupeFields.

Il corpo della richiesta contiene una matrice di input di ruoli opportunità da eliminare. Ogni chiamata consente un massimo di 300 ruoli di opportunità.

```http
POST /rest/v1/opportunities/roles/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
        "externalOpportunityId": "19UYA31581L000000",
        "leadId": 456783,
        "role": "Technical Buyer"
      }
   ]
}
```

```json
{
    "requestId": "10f7c#173264db42d",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
            "status": "deleted"
        }
    ]
    "success": true
}
```

## Timeout

- Gli endpoint del ruolo dell’opportunità hanno un timeout di 30 secondi, a meno che non venga indicato diversamente.
- Timeout dei ruoli dell’opportunità di sincronizzazione su 60 secondi.
- Timeout dell’eliminazione dei ruoli dell’opportunità su 60 anni.
