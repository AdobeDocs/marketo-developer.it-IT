---
title: "Snippet"
feature: REST API, Snippets
description: '"Gestione dei snippet tramite l’API Marketo".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# Snippet

[Riferimento endpoint snippet](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets)

I snippet sono componenti HTML riutilizzabili che possono essere incorporati nelle e-mail e nelle pagine di destinazione e che possono essere segmentati per il contenuto dinamico. Ai frammenti non sono associati modelli e possono essere creati e distribuiti all&#39;interno di altre risorse in Marketo.

## Query

La query dei frammenti segue il modello standard per le risorse, tranne per il fatto che non dispone del metodo By Name. Entrambe [Per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/getSnippetByIdUsingGET) e [Sfoglia](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/getSnippetUsingGET) i metodi consentono di utilizzare il campo stato per recuperare le versioni approvate o bozza del frammento.

### Per ID

```
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

```
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

Il contenuto di un determinato frammento può essere recuperato in base all’ID del frammento.

```
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

La chiamata restituisce un elenco di sezioni di contenuto, costituite da sezioni di tipo HTML o DynamicContent e, facoltativamente, da una sezione con un tipo di testo.

## Crea e aggiorna

I frammenti seguono il pattern di creazione delle risorse complesse, in cui la chiamata a [crea snippet](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/createSnippetUsingPOST), e il relativo contenuto sono effettuati separatamente, pertanto la prima chiamata deve essere all&#39;endpoint di creazione, con una descrizione facoltativa.   I dati vengono passati come x-www-form-urlencoded, non come JSON.

```
POST /rest/asset/v1/snippets.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

L’aggiunta o la sostituzione del contenuto in uno snippet viene eseguita per ID. Il contenuto può essere di tipo Testo, HTML o DynamicContent. Se il tipo è Testo, il parametro di contenuto è l’endpoint di testo normale; se invece è HTML, si tratta del testo di markup desiderato. Se il tipo è impostato su DynamicContent, il parametro di contenuto deve essere impostato sull&#39;ID della segmentazione da associare al frammento.

```
POST /rest/asset/v1/snippet/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[Aggiornamento dei metadati](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/updateSnippetUsingPOST) viene eseguito anche dall’id. È possibile aggiornare solo il nome e la descrizione:

```
POST /rest/asset/v1/snippet/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

I frammenti seguono il pattern standard per il contenuto dinamico, ma rappresentano una sola sezione di contenuto intera, pertanto ogni frammento può contenere solo una sezione dinamica, con un elenco di sezioni interne facoltativamente per ogni segmento nella segmentazione utilizzata. È possibile eseguire query sul contenuto dinamico solo tramite l&#39;ID frammento, poiché in un frammento può essere presente una sola sezione di contenuto dinamico.

```
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

Gli snippet dispongono di endpoint per l&#39;approvazione, la revoca dell&#39;approvazione e l&#39;eliminazione delle bozze, che seguono il modello di risorsa standard. Lo stato di bozza di uno snippet deve essere quello di approvazione.

### Approva

```
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

Il `unapprove` l&#39;endpoint può essere utilizzato solo su snippet approvati.

```
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

```
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

[Clonazione di uno snippet](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/cloneSnippetUsingPOST) con l’API è semplice e segue il modello standard, con un nome obbligatorio, un ID dello snippet e della cartella originali e una descrizione facoltativa.  Se non esiste alcuna versione approvata, la versione bozza viene clonata.

```
POST /rest/asset/v1/snippet/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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
