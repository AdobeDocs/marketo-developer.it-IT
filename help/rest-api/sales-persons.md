---
title: Venditori
feature: REST API
description: Guida REST API di Marketo per i record Persona di vendita con SFDC o Dynamics sync, utilizzando externalSalesPersonId per relazionarsi ai lead ed eseguire query, upsert, delete.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
TQID: https://experienceleague.adobe.com/JwLNgM0zgztyoYJotCiSdGxMixnzA0kvkFbvq8kEkzE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 369
ht-degree: 0%

---

# Venditori

[Riferimento endpoint persona di vendita](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)

Le API addetto alle vendite forniscono l&#39;accesso in sola lettura per gli abbonamenti in cui è abilitato [SFDC Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync).

I Sales Persons sono record di persone che rappresentano i proprietari delle vendite dei record dei lead. Il campo externalSalesPersonId di ogni record Lead fa riferimento a un lead a un venditore. Quando questo campo viene compilato, Marketo compila i campi di ricerca del Proprietario lead corrispondenti nel record del lead. Puoi quindi utilizzare i filtri e i token associati.

Associa i venditori ad altri record passando l&#39;attributo externalSalesPersonId all&#39;endpoint corrispondente:

- Record lead: [Sincronizza lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST).
- Record di opportunità: [Sincronizza opportunità](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities/operation/syncOpportunitiesUsingPOST).
- Record società: [Sincronizza società](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST).

I record Persona di vendita sono modificabili solo tramite l’API.

## Descrivere

Descrivere i record della persona di vendita utilizzando il modello standard per gli oggetti del database lead.

```http
GET /rest/v1/salespersons/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"SalesPerson",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"id",
         "dedupeFields":[
            "externalSalesPersonId"
         ],
         "searchableFields":[
            [
               "email"
            ],
            [
               "id"
            ],
            [
               "externalSalesPersonId"
            ]
         ],
         "fields":[
            {
               "name":"id",
               "displayName":"Marketo Id",
               "dataType":"integer",
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
               "name":"email",
               "displayName":"Email",
               "dataType":"string",
               "length":255,
               "updateable":false
            },
            {
               "name":"externalSalesPersonId",
               "displayName":"External Sales Person Id",
               "dataType":"string",
               "length":255,
               "updateable":false
            }
         ]
      }
   ]
}
```

Per impostazione predefinita, la persona di vendita `idField` è &quot;id&quot; e `dedupeFields` è &quot;externalSalesPersonId&quot;.

## Query

Eseguire query sui venditori utilizzando il modello di query standard per le chiavi semplici. Nell&#39;esempio seguente l&#39;e-mail dell&#39;utente viene utilizzata come externalSalesPersonId.

Per impostazione predefinita, la query restituisce tutti i campi compilati per i record corrispondenti.

```http
GET /rest/v1/salespersons.json?filterType=dedupeFields&filterValues=david@test.com,sam@test.com
```

```json
 {
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":53453,
         "externalSalesPersonId":"sam@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      },
      {
         "seq":1,
         "id":53454,
         "externalSalesPersonId":"david@test.com",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:23Z"
      }
   ]
}
```

## Crea e aggiorna

Crea o aggiorna i venditori utilizzando il modello di aggiornamento standard.

```http
POST /rest/v1/salespersons.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com",
         "email":"sam@test.com",
         "firstName":"Sam",
         "lastName":"Sanosin"
      },
      {
         "externalSalesPersonId":"david@test.com",
         "email":"david@test.com",
         "firstName":"David",
         "lastName":"Aulassak"
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
         "id":45232
      },
      {
         "seq":1,
         "status": "created",
         "id":45236
      }
   ]
}
```

## Elimina

Eliminare i venditori utilizzando il modello di eliminazione standard.

Impossibile eliminare un venditore &quot;in uso&quot;. La richiesta ignora il venditore nei seguenti casi:

- Il venditore è associato ai lead attivi.
- Il venditore è associato a una società che è stata eliminata.

```http
POST /rest/v1/salespersons/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalSalesPersonId":"sam@test.com"
      },
      {
         "externalSalesPersonId":"david@test.com"
      },
      {
         "externalSalesPersonId":"raj@test.com"
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
         "id":56343,
         "status": "deleted"
      },
      {
         "seq":1,
         "id":53453,
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
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

- Gli endpoint persona di vendita hanno un timeout di 30 secondi, salvo diversa indicazione.
- Timeout di Sync Sales Persons su 60.
- Timeout di Elimina addetti alle vendite: 60 secondi.
