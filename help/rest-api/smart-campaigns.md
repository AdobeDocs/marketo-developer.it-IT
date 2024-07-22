---
title: Campagne avanzate
feature: REST API, Smart Campaigns
description: Panoramica di Smart Campaign
exl-id: 540bdf59-b102-4081-a3d7-225494a19fdd
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 1%

---

# Campagne avanzate

[Riferimento endpoint per campagne avanzate (risorsa)](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns)

[Riferimento endpoint campagne (lead)](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns)

Marketo offre un set di API REST per l’esecuzione di operazioni sulle campagne intelligenti. Queste API seguono il pattern di interfaccia standard per le API delle risorse, fornendo opzioni di query, creazione, clonazione ed eliminazione. Inoltre, puoi gestire l’esecuzione intelligente delle campagne pianificando campagne batch o richiedendo campagne trigger.

## Query

La query delle campagne avanzate segue i tipi di query standard per le risorse di [per ID](#by_id), [per nome](#by_name) e [navigazione](#browse).

### Per ID

L&#39;endpoint [Get Smart Campaign by ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByIdUsingGET) accetta una singola campagna avanzata `id` come parametro di percorso e restituisce un singolo record di campagna avanzata.

```
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

Con questo endpoint, ci sarà sempre un singolo record nella prima posizione dell&#39;array `result`.

### Per nome

L&#39;endpoint [Get Smart Campaign by Name](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getSmartCampaignByNameUsingGET) accetta una singola campagna avanzata `name` come parametro e restituisce un singolo record di campagna avanzata.

```
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

Con questo endpoint, ci sarà sempre un singolo record nella prima posizione dell&#39;array `result`.

### Sfogliare

L&#39;endpoint [Get Smart Campaigns](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/getAllSmartCampaignsGET) funziona come altri endpoint di esplorazione API di Asset e consente a diversi parametri di query facoltativi di specificare criteri di filtro.

I parametri `earliestUpdatedAt` e `latestUpdatedAt` accettano `datetimes` nel formato ISO-8601 (senza millisecondi). Se entrambi sono impostati, il parametro earlyUpdatedAt deve precedere il parametro latestUpdatedAt.

Il parametro `folder` specifica la cartella principale in cui sfogliare. Il formato è un blocco JSON contenente gli attributi `id` e `type`.

Il parametro `maxReturn` è un numero intero che specifica il numero massimo di voci da restituire. Il valore predefinito è 20. Il massimo è 200.

Il parametro `offset` è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato in combinazione con `maxReturn`. Il valore predefinito è 0.

Il parametro `isActive` è un valore booleano che specifica di restituire solo campagne Trigger attive.

```
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

Con questo endpoint, ci saranno uno o più record nell&#39;array `result`.

## Crea

L&#39;endpoint [Create Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/createSmartCampaignUsingPOST) viene eseguito con un application/x-www-form-urlencoded POST con due parametri richiesti. Il parametro `name` specifica il nome della smart campaign da creare. Il parametro `folder` specifica la cartella principale in cui viene creata la smart campaign. Il formato è un blocco JSON contenente gli attributi `id` e `type`.

Facoltativamente, puoi descrivere la campagna avanzata utilizzando il parametro `description` (massimo 2.000 caratteri).

```
POST /rest/asset/v1/smartCampaigns.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

L&#39;endpoint [Update Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/) viene eseguito con un POST application/x-www-form-urlencoded. È necessaria una singola campagna avanzata `id` come parametro di percorso. È possibile utilizzare il parametro `name` per aggiornare il nome della smart campaign oppure il parametro `description` per aggiornare la descrizione della smart campaign.

```
POST /rest/asset/v1/smartCampaign/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

L&#39;endpoint [Clone Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) viene eseguito con un application/x-www-form-urlencoded POST con tre parametri richiesti. Sono necessari un parametro `id` che specifica la smart campaign da clonare, un parametro `name` che specifica il nome della nuova smart campaign e un parametro `folder` per specificare la cartella principale in cui viene creata la nuova smart campaign. Il formato è un blocco JSON contenente gli attributi `id` e `type`.

Facoltativamente, puoi descrivere la campagna avanzata utilizzando il parametro `description` (massimo 2.000 caratteri).

```
POST /rest/asset/v1/smartCampaign/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

L&#39;endpoint [Elimina campagna avanzata](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deleteSmartCampaignUsingPOST) accetta una singola campagna avanzata `id` come parametro del percorso.

```
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

Campagne intelligenti batch avviate in un momento specifico e influenzano un set specifico di lead in una sola volta.

## Pianificazione

Utilizza l&#39;endpoint [Pianifica campagna](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/scheduleCampaignUsingPOST) per pianificare l&#39;esecuzione di una campagna batch immediatamente o in una data futura. La campagna `id` è un parametro di percorso obbligatorio. I parametri facoltativi sono `tokens`, `runAt` e `cloneToProgram`, passati nel corpo della richiesta come application/json.

Il parametro dell’array token è un array di I miei token che esegue l’override dei token del programma esistenti. Dopo l’esecuzione della campagna, i token vengono scartati.  Ogni elemento di array di token contiene coppie nome/valore. Il nome del token deve essere formattato come &quot;{{my.name}}&quot;.

Il parametro datetime runAt specifica quando eseguire la campagna. Se non viene specificato, la campagna verrà eseguita 5 minuti dopo la chiamata dell’endpoint. Il valore datetime non può essere successivo a più di due anni.

Le campagne pianificate tramite questa API attendono sempre almeno cinque minuti prima di essere eseguite.

Il parametro stringa `cloneToProgram` contiene il nome di un programma risultante.  Se questa opzione è impostata, la campagna, il programma principale e tutte le relative risorse vengono creati con il nuovo nome risultante. Il programma principale viene clonato e la campagna appena creata verrà pianificata. Il programma risultante viene creato sotto l&#39;elemento padre. I programmi con snippet, notifiche push, messaggi in-app, elenchi statici, rapporti e risorse social non possono essere clonati in questo modo. Se utilizzato, questo endpoint è limitato a 20 chiamate al giorno. L&#39;endpoint [clone program](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) è l&#39;alternativa consigliata.

```
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

Le campagne intelligenti Trigger interessano una persona alla volta in base a un evento attivato.

### Richiesta

Utilizza l&#39;endpoint [Richiedi campagna](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Campaigns/operation/triggerCampaignUsingPOST) per trasmettere un set di lead a una campagna trigger da eseguire nel flusso della campagna. La campagna deve avere un trigger &quot;Campaign is Requested&quot; (È richiesta una campagna) con &quot;Web Service API&quot; come origine.

Questo endpoint richiede una campagna `id` come parametro di percorso e un parametro di array intero `leads` contenente ID lead. È consentito un massimo di 100 lead per chiamata.

Facoltativamente, il parametro dell&#39;array `tokens` può essere utilizzato per sostituire I miei token locali nel programma padre della campagna. `tokens` accetta un massimo di 100 token. Ogni elemento dell&#39;array `tokens` contiene una coppia nome/valore. Il nome del token deve essere formattato come &quot;{{my.name}}&quot;. Se si utilizza [Add a System Token as a Link in un approccio Email](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/add-a-system-token-as-a-link-in-an-email) per aggiungere il token di sistema &quot;viewAsWebpageLink&quot;, non è possibile eseguirne l&#39;override utilizzando `tokens`. Utilizzare invece [Aggiungere un collegamento Visualizza come pagina Web a un&#39;impostazione E-mail](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/add-a-view-as-web-page-link-to-an-email) che consente di ignorare &quot;viewAsWebPageLink&quot; utilizzando `tokens`.

I parametri `leads` e `tokens` vengono passati nel corpo della richiesta come application/json.

```
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

L&#39;endpoint [Activate Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/activateSmartCampaignUsingPOST) è semplice. È necessario un parametro di percorso `id`. Affinché l&#39;attivazione venga eseguita correttamente, la campagna deve soddisfare le condizioni seguenti:

- Deve essere disattivato
- Deve avere almeno un trigger e un passaggio di flusso
- Devono essere presenti trigger privi di errori, filtri e passaggi di flusso

```
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

[Disattiva Smart Campaign](https://developer.adobe.com/marketo-apis/api/asset/#tag/Smart-Campaigns/operation/deactivateSmartCampaignUsingPOST) è semplice. È necessario un parametro di percorso `id`. Affinché la disattivazione abbia esito positivo, è necessario attivare la campagna.

```
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
