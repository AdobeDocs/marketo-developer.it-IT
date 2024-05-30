---
title: "Programmi"
feature: REST API, Programs
description: '"Creare e modificare le informazioni sul programma".'
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 1%

---


# Programmi

[Riferimento endpoint programmi](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs)

I programmi sono un componente organizzativo principale delle attività di marketing di Marketo. Possono essere elementi principali per la maggior parte dei tipi di risorse e consentire il tracciamento dell’appartenenza e del successo dei lead nel contesto di singole iniziative di marketing. I programmi possono essere padri di tutti i tipi di record ad eccezione di LP, Modelli e-mail e File.

## Tipi di programmi

In Marketo sono disponibili cinque tipi principali di programmi:

- Predefinito
- Evento
- Evento con webinar
- Coinvolgimento
- E-mail

I programmi di coinvolgimento possono essere padri per ogni altro tipo di programma, mentre Predefinito, Evento ed Evento con webinar possono essere padri solo per i programmi e-mail.

I programmi dispongono sempre di un canale, derivano la possibile configurazione degli Stati membri del programma dal canale con cui sono stati creati, che può essere recuperato con l’API Get Channels. A un programma può essere associato anche un set di tag. I tag sono campi personalizzabili che possono essere configurati come facoltativi o obbligatori per qualsiasi tipo di programma e per i quali verrà selezionato un valore da un elenco configurato in Marketo Admin.

## Query

I programmi seguono il modello standard per le query su risorse con un’opzione aggiuntiva per eseguire query per tipo di tag e valori. I tag e i valori disponibili possono essere recuperati con [Ottieni tipi di tag](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags/operation/getTagTypesUsingGET).

### Per ID

Il [Ottieni programma per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) l&#39;endpoint richiede un `id` parametro di percorso.

L’ID del programma può essere ottenuto dall’URL del programma nell’interfaccia utente, dove l’URL sarà simile a `https://app-\*\*\*.marketo.com/#PG1001A1`. In questo URL, il `id` è 1001. Sarà sempre compreso tra il primo set di lettere nell’URL e il secondo set di lettere.

```
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

Il [Ottieni programma per nome](https://developer.adobe.com/marketo-apis/api/asset/) l&#39;endpoint richiede un `name` parametro di query. I parametri di query booleani opzionali sono `includeTags` e `includeCosts` che vengono utilizzati rispettivamente per restituire i tag del programma e i costi del programma.

```
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

### Sfogliare

Il [Ottieni programmi](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) consente di cercare i programmi.

L&#39;opzione `status` Il parametro ti consente di filtrare in base allo stato del programma. Questo parametro si applica solo ai programmi di coinvolgimento e e-mail. I valori possibili sono &quot;on&quot; e &quot;off&quot; per i programmi di coinvolgimento e &quot;unlocked&quot; per i programmi e-mail.

L&#39;opzione `maxReturn` Il parametro controlla il numero di programmi da restituire (il massimo è 200, il valore predefinito è 20). L&#39;opzione `offset` parametro utilizzato per i risultati di paging (il valore predefinito è 0).

Tieni presente che i tag associati a un programma non vengono restituiti da questo endpoint. I tag del programma possono essere recuperati utilizzando [Ottieni programmi per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) o [Ottieni programmi per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET).

```
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

Il `earliestUpdatedAt` e `latestUpdatedAt` parametri per il [Ottieni programmi](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) L&#39;endpoint consente di impostare filigrane di data e ora basse e alte per i programmi restituiti che sono stati aggiornati o creati inizialmente all&#39;interno dell&#39;intervallo specificato.

```
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

Il [Ottieni programmi per tag](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramListByTagUsingGET) l’endpoint recupera un elenco di programmi che corrispondono al tipo di tag e ai valori di tag forniti.

Sono necessari due parametri: `tagType` che è il tipo di tag su cui filtrare e `tagValue` che è il valore tag su cui filtrare.  È presente un numero intero facoltativo `maxReturn` parametro che controlla il numero di programmi da restituire (il massimo è 200, il valore predefinito è 20) e un numero intero facoltativo `offset` parametro utilizzato per i risultati di paging (il valore predefinito è 0).  I risultati vengono restituiti in ordine casuale.

```
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

[Creazione]https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/createProgramUsingPOST) e [aggiornamento](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) segue il modello di risorsa standard e dispone di `folder`, `name`, `type` e `channel` come parametri richiesti, con `description`, `costs` e `tags` essere facoltativi. Il canale e il tipo possono essere impostati solo al momento della creazione del programma. Solo descrizione, nome, `tags` e `costs` può essere aggiornato dopo la creazione, con un `costsDestructiveUpdate` parametro consentito. Passaggio `costsDestructiveUpdate` poiché true, tutti i costi esistenti verranno cancellati e sostituiti con eventuali costi inclusi nell&#39;invito. Tieni presente che i tag possono essere necessari per alcuni tipi di programmi in alcune sottoscrizioni, ma questo dipende dalla configurazione e deve prima essere verificato con Ottieni tag per verificare se sono presenti requisiti specifici per l’istanza.

Durante la creazione o l’aggiornamento di un programma e-mail, viene `startDate` e `endDate` può anche essere superato.

### Crea

```
POST /rest/asset/v1/programs.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Per aggiungere nuovi costi durante l&#39;aggiornamento dei costi del programma, è sufficiente aggiungerli al `costs` array. Per eseguire un aggiornamento distruttivo, trasmettere i nuovi costi, insieme al parametro `costsDestructiveUpdate` imposta su `true`. Per cancellare tutti i costi da un programma, non superare un `costs` , e passa `costsDestructiveUpdate` imposta su `true`.

```
POST /rest/asset/v1/program/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

I programmi e-mail possono essere approvati o non approvati in remoto, il che causerà l’esecuzione del programma alla data di inizio specificata e la sua conclusione alla data di fine specificata. Entrambi devono essere impostati per approvare il programma, nonché per disporre di un’e-mail e di un elenco smart validi e approvati configurati tramite l’interfaccia utente.

### Approva

```
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

```
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

[Clonazione di programmi](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/cloneProgramUsingPOST) segue il modello di risorsa standard il nuovo nome e la nuova cartella come parametri obbligatori e una descrizione facoltativa.  Il `name` Il parametro deve essere univoco a livello globale e non può superare i 255 caratteri.  Il `folder` Il parametro è la cartella principale.  Il `folder` L&#39;attributo del tipo di parametro deve essere impostato su &quot;Folder&quot; (Cartella) e la cartella di destinazione deve trovarsi nella stessa area di lavoro del programma clonato.

I programmi che contengono determinati tipi di risorse non possono essere clonati tramite questa API, tra cui notifiche push, messaggi in-app, rapporti e risorse social. I programmi in-app non possono essere clonati tramite questa API.

```
POST /rest/asset/v1/program/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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
