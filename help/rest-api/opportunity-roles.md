---
title: Ruoli opportunità
feature: REST API
description: Gestione dei ruoli opportunità in Marketo.
exl-id: 2ba84f4d-82d0-4368-94e8-1fc6d17b69ed
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Ruoli opportunità

[Riferimento endpoint ruoli opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityRolesUsingGET)

I lead sono collegati alle opportunità tramite l&#39;oggetto `opportunityRole` intermedio.

Le API del ruolo opportunità sono esposte solo per le sottoscrizioni per le quali non è abilitata la sincronizzazione CRM nativa.

## Descrivere

Analogamente alle opportunità, per i ruoli opportunità vengono esposte una chiamata di descrizione e operazioni CRUD.

```
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

Tieni presente che sia `dedupeFields` che `searchableFields` sono un po&#39; diversi dalle opportunità. `dedupeFields` fornisce in realtà una chiave composta, in cui sono necessari tutti e tre i valori `externalOpportunityId`, `leadId` e `role`. Affinché la creazione dei record abbia esito positivo, è necessario che nell’istanza di destinazione siano presenti sia il collegamento di opportunità che quello di lead in base ai campi ID. Per `searchableFields`, `marketoGUID`, `leadId` e `externalOpportunityId` sono tutti validi per le query e utilizzano un pattern identico a Opportunities, ma esiste un&#39;opzione aggiuntiva di utilizzo della chiave composta per eseguire query, che richiede l&#39;invio di un oggetto JSON tramite POST, con il parametro di query aggiuntivo `_method=GET`.

```
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

Questo produce lo stesso tipo di risposta di una query GET standard, ma dispone semplicemente di un’interfaccia diversa per effettuare la richiesta.

## Crea e aggiorna

I ruoli opportunità hanno la stessa interfaccia per la creazione e l’aggiornamento dei record delle opportunità.

```
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

Puoi eliminare i ruoli opportunità deduplicando i campi o il campo ID. Specificare utilizzando il parametro deleteBy con il valore dedupeFields o idField. Se non viene specificato, il valore predefinito è dedupeFields. Il corpo della richiesta contiene una matrice di input di ruoli opportunità da eliminare. È consentito un massimo di 300 ruoli opportunità per chiamata.

```
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

- Gli endpoint per il ruolo dell’opportunità hanno un timeout di 30 secondi, a meno che non sia indicato di seguito
   - Ruoli opportunità di sincronizzazione: 60s
   - Elimina ruoli opportunità: 60s
