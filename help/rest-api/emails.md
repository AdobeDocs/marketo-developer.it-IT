---
title: E-mail
feature: REST API
description: Scopri come utilizzare l’API REST di Marketo Asset per eseguire query e gestire le risorse e-mail in base all’ID, al nome o alla navigazione delle cartelle, con note sui contenuti predittivi e sui limiti dei test A/B.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
TQID: https://experienceleague.adobe.com/t2FyPbwS836MvOe5rL0rVS7ibtzzZMmXwmgHBDZEr8Q
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1813
ht-degree: 1%

---

# E-mail

[Riferimento endpoint e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails)

Utilizza gli endpoint REST delle e-mail per eseguire query e gestire le risorse e-mail.

Se un&#39;e-mail contiene [Marketo Predictive Content](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), i seguenti endpoint non riescono con codice di errore 709 e un messaggio di errore corrispondente:

- [Ottieni contenuto e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET)
- [Sezione Aggiorna contenuto e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST)
- [Approva bozza e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/approveDraftUsingPOST)

## Query

Le e-mail supportano gli stessi pattern di query dei modelli: [per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByNameUsingGET) e per [navigazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailUsingGET). Gli endpoint &quot;by-name&quot; e &quot;browse&quot; supportano anche il filtro delle cartelle.

Se un&#39;e-mail appartiene a un programma e-mail che utilizza [Test A/B](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), i seguenti endpoint non restituiscono l&#39;e-mail:

- [Ricevi e-mail per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByIdUsingGET)
- [Ricevi e-mail per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailByNameUsingGET)
- [Ricevi e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailUsingGET)

La chiamata indica il completamento ma include l&#39;avviso `No assets found for the given search criteria.`

### Per ID

```http
GET /rest/asset/v1/email/1351.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14a1832af8c",
   "result":[
      {
         "id":1356,
         "name":"sakZxhxkwV",
         "description":"sample description",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "subject":{
            "type":"Text",
            "value":"sample subject"
         },
         "fromName":{
            "type":"Text",
            "value":"RBxEtmdQZz"
         },
         "fromEmail":null,
         "replyEmail":{
            "type":"Text",
            "value":"Qlikf@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":10421
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":false,
         "template":338,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
         "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Per nome

Quando si esegue una query per nome, è possibile passare una cartella per limitare la ricerca a tale cartella.

```http
GET /rest/asset/v1/email/byName.json?name=My Email&folder={"id":1056,"type"="Folder"}
```

```json
{
   "success":true,
   "warnings":[
   ],
   "errors":[
   ],
   "requestId":"3a7f#14c484de875",
   "result":[
      {
         "id":1032,
         "name":"My Email",
         "description":"eCjxjIHmYPLtecoSphkvIXlrygOBDLhgyQKnsKMpiKWgSCKhkPMUFvFPUvEylmFiLjQGnffXGaiNLxAwiFOmIDvxEINoaSYascJw",
         "createdAt":"2015-03-23T20:23:25Z+0000",
         "updatedAt":"2015-03-23T20:23:25Z+0000",
         "subject":{
            "type":"Text",
            "value":"ezyKBmDcyCcUIrXASrLSvRuWQgWpRZxQstJoStgMSLEBASGKMpAnVeWrgJsaVFoFJUEXhEIPpDAWpzajzingUruFpiMcRRwtoBzU"
         },
         "fromName":{
            "type":"Text",
            "value":"dAiqRNJOdY"
         },
         "fromEmail":{
            "type":"Text",
            "value":"ilZxG@testmail.com"
         },
         "replyEmail":{
            "type":"Text",
            "value":"VYsCS@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":1056
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":"draft",
         "template":32,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
        "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Sfoglia

La navigazione e-mail segue il pattern API standard di Asset e supporta i seguenti filtri opzionali:

- `status`: `Approved` o `Draft`.
- `folder`: oggetto JSON contenente `id` e `type`.
- `earliestUpdatedAt` e `latestUpdatedAt`: l&#39;intervallo di tempo dell&#39;aggiornamento.
- `maxReturn`: numero di risultati da restituire. Il valore predefinito è 20 e il massimo è 200.
- `offset`: funziona con `maxReturn` per passare da un set di risultati di grandi dimensioni all&#39;altro. Il valore predefinito è 0.

```http
GET /rest/asset/v1/emails.json?maxReturn=3&folder={"id":341,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17576#14e22eb29cb",
    "result": [
        {
            "id": 2137,
            "name": "Social Sharing in Email",
            "description": "",
            "createdAt": "2011-03-04T17:12:42Z+0000",
            "updatedAt": "2011-03-04T19:04:36Z+0000",
            "url": null,
            "subject": {
                "type": "Text",
                "value": "Republish this content to your favorite social site!"
            },
            "fromName": {
                "type": "Text",
                "value": "Demo Master Marketo"
            },
            "fromEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 341,
                "folderName": "Social Media"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": true,
            "status": "approved",
            "template": null,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
               {
                 "attributeId": "157",
                 "objectName": "lead",
                 "displayName": "Lead Owner Email Address",
                 "apiName": null
               }
             ],
            "preHeader": "My awesome preheader!"
        }
    ]
}
```

## Contenuto della query

Per [recuperare le sezioni modificabili di un&#39;e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET), eseguire una query sul contenuto. Facoltativamente, filtra per stato per restituire sezioni dalla versione Approvata o Bozza.

```http
GET /rest/asset/v1/email/1356/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"8a44#14c484de8c8",
   "result":[
      {
         "htmlId":"edit_text_3",
         "value":[
            {
               "type":"HTML",
               "value":"Content from testCreateEmailTemplate2"
            },
            {
               "type":"Text",
               "value":"Content from testCreateEmailTemplate2"
            }
         ],
         "contentType":"Text"
      }
   ]
}
```

Una sezione può avere un tipo `dynamicContent`. Per ulteriori informazioni, vedere [Contenuto dinamico](dynamic-content.md).

## Interroga campi CC

Chiamare l&#39;endpoint [Get Email CC Fields](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailCCFieldsUsingGET) per recuperare i campi abilitati per e-mail CC nell&#39;istanza di destinazione.

```http
GET /rest/asset/v1/email/ccFields.json
```

```json
{
   "success":true,
   "errors":[ ],
   "requestId":"e54b#16796fdbd4e",
   "warnings":[ ],
   "result":[
      {
         "attributeId":"157",
         "objectName":"lead",
         "displayName":"Lead Owner Email Address",
         "apiName":"leadOwnerEmailAddress"
      },
      {
         "attributeId":"396",
         "objectName":"company",
         "displayName":"Account Owner Email Address",
         "apiName":"accountOwnderEmailAddress"
      }
   ]
}
```

## Crea e aggiorna

[Crea un&#39;e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/createEmailUsingPOST) da un modello di origine. Le sezioni modificabili dell&#39;e-mail provengono dagli elementi HTML del modello che hanno la classe `mktEditable` e una proprietà `id` univoca.

La chiamata Crea e-mail richiede i seguenti parametri:

- `name`: nome dell&#39;e-mail.
- `template`: modello di origine.
- `folder`: la cartella principale.

I parametri di creazione facoltativi sono `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational` e `isOpenTrackingDisabled`. Quando questi parametri vengono omessi, vengono applicate le seguenti impostazioni predefinite:

- `subject` è vuoto.
- `fromName`, `fromEmail` e `replyEmail` utilizzano i valori predefiniti dell&#39;istanza.
- `operational` e `isOpenTrackingDisabled` sono `false`.

Il parametro `isOpenTrackingDisabled` determina se l&#39;e-mail inviata include il pixel di tracciamento aperto.

```http
POST /rest/asset/v1/emails.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=My New Email 02 - deverly&folder={"id":1017,"type":"Program"}&template=24&description=This is a test email&subject=Hey There&fromName=SomeBody&fromEmail=somebody@marketo.com&replyEmail=somebody@marketo.com
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "My New Email 02 - deverly",
            "description": "This is a test email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

Per [aggiornare un&#39;e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailContentUsingPOST), passare il relativo ID e aggiornare la descrizione o il nome dell&#39;e-mail.

```http
POST /rest/asset/v1/email/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
description=This is an Email&name=Updated Email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "Updated Email",
            "description": "This is an Email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,
            "preHeader": null
        }
    ]
}
```

### Sezione contenuto, tipo e aggiornamento

Aggiorna ogni sezione del contenuto e-mail singolarmente. Utilizzare l&#39;endpoint [Aggiorna contenuto e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailContentUsingPOST) per aggiornare `subject`, `fromName`, `fromEmail` e `replyEmail`. Questo endpoint consente inoltre di impostare questi valori in modo che utilizzino contenuto dinamico anziché contenuto statico.

Ogni parametro è un oggetto JSON di tipo/valore. Il tipo è `Text` o `DynamicContent`. Il valore è il testo corrispondente o l’ID della segmentazione utilizzata per il contenuto dinamico. Invia i dati come POST con `application/x-www-form-urlencoded`, non come JSON. È inoltre possibile impostare `isOpenTrackingDisabled` con Aggiorna contenuto e-mail.

```http
POST /rest/asset/v1/email/{id}/content.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
subject={"type":"Text","value":"Gettysburg Address"}&fromEmail={"type":"Text","value":"abe@testmail.com"}&fromName={"type":"Text","value":"Abe Lincoln"}&replyTO={"type":"Text","value":"replies@testmail.com"}
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"c865#14a1832afac",
   "result":[
      {
         "id":1356
      }
   ]
}
```

Per configurare una sezione per l’utilizzo di contenuti dinamici, recuperane prima l’ID con l’opzione Ottieni contenuto e-mail.

### Aggiorna sezione modificabile

Aggiornare una sezione modificabile in base ai relativi `htmlId`. L&#39;e-mail `id` e la sezione `htmlId` sono parametri di percorso obbligatori. I parametri `type`, `value` e `textValue` sono facoltativi.

`type` determina il contenuto di `value`:

- `Text`: `value` è una stringa contenente il contenuto HTML della sezione.
- `DynamicContent`: `value` è un oggetto JSON contenente `type` impostato su `DynamicContent`, `segmentation` impostato sull&#39;ID segmentazione e `default` impostato sul contenuto HTML predefinito.
- `Snippet`: tipo di sezione supportato.

Il parametro facoltativo `textValue` contiene la versione testuale della sezione. Invia i dati come POST con `application/x-www-form-urlencoded`, non come JSON.

```http
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
type=Text&value=<h1>Hello World!</h1>&textValue=Hello World!
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "155ac#14d58dfa9ad",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

Se l&#39;opzione Copia automatica nel testo è disattivata per uno snippet incorporato, l&#39;aggiornamento del frammento di codice HTML e quindi della versione del testo di un&#39;altra sezione fa sì che la versione del testo e-mail rifletta il frammento di codice HTML aggiornato. Quando la copia automatica è disattivata, il testo dello snippet precedente non viene mantenuto come previsto.

## Moduli

In Email Editor 1.0, un modulo è una sezione e-mail definita nel modello. I moduli possono contenere elementi, variabili e altro contenuto HTML come descritto in [Sintassi del modello e-mail](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules).

Utilizza le API dei moduli per gestire i moduli all’interno di un messaggio e-mail. Per gli endpoint del modulo che utilizzano HTTP POST, formattare il corpo della richiesta come `application/x-www-form-urlencoded`, non come JSON.

La maggior parte degli endpoint del modulo richiede `moduleId` come parametro di percorso. L&#39;endpoint [Get Email Content](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET) restituisce gli ID del modulo nell&#39;attributo `htmlId`. Vedi [Query](#modules_query).

### Query

Per utilizzare i moduli, specificare `moduleId` che identifica il modulo in modo univoco. Potresti anche aver bisogno dell’indice del modulo numero intero, che descrive l’ordine del modulo nell’e-mail.

Per [recuperare gli ID modulo e i relativi indici](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailContentByIdUsingGET), specificare l&#39;ID e-mail come parametro del percorso.

L&#39;esempio seguente esegue una query su un messaggio e-mail 1.0 basato sul modello `Skeleton` nella sezione Modelli iniziali dell&#39;interfaccia utente di Selezione modelli.

```http
GET /rest/asset/v1/email/{moduleId}/content.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "3d79#158da6492bd",
  "result": [
    {
      "htmlId": "free-image",
      "contentType": "Module",
      "index": 1,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "single",
      "value": {
        "width": "600",
        "altText": "",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 600px"
      },
      "contentType": "Image",
      "parentHtmlId": "free-image",
      "isLocked": false
    },
    {
      "htmlId": "video",
      "contentType": "Module",
      "index": 2,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "video2",
      "value": {

      },
      "contentType": "Video",
      "parentHtmlId": "video",
      "isLocked": false
    },
    {
      "htmlId": "free-text",
      "contentType": "Module",
      "index": 3,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "text",
      "value": [
        {
          "type": "HTML",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        },
        {
          "type": "Text",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "free-text",
      "isLocked": false
    },
    {
      "htmlId": "two-articles",
      "contentType": "Module",
      "index": 6,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "article3",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text2",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "article4",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle2",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text3",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "footer",
      "contentType": "Module",
      "index": 7,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "footerText",
      "value": [
        {
          "type": "HTML",
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you have subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you have subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "footer",
      "isLocked": false
    },
    {
      "htmlId": "spacer",
      "contentType": "Module",
      "index": 0,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "CTA",
      "contentType": "Module",
      "index": 4,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "hr",
      "contentType": "Module",
      "index": 5,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    }
  ]
}
```

L’array dei risultati contiene sia la descrizione del modulo che quella dell’elemento HTML. Gli elementi del modulo hanno `contentType` di `Module` e includono `index`. L&#39;attributo `htmlId` contiene `moduleId`.

Per l&#39;esempio `Skeleton`, la tabella seguente mappa ogni `moduleId` al relativo indice nell&#39;e-mail.

| moduleId (anche noto come htmlId) | Indice |
| --- | --- |
| distanziatore | 0 |
| immagine libera | 1 |
| video | 2 |
| testo libero | 3 |
| CTA | 4 |
| h | 5 |
| due articoli | 6 |
| footer | 7 |

#### Add

Per [aggiungere un modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/addModuleUsingPOST), seleziona un modulo esistente dal modello dell&#39;e-mail. Specifica l&#39;ID e-mail e `moduleId` come parametri del percorso. Il parametro di query `index` richiesto determina la posizione del modulo. Se `index` supera l&#39;indice esistente più grande, l&#39;API aggiunge il modulo all&#39;e-mail.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
index=10
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "1063e#158d6ad2c3f",
    "result": [
        {
            "id": 1028
        }
    ]
}
```

#### Elimina

Per [eliminare un modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/deleteModuleUsingPOST), specificare l&#39;ID e-mail e `moduleId` come parametri del percorso.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "2356#158d6f6104a",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Duplica

Per [duplicare un modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/duplicateModuleUsingPOST), specificare l&#39;ID e-mail e `moduleId` come parametri del percorso. L’API inserisce il duplicato sotto il modulo originale e sposta i moduli rimanenti verso il basso.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "e740#158d705d967",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Ridisponi

Per [ridisporre i moduli](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/rearrangeModulesUsingPOST), inviare un array contenente ogni modulo e la relativa posizione desiderata. Ogni elemento array è un oggetto JSON nel formato `{ "index": <_index_>, "moduleId": "<_moduleId_>" }`, dove `<_index_>` è la posizione del modulo basata su zero e `<_moduleId_>` è l&#39;ID del modulo.

```http
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "e67a#158d72d1cde",
    "result":[
        {
            "id": 1030
        }
    ]
}
```

#### Rinomina

Per [rinominare un modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/renameUsingPOST), passare il nuovo nome nel parametro `name`. Specifica l&#39;ID e-mail e `moduleId` esistenti come parametri del percorso.

```http
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=MarketoVideo
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors": [ ],
    "requestId":"11521#158d740abc0",
    "result": [
        {
            "id": 1030
        }
    ]
}
```

## Variabili

Nell’editor e-mail 1.0, le variabili memorizzano i valori per gli elementi e-mail. Definire ogni variabile aggiungendo la sintassi specifica di Marketo al HTML, come descritto in [Sintassi del modello di e-mail](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Utilizza le API delle variabili per gestire le variabili all’interno di un messaggio e-mail.

### Query

Per [recuperare le variabili](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailVariablesUsingGET), specificare l&#39;ID e-mail come parametro di percorso.

L&#39;esempio seguente esegue una query su un messaggio e-mail 1.0 basato sul modello `Skeleton` nella sezione Modelli iniziali dell&#39;interfaccia utente di Selezione modelli.

```http
GET /rest/asset/v1/email/{id}/variables.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [  ],
  "requestId": "756#158dade55e8",
  "result": [
    {
      "name": "twoArticlesSpacer5",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer6",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer7",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText2",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer8",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer2",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "hrBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "freeTextBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink2",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "hrBorderColor",
      "value": "#e6e6e6",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "freeImageBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "footerSpacer",
      "value": "10",
      "moduleScope": false
    },
    {
      "name": "ctaLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "hrBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize2",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer4",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer3",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer",
      "value": "20",
      "moduleScope": false
    }
  ]
}
```

Ogni elemento nella matrice dei risultati descrive una variabile.

Le variabili possono avere ambito globale per l’intero messaggio e-mail o ambito locale per un modulo. Ogni variabile contiene gli attributi `name`, `value` e `moduleScope`. L&#39;attributo booleano `moduleScope` è `false` per le variabili globali e `true` per le variabili locali. Una variabile locale include anche `moduleId` del relativo modulo associato.

#### Aggiornamento

Per [aggiornare una variabile](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateVariableUsingPOST), passare il nuovo valore nel parametro `value`. Specifica l’ID e-mail e il nome della variabile come parametri del percorso. Quando si aggiorna una variabile di modulo, passare anche `moduleId` per identificare il modulo associato.

Nell&#39;esempio seguente viene aggiornata la variabile globale `hrBorderSize`.

```http
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```text
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```text
value=2
```

```json
{
    "success":true,
    "warnings":[ ],
    "errors":[ ],
    "requestId":"feb5#158db4be57e",
    "result": [
        {
            "name":"hrBorderSize",
            "value":"2",
            "moduleScope":false
        }
    ]
}
```

Nell&#39;esempio seguente la variabile locale `ctaLinkText` viene aggiornata a `Click this button!` nel modulo `CTA`.

```http
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
value=Click this button!&moduleId=CTA
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "7f34#158dc28d2f7",
    "result": [
        {
            "name":"ctaLinkText",
            "value":"Click this button!",
            "moduleScope":true,
            "moduleId":"CTA"
        }
    ]
}
```

## Approvazione

Le e-mail seguono il ciclo di vita standard di approvazione delle risorse. Gli endpoint separati consentono di approvare una bozza, annullare l&#39;approvazione di una versione approvata o eliminare una bozza esistente.

### Approvazione

L’endpoint di approvazione convalida l’e-mail in base alle regole e-mail di Marketo. Compilare `from name`, `from email`, `reply to email` e `subject` prima dell&#39;approvazione.

```http
POST /rest/asset/v1/email/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"15dbf#14a1832ae86",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Annulla approvazione

Utilizzare l&#39;operazione `unapprove` solo su un messaggio e-mail approvato.

```http
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

#### Elimina

Puoi eliminare solo un’e-mail nello stato di bozza. Non puoi eliminare un’e-mail approvata.

```http
POST /rest/asset/v1/email/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"182c0#14a1832af4f",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Elimina

```http
POST /rest/asset/v1/email/{id}/delete.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"169cd#14a1832adba",
   "result":[
      {
         "id":1361
      }
   ]
}
```

## Duplica

Per clonare un&#39;e-mail, inviare una richiesta POST `application/x-www-form-urlencoded` con i seguenti parametri:

- `name`: obbligatorio. Il nome e-mail clonato.
- `folder`: obbligatorio. Oggetto JSON incorporato con `id` e `type`.
- `description`: facoltativo. La descrizione dell’e-mail clonata.

Se l’e-mail di origine non dispone di una versione approvata, l’endpoint clona la versione bozza.

```http
POST /rest/asset/v1/email/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=Clone of Social Sharing in Email&folder={"id":239,"type":"Folder"}&description=This is a test of clone email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd49#15706f43d96",
    "result": [
        {
            "id": 2250,
            "name": "Clone of Social Sharing in Email",
            "description": "This is a test of clone email",
            "createdAt": "2016-09-07T23:20:52Z+0000",
            "updatedAt": "2016-09-07T23:20:52Z+0000",
            "url": "https://app-abm.marketo.com/#EM2250B2",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 239,
                "folderName": "Tradeshows and Events"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false
        }
    ]
}
```

## Invia esempio

Per inviare un messaggio e-mail di esempio, specificare il destinatario nel parametro di query `emailAddress`. È inoltre possibile includere i seguenti parametri facoltativi:

- `leadId`: rappresenta un lead specifico dal database.
- `textOnly`: invia solo la versione testuale del messaggio e-mail.

```http
POST /rest/asset/v1/email/{id}/sendSample.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
emailAddress=abe@testmail.com&textOnly=true
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "360b#14cce7d2708",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

## Anteprima e-mail

Utilizza l&#39;endpoint [Ottieni contenuto completo e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/getEmailFullContentUsingGET) per recuperare un&#39;anteprima live di un&#39;e-mail come riceverebbe un destinatario. Questo endpoint supporta solo e-mail versione 1.0.

Il parametro di percorso `id` richiesto identifica l&#39;e-mail da visualizzare in anteprima. L’endpoint accetta anche tre parametri di query facoltativi:

- `status`: accetta `draft` o `approved`. L’impostazione predefinita è la versione approvata per un’e-mail approvata o la versione bozza per un’e-mail non approvata.
- `type`: accetta `Text` o `HTML`. Il valore predefinito è `HTML`.
- `leadId`: accetta l&#39;ID intero di un lead e visualizza l&#39;anteprima dell&#39;e-mail come se il lead lo avesse ricevuto.

```http
GET /rest/asset/v1/email/{id}/fullContent.json
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": null,
   "result": [
      {
         "id": 339,
         "status": "draft",
         "content": "<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 1.01 Transitional//EN\" \"http://www.w1.org/TR/html4/loose.dtd\">\n<html>\n  <head>\n    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\"/>\n    <title></title>\n  </head>\n  <body>\n      <div style=\"font: 14px tahoma; width: 100%\" class=\"mktEditable\" id=\"edit_text_3\">\n        Content from testCreateEmailTemplate2\n    </div>\n  </body>\n</html>"
      }
   ]
}
```

## Sostituisci HTML

Utilizza l&#39;endpoint [Aggiorna contenuto completo e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/createEmailFullContentUsingPOST) per sostituire tutto il contenuto di una risorsa e-mail. Questo endpoint supporta solo le e-mail della versione 1.0 che hanno utilizzato la funzione Modifica codice nell’interfaccia utente e non sono più collegate al modello principale.

L’endpoint è destinato principalmente alle risorse clonate come parte di un programma che non possono essere modificate con gli endpoint di contenuto standard. Non supporta le e-mail con contenuto dinamico. Se l’e-mail è ancora collegata al relativo modello, l’endpoint restituisce un errore.

Invia una richiesta `multipart/form-data` con l&#39;e-mail `id` nel percorso. Il corpo della richiesta deve contenere un parametro `content` con un documento e-mail HTML completo e un tipo di contenuto `text/html`.

Un documento HTML in formato non valido genera un avviso e potrebbe impedire l&#39;approvazione. L&#39;inclusione di tag JavaScript o `<script>` causa l&#39;esito negativo della chiamata con un errore.

```http
POST /rest/asset/v1/email/{id}/fullContent.json
```

```text
content-type: multipart/form-data; boundary=--------------------------116301888604800085728247
content-length: 599
```

```html
----------------------------116301888604800085728247
Content-Disposition: form-data; name="content"; filename="email_content.html"
Content-Type: text/html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 1.01 Transitional//EN" "http://www.w1.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
 <title></title>
 </head>
 <body>
 <div style="font: 14px tahoma; width: 100%" class="mktEditable" id="edit_text_3">
 EMAIL TEST CONTENT
 </div>
 </body>
 </html>
----------------------------116301888604800085728247--
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": "15dbf#14a1832ae86",
   "result": [
      {
         "id": 1001
      }
   ]
}
```
