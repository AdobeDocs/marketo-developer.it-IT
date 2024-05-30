---
title: "Cartelle"
feature: REST API
description: "Manipolazione delle cartelle con l’API di Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---


# Cartelle

[Riferimento endpoint cartelle](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders)

Le cartelle sono la risorsa organizzativa principale in Marketo e ogni altro tipo di risorsa ha almeno una cartella come elemento principale. Questa cartella principale può essere una cartella puramente organizzativa o un programma con una relazione funzionale con altri tipi di risorse e può anche essere l’elemento principale di altre risorse. Le cartelle possono essere create, interrogate, aggiornate ed eliminate tramite l’API, e consentono inoltre di recuperare un elenco dei relativi contenuti. Anche se i programmi possono essere restituiti tramite l&#39;esecuzione di query sull&#39;API delle cartelle, la creazione, l&#39;aggiornamento e l&#39;eliminazione dei programmi devono essere eseguiti tramite l&#39;API dei programmi.

## Query

La query delle cartelle segue i tipi di query standard per le risorse di [per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET), e [navigazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET).

### Per ID

```
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

Il parametro di tipo è obbligatorio e deve essere uno tra &quot;Folder&quot; (Cartella) o &quot;Program&quot; (Programma).  Il tipo determina se la ricerca nella cartella viene eseguita in base a un ID cartella o a un ID programma. Per questo endpoint, nell&#39;array dei risultati viene restituito un solo record. Osserva il parametro folderType nella risposta. Questo può indicare molti tipi diversi di cartelle. Le cartelle Attività di Marketo dispongono di un tipo Cartella di marketing o Programma, che può contenere diversi tipi di risorse, mentre le cartelle di Design Studio hanno un tipo corrispondente al tipo di risorsa che possono contenere. Ad esempio, una cartella con folderType impostato su &quot;E-mail&quot; può contenere solo e-mail o altre sottocartelle, che possono avere folderType impostato su E-mail o Modello e-mail. I tipi possono includere:

- E-mail
- Modello e-mail
- Pagina di destinazione
- Modello per pagina di destinazione
- Frammento
- File

### Per nome

[Query per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) è consentito anche. L&#39;endpoint di query per nome presenta il nome come unico parametro obbligatorio. Name esegue una corrispondenza esatta di stringa con il campo name delle cartelle nell’istanza e restituisce i risultati per ogni cartella che corrisponde a tale nome. Inoltre, dispone dei parametri di query facoltativi &quot;type&quot;, che possono essere Folder o Program (Cartella o Programma), &quot;root&quot; (Radice) l’ID della cartella da cercare, oppure &quot;workspace&quot; il nome dell’area di lavoro in cui eseguire la ricerca. Se è impostato il parametro root, è necessario impostare anche il parametro type.

```
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

Durante la ricerca per nome, è importante notare che sia Marketing Activities che Design Studio sono cartelle principali proprie, e possono quindi essere recuperate per nome e utilizzate per scorrere il resto della gerarchia di cartelle in un’istanza di destinazione.

### Sfogliare

Le cartelle possono anche essere [recuperato in blocco](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET). Il parametro &quot;root&quot; può essere utilizzato per specificare la cartella principale in cui verrà eseguita la query e viene formattato come oggetto JSON incorporato come valore per il parametro di query. La radice ha due membri:

1. id: l’ID della cartella o del programma.
1. tipo: cartella o programma, a seconda del tipo di cartella principale da browser.

Se la cartella principale non è nota o l’obiettivo è quello di recuperare tutte le cartelle in una determinata area, è possibile specificarla come area &quot;Marketing Activities&quot;, &quot;Design Studio&quot; o &quot;Lead Database&quot;. Gli ID di ciascuno di questi possono essere recuperati tramite il [Ottieni cartella per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) e specificando il nome dell’area desiderata.

Come altri endpoint per il recupero di risorse in blocco, offset e maxReturn sono parametri facoltativi per il paging.   Altri parametri facoltativi sono:

- workSpace: il nome dell’area di lavoro a cui applicare il filtro.
- maxDepth: il numero massimo di livelli da attraversare nella gerarchia di cartelle. Se è impostato su 0, viene restituita solo la cartella specificata nella directory principale. Se non viene specificato, il valore predefinito è 2.

```
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

Gran parte della struttura di risposta delle cartelle è auto-esplicativa, ma alcuni campi meritano di essere annotati singolarmente. Il `folderId` I campi padre e sono oggetti JSON che includono l’id esplicito e il tipo della cartella stessa. Questo tipo viene utilizzato dall’API nelle query, nei parametri radice e padre per garantire una corretta distinzione tra i tipi di cartelle Folder e Program. `folderType` riflette l’utilizzo della cartella, che può essere &quot;Cartella di marketing&quot;, &quot;Programma&quot;, &quot;E-mail&quot;, &quot;Modello e-mail&quot;, Pagina di destinazione&quot;, Modello pagina di destinazione&quot;, &quot;Snippet&quot;, &quot;Immagine&quot;, &quot;Zona&quot; o &quot;File&quot;.  I tipi Cartella di marketing e Programma indicano che sono presenti nelle Attività di marketing e possono contenere più tipi di risorse. Gli altri tipi indicano che possono contenere solo quel tipo di risorsa, le sottocartelle e la versione modello di quel tipo, se applicabile. Il tipo Zone rappresenta le cartelle a livello principale presenti in Attività di marketing.

Il percorso di una cartella mostra la relativa gerarchia nella struttura ad albero delle cartelle, simile a un percorso in stile Unix. La prima voce nel percorso sarà sempre Marketing Activities (Attività di marketing) o Design Studio. Se l’istanza di destinazione dispone di workspace, la seconda voce nel percorso sarà il nome del workspace proprietario. Il `url` mostra l’URL esplicito della risorsa nell’istanza designata. Questo non è un collegamento universale e per funzionare correttamente deve essere autenticato come utente. `isSystem` indica se la cartella è di sistema. Se questa opzione è impostata su true, la cartella stessa è di sola lettura, anche se le cartelle possono essere create come elementi secondari.

## Crea e aggiorna

[Creazione di cartelle](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/createFolderUsingPOST) è semplice e viene eseguito con un application/x-www-form-urlencoded POST che dispone di due parametri obbligatori, &quot;name&quot;, una stringa e &quot;parent&quot;, l’elemento principale per la creazione della cartella in, un oggetto JSON incorporato con due membri, id e tipo, Folder o Program, a seconda del tipo di cartella di destinazione. Facoltativamente, è possibile includere anche &quot;description&quot;, una stringa, che può contenere fino a 2000 caratteri.

```
POST /rest/asset/v1/folders.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Gli aggiornamenti alle cartelle vengono effettuati tramite un endpoint separato, oltre a descrizione, nome e `isArchive` sono parametri facoltativi da aggiornare. Se `isArchive` viene modificato da un aggiornamento, ciò determina l’archiviazione della cartella, se viene modificato in true, o l’annullamento dell’archiviazione, se viene modificato in false, nell’interfaccia utente di Marketo. I programmi non possono essere aggiornati con questa API.

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Le eliminazioni possono essere eseguite su singole cartelle se vuote, ovvero se non contengono risorse o sottocartelle. Se una cartella è di tipo Program o il campo isSystem è impostato su true, non può essere eliminata con questa API.

```
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
