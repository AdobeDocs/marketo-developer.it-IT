---
title: Appartenenza a elenco (elenchi statici)
feature: REST API, Static Lists
description: Utilizza le API REST del database dei lead di Marketo per aggiungere lead a elenchi statici, rimuovere lead, recuperare membri dell’elenco e controllare l’appartenenza all’elenco.
exl-id: b8f74bcf-834a-44db-81fd-621048afeba4
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 5%

---

# Appartenenza a elenco (elenchi statici)

[Riferimento endpoint appartenenza elenco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists)

Le API di appartenenza a elenco forniscono endpoint di database lead per la gestione dei membri di elenco statici. Utilizza questi endpoint per:

- Aggiungere lead a un elenco.
- Rimuovere i lead da un elenco.
- Recuperare i membri di un elenco.
- Determinare se i lead sono membri di un elenco.

## Endpoint

| Endpoint | Metodo | Percorso |
| --- | --- | --- |
| Aggiungi all’elenco | POST | `/rest/v1/lists/{listId}/leads.json` |
| Rimuovi dall’elenco | DELETE | `/rest/v1/lists/{listId}/leads.json` |
| Ottieni lead per ID elenco | GET | `/rest/v1/lists/{listId}/leads.json` |
| Membro dell’elenco | GET | `/rest/v1/lists/{listId}/leads/ismember.json` |

## Aggiungi all’elenco

Utilizzare l&#39;endpoint [Aggiungi all&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/addLeadsToListUsingPOST) per aggiungere uno o più membri a un elenco. Passa il parametro di percorso `listId` richiesto e uno o più parametri di query `id` che contengono ID lead. Il numero massimo di ID lead è 300.

La risposta contiene un array `result` con lo stato di ogni ID lead nella richiesta.

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

Utilizzare l&#39;endpoint [Rimuovi dall&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) per rimuovere uno o più membri da un elenco. Passa il parametro di percorso `listId` richiesto e uno o più parametri di query `id` che contengono ID lead. Il numero massimo di ID lead è 300.

La risposta contiene un array `result` con lo stato di ogni ID lead nella richiesta.

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

Utilizzare l&#39;endpoint [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET) per recuperare i membri di un elenco. Passa il parametro di percorso `listId` richiesto. Puoi anche trasmettere parametri di query facoltativi per specificare i criteri di filtro.

I parametri di query facoltativi sono:

- `batchSize`: specifica il numero di record lead da restituire in una chiamata. Il valore predefinito e massimo è 300.
- `nextPageToken`: impaginazione tramite set di risultati di grandi dimensioni. Ometti questo parametro dalla prima chiamata e includilo nelle chiamate successive.
- `fields`: specifica un elenco separato da virgole di nomi di campo da restituire. Se si omette questo parametro, la risposta include `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` e `id`.

La risposta contiene un array `result` con i campi lead specificati nella richiesta.

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

Utilizzare l&#39;endpoint [Member of List](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) per determinare se uno o più lead sono membri di un elenco. Passa il parametro di percorso `listId` richiesto e uno o più parametri di query `id` che contengono ID lead. Il numero massimo di ID lead è 300.

La risposta contiene un array `result` con lo stato di ogni ID lead nella richiesta.

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
