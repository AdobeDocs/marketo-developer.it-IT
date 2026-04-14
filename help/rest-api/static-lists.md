---
title: Elenchi statici
feature: REST API, Static Lists
description: Utilizza le API REST di Marketo per eseguire query, creare, aggiornare ed eliminare elenchi statici, con endpoint per filtri ID, nome e visualizzazione, ambito cartella, paging e data.
exl-id: 20679fd2-fae2-473e-84bc-cb4fdf2f5151
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---

# Elenchi statici

[Riferimento endpoint elenchi statici](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

Marketo offre un set di API REST per l’esecuzione di operazioni CRUD su elenchi statici. Queste API seguono il pattern di interfaccia standard per le API delle risorse, fornendo le opzioni Query, Create, Update e Delete.

Per le operazioni del database lead sui membri elenco, vedere [Appartenenza elenco](list-membership.md).

## Query

La query degli elenchi statici segue i tipi di query standard per le risorse di [per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) e [sfoglia](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListsUsingGET).

### Per ID

[La query per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) accetta un singolo elenco statico `id` come parametro di percorso e restituisce un singolo record di elenco statico.

```http
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

[La query per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) accetta un elenco statico `name` come parametro e restituisce un singolo record di elenco statico. Viene eseguita una corrispondenza esatta di stringa su tutti i nomi di elenco statici nell’istanza e restituisce un risultato per l’elenco statico che corrisponde a tale nome.

```http
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

Gli elenchi statici possono anche essere [recuperati in batch](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListsUsingGET). Il parametro `folder` può essere utilizzato per specificare la cartella principale in cui verrà eseguita la query e viene formattato come oggetto JSON contenente `id` e `type`. Come altri endpoint per il recupero di risorse in blocco, `offset` e `maxReturn` sono parametri facoltativi che possono essere utilizzati per il paging. I parametri `earliestUpdatedAt` e `latestUpdatedAt` consentono di impostare filigrane di data/ora basse e alte per la restituzione di elenchi statici creati o aggiornati all&#39;interno dell&#39;intervallo specificato. I valori Datetime devono essere stringhe ISO-8601 valide e non devono includere millisecondi.

```http
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

[La creazione di un elenco statico](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/createStaticListUsingPOST) viene eseguita con un POST `application/x-www-form-urlencoded` con due parametri richiesti. Il parametro `folder` viene utilizzato per specificare la cartella principale in cui verrà creato l&#39;elenco statico e viene formattato come oggetto JSON contenente `id` e `type`. Il parametro `name` viene utilizzato per denominare l&#39;elenco statico e deve essere univoco. Facoltativamente, è possibile utilizzare il parametro `description` per descrivere l&#39;elenco statico.

```http
POST /rest/asset/v1/staticLists.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

[L&#39;aggiornamento di un elenco statico](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/updateStaticListUsingPOST) viene eseguito tramite un endpoint separato con due parametri facoltativi. Il parametro `description` può essere utilizzato per aggiornare la descrizione dell&#39;elenco statico. Il parametro `name` può essere utilizzato per aggiornare il nome dell&#39;elenco statico e deve essere univoco.

```http
POST /rest/asset/v1/staticList/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

## Elimina

[L&#39;eliminazione di un elenco statico](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/deleteStaticListByIdUsingPOST) richiede un singolo elenco statico `id` come parametro di percorso. Non è possibile eliminare elenchi statici utilizzati da un’operazione di importazione o esportazione o da altre risorse.

```http
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
