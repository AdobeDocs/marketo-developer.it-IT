---
title: Programmi
feature: REST API, Programs
description: Guida ai programmi Marketo per l’API REST di Asset, che tratta tipi, canali, tag, stati membri ed endpoint da ottenere per ID o nome, sfogliare e filtrare per stato.
exl-id: 30700de2-8f4a-4580-92f2-7036905deb80
TQID: https://experienceleague.adobe.com/5ILyahSn3Pp-lF6YPogVnkXjXP-QLtEmyLm7iKMIgo0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 741
ht-degree: 1%

---

# Programmi

[Riferimento endpoint programmi](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs)

I programmi organizzano le attività di marketing Marketo e tengono traccia dell’iscrizione dei lead e del successo delle singole iniziative di marketing. Un programma può contenere la maggior parte dei tipi di risorse, ad eccezione di pagine di destinazione, modelli e-mail e file.

## Tipi di programmi

In Marketo sono disponibili cinque tipi principali di programmi:

- Predefinito
- Evento
- Evento con webinar
- Coinvolgimento
- E-mail

I programmi di coinvolgimento possono contenere qualsiasi altro tipo di programma. Predefinito, Evento ed Evento con programmi Webinar può contenere solo programmi e-mail.

Ogni programma ha un canale. Il canale definisce gli Stati membri del programma disponibili e può essere recuperato con l’API Get Channels.

Un programma può anche avere dei tag. I tag sono campi personalizzabili che possono essere facoltativi o obbligatori per un tipo di programma. Ogni tag utilizza un valore da un elenco configurato in Marketo Admin.

## Query

Eseguire query sui programmi per ID, nome, esplorazione o tipo e valore di tag. Utilizza [Ottieni tipi di tag](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags/operation/getTagTypesUsingGET) per recuperare tag e valori disponibili.

### Per ID

L&#39;endpoint [Get Program by Id](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) richiede un parametro di percorso `id`.

È possibile ottenere l&#39;ID del programma dall&#39;URL dell&#39;interfaccia utente, ad esempio `https://app-\*\*\*.marketo.com/#PG1001A1`. In questo esempio, l&#39;ID è `1001`, tra il primo e il secondo set di lettere.

```http
GET /rest/asset/v1/program/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#14db037ec71",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Per nome

L&#39;endpoint [Get Program by Name](https://developer.adobe.com/marketo-apis/api/asset) richiede un parametro di query `name`. Impostare i parametri booleani facoltativi `includeTags` e `includeCosts` per restituire rispettivamente tag e costi.

```http
GET /rest/asset/v1/program/byName.json?name=TestProgramName&includeTags=true
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#14db03e070c",
    "result": [
        {
            "id": 1107,
            "name": "AAA2QueryProgramName",
            "description": "AssetAPI: getProgram tests",
            "createdAt": "2015-05-21T22:45:13Z+0000",
            "updatedAt": "2015-05-21T22:45:13Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1107A1",
            "type": "Default",
            "channel": "Online Advertising",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "",
            "workspace": "Default",
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                }
            ],
            "costs": null,
            "headStart": false
        }
    ]
}
```

### Sfoglia

Utilizza l&#39;endpoint [Ottieni programmi](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) per sfogliare i programmi.

Il parametro facoltativo `status` filtra i programmi di coinvolgimento e di posta elettronica in base allo stato. I valori validi sono `on` e `off` per i programmi di coinvolgimento e `unlocked` per i programmi e-mail.

Il parametro facoltativo `maxReturn` controlla il numero di programmi restituiti. Il valore predefinito è 20 e il massimo è 200. Utilizzare il parametro facoltativo `offset` per l&#39;impaginazione; il valore predefinito è 0.

Questo endpoint non restituisce i tag del programma. Recuperare i tag con [Ottieni programmi per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) o [Ottieni programmi per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET).

```http
GET /rest/asset/v1/programs.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#1511bf8a41c",
    "result": [
        {
            "id": 1035,
            "name": "clone it",
            "description": "",
            "createdAt": "2015-11-18T15:25:35Z+0000",
            "updatedAt": "2015-11-18T15:25:46Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1035A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 28,
                "folderName": "Nurturing"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1032,
            "name": "email prog",
            "description": "",
            "createdAt": "2015-11-18T14:56:28Z+0000",
            "updatedAt": "2015-11-18T14:56:28Z+0000",
            "url": "https://app-devlocal1.marketo.com/#EBP1032A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 26,
                "folderName": "Data Management"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Per intervallo di date

Utilizzare i parametri `earliestUpdatedAt` e `latestUpdatedAt` con [Ottieni programmi](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) per impostare limiti di data e ora elevati. L’endpoint restituisce i programmi creati o aggiornati all’interno dell’intervallo.

```http
GET /rest/asset/v1/programs.json?earliestUpdatedAt=2017-01-01T00:00:00-05:00&latestUpdatedAt=2017-01-30T00:00:00-05:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1225a#15f82a83875",
    "warnings": [],
    "result": [
        {
            "id": 1070,
            "name": "Bulk Import - Test",
            "description": "",
            "createdAt": "2017-01-13T19:34:17Z+0000",
            "updatedAt": "2017-01-13T19:34:18Z+0000",
            "url": "https://app-abm.marketo.com/#PG1070A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 637,
                "folderName": "Avention"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1069,
            "name": "Program With Email",
            "description": "",
            "createdAt": "2017-01-03T22:53:14Z+0000",
            "updatedAt": "2017-01-03T22:53:15Z+0000",
            "url": "https://app-abm.marketo.com/#EBP1069A1",
            "type": "Email",
            "channel": "Email Send",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "unlocked",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1071,
            "name": "Program with Guided Landing Page Template",
            "description": "",
            "createdAt": "2017-01-24T22:59:21Z+0000",
            "updatedAt": "2017-01-24T22:59:22Z+0000",
            "url": "https://app-abm.marketo.com/#PG1071A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 621,
                "folderName": "Smartling"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        },
        {
            "id": 1047,
            "name": "ReachForce List Update",
            "description": "",
            "createdAt": "2016-05-24T19:38:35Z+0000",
            "updatedAt": "2017-01-13T19:28:09Z+0000",
            "url": "https://app-abm.marketo.com/#PG1047A1",
            "type": "Default",
            "channel": "Content",
            "folder": {
                "type": "Folder",
                "value": 407,
                "folderName": "Everly Tests"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
        }
    ]
}
```

### Per tipo di tag

L&#39;endpoint [Get Programs by Tag](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramListByTagUsingGET) restituisce programmi che corrispondono al tipo e al valore di tag specificati.

I parametri `tagType` e `tagValue` sono obbligatori. Il numero intero facoltativo `maxReturn` controlla il numero di programmi restituiti. Il valore predefinito è 20 e il valore massimo è 200. Utilizzare il numero intero facoltativo `offset` per l&#39;impaginazione; il valore predefinito è 0. I risultati vengono restituiti in ordine casuale.

```http
GET /rest/asset/v1/program/byTag.json?tagType=Presenter&tagValue=Dennis
```

```json
{
    "success" : true,
    "warnings" : [],
    "errors" : [],
    "requestId" : "13b6d#152b38d5be4",
    "result" : [{
            "id" : 1004,
            "name" : "It's a Program",
            "description" : "",
            "createdAt" : "2013-02-26T00:37:37Z+0000",
            "updatedAt" : "2013-03-11T15:32:02Z+0000",
            "url" : "https://app-sjst.marketo.com/#PG1004A1",
            "type" : "Default",
            "channel" : "Email Blast",
            "folder" : {
                "type" : "Folder",
                "value" : 38,
                "folderName" : "Test"
            },
            "status" : "",
            "workspace" : "Default",
            "tags" : [{
                    "tagType" : "Presenter",
                    "tagValue" : "Dennis"
                }
            ],
                        "headStart": false
    ]
}
```

## Crea e aggiorna

[Per creare](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/createProgramUsingPOST) un programma sono necessari `folder`, `name`, `type` e `channel`. I parametri facoltativi sono `description`, `costs` e `tags`. Alcuni abbonamenti richiedono tag per tipi di programmi specifici. Utilizza Ottieni tag per verificare i requisiti dell’istanza.

Quando [aggiorna](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST), è possibile modificare solo la descrizione, il nome, `tags` e `costs`. Potete impostare il canale e digitare solo durante la creazione. L&#39;impostazione di `costsDestructiveUpdate` su `true` cancella tutti i costi esistenti e li sostituisce con i costi inclusi nella richiesta.

Durante la creazione o l&#39;aggiornamento di un programma e-mail, è possibile passare `startDate` e `endDate` anche come data/ora UTC:

`"startDate": "2022-10-19T15:00:00.000Z"`
`"endDate": "2022-10-19T15:00:00.000Z"`

### Creare

```http
POST /rest/asset/v1/programs.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=API Test Program&folder={"id":1035,"type":"Folder"}&description=Sample API Program&type=Default&channel=Email Blast&costs=[{"startDate":"2015-01-01","cost":2000}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "d505#14d9bd96352",
    "result": [
        {
            "id": 1207,
            "name": "newProgram",
            "description": "This is a test",
            "createdAt": "2015-05-28T18:47:15Z+0000",
            "updatedAt": "2015-05-28T18:47:15Z+0000",
            "url": "https://app-devlocal1.marketo.com/#ME1207A1",
            "type": "Event",
            "channel": "channelOne",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": [
                {
                    "startDate":"2015-01-01",
                    "cost":2000
                }
            ]
        }
    ]
}
```

### Aggiornamento

Per aggiungere i costi del programma, aggiungerli all&#39;array `costs`. Per sostituire i costi esistenti, passare i nuovi costi e impostare `costsDestructiveUpdate` su `true`. Per cancellare tutti i costi, omettere `costs` e impostare `costsDestructiveUpdate` su `true`.

```http
POST /rest/asset/v1/program/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is an updated description&name=Updated Program Name&costs=[{"startDate":"2016-01-01","cost":200,"note":"Google Adwords"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#14db05608aa",
    "result": [
        {
            "id": 1110,
            "name": "Updated Program Name",
            "description": "This is a updated description",
            "createdAt": "2015-05-21T22:45:14Z+0000",
            "updatedAt": "2015-06-01T18:13:58Z+0000",
            "url": "https://app-devlocal1.marketo.com/#NP1110A1",
            "type": "Engagement",
            "channel": "Nurture",
            "folder": {
                "type": "Folder",
                "value": 1910,
                "folderName": "ProgramQueryTestFolder"
            },
            "status": "on",
            "workspace": "Default",
            "headStart": false,
            "tags": [
                {
                    "tagType": "AAA1 Required Tag Type",
                    "tagValue": "AAA1 RT1"
                },
                {
                    "tagType": "tagTypeOne",
                    "tagValue": "tagTypeValue1"
                }
            ],
            "costs": [
                {
                    "startDate": "2016-01-01",
                    "cost": 200,
                    "note": "Google Adwords"
                }
            ]
        }
    ]
}
```

## Approvazione

Puoi approvare o annullare l’approvazione dei programmi e-mail in remoto. Un programma approvato viene eseguito alle `startDate` e termina alle `endDate`.

Prima dell’approvazione, imposta entrambe le date e configura un’e-mail e un elenco smart validi e approvati nell’interfaccia utente.

### Approvazione

```http
POST /rest/asset/v1/program/{id}/approve.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

### Annulla approvazione

```http
POST /rest/asset/v1/program/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16026#150b5bf7692",
    "result": [
        {
            "id": 11062
        }
    ]
}
```

## Duplica

[Per clonare i programmi](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/cloneProgramUsingPOST) è necessario un nuovo nome e una nuova cartella padre. La descrizione è facoltativa. `name` deve essere univoco e non può superare i 255 caratteri.

Impostare l&#39;attributo del tipo del parametro `folder` su `Folder`. La cartella di destinazione deve trovarsi nello stesso workspace del programma di origine.

Non puoi utilizzare questa API per clonare programmi in-app o programmi contenenti notifiche push, messaggi in-app, rapporti o risorse social.

```http
POST /rest/asset/v1/program/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Cloned Program - PHP&folder={"id":5562,"type":"Folder"}&description=Description
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "3a7f#14db06990cc",
    "result": [
        {
            "id": 1221,
            "name": "cloneProgram",
            "description": "This is a description for the cloned program",
            "createdAt": "2015-06-01T18:36:57Z+0000",
            "updatedAt": "2015-06-01T18:36:57Z+0000",
            "url": "https://app-devlocal1.marketo.com/#PG1221A1",
            "type": "Default",
            "channel": "Blog",
            "folder": {
                "type": "Folder",
                "value": 59,
                "folderName": "blah blah"
            },
            "status": "",
            "workspace": "Default",
            "headStart": false
            "tags": null,
            "costs": null
        }
    ]
}
```

## Elimina programma

L’eliminazione dei programmi segue il modello standard di eliminazione delle risorse.

```http
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
