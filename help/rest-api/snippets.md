---
title: Snippet
feature: REST API, Snippets
description: API REST di Marketo Asset per snippet, che copre le query per ID e sfoglia con lo stato, l’ottenimento di contenuti, la creazione e l’aggiornamento di HTML, testo e contenuti dinamici.
exl-id: 87901c29-ee59-4224-848d-3bd6a6c52718
TQID: https://experienceleague.adobe.com/1UpwX-ZzXTzkTRheu8exBDIoIvAGgoZgpA851PuL8sI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 386
ht-degree: 2%

---

# Snippet

[Riferimento endpoint snippet](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets)

I snippet sono componenti HTML riutilizzabili che possono essere incorporati nelle e-mail e nelle pagine di destinazione. Puoi segmentare snippet per il contenuto dinamico. I snippet non utilizzano modelli e possono essere creati e distribuiti all’interno di altre risorse Marketo.

## Query

Query snippet [per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets/operation/getSnippetByIdUsingGET) o per [navigazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets/operation/getSnippetUsingGET). L’API non fornisce un metodo query per nome. Entrambi gli endpoint accettano il campo `status` per recuperare una versione approvata o bozza.

### Per ID

```http
GET /rest/asset/v1/snippet/{id}.json?status=approved
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"fa0f#14b04375f0a",
   "result":[
      {
         "id":83,
         "name":"BYkHVJEedl",
         "description":"yzSLvNFyrmeVmyLzqryUfGlDOJTnvyyfsQTXPDCGdCwcWUlfoCNApUqYgwZGElrUFoxBHJcMdXdqTKvtjtfsmPgokyRgVLeHyJCw",
         "createdAt":"2015-01-19T22:01:52Z+0000",
         "updatedAt":"2015-01-19T22:01:52Z+0000",
         "folder":{
            "type":"Folder",
            "value":662
         },
         "status":"approved",
         "workspace":"Default"
      }
   ]
}
```

### Sfogliare

```http
GET /rest/asset/v1/snippets.json?maxReturn=3
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"f9cc#14b04376181",
   "result":[
      {
         "id":23,
         "name":"ADJvMLBMpS",
         "description":"XkzFUVLXVHrojLGLJVLPpwguOXuDvhAqaaSkBUVzgHrgDhqqRzyXlULIXSHJvfBHjCSaMwjyEdrdxcjFCRoNFVvdBBTDfSrUJzaR",
         "createdAt":"2015-01-15T20:10:39Z+0000",
         "updatedAt":"2015-01-15T20:10:39Z+0000",
         "url": null,
         "folder":{
            "type":"Folder",
            "value":620,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      },
      {
         "id":46,
         "name":"Biswa Snippet",
         "description":"",
         "createdAt":"2015-01-16T05:18:55Z+0000",
         "updatedAt":"2015-01-16T05:19:27Z+0000",
         "url": null,
         "folder":{
            "type":"Folder",
            "value":630,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      },
      {
         "id":12,
         "name":"dJJQkKbUYq",
         "description":"VXuHkYMREHrhxUSgYbKfaNeLisdFxOromCXQNrgmModvkuoyZdQjtAbXxDUbBvoDVCZmAVbasiHyWoWfTwgrGxnzpKepGrAUvfen",
         "createdAt":"2015-01-15T05:12:33Z+0000",
         "updatedAt":"2015-01-15T05:12:33Z+0000",
         "url": null,
         "folder":{
            "type":"Folder",
            "value":615,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      }
   ]
}
```

## Contenuto della query

Recupera il contenuto dei frammenti per ID frammento.

```http
GET /rest/asset/v1/snippet/{id}/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"5c50#14b04376159",
   "result":[
      {
         "type":"HTML",
         "content":"draft testUpdateSnippetContent1 HTML Content"
      },
      {
         "type":"Text",
         "content":"draft testUpdateSnippetContent1 Text Content"
      }
   ]
}
```

La risposta contiene sezioni di tipo `HTML` o `DynamicContent`. Può inoltre contenere una sezione di tipo `Text`.

## Crea e aggiorna

Crea separatamente la risorsa frammento e il relativo contenuto. Innanzitutto, chiama l&#39;endpoint [create snippet](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets/operation/createSnippetUsingPOST). La descrizione è facoltativa. Passa i dati come `x-www-form-urlencoded`, non come JSON.

```http
POST /rest/asset/v1/snippets.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Test Snippet 09 - deverly&folder={"id":395,"type":"Folder"}&description=This is a test snippet
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd57#14e231ee3a1",
    "result": [
        {
            "id": 13,
            "name": "Test Snippet 09 - deverly",
            "description": "This is a test snippet",
            "createdAt": "2015-06-24T01:11:43Z+0000",
            "updatedAt": "2015-06-24T01:11:43Z+0000",
            "url": "https://app-abm.marketo.com/#SN13B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Aggiungi o sostituisci il contenuto del frammento per ID. Il tipo di contenuto può essere `Text`, `HTML` o `DynamicContent`.

- Per `Text`, passare il testo normale nel parametro `content`.
- Per `HTML`, passare il markup nel parametro `content`.
- Per `DynamicContent`, impostare `content` sull&#39;ID della segmentazione associata al frammento.

```http
POST /rest/asset/v1/snippet/{id}/content.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
type=HTML&content=draft testUpdateSnippetContent1 HTML Content
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"73d9#14b04376139",
   "result":[
      {
         "id":82
      }
   ]
}
```

Per [aggiornare i metadati](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets/operation/updateSnippetUsingPOST), specificare l&#39;ID del frammento. È possibile aggiornare solo il nome e la descrizione.

```http
POST /rest/asset/v1/snippet/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Test Snippet&description=New Description
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14b043762b1",
   "result":[
      {
         "id":82,
         "name":"Test Snippet",
         "description":"New Description",
         "createdAt":"2015-01-19T22:01:52Z+0000",
         "updatedAt":"2015-01-19T22:01:53Z+0000",
         "url": "https://app-abm.marketo.com/#SN3B2ZN395",
         "folder":{
            "type":"Folder",
            "value":662,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      }
   ]
}
```

## Contenuto dinamico

Uno snippet rappresenta una sezione di contenuto completa e può contenere una sola sezione dinamica. Tale sezione può contenere una sezione interna per ciascun segmento nella segmentazione associata.

Poiché uno snippet può avere una sola sezione dinamica, esegui una query sul contenuto dinamico in base all’ID dello snippet.

```http
GET /rest/asset/v1/snippet/{id}/dynamicContent.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "ae3#14c2b499111",
    "result": [
        {
            "createdAt": "2015-03-13T06:24:35Z+0000",
            "updatedAt": "2015-03-17T20:29:42Z+0000",
            "id": 70,
            "segmentation": 1001,
            "content": [
                {
                    "id": "Nzk*",
                    "segmentId": 1001,
                    "segmentName": "Area",
                    "content": "Sample HTML for Area",
                    "type": "HTML"
                },
                {
                    "id": "Nzk*",
                    "segmentId": 1001,
                    "segmentName": "Area",
                    "content": "Sample Text for Area",
                    "type": "Text"
                },
                {
                    "id": "Nzk*",
                    "segmentId": 1002,
                    "segmentName": "Default",
                    "content": "Sample HTML for Default",
                    "type": "HTML"
                },
                {
                    "id": "Nzk*",
                    "segmentId": 1002,
                    "segmentName": "Default",
                    "content": "Sample Text for Default",
                    "type": "Text"
                }
            ]
        }
    ]
}
```

## Approvazione

I frammenti forniscono endpoint per l&#39;approvazione, la rimozione dell&#39;approvazione e l&#39;eliminazione delle bozze. Lo stato di bozza di uno snippet deve essere impostato prima dell&#39;approvazione.

### Approvazione

```http
POST /rest/asset/v1/snippet/{id}/approveDraft.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "11903#14db1af2f6c",
    "result": [
        {
            "id": 3,
            "name": "Test Snippet 02 - deverly",
            "description": "hey this is a test snippet!",
            "createdAt": "2015-06-02T00:32:37Z+0000",
            "updatedAt": "2015-06-02T00:32:37Z+0000",
            "url": "https://app-abm.marketo.com/#SN3B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "approved",
            "workspace": "Default"
        }
    ]
}
```

### Annulla approvazione

L&#39;endpoint `unapprove` può essere utilizzato solo su snippet approvati.

```http
POST /rest/asset/v1/snippet/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "7d20#14db1c7a2a9",
    "result": [
        {
            "id": 89,
            "name": "Test Snippet 01 - deverly",
            "description": "",
            "createdAt": "2015-05-15T19:01:22Z+0000",
            "updatedAt": "2015-05-15T19:07:07Z+0000",
            "url": "https://app-abm.marketo.com/#SN1B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

### Elimina bozza

Lo stato del frammento deve essere Bozza per essere eliminato.  Uno snippet approvato non può essere eliminato.

```http
POST /rest/asset/v1/snippet/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"674c#14b043760de",
   "result":[
      {
         "id":88
      }
   ]
}
```

## Duplica

Per [clonare un frammento](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets/operation/cloneSnippetUsingPOST), specificare un nome, l&#39;ID del frammento di origine e una cartella. La descrizione è facoltativa. Se l’origine non dispone di una versione approvata, l’endpoint clona la relativa bozza.

```http
POST /rest/asset/v1/snippet/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Test Snippet Clone 3 - deverly&folder={"id":395,"type":"Folder"}&description=This is a test snippet
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "21c9#14e2327e33d",
    "result": [
        {
            "id": 14,
            "name": "Test Snippet Clone 3 - deverly",
            "description": "This is a test snippet",
            "createdAt": "2015-06-24T01:21:33Z+0000",
            "updatedAt": "2015-06-24T01:21:33Z+0000",
            "url": "https://app-abm.marketo.com/#SN14B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```
