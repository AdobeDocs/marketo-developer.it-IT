---
title: Elenchi statici
feature: REST API, Static Lists
description: Utilizza le API REST di Marketo per eseguire query, creare, aggiornare ed eliminare elenchi statici, con endpoint per filtri ID, nome e visualizzazione, ambito cartella, paging e data.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# Elenchi statici

[Riferimento endpoint elenchi statici](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists)

[Riferimento endpoint appartenenza elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

Marketo offre un set di API REST per l’esecuzione di operazioni CRUD su elenchi statici. Queste API seguono il pattern di interfaccia standard per le API delle risorse, fornendo le opzioni Query, Create, Update e Delete.

## Query

La query degli elenchi statici segue i tipi di query standard per le risorse di [per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) e [sfoglia](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET).

### Per ID

[La query per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) accetta un singolo elenco statico `id` come parametro di percorso e restituisce un singolo record di elenco statico.

```
GET /rest/asset/v1/staticList/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "843c#1641f969e96",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Per nome

[La query per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Lists/operation/getSmartListByNameUsingGET) accetta un elenco statico `name` come parametro e restituisce un singolo record di elenco statico. Viene eseguita una corrispondenza esatta di stringa su tutti i nomi di elenco statici nell’istanza e restituisce un risultato per l’elenco statico che corrisponde a tale nome.

```
GET /rest/asset/v1/staticList/byName.json?name=Foundation Seed List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "28ab#1641fa246b9",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        }
    ]
}
```

#### Sfogliare

Gli elenchi statici possono anche essere [recuperati in batch](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListsUsingGET). Il parametro `folder` può essere utilizzato per specificare la cartella principale in cui verrà eseguita la query e viene formattato come oggetto JSON contenente ID e tipo. Come altri endpoint per il recupero di risorse in blocco, `offset` e `maxReturn` sono parametri facoltativi che possono essere utilizzati per il paging. I parametri `earliestUpdatedAt` e `latestUpdatedAt` consentono di impostare filigrane di data/ora basse e alte per la restituzione di elenchi statici creati o aggiornati all&#39;interno dell&#39;intervallo specificato. I valori Datetime devono essere stringhe ISO-8601 valide e non devono includere millisecondi

```
GET /rest/asset/v1/staticLists.json?folder={"id":13,"type":"Folder"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2dc0#1641f846633",
    "result": [
        {
            "id": 1021,
            "name": "Foundation Seed List",
            "createdAt": "2017-07-27T01:38:33Z+0000",
            "updatedAt": "2017-07-27T01:39:26Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1021A1"
        },
        {
            "id": 1022,
            "name": "Blacklist Seed List",
            "createdAt": "2017-07-27T23:19:33Z+0000",
            "updatedAt": "2017-07-27T23:21:29Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1022A1"
        },
        {
            "id": 1023,
            "name": "Possible Duplicates Seed List",
            "createdAt": "2017-07-28T00:10:02Z+0000",
            "updatedAt": "2017-07-28T00:11:22Z+0000",
            "folder": {
                "id": 13,
                "type": "Folder"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1023A1"
        }
    ]
}
```

## Crea e aggiorna

[La creazione di un elenco statico](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/createStaticListUsingPOST) viene eseguita con un&#39;applicazione/x-www-form-urlencoded POST con due parametri richiesti. Il parametro `folder` viene utilizzato per specificare la cartella principale in cui verrà creato l&#39;elenco statico e viene formattato come oggetto JSON contenente ID e tipo. Il parametro `name` viene utilizzato per denominare l&#39;elenco statico e deve essere univoco. Facoltativamente, è possibile utilizzare il parametro `description` per descrivere l&#39;elenco statico.

```
POST /rest/asset/v1/staticLists.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
folder={"id":1034,"type":"Program"}&name=My Static List
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1269d#164209d6e1e",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "createdAt": "2018-06-21T04:32:25Z+0000",
            "updatedAt": "2018-06-21T04:32:25Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

[Gli aggiornamenti a un elenco statico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/updateStaticListUsingPOST) vengono effettuati tramite un endpoint separato con due parametri facoltativi. Il parametro `description` può essere utilizzato per aggiornare la descrizione dell&#39;elenco statico. Il parametro `name` può essere utilizzato per aggiornare il nome dell&#39;elenco statico e deve essere univoco.

```
POST /rest/asset/v1/staticList/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is a static list used for testing
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "f84f#16420b4c746",
    "result": [
        {
            "id": 1027,
            "name": "My Static List",
            "description": "This is a static list used for testing",
            "createdAt": "2018-06-21T04:32:26Z+0000",
            "updatedAt": "2018-06-21T04:57:55Z+0000",
            "folder": {
                "id": 1034,
                "type": "Program"
            },
            "computedUrl": "https://app-sjqe.marketo.com/#ST1027A1"
        }
    ]
}
```

### Elimina

[L&#39;eliminazione di un elenco statico](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) richiede un singolo elenco statico `id` come parametro di percorso. Non è possibile eliminare elenchi statici utilizzati da un’operazione di importazione o esportazione o da altre risorse.

```
POST /rest/asset/v1/staticList/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "2c79#16420ded0e9",
    "result": [
        {
            "id": 1027
        }
    ]
}
```

## Appartenenza a elenco

Gli endpoint di appartenenza agli elenchi consentono di aggiungere, rimuovere ed eseguire query sui membri di elenchi statici. È inoltre possibile eseguire una query sull&#39;appartenenza a un elenco statico.

### Aggiungi all’elenco

L&#39;endpoint [Aggiungi all&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/addLeadsToListUsingPOST) è utilizzato per aggiungere uno o più membri a un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e uno o più parametri di query ID che contengono ID lead (il massimo consentito è 300).

La risposta contiene un array `result` composto da oggetti JSON con lo stato per ogni ID lead specificato nella richiesta.

```
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

### Rimuovi dall’elenco

L&#39;endpoint [Rimuovi dall&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/removeLeadsFromListUsingDELETE) è utilizzato per rimuovere uno o più membri da un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e uno o più parametri di query `id` che contengono ID lead (il massimo consentito è 300).

La risposta contiene un array `result` composto da oggetti JSON con lo stato per ogni ID lead specificato nella richiesta.

```
DELETE /rest/v1/lists/{listId}/leads.json?id=318603&id=318595&id=999999
```

```
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

### Elenco query

L&#39;endpoint [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET) viene utilizzato per recuperare i membri di un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e consente a diversi parametri di query facoltativi di specificare criteri di filtro.

Il parametro `batchSize` viene utilizzato per specificare il numero di record di lead da restituire in una singola chiamata (il valore predefinito è 300).

Il parametro `nextPageToken` viene utilizzato per impaginare tramite set di risultati di grandi dimensioni. Questo parametro non viene passato nella prima chiamata, ma solo nelle chiamate successive per l’impaginazione.

Il parametro `fields` contiene un elenco separato da virgole di nomi di campo da restituire nella risposta. Se il parametro fields non è incluso in questa richiesta, vengono restituiti i seguenti campi predefiniti: email, updatedAt, createdAt, lastName, firstName e id.

La risposta contiene un array `result` composto da oggetti JSON contenenti i campi lead specificati nella richiesta.

```
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

#### Query appartenenza a elenco per ID lead

L&#39;endpoint [Member of List](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/areLeadsMemberOfListUsingGET) viene utilizzato per verificare se uno o più lead sono membri di un elenco. L&#39;endpoint accetta un parametro di percorso `listId` obbligatorio e uno o più parametri di query `id` che contengono ID lead (il massimo consentito è 300).

La risposta contiene un array `result` composto da oggetti JSON con lo stato per ogni ID lead specificato nella richiesta.

```
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
