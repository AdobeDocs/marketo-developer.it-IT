---
title: Appartenenza a elenco (elenchi statici)
feature: REST API, Static Lists
description: Utilizza le API REST del database dei lead di Marketo per aggiungere lead a elenchi statici, rimuovere lead, recuperare membri dell’elenco e controllare l’appartenenza all’elenco.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 4%

---

# Appartenenza a elenco (elenchi statici)

[Riferimento endpoint appartenenza elenco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists)

Le API di appartenenza a elenco forniscono endpoint di database lead per l&#39;utilizzo di membri di elenco statici. Questi endpoint possono essere utilizzati per aggiungere lead a un elenco, rimuovere lead da un elenco, recuperare membri di un elenco e determinare se uno o più lead sono membri di un elenco.

## Endpoint

| Endpoint | Metodo | Percorso |
| --- | --- | --- |
| Aggiungi all’elenco | POST | `/rest/v1/lists/{listId}/leads.json` |
| Rimuovi dall’elenco | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Ottieni lead per ID elenco | GET | `/rest/v1/lists/{listId}/leads.json` |
| Membro dell’elenco | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Aggiungi all’elenco

L&#39;endpoint [Aggiungi all&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/addLeadsToListUsingPOST) viene utilizzato per aggiungere uno o più membri a un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e uno o più parametri di query `id` che contengono ID lead (il massimo consentito è 300).

La risposta contiene un array `result` composto da oggetti JSON con lo stato per ogni ID lead specificato nella richiesta.

```http
POST /rest/v1/lists/{listId}/leads.json?id=318594&id=318595
```

```json
{
    "requestId": "6860#1706170ba29",
    "result": [
        {
            "id": 318594,
            "status": "added"
        },
        {
            "id": 318595,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Rimuovi dall’elenco

L&#39;endpoint [Rimuovi dall&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) viene utilizzato per rimuovere uno o più membri da un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e uno o più parametri di query `id` che contengono ID lead (il massimo consentito è 300).

La risposta contiene un array `result` composto da oggetti JSON con lo stato per ogni ID lead specificato nella richiesta.

```http
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```json
{
    "requestId": "9e79#17061689ac3",
    "result": [
        {
            "id": 318603,
            "status": "removed"
        },
        {
            "id": 318595,
            "status": "removed"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Ottieni lead per ID elenco

L&#39;endpoint [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET) viene utilizzato per recuperare i membri di un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e consente a diversi parametri di query facoltativi di specificare criteri di filtro.

Il parametro `batchSize` viene utilizzato per specificare il numero di record di lead da restituire in una singola chiamata. Il valore predefinito e massimo è 300.

Il parametro `nextPageToken` viene utilizzato per impaginare tramite set di risultati di grandi dimensioni. Questo parametro non viene passato nella prima chiamata, ma solo nelle chiamate successive per l’impaginazione.

Il parametro `fields` contiene un elenco separato da virgole di nomi di campo da restituire nella risposta. Se il parametro `fields` non è incluso in questa richiesta, vengono restituiti i campi predefiniti seguenti: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` e `id`.

La risposta contiene un array `result` composto da oggetti JSON contenenti i campi lead specificati nella richiesta.

```http
GET /rest/v1/lists/{listId}/leads.json?batchSize=3
```

```json
{
    "requestId": "ddae#170615ba0cc",
    "result": [
        {
            "id": 318594,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Robert.L.Deacon@pookmail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318595,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Tyrone.V.Dyer@trashymail.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        },
        {
            "id": 318596,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Rex.M.Bailey@dodgit.com",
            "updatedAt": "2015-04-06T17:13:50Z",
            "createdAt": "2015-04-06T17:13:50Z"
        }
    ],
    "success": true,
    "nextPageToken": "PS5VL5WD4UOWGOUCJR6VY7JQO24LC2U5DRBU4WO4RQMPHDHTK2T3BEZOR75VLQXYB3245WW2GMDSK==="
}
```

## Membro dell’elenco

L&#39;endpoint [Member of List](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) viene utilizzato per verificare se uno o più lead sono membri di un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e uno o più parametri di query `id` che contengono ID lead (il massimo consentito è 300).

La risposta contiene un array `result` composto da oggetti JSON con lo stato per ogni ID lead specificato nella richiesta.

```http
GET /rest/v1/lists/{listId}/leads/ismember.json?id=309901&id=318603&id=999999
```

```json
{
    "requestId": "693a#17061475cf9",
    "result": [
        {
            "id": 309901,
            "status": "memberof"
        },
        {
            "id": 318603,
            "status": "notmemberof"
        },
        {
            "id": 999999,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1004",
                    "message": "Lead not found"
                }
            ]
        }
    ],
    "success": true
}
```
