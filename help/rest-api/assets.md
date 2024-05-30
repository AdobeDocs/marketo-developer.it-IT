---
title: "Assets"
feature: REST API
description: '"API per l’utilizzo delle risorse Marketo".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 1%

---


# Risorse

Marketo fornisce API per interagire con la maggior parte delle risorse di marketing e organizzative all’interno di Marketo.

## Risorse

Le risorse Marketo includono:

- Cartelle
- Programmi
- E-mail
- Modelli e-mail
- Pagine di destinazione
- Modelli di pagina di destinazione
- Snippet
- Forms
- Token
- File

## API

Per un elenco completo degli endpoint API di Asset, inclusi i parametri e le informazioni di modellazione, consulta [Riferimento endpoint API di Asset](endpoint-reference.md).

## Query

Le risorse in genere hanno tre pattern tramite i quali possono essere recuperate: per ID, per nome e tramite navigazione.  Sia per ID che per nome verrà recuperata una singola risorsa per un dato parametro, mentre la navigazione restituirà e consentirà la paginazione attraverso l’intero elenco di risorse di quel tipo.  I singoli tipi di risorse hanno parametri diversi in base ai quali possono essere filtrati; per informazioni specifiche, consulta i singoli documenti.

In alcuni casi, l’endpoint &quot;browse&quot; per alcuni tipi di risorse non restituirà risorse secondarie, come i valori consentiti per un tag, e deve essere recuperato singolarmente utilizzando l’endpoint &quot;By Name&quot; o &quot;By Id&quot; per restituire il set completo di metadati.  Altri possono avere endpoint separati interamente per il recupero di oggetti dipendenti come Campi modulo.

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

### Per nome

Per motivi tecnici, le API di Asset non sono in grado di cercare i nomi delle risorse contenenti virgole (,).  È consigliabile che la convenzione di denominazione escluda le virgole per tutti i tipi di risorse.

```
GET /rest/asset/v1/file/byName.json?name=My File
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":148,
         "size":270313,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/piKLbhVFvW",
         "folder":{
            "type":"Email",
            "id":10614
         },
         "name":"My File",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Sfogliare

L’esplorazione delle risorse consente sempre due parametri di query:

- offset - Offer intero da cui restituire i risultati.
- maxReturn - Limita il numero di record restituiti.  Il valore predefinito è 20 se non impostato e ha un massimo di 200.

```
GET /rest/asset/v1/emailTemplates.json?offset=10&maxReturn=50
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"33c4#14a1832b4a8",
   "result":[
      {
         "id":18,
         "name":"AAA0unit3CreateTestEmailTemplateName.2314673e-7bc2-47da-a1e8-66dfdd8a1f1d",
         "description":"AssetAPI: getTemplates test",
         "createdAt":"2014-11-03T19:52:58Z+0000",
         "updatedAt":"2014-11-03T19:52:58Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":177,
         "name":"ABfRGutnwN",
         "description":"HMmHkdTRrGaRpPakdgGKICxfMunCEWDUWiThgAbInfaBXxGxSFfjKQIwerngCHRlGTnAJhKPmwlXLcsjGPtWEiILGyeIJTNVHoHg",
         "createdAt":"2014-11-20T19:31:06Z+0000",
         "updatedAt":"2014-11-20T19:31:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":148,
         "name":"ADVHJBQLyw",
         "description":null,
         "createdAt":"2014-11-20T06:42:57Z+0000",
         "updatedAt":"2014-11-20T06:42:57Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

## Crea e aggiorna

Per i tipi di risorse semplici come Cartelle, Token e File, in genere è disponibile un solo endpoint per la creazione e quindi un endpoint aggiuntivo per l’aggiornamento dei record per ID.  Le risorse vengono create con un nome che è sempre obbligatorio, quindi tutti i metadati e gli ID vengono restituiti dalla risposta di creazione o aggiornamento.

Ad esempio, di seguito è riportato come creare un token:

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&value=2015-04-01&type=date&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

Per aggiornare una cartella, effettua le seguenti operazioni:

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

Altre risorse hanno strutture più complesse e richiedono aggiornamenti a sottosezioni aggiuntive o oggetti secondari; infine, devono essere approvate prima di poter essere utilizzate.  Questi tipi di risorse includono Forms, E-mail, Modelli e-mail, Pagine di destinazione e Modelli di pagina di destinazione.  Ognuno di questi avrà un singolo endpoint per la creazione di un record, quindi ulteriori endpoint per l’aggiornamento di metadati, contenuto e sezioni di contenuto.

Ad esempio, per creare una pagina di destinazione, devi chiamare il relativo endpoint di creazione con un ID modello, quindi recuperarne le sezioni di contenuto e aggiornarle singolarmente per aggiungere contenuto, prima di approvarle in modo che possa essere distribuita in tempo reale.

### Creazione complessa

Le pagine di destinazione richiedono innanzitutto la creazione di una risorsa Pagina di destinazione utilizzando un modello principale.  Viene creata una nuova pagina di destinazione contenente il contenuto predefinito del modello per ogni sezione di contenuto.

```
POST rest/asset/v1/landingPages.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=createLandingPage&folder={"type": "Folder", "id": 11}&template=1&description=this is a test&workspace=default&title=test create&keywords=awesome&formPrefill=false
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#154cf7922c6",
    "result": [
        {
            "id": 27,
            "name": "createLandingPage",
            "description": "this is a test",
            "createdAt": "2016-05-20T18:41:43Z+0000",
            "updatedAt": "2016-05-20T18:41:43Z+0000",
            "folder": {
                "type": "Folder",
                "value": 11,
                "folderName": "Landing Pages"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 1,
            "title": "test create",
            "keywords": "awesome",
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "https://app-devlocal1.marketo.com/lp/622-LME-718/createLandingPage.html",
            "computedUrl": "https://app-devlocal1.marketo.com/#LP27B2"
        }
    ]
}
```

#### Recupera sezioni

Per compilare il contenuto di una pagina di destinazione, è necessario recuperare l’elenco delle sezioni di contenuto e quindi eseguire singoli aggiornamenti per qualsiasi sezione che si discosta dal modello.

```
GET /rest/asset/v1/landingPage/{id}/content.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154ea1689d7",
    "result": [
        {
            "id": "67",
            "type": "Form",
            "index": 1,
            "content": {
                "content": "189",
                "contentType": "Form",
                "contentUrl": "https://app-devlocal1.marketo.com/#FO189A1ZN13LA1"
            },
            "formattingOptions": {
                "zIndex": 15,
                "left": "359px",
                "top": "122px"
            }
        }
    ]
}
```

#### Aggiorna sezione

```
POST /rest/asset/v1/landingPage/{id}/content/{contentId}.json?type=Form&value=1
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#154ea32cf11",
    "result": [
        {
            "id": 174
        }
    ]
}
```

## Approvazione

A molti tipi di risorse è associato un sistema di bozza e approvazione, tra cui e-mail, pagine di destinazione, snippet, Forms e i modelli corrispondenti.  Se si tenta di approvare una risorsa, questa verrà valutata in base a un set specifico di regole di convalida e quindi impostata su uno stato approvato oppure verrà restituito un motivo di errore.  Per questi tipi di risorse, ogni volta che viene effettuato un aggiornamento al contenuto di una particolare risorsa, vengono apportate modifiche a una bozza della risorsa, che non influiscono sulla versione approvata.  Questo consente di apportare modifiche al contenuto in modo sicuro senza influire sulle versioni live della risorsa.  Le modifiche possono quindi essere applicate alla versione live utilizzando l’endpoint di approvazione.  Questo cancella anche lo stato di bozza della risorsa fino a quando non vengono applicati eventuali aggiornamenti aggiuntivi.

```
POST /rest/asset/v1/emailTemplate/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"abe2#14a1832a97d",
   "result":[
      {
         "id":338,
         "name":"lvAVYMZqPS",
         "description":"fZLJQSJRvnYbjGTUpIHHqDOuQgQzXQcWIXoOUPwrVLdMHKcbRqwLoSLkWZTUmaMiCIJSfQiufnnrgITUIqjuAPBLpmliiKuIUFYG",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Approved",
         "workspace":"Default"
      }
   ]
}
```

L’approvazione corretta sostituisce la versione live precedente con la versione aggiornata.

L’eliminazione delle bozze è disponibile anche tramite un endpoint per ogni tipo di risorsa valido.  Se si utilizza questa opzione su una risorsa in stato Approvato con bozza, la bozza corrente e le eventuali modifiche in sospeso verranno eliminate.  Se si utilizza questa su una risorsa per la quale non è al momento disponibile una versione approvata, non viene eseguita alcuna operazione e viene restituito un errore.  Le risorse di sola bozza possono essere eliminate, ma non possono essere eliminate.

```
POST /rest/asset/v1/emailTemplate/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"17bfa#14a1832b3c4",
   "result":[
      {
         "id":344,
         "name":"LkilkvKrkp",
         "description":"yAyUEXuWMtdhpODUmnCkGjpBcyEKnYucxaSoTyYeQzyNbYanxCXWPOzwiIWmeXPUwjfGAUmgnxlhgOPluVqwNittuvxJmNTaHxYM",
         "createdAt":"2014-12-05T02:06:23Z+0000",
         "updatedAt":"2014-12-05T02:06:23Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

Le risorse possono anche non essere approvate se si trovano in uno stato di sola approvazione.  In questo modo verranno eliminate tutte le versioni live della risorsa e la risorsa tornerà allo stato di sola bozza, scartando anche le bozze associate.  Questa azione può essere eseguita solo sulla maggior parte delle risorse se non è in uso in alcun punto di Marketo, ad esempio un’e-mail a cui si fa riferimento in un passaggio del flusso Invia e-mail o uno snippet incorporato in un messaggio e-mail.

```
POST /rest/asset/v1/email/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"3514#14a1832b0fa",
   "result":[
      {
         "id":1364
      }
   ]
}
```

## Elimina

Le risorse con stato di approvazione e bozza, ad eccezione dei moduli, non possono essere eliminate durante l&#39;approvazione e devono essere non approvate prima dell&#39;eliminazione.  In genere, le eliminazioni possono essere eseguite solo quando una risorsa non è approvata e non è più utilizzata e, nel caso delle cartelle, quando le risorse sono vuote.  Un&#39;eccezione di rilievo è rappresentata dai programmi, che possono essere eliminati insieme a tutti i contenuti secondari, purché il programma e il suo contenuto non siano in uso al di fuori dei limiti del programma.

```
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```

## Timeout

Le API delle risorse hanno un timeout di 300 secondi
