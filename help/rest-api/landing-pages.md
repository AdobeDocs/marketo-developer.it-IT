---
title: Pagine di destinazione
feature: REST API, Landing Pages
description: Utilizza l’API REST di Marketo per eseguire query su metadati e contenuti, creare, aggiornare, approvare, eliminare e clonare le pagine di destinazione, inclusi i tipi guidati e in formato libero.
exl-id: 2f986fb0-0a6b-469f-b199-1c526cd5a882
TQID: https://experienceleague.adobe.com/NssOtB6BEMGOQzzauLI7AszLpN3fVcEeJcr9VNTkpJE
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 917
ht-degree: 2%

---

# Pagine di destinazione

[Riferimento endpoint pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages)

Le pagine di destinazione sono pagine web ospitate da Marketo. Utilizza le API REST per le pagine di destinazione per eseguire query e gestirne metadati, contenuti, ciclo di vita e anteprima.

## Query

Eseguire query sulle pagine di destinazione [per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages/operation/getLandingPageByIdUsingGET) o per [navigazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages/operation/browseLandingPagesUsingGET). Queste query restituiscono solo i metadati. Eseguire una query sulle sezioni di contenuto di una pagina di destinazione separatamente per ID pagina.

La query del contenuto di una pagina di destinazione restituisce le sezioni di contenuto disponibili. Prima di poter aggiornare l&#39;elenco, è necessario che venga visualizzata una sezione.

```http
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

Le pagine di destinazione guidate includono sezioni definite dal relativo modello. Le pagine in formato libero non includono sezioni predefinite, pertanto aggiungi il contenuto prima di modificarlo.

Il formato dell&#39;attributo `content` dipende dall&#39;attributo `type` e dalla natura statica o dinamica del campo.

## Crea e aggiorna

[Crea una pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages/operation/createLandingPageUsingPOST) da un modello. Sono necessari il nome della pagina, l’ID modello e la cartella di destinazione. Vedi il riferimento dell’endpoint per i metadati facoltativi.

Gli endpoint [per il contenuto della pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content) supportano i seguenti tipi di contenuto: `richText`, `HTML`, `Form`, `Image`, `Rectangle` e `Snippet`.

```http
POST rest/asset/v1/landingPages.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

I metadati della pagina di destinazione possono essere aggiornati con l&#39;endpoint [Aggiorna metadati pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Approvazione

Le pagine di destinazione utilizzano la bozza standard e il modello approvato. Gli aggiornamenti si applicano alla bozza e diventano attivi solo dopo l’approvazione.

## Elimina

Prima di eliminare una pagina di destinazione, accertati che non sia approvata e che nessun’altra risorsa Marketo vi faccia riferimento. Elimina le singole pagine con l&#39;endpoint [Elimina pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST). Non è possibile utilizzare questa API per eliminare pagine con pulsanti social incorporati.

## Duplica

Clona una pagina di destinazione con una richiesta POST `application/x-www-url-formencoded`.

Il parametro percorso `id` specifica la pagina di destinazione di origine.

Il parametro `name` specifica il nuovo nome della pagina di destinazione.

Il parametro `folder` specifica la cartella padre. Passarlo come oggetto JSON incorporato contenente `id` e `type`.

Il parametro `template` specifica l&#39;ID del modello della pagina di destinazione di origine.

Il parametro facoltativo `description` descrive la nuova pagina di destinazione.

```http
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=MyNewLandingPage&folder={"type":"Program","id":1119}&template=57
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1078d#1683e4881c6",
    "warnings": [],
    "result": [
        {
            "id": 3291,
            "name": "MyNewLandingPage",
            "createdAt": "2019-01-11T18:59:25Z+0000",
            "updatedAt": "2019-01-11T18:59:25Z+0000",
            "folder": {
                "type": "Program",
                "value": 1119,
                "folderName": "DefaultProgramWithGuidedLP"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 57,
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "http://na-abm.marketo.com/lp/284-RPR-133/DefaultProgramWithGuidedLPPerkutoTestLP-Clone-1.html",
            "computedUrl": "https://app-abm.marketo.com/#LP3291A1LA1"
        }
    ]
}
```

## Sezione Gestisci contenuto

Le sezioni di contenuto sono ordinate in base alla proprietà `index` e visualizzate in base alle regole CSS del client. Utilizza gli endpoint [Add](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Update](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) e [Delete](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) per gestire le sezioni. Utilizza [Ottieni contenuto pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET) per eseguire una query.

Ogni sezione ha `type` e `value` parametri. `type` determina il `value` previsto. Trasmettere i dati a questi endpoint come POST `x-www-form-urlencoded`, non come JSON.

**Tipi di sezione**

| Tipo | Valore |
| --- | --- |
| DynamicContent | ID della segmentazione. |
| Modulo | ID del modulo. |
| HTML | Contenuto HTML di testo. |
| Immagine | ID della risorsa immagine. |
| Rettangolo | Vuoto. |
| RichText | Contenuto HTML di testo.  Può contenere solo elementi in formato Rich Text. |
| Snippet | ID del frammento. |
| Pulsante social | ID del pulsante Social. |
| Video | ID del video. |

Per le pagine in formato libero, aggiungi ogni sezione di contenuto richiesta. Marketo le incorpora nell&#39;elemento `div` con l&#39;ID `mktoContent`.

Le pagine guidate possono includere elementi predefiniti restituiti da [Ottieni contenuto pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Utilizza gli endpoint corrispondenti per aggiungere elementi o [aggiornarne il contenuto](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST).

### Contenuto dinamico

Per rendere dinamica una sezione, accertati innanzitutto che venga visualizzata nell’elenco del contenuto della pagina di destinazione. Quindi utilizzare [Aggiorna sezione contenuto pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) per impostarne il tipo su `DynamicContent`.

Marketo crea sezioni dinamiche sottostanti che ereditano il tipo e il contenuto di base dell’elemento convertito.

```http
GET /rest/asset/v1/landingPage/{id}/dynamicContent/RVMtNDg=.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "46e#1560fa169d9",
  "result": [
    {
      "createdAt": "2016-07-21",
      "updatedAt": "2016-07-21",
      "segmentation": 1007,
      "segments": [
        {
          "segmentId": 1018,
          "segmentName": "Default",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        },
        {
          "segmentId": 1017,
          "segmentName": "New Segment",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        }
      ]
    }
  ]
}
```

[L&#39;aggiornamento del contenuto](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) per ogni singolo segmento viene eseguito in base all&#39;ID del segmento.

```http
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
segment=New Segment&value=New Content
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e08fe7cbbc",
  "result": [
    {
      "id": 1012
    }
  ]
}
```

## Variabili

Le pagine di destinazione guidate supportano variabili modificabili che contengono valori di elemento. Modifica le variabili nell’editor pagina di destinazione:

![Variabili della pagina di destinazione](assets/landing-page-variables.png)

Le variabili sono metatag nell&#39;elemento `<head>` di un modello di pagina di destinazione guidata. I tipi supportati sono String, Color e Boolean. L’esempio che segue definisce una variabile di ciascun tipo:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Per ulteriori informazioni, consulta la sezione &quot;Variabile modificabile&quot; nella documentazione di [Creazione di un modello di pagina di destinazione guidata](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template).

### Query

Recupera le variabili per una pagina di destinazione guidata passando l’ID della pagina di destinazione all’endpoint &quot;Get Landing Page Variables&quot;.

```http
GET /rest/asset/v1/landingPage/{id}/variables.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "10843#15a6d7e5fa1",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello World!",
            "type": "string"
        },
        {
            "id": "colorVar",
            "value": "#FFFFFF",
            "type": "color"
        },
        {
            "id": "boolVar",
            "value": "true",
            "type": "boolean"
        }
    ]
}
```

Questa pagina di destinazione guidata contiene tre variabili: `stringVar`, `colorVar` e `boolVar`.

### Aggiornamento

Aggiorna una variabile per una pagina di destinazione guidata passando l’ID della pagina di destinazione, l’ID della variabile e il valore della variabile all’endpoint &quot;Update Landing Page Variables&quot;.

```http
POST /rest/asset/v1/landingPage/{id}/variable/{variableId}.json?value={newValue}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2b07#15a6db77da3",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello Brave New World!",
            "type": "String"
        }
    ]
}
```

## Anteprima pagina di destinazione

Utilizza [Ottieni contenuto pagina di destinazione completo](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) per recuperare un&#39;anteprima con rendering del browser. Il parametro del percorso della pagina di destinazione `id` è obbligatorio. L’endpoint accetta anche due parametri di query facoltativi:

- `segmentation`: matrice di oggetti JSON contenente `segmentationId` e `segmentId`. L’anteprima rappresenta un lead che corrisponde a tali segmenti.
- `leadId`: un ID lead intero. L&#39;anteprima rappresenta il lead specificato.

```http
GET /rest/asset/v1/landingPage/{id}/fullContent.json?leadId=1001&segmentation=[{"segmentationId":1030,"segmentId":1103}]
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "119ab#17692849f1e",
  "warnings": [],
  "result": [
    {
      "id": 1023,
      "content": "<!DOCTYPE html>\n<html>\n <head>\n <meta charset=\"utf-8\">\n \n \n <meta name=\"robots\" content=\"index, nofollow\">\n <title></title>\n <style>\n body {background:#FFFFFF} \n #myConditionalDisplayArea {\n display: true;\n }\n </style>\n <link rel=\"shortcut icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n<link rel=\"icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n\n\n<style>.mktoGen.mktoImg {display:inline-block; line-height:0;}</style>\n </head>\n <body id=\"bodyId\">\n \n Hello Brave New World!\n <div class=\"mktoText\" id=\"exampleText\"><div>This is an example editable text area.</div>\n<div>Lead Full Name = Hanna Crawford</div>\n<div><br /></div>\n <script type=\"text/javascript\" src=\"//munchkin.marketo.net//munchkin.js\"></script><script>Munchkin.init('123-ABC-456', {customName: 'Test-Landing-Page-APIs_Guided-Landing-Page---deverly', PURL_VISIT_TOKEN, wsInfo: 'j1RR'});</script>\n<div id=\"mktoClickBlockingDiv\"></div>\n </body>\n</html>\n"
    }
  ]
}
```
