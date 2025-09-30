---
title: Venditori
feature: REST API
description: Guida REST API di Marketo per i record Persona di vendita con SFDC o Dynamics sync, utilizzando externalSalesPersonId per relazionarsi ai lead ed eseguire query, upsert, delete.
exl-id: f8ed5aa5-63c1-4c5b-8683-bf47eed1ea18
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Venditori

[Riferimento endpoint persona di vendita](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Sales-Persons)

Le API addetto alle vendite sono di sola lettura per gli abbonamenti che hanno [SFDC Sync](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) abilitati. Le persone di vendita sono un tipo di record persona che sono i proprietari delle vendite dei record lead. Sono correlati ai record Lead dal campo externalSalesPersonId di ogni record Lead. Quando un lead viene correlato a una persona di vendita da un campo externalSalesPersonId popolato, i campi di ricerca del proprietario del lead corrispondenti vengono compilati per tale record di lead in Marketo, consentendo l&#39;utilizzo dei filtri e dei token corrispondenti.

I venditori sono correlati ai record dei lead utilizzando l&#39;endpoint [Sync Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) e passando l&#39;attributo externalSalesPersonId.

I venditori sono correlati ai record Opportunità utilizzando l&#39;endpoint [Opportunità di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/syncOpportunitiesUsingPOST) e passando l&#39;attributo externalSalesPersonId.

Gli addetti alle vendite sono correlati ai record aziendali utilizzando l&#39;endpoint [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) e passando l&#39;attributo externalSalesPersonId.

I record Persona di vendita sono modificabili solo tramite l’API.

## Descrivere

La descrizione dei record Venditore segue il modello standard per gli oggetti di database dei lead.

```
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

Per impostazione predefinita, il `idField` di Sales Person è &quot;id&quot; e il `dedupeFields` è solo &quot;externalSalesPersonId&quot;.

## Query

Venditori che utilizzano il modello di query standard per le chiavi semplici. Questo esempio mostra l’e-mail dell’utente utilizzata come externalSalesPersonId. Per impostazione predefinita, la query restituisce tutti i campi compilati per i record restituiti.

```
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

Il modello per gli aggiornamenti è standard.

```
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

Il modello per le eliminazioni è standard.

L’eliminazione di Sales Persons (Persone vendita) non è consentita quando &quot;in uso&quot;. In questo caso il venditore viene ignorato. Esempi:

- Quando la persona di vendita è associata ai lead attivi
- Quando il venditore è associato a una società che è stata eliminata

```
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

- Gli endpoint persona di vendita hanno un timeout di 30 secondi, a meno che non sia indicato di seguito
   - Sincronizza addetti alle vendite: anni 60
   - Cancella Venditori: 60s
