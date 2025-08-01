---
title: E-mail
feature: REST API
description: API per la manipolazione delle risorse e-mail.
exl-id: 6875730d-c74a-42cf-a3d2-dad7a3ac535d
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 1%

---

# E-mail

[Riferimento endpoint e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails) È disponibile un set completo di endpoint REST per la manipolazione delle risorse e-mail.

Nota: se si utilizza [Marketo Predictive Content](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), i seguenti endpoint non riusciranno se fanno riferimento a un&#39;e-mail con contenuto predittivo: [Get Email Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Update Email Content Section](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST), [Approve Email Draft](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). La chiamata restituisce un codice di errore 709 e il messaggio di errore corrispondente.

## Query

Il modello di query per le e-mail è identico a quello dei modelli, consentendo query [per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET) e [esplorazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET) e filtri basati sulla cartella con le API Sfoglia e per nome.

Nota: se un&#39;e-mail fa parte di un programma e-mail che utilizza [Test A/B](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), tale e-mail non è disponibile per le query che utilizzano i seguenti endpoint: [Ricevi e-mail per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [Ricevi e-mail per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [Ricevi e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). La chiamata indica che l’operazione è riuscita, ma conterrà il seguente avviso: &quot;Nessuna risorsa trovata per i criteri di ricerca specificati&quot;.

### Per ID

```
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

Se lo si desidera, è possibile passare una cartella per eseguire ricerche solo in tale cartella.

```
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

### Sfogliare

La navigazione nelle cartelle funziona come gli altri endpoint di navigazione dell&#39;API Asset e consente un filtro facoltativo su `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn` e `offset`. `status` è Approvato o Bozza. `folder` è un oggetto JSON contenente `id` e `type`. `maxReturn` è un numero intero che limita il numero di risultati (il valore predefinito è 20, il valore massimo è 200) e `offset` è un numero intero che può essere utilizzato con `maxReturn` per la lettura di set di risultati di grandi dimensioni (il valore predefinito è 0).

```
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

È possibile [recuperare le sezioni modificabili disponibili](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) per un&#39;e-mail eseguendo una query sul contenuto e, facoltativamente, filtrare in base allo stato per ottenere le sezioni per le versioni Approvate o Bozza.

```
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

Le sezioni possono restituire un tipo di dynamicContent. Per ulteriori informazioni, consulta la sezione [Contenuto dinamico](dynamic-content.md).

## Interroga campi CC

È possibile recuperare il set di campi abilitati per E-mail CC nell&#39;istanza di destinazione chiamando l&#39;endpoint [Get Email CC Fields](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET).

```
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

[Le e-mail vengono create](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) in base a un modello di origine e dispongono di un elenco di sezioni modificabili derivate da ogni elemento HTML separato del modello con una classe &quot;mktEditable&quot; e una proprietà ID univoca. La creazione di un messaggio e-mail con l’API crea un record basato sul modello ed eventuali metadati aggiuntivi passati. Per una chiamata di creazione e-mail riuscita sono necessari i seguenti parametri: nome, modello, cartella.

I seguenti parametri sono facoltativi per la creazione: `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Se non viene impostato, `subject` sarà vuoto, `fromName`, `fromEmail` e `replyEmail` saranno impostati sui valori predefiniti dell&#39;istanza e `operational` e `isOpenTrackingDisabled` saranno falsi. `isOpenTrackingDisabled` determina se il pixel di tracciamento aperto è incluso in un&#39;e-mail quando viene inviato.

```
POST /rest/asset/v1/emails.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

[L&#39;aggiornamento di un record e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) può essere eseguito per ID. Questo consente di aggiornare la descrizione o il nome dell’e-mail.

```
POST /rest/asset/v1/email/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Il contenuto di ogni sezione di un&#39;e-mail deve essere aggiornato singolarmente, a parte l&#39;oggetto, fromName, fromEmail e replyEmail, che vengono aggiornati utilizzando l&#39;endpoint [Aggiorna contenuto e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST). Quando si utilizza questo endpoint, è possibile impostare questi valori anche per l’utilizzo di contenuto dinamico anziché di contenuto statico. Ogni parametro è un oggetto JSON di tipo/valore, dove tipo è &quot;Text&quot; o &quot;DynamicContent&quot; e valore è il valore di testo appropriato o l’ID della segmentazione da utilizzare per il contenuto dinamico. I dati vengono passati come CODIFICATI POST x-www-form-urlencoded, non come JSON.  isOpenTrackingDisabled può essere impostato con Update Email Content

```
POST /rest/asset/v1/email/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Se imposti una sezione per l’utilizzo di contenuto dinamico, l’ID sezione deve essere recuperato tramite la chiamata Get Email Content.

### Aggiorna sezione modificabile

Le sezioni modificabili vengono aggiornate dai singoli htmlId. Come parametri del percorso sono richiesti solo l’ID dell’e-mail e dell’htmlId della sezione, mentre tipo, valore e textValue sono facoltativi. Il tipo può essere &quot;Text&quot;, &quot;DynamicContent&quot; o &quot;Snippet&quot; e influisce su ciò che viene passato nel valore. Se il tipo è Testo, il valore è una stringa contenente il contenuto HTML della sezione. Se si tratta di DynamicContent, si tratta di un blocco JSON, con tre membri, tipo, che sarà &quot;DynamicContent&quot;, segmentazione che è l’id della segmentazione da utilizzare per il contenuto, e predefinito, che è una stringa contenente il contenuto HTML predefinito della sezione. Il parametro facoltativo textValue è una stringa contenente la versione testuale della sezione. I dati vengono passati come CODIFICATI POST x-www-form-urlencoded, non come JSON.

```
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Nota: se l&#39;opzione Copia automatica nel testo è disabilitata per uno snippet incorporato in un messaggio e-mail, il valore HTML dello snippet viene aggiornato e quindi viene aggiornata la versione testuale di un&#39;altra sezione dell&#39;e-mail, la versione testuale dell&#39;e-mail includerà il testo che riflette il valore aggiornato dello snippet HTML, non la versione precedente come previsto con l&#39;opzione Copia automatica disabilitata.

## Moduli

Nell’Editor e-mail 1.0, un modulo è una sezione dell’e-mail definita nel modello. I moduli possono contenere qualsiasi combinazione di elementi, variabili e altro contenuto HTML come descritto [qui](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules). Marketo offre un set di API per la gestione dei moduli all’interno di un messaggio e-mail. Per gli endpoint relativi al modulo che richiedono il metodo HTTP POST, il corpo del messaggio è formattato come &quot;application/x-www-form-urlencoded&quot; (non come JSON).

La maggior parte degli endpoint correlati al modulo richiede un &quot;moduleId&quot; come parametro del percorso. Questa è una stringa che descrive il modulo. moduleIds restituiti dall&#39;endpoint [Get Email Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) come attributo &quot;htmlId&quot; (vedere la sezione [Query](#modules_query) di seguito).

### Query

Per lavorare con i moduli, devi specificare un parametro moduleId che identifichi il modulo in modo univoco. Puoi anche specificare il parametro di indice del modulo, che è un numero intero che descrive l’ordine del modulo nell’e-mail.

[Recupera i moduleId e i relativi indici](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) specificando l&#39;ID e-mail come parametro del percorso.

L’esempio seguente esegue una query su un’e-mail 1.0 basata sul modello &quot;Skeleton&quot; presente nella sezione &quot;Starter Templates&quot; (Modelli iniziali) dell’interfaccia utente di Selezione modelli.

```
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
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you've subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you've subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
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

L’array dei risultati contiene elementi che descrivono sia i moduli che gli elementi di HTML combinati. Gli elementi modulo contengono un attributo &quot;contentType&quot;: &quot;Module&quot; e un attributo &quot;index&quot;. Il moduleId viene memorizzato nell’attributo &quot;htmlId&quot;.

Continuando con l’esempio precedente di &quot;Skeleton&quot;, la tabella seguente contiene un riepilogo dei moduleId e dei relativi indici contenuti nell’e-mail.

| moduleId (anche noto come htmlId) | Indice |
|---|---|
| distanziatore | 0 |
| immagine libera | 1 |
| video | 2 |
| testo libero | 3 |
| CTA | 4 |
| h | 5 |
| due articoli | 6 |
| footer | 7 |

#### Add

[Aggiungere un modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) a un&#39;e-mail selezionando uno dei moduli esistenti contenuti nel modello e-mail in uso. A tale scopo, specifica l’ID e-mail e il moduleId come parametri del percorso. Il parametro di query dell’indice è obbligatorio e determina l’ordine del modulo nell’e-mail. Se il valore di indice supera il valore di indice esistente più grande, il modulo viene aggiunto all’e-mail.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
index=10
```

```
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

[Eliminare un modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) specificando l&#39;ID e-mail e il moduleId come parametri del percorso.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```
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

[Duplicare un modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST) specificando l&#39;ID e-mail e il moduleId come parametri del percorso. Questa chiamata duplica il modulo, posizionandolo sotto il modulo originale e spingendo gli altri moduli verso il basso.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```
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

[Ridisponi moduli](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)array contenente tutti i moduli e la posizione desiderata all&#39;interno dell&#39;e-mail per ciascuno di essi. Ogni elemento array contiene un oggetto JSON del seguente formato:  { &quot;index&quot;: &lt;_index_>, &quot;moduleId&quot;: &quot;&lt;_moduleId_>&quot; }, dove &lt;_index_> è il numero di ordine del modulo basato su zero e &lt;_moduleId_> è il moduleId.

```
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```
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

[Rinominare un modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) in un&#39;e-mail passando il nuovo nome tramite il parametro name. Specifica l’ID e-mail e il moduleId (nome esistente) come parametri del percorso.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MarketoVideo
```

```
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

Nell’editor e-mail 1.0, le variabili vengono utilizzate per memorizzare i valori degli elementi nell’e-mail. Ogni variabile viene definita aggiungendo la sintassi specifica di Marketo al HTML come descritto [qui](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Marketo offre un set di API per la gestione delle variabili all’interno di un messaggio e-mail.

### Query

[Recupera le variabili](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) per un messaggio e-mail specificando l&#39;ID e-mail come parametro del percorso.

L’esempio seguente esegue una query su un’e-mail 1.0 basata sul modello &quot;Skeleton&quot; presente nella sezione &quot;Starter Templates&quot; (Modelli iniziali) dell’interfaccia utente di Selezione modelli.

```
GET /rest/asset/v1/email/{id}/variables.json
```

```
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

L’array dei risultati contiene elementi che descrivono le variabili, una variabile per elemento.

Le variabili possono avere un ambito globale per l’intera e-mail o locale per un modulo specifico. Le variabili di entrambi gli ambiti contengono gli attributi &quot;name&quot;, &quot;value&quot; e &quot;moduleScope&quot;. L’attributo &quot;moduleScope&quot; è booleano, dove false indica globale e true indica locale. Le variabili locali contengono un attributo &quot;moduleId&quot; aggiuntivo che specifica il modulo a cui è associata la variabile.

#### Aggiornamento

[Aggiorna una variabile](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) in un messaggio e-mail passando il nuovo valore desiderato tramite il parametro value. Specifica l’ID e-mail e il nome della variabile come parametri del percorso. Se stai aggiornando una variabile del modulo, devi passare anche il parametro moduleId per specificare il modulo a cui è associata la variabile.

Nell’esempio seguente viene aggiornata una variabile globale denominata &quot;hrBorderSize&quot; con il valore 1.

```
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```
value=2
```

```
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

Nell’esempio seguente viene aggiornata una variabile locale denominata &quot;ctaLinkText&quot; con il valore &quot;Click this button!&quot;. in moduleId &quot;CTA&quot;.

```
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Le e-mail seguono il modello standard per l’approvazione dei record di risorse. Puoi approvare una bozza, annullare l’approvazione di una versione approvata e scartare una bozza esistente di un’e-mail tramite ciascuno dei propri endpoint.

### Approvazione

Quando si chiama l’endpoint di approvazione, l’e-mail verrà convalidata in base alle regole per le e-mail di Marketo. È necessario popolare `from name`, `from email`, `reply to email` e `subject` prima di approvare l&#39;e-mail.

```
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

L&#39;operazione `unapprove` può essere eseguita solo su e-mail approvate.

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

#### Elimina

L’e-mail deve essere in stato di bozza per essere eliminata. Un&#39;e-mail approvata non può essere eliminata.

```
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

```
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

Marketo fornisce un metodo semplice per clonare un messaggio e-mail. Questo tipo di richiesta viene effettuato con un POST di tipo application/x-www-url-urlencoded e accetta due parametri obbligatori, nome e cartella, un oggetto JSON incorporato con ID e tipo. description è anche un parametro facoltativo. Se non esiste alcuna versione approvata, la versione bozza viene clonata.

```
POST /rest/asset/v1/email/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Puoi attivare un’e-mail di esempio tramite l’API, da inviare al parametro di query emailAddress. Facoltativamente, puoi anche aggiungere un parametro leadId per rappresentare un lead specifico dal database e un parametro textOnly per inviare solo la versione testuale dell’e-mail.

```
POST /rest/asset/v1/email/{id}/sendSample.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Marketo fornisce l&#39;endpoint [Get Email Full Content](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) per recuperare un&#39;anteprima live di un&#39;e-mail come verrebbe inviata a un destinatario. Questo endpoint può essere utilizzato solo nelle e-mail della versione 1.0. Esiste un parametro obbligatorio, il parametro del percorso ID, che è l’ID della risorsa e-mail che desideri visualizzare in anteprima. Sono disponibili tre parametri di query facoltativi aggiuntivi:

- stato: accetta i valori &quot;bozza&quot; o &quot;approvato&quot; che, per impostazione predefinita, vengono utilizzati come versione approvata, se non approvata
- type (Tipo): accetta &quot;Text&quot; (Testo) o &quot;HTML&quot; e utilizza HTML come impostazione predefinita.
- leadId:. Accetta l’ID intero di un lead. Se questa opzione è impostata, visualizza l’e-mail in anteprima come se fosse stata ricevuta dal lead designato

```
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

Marketo fornisce l&#39;endpoint [Aggiorna contenuto completo e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) per sostituire l&#39;intero contenuto di una risorsa e-mail. Questo endpoint può essere utilizzato solo nelle e-mail versione 1.0 per le quali è stata utilizzata la funzione &quot;Modifica codice&quot; dell’interfaccia utente e la relazione con il modello principale è stata interrotta. Questa API è destinata principalmente all’utilizzo con risorse che sono state clonate come parte di un programma e non può essere modificata con gli endpoint di contenuto standard. Le e-mail con contenuto dinamico non sono supportate. Inoltre, se tenti di sostituire HTML in un messaggio e-mail in cui la relazione è intatta, viene restituito un errore.

Questo endpoint richiede un Content-Type : multipart/form-data con il parametro id nel percorso, l’ID dell’e-mail e un parametro nel corpo, contenuto come documento e-mail HTML completo con il Content-Type &quot;text/html&quot;. Un documento HTML in formato non valido genera un avviso, ma potrebbe non consentire l&#39;approvazione, mentre l&#39;inclusione di JavaScript e/o `<script>` tag nel documento causa una chiamata non riuscita e genera un errore.

```
POST /rest/asset/v1/email/{id}/fullContent.json
```

```
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
