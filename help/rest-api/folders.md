---
title: Cartelle
feature: REST API
description: Guida all’API REST di Marketo per le cartelle che includono creazione, aggiornamento, eliminazione, query per ID e nome, navigazione in massa con root, workspace, maxDepth e paginazione.
exl-id: 4b55c256-ef0a-42b4-9548-ff8a4106f064
TQID: https://experienceleague.adobe.com/OxCNdy8qW6jwq8u57RF9mqVKPVvH99UmuiOBjFprHCM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: c5f60233-d5ea-4453-a799-0ad258b4d399id: d65b4a73-87a3-4d56-b638-74e74d9939ceid: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 806
ht-degree: 1%

---

# Cartelle

[Riferimento endpoint cartelle](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders)

Le cartelle sono le risorse organizzative principali in Marketo. Per ogni altro tipo di risorsa è presente almeno un elemento padre, una cartella o un programma. Una cartella è puramente organizzativa, mentre un programma ha una relazione funzionale con altri tipi di risorse e può anche contenere risorse.

Utilizza l’API Cartelle per creare, eseguire query, aggiornare ed eliminare cartelle o recuperarne il contenuto. Le query delle cartelle possono restituire Programmi, ma è necessario utilizzare l&#39;API Programmi per creare, aggiornare o eliminare un Programma.

## Query

Le cartelle supportano i pattern di query delle risorse standard: [per id](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET) e per [navigazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderUsingGET).

### Per ID

```http
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

Il parametro `type` è obbligatorio e deve essere `Folder` o `Program`. Determina se l’endpoint cerca un ID cartella o un ID programma. L’endpoint restituisce un record nella matrice dei risultati.

La risposta `folderType` identifica ciò che la cartella può contenere. Le cartelle Attività di marketing hanno un tipo di cartella di marketing o programma e possono contenere più tipi di risorse. Le cartelle di Design Studio hanno un tipo che corrisponde alle risorse che possono contenere. Ad esempio, una cartella E-mail può contenere e-mail e sottocartelle con un tipo di cartella E-mail o Modello e-mail.

I tipi di cartella includono:

- E-mail
- Modello e-mail
- Pagina di destinazione
- Modello pagina di destinazione
- Snippet
- File

### Per nome

L&#39;endpoint [query per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET) richiede `name`, che esegue una corrispondenza esatta con i nomi delle cartelle e restituisce ogni cartella corrispondente.

L’endpoint accetta anche i seguenti parametri opzionali:

- `type`: tipo di cartella, `Folder` o `Program`.
- `root`: ID della cartella in cui eseguire la ricerca. Se imposti `root`, devi impostare anche `type`.
- `workspace`: nome dell&#39;area di lavoro in cui eseguire la ricerca.

```http
GET /rest/asset/v1/folder/byName.json?name=Test%2010%20-%20deverly
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "19#14e1f2f3688",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Marketing Programs - deverly/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Le attività di marketing e Design Studio sono cartelle principali. Recupera una directory principale per nome, quindi utilizzala per scorrere la gerarchia delle cartelle nell’istanza di destinazione.

### Sfoglia

È inoltre possibile [recuperare le cartelle in blocco](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderUsingGET). Utilizzare il parametro `root` per specificare la cartella padre in cui eseguire la query. Passa `root` come oggetto JSON incorporato con due membri:

1. `id`: ID della cartella o del programma.
1. `type`: `Folder` o `Program`, a seconda del tipo di cartella principale.

Se non conosci la cartella principale o desideri recuperare tutte le cartelle in un’area, utilizza la directory principale Marketing Activities, Design Studio o Lead Database. Recuperare l&#39;ID radice passando il nome dell&#39;area all&#39;API [Get Folder By Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/getFolderByNameUsingGET).

Come con altri endpoint per il recupero di risorse in blocco, utilizza i parametri opzionali `offset` e `maxReturn` per l&#39;impaginazione. Altri parametri facoltativi sono:

- `workSpace`: nome dell&#39;area di lavoro in base alla quale filtrare.
- `maxDepth`: numero massimo di livelli da attraversare nella gerarchia delle cartelle. Il valore 0 restituisce solo la cartella specificata da `root`. Il valore predefinito è 2.

```http
GET /rest/asset/v1/folders.json?root={"id":14,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9bd8#14e1f49047c",
    "result": [
        {
            "name": "Marketing Activities",
            "description": "Root node for the Marketing Activities app area",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 14,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": null,
            "path": "/Marketing Activities",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 14
        },
        {
            "name": "Default",
            "description": "Root node of the Marketing activities Default",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 15,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": {
                "id": 14,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 15
        },
        {
            "name": "Archive",
            "description": "",
            "createdAt": "2010-03-27T18:28:17Z+0000",
            "updatedAt": "2010-03-27T18:28:17Z+0000",
            "url": "https://app-abm.marketo.com/#MF157A1",
            "folderId": {
                "id": 310,
                "type": "Folder"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default/Archive",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 310
        }
    ]
}
```

## Struttura di risposta

I campi `folderId` e `parent` sono oggetti JSON che contengono l&#39;ID e il tipo della cartella. L&#39;API utilizza questo tipo nei parametri query, `root` e `parent` per distinguere i tipi di cartelle Cartella e Programma.

Il campo `folderType` descrive come viene utilizzata la cartella. Il suo valore può essere Cartella di marketing, Programma, E-mail, Modello e-mail, Pagina di destinazione, Modello pagina di destinazione, Snippet, Immagine, Zona o File. La cartella di marketing e il programma sono presenti nelle attività di marketing e possono contenere più tipi di risorse. Gli altri tipi di cartelle contengono solo il tipo di risorsa corrispondente, le sottocartelle e la versione modello di quel tipo di risorsa, se applicabile. La zona rappresenta una cartella a livello principale nelle attività di marketing.

La cartella `path` mostra la gerarchia come un percorso in stile Unix. La prima voce è sempre Marketing Activities (Attività di marketing) o Design Studio. Se l&#39;istanza dispone di aree di lavoro, la seconda voce corrisponde al nome dell&#39;area di lavoro proprietaria.

Il campo `url` contiene l&#39;URL della risorsa per l&#39;istanza designata. Non è un collegamento universale e richiede l’autenticazione dell’utente. Il campo `isSystem` indica se la cartella è di sistema di sola lettura. È possibile creare cartelle secondarie in una cartella di sistema.

## Crea e aggiorna

Per [creare una cartella](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders/operation/createFolderUsingPOST), inviare una richiesta POST `application/x-www-form-urlencoded` con i seguenti parametri:

- `name`: stringa obbligatoria contenente il nome della cartella.
- `parent`: oggetto JSON incorporato obbligatorio contenente `id` e `type`. Il tipo è `Folder` o `Program`, a seconda dell&#39;elemento padre.
- `description`: Stringa opzionale di massimo 2000 caratteri.

```http
POST /rest/asset/v1/folders.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
parent={"id":416,"type":"Folder"}&name=Test 10 - deverly&description=This is a test
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "111be#14e1f193e31",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Utilizzare l&#39;endpoint di aggiornamento per modificare i parametri facoltativi `description`, `name` o `isArchive`. Se si imposta `isArchive` su `true`, la cartella verrà archiviata nell&#39;interfaccia utente di Marketo. Se viene impostato su `false`, la cartella verrà rimossa dall&#39;archivio.

Non è possibile aggiornare i programmi con questa API.

```http
POST /rest/asset/v1/folder/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

### Elimina

Puoi eliminare una singola cartella solo se non contiene risorse o sottocartelle. Impossibile utilizzare questa API per eliminare un programma o una cartella il cui campo `isSystem` è `true`.

```http
POST /rest/asset/v1/folder/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4180#14e1f3fc017",
    "result": [
        {
            "id": 453
        }
    ]
}
```
