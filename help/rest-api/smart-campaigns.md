---
title: Campagne avanzate
feature: REST API, Smart Campaigns
description: Scopri come utilizzare le API REST di Marketo per campagne avanzate, incluse le query per ID o nome, sfogliare i filtri, creare l’eliminazione dei cloni e pianificare o richiedere i trigger
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
TQID: https://experienceleague.adobe.com/iysRjtqd9plkreyIMuNjAF3YVFHtDUIrc-GInB4V8mg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1009
ht-degree: 1%

---

# Campagne avanzate

[Riferimento endpoint per campagne avanzate (risorsa)](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns)

[Riferimento all’endpoint &quot;Campaigns&quot; (lead)](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

Utilizza le API REST di Smart Campaign per eseguire query, creare, clonare ed eliminare campagne avanzate. Puoi anche pianificare campagne batch, richiedere campagne di attivazione e gestire l’attivazione di una campagna.

## Query

Esegui query sulle campagne avanzate [per ID](#by_id), [per nome](#by_name) o per [navigazione](#browse).

### Per ID

L&#39;endpoint [Get Smart Campaign by ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) accetta una singola campagna avanzata `id` come parametro di percorso e restituisce un singolo record di campagna avanzata.

```http
GET /rest/asset/v1/smartCampaign/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "7883#169838a32f0",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        }
    ]
}
```

L&#39;endpoint restituisce un record nella prima posizione dell&#39;array `result`.

### Per nome

L&#39;endpoint [Get Smart Campaign by Name](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) accetta una singola campagna avanzata `name` come parametro e restituisce un singolo record di campagna avanzata.

```http
GET /rest/asset/v1/smartCampaign/byName.json?name=Test Trigger Campaign
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14494#16c886ffa44",
    "warnings": [],
    "result": [
        {
            "id": 1069,
            "name": "Test Trigger Campaign",
            "description": "",
            "createdAt": "2018-02-16T01:34:39Z+0000",
            "updatedAt": "2019-08-13T00:45:21Z+0000",
            "folder": {
                "id": 327,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 2747,
            "flowId": 1088,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1069A1"
        }
    ]
}
```

L&#39;endpoint restituisce un record nella prima posizione dell&#39;array `result`.

### Sfoglia

L&#39;endpoint [Get Smart Campaigns](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) supporta parametri di query facoltativi per il filtro e l&#39;impaginazione.

I parametri `earliestUpdatedAt` e `latestUpdatedAt` accettano `datetimes` nel formato ISO-8601 (senza millisecondi). Se entrambi sono impostati, il parametro earlyUpdatedAt deve precedere il parametro latestUpdatedAt.

Il parametro `folder` specifica la cartella principale da sfogliare. Passarlo come oggetto JSON contenente `id` e `type`.

Il numero intero `maxReturn` specifica il numero massimo di voci. Il valore predefinito è 20 e il massimo è 200.

Il numero intero `offset` specifica dove iniziare a recuperare le voci. Utilizzala con `maxReturn`. Il valore predefinito è 0.

Imposta il parametro booleano `isActive` per restituire solo le campagne di trigger attive.

```http
GET /rest/asset/v1/smartCampaigns.json?earliestUpdatedAt=2016-09-10T23:15:00-00:00&latestUpdatedAt=2016-09-10T23:17:00-00:00
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "626#16983a92965",
    "warnings": [],
    "result": [
        {
            "id": 1001,
            "name": "Process Bounced Emails",
            "description": "System smart campaign for processing bounced email events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1001,
            "flowId": 1001,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1001A1"
        },
        {
            "id": 1002,
            "name": "Process Unsubscribes",
            "description": "System smart campaign for processing unsubscribe events",
            "createdAt": "2016-09-10T23:16:19Z+0000",
            "updatedAt": "2016-09-10T23:16:19Z+0000",
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 1002,
            "flowId": 1002,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1002A1"
        }
    ]
}
```

L&#39;endpoint restituisce uno o più record nell&#39;array `result`.

## Crea

Invia una richiesta POST `application/x-www-form-urlencoded` all&#39;endpoint [Crea campagna avanzata](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST). I parametri `name` e `folder` sono obbligatori. Passa `folder` come oggetto JSON contenente `id` e `type`.

Facoltativamente, puoi descrivere la campagna avanzata utilizzando il parametro `description` (massimo 2.000 caratteri).

```http
POST /rest/asset/v1/smartCampaigns.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Smart Campaign 02&folder={"type": "folder","id": 640}&description=This is a smart campaign creation test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "25bc#16c9138f148",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02",
            "description": "This is a smart campaign creation test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T17:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Aggiornamento

Invia una richiesta POST `application/x-www-form-urlencoded` all&#39;endpoint [Aggiorna Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset). Il parametro di percorso `id` della campagna avanzata è obbligatorio. Utilizzare `name` per modificare il nome o `description` per modificare la descrizione.

```http
POST /rest/asset/v1/smartCampaign/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
name=Smart Campaign 02 Update&description=This is a smart campaign update test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "14b6a#16c924b992f",
    "warnings": [],
    "result": [
        {
            "id": 1076,
            "name": "Smart Campaign 02 Update",
            "description": "This is a smart campaign update test.",
            "createdAt": "2019-08-14T17:42:04Z+0000",
            "updatedAt": "2019-08-14T22:42:04Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Never Run",
            "type": "batch",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": true,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5132,
            "flowId": 1095,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1076A1"
        }
    ]
}
```

## Duplica

Invia una richiesta POST `application/x-www-form-urlencoded` all&#39;endpoint [Clone Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5). I parametri `id`, `name` e `folder` sono obbligatori. Specificano la campagna di origine, il nome della nuova campagna e la cartella principale. Passa `folder` come oggetto JSON contenente `id` e `type`.

Facoltativamente, puoi descrivere la campagna avanzata utilizzando il parametro `description` (massimo 2.000 caratteri).

```http
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Test Trigger Campaign Clone&folder={"type": "folder","id": 640}&description=This is a smart campaign clone test.
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "681d#16c9339499b",
    "warnings": [],
    "result": [
        {
            "id": 1077,
            "name": "Test Trigger Campaign Clone",
            "description": "This is a smart campaign clone test.",
            "createdAt": "2019-08-15T03:01:41Z+0000",
            "updatedAt": "2019-08-15T03:01:41Z+0000",
            "folder": {
                "id": 640,
                "type": "Folder"
            },
            "status": "Inactive",
            "type": "trigger",
            "isSystem": false,
            "isActive": false,
            "isRequestable": false,
            "isCommunicationLimitEnabled": false,
            "recurrence": {
                "weekdayOnly": false
            },
            "qualificationRuleType": "once",
            "workspace": "Default",
            "smartListId": 5135,
            "flowId": 1096,
            "computedUrl": "https://app-sjqe.marketo.com/#SC1077A1"
        }
    ]
}
```

## Elimina

L&#39;endpoint [Elimina campagna avanzata](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) accetta una singola campagna avanzata `id` come parametro del percorso.

```http
POST /rest/asset/v1/smartCampaign/{id}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d757#16c934216ac",
    "warnings": [],
    "result": [
        {
            "id": 1077
        }
    ]
}
```

## Batch

Le campagne intelligenti batch vengono eseguite in un determinato momento ed elaborano insieme un set definito di lead.

## Pianificazione

Utilizza [Pianifica campagna](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/scheduleCampaignUsingPOST) per pianificare una campagna batch. Il parametro di percorso `id` della campagna è obbligatorio. Passa i parametri facoltativi `tokens`, `runAt` e `cloneToProgram` nel corpo della richiesta JSON.

L&#39;array `tokens` sostituisce il programma esistente I miei token per questa esecuzione. Marketo elimina le sostituzioni dopo l’esecuzione della campagna. Ogni elemento contiene una coppia nome/valore e il nome del token deve utilizzare il formato `{{my.name}}`.

Il parametro data-ora `runAt` specifica quando eseguire la campagna. Se omessa, la campagna viene eseguita cinque minuti dopo la richiesta. Il valore non può superare i due anni nel futuro.

Le campagne pianificate tramite questa API attendono sempre almeno cinque minuti prima di essere eseguite.

Il parametro stringa `cloneToProgram` contiene il nome di un programma risultante.  Se questa opzione è impostata, la campagna, il programma principale e tutte le relative risorse vengono creati con il nuovo nome risultante. Il programma principale viene clonato e la campagna appena creata verrà pianificata. Il programma risultante viene creato sotto l&#39;elemento padre. I programmi con snippet, notifiche push, messaggi in-app, elenchi statici, rapporti e risorse social non possono essere clonati in questo modo. Se utilizzato, questo endpoint è limitato a 20 chiamate al giorno. L&#39;endpoint [clone program](https://developer.adobe.com/marketo-apis/api/asset#tag/Sales-Persons/operation/describeUsingGET_5) è l&#39;alternativa consigliata.

```http
POST /rest/v1/campaigns/{id}/schedule.json
```

```json
{
   "input":
      {
         "runAt": "2018-03-28T18:05:00+0000",
         "tokens": [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
          ]
      }
}
```

```json
{
    "requestId": "52b#161d90e1743",
    "result": [
        {
            "id": 3713
        }
    ],
    "success": true
}
```

## Trigger

Attiva le campagne intelligenti per elaborare una persona alla volta in risposta a un evento.

### Richiesta

Utilizza [Richiedi campagna](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns/operation/triggerCampaignUsingPOST) per passare i lead attraverso il flusso di una campagna trigger. La campagna deve utilizzare un trigger Campaign is Requested con API di servizio web come origine.

Sono necessari il parametro di percorso `id` della campagna e un array intero `leads` di ID lead. Ogni chiamata accetta un massimo di 100 lead.

Facoltativamente, il parametro dell&#39;array `tokens` può essere utilizzato per sostituire I miei token locali nel programma padre della campagna. `tokens` accetta un massimo di 100 token. Ogni elemento dell&#39;array `tokens` contiene una coppia nome/valore. Il nome del token deve essere formattato come &quot;`{{my.name}}`&quot;. Se si utilizza [Add a System Token as a Link in un approccio Email](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) per aggiungere il token di sistema &quot;viewAsWebpageLink&quot;, non è possibile eseguirne l&#39;override utilizzando `tokens`. Utilizzare invece [Aggiungere un collegamento Visualizza come pagina Web a un&#39;impostazione E-mail](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) che consente di ignorare &quot;viewAsWebPageLink&quot; utilizzando `tokens`.

Passa i parametri `leads` e `tokens` nel corpo della richiesta JSON.

```http
POST /rest/v1/campaigns/{id}/trigger.json
```

```json
{
   "input":
      {
         "leads" : [
            {
               "id" : 318592
            },
            {
               "id" : 318593
            }
         ],
         "tokens" : [
            {
               "name": "{{my.message}}",
               "value": "Updated message"
            },
            {
               "name": "{{my.other token}}",
               "value": "Value for other token"
            }
         ]
      }
}
```

```json
{
    "requestId": "9e01#161d922f1aa",
    "result": [
        {
            "id": 3712
        }
    ],
    "success": true
}
```

### Attiva

L&#39;endpoint [Activate Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) è semplice. È necessario un parametro di percorso `id`. Affinché l&#39;attivazione venga eseguita correttamente, la campagna deve soddisfare le condizioni seguenti:

- La campagna è disattivata.
- La campagna ha almeno un trigger e un passaggio di flusso.
- La campagna dispone di trigger privi di errori, filtri e passaggi di flusso.

```http
POST /rest/asset/v1/smartCampaign/{id}/activate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "a33a#161d9c0dcf3",
    "result": [
        {
            "id": 1069
        }
    ]
}
```

### Disattiva

[Disattiva Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) è semplice. È necessario un parametro di percorso `id`. Affinché la disattivazione abbia esito positivo, è necessario attivare la campagna.

```http
POST /rest/asset/v1/smartCampaign/{id}/deactivate.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6228#161d9c29fbf",
    "result": [
        {
            "id": 1069
        }
    ]
}
```
