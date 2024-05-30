---
title: "Pagine di destinazione"
feature: REST API, Landing Pages
description: "Eseguire query sulle pagine di destinazione in Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 1%

---


# Pagine di destinazione

[Riferimento endpoint pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Le pagine di destinazione sono pagine web ospitate da Marketo.

## Query

Come la maggior parte delle altre risorse, è possibile eseguire una query sulle pagine di destinazione [per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET), e da [navigazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET). Queste query restituiranno solo i metadati e l’elenco delle sezioni di contenuto per una pagina di destinazione deve essere interrogato separatamente dall’ID della pagina di destinazione.

La query del contenuto della pagina di destinazione restituirà un elenco di sezioni di contenuto disponibili nella pagina di destinazione. Una sezione deve essere presente nell’elenco dei contenuti di una pagina per aggiornare il contenuto:

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

I risultati differiscono tra i modelli in formato guidato e quelli in formato libero, in quanto le pagine di destinazione guidate presentano un set di sezioni definite dal modello da cui derivano, mentre le pagine in formato libero non includono sezioni predefinite ed è necessario aggiungerne il contenuto prima di modificarle.  Il formato dell’attributo &quot;content&quot; può variare a seconda dell’attributo &quot;type&quot; e del fatto che il campo sia statico o dinamico.

## Crea e aggiorna

[Vengono create pagine di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST) facendo riferimento a un modello. Gli unici campi obbligatori per la creazione sono nome, modello (l’ID del modello) e la cartella in cui inserire la pagina. Per ulteriori metadati che possono essere compilati, vedi il riferimento all’endpoint.

Tipi di contenuto validi per [contenuto della pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) Gli endpoint sono: richText, HTML, Form, Image, Rectangle, Snippet.

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

I metadati della pagina di destinazione possono essere aggiornati con [Aggiorna endpoint metadati pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Approvazione

Le pagine di destinazione seguono il modello standard approvato dalla bozza, in cui può essere presente una bozza di versione e/o una versione approvata. Ogni volta che si applicano aggiornamenti a una pagina, questi vengono sempre applicati prima alla versione della bozza e vengono visualizzati in diretta solo dopo che la pagina è stata approvata.

## Elimina

Per eliminare una pagina di destinazione, prima deve essere non utilizzata e non deve essere utilizzata come riferimento da altre risorse Marketo, oltre a non essere approvata. Le pagine vengono eliminate singolarmente con [Elimina pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST) endpoint. Questa API non consente di eliminare le pagine di destinazione con pulsanti social incorporati. 

## Duplica

Marketo fornisce un metodo semplice per clonare una pagina di destinazione. Si tratta di una richiesta POST con codifica application/x-www-url-formencoded.

Il `id` Il parametro path specifica l&#39;ID della pagina di destinazione di origine da clonare.

Il `name` Il parametro viene utilizzato per specificare il nome della nuova pagina di destinazione.

Il `folder` Il parametro viene utilizzato per specificare la cartella principale in cui viene creata la nuova pagina di destinazione. È un oggetto JSON incorporato contenente `id` e `type`.

Il `template` Il parametro viene utilizzato per specificare l’ID del modello della pagina di destinazione di origine.

L&#39;opzione `description` Il parametro viene utilizzato per descrivere la nuova pagina di destinazione.

```
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Le sezioni di contenuto sono ordinate in base alla loro proprietà di indice e infine strutturate in base alle regole CSS applicate al momento della visualizzazione da parte del client. Le sezioni di contenuto sono incluse e gestite con i corrispondenti [Aggiungi](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Aggiorna](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) e [Elimina](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) Endpoint della sezione del contenuto della pagina di destinazione, e possono essere interrogati utilizzando [Ottieni contenuto pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Ogni sezione ha un tipo e un parametro di valore. Il tipo determina cosa deve essere inserito nel valore.  Per questi endpoint, i dati vengono passati come POST x-www-form-urlencoded, non come JSON.

**Tipi di sezione**

| Tipo | Valore |
|--- |--- |
| DynamicContent | ID della segmentazione. |
| Modulo | ID del modulo. |
| HTML | Contenuto di Text HTML. |
| Immagine | ID della risorsa immagine. |
| Rettangolo | Vuoto. |
| RichText | Contenuto di Text HTML.  Può contenere solo elementi in formato Rich Text. |
| Frammento | ID del frammento. |
| Pulsante social | ID del pulsante Social. |
| Video | ID del video. |

Per le pagine in formato libero, è necessario aggiungere tutte le sezioni di contenuto desiderate che verranno incorporate nell’elemento div con l’ID `mktoContent`. Per le pagine guidate, un elenco di elementi predefiniti può essere presente nell’elenco da [Ottieni contenuto pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET) endpoint. È possibile aggiungere altri elementi o il relativo [contenuto aggiornato](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) tramite i rispettivi endpoint.

### Contenuto dinamico

Per creare una sezione di contenuto dinamico, questa deve essere già presente nell’elenco del contenuto della pagina di destinazione. Il [Sezione Aggiorna contenuto pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) deve quindi essere utilizzato per impostare il tipo su &quot;DynamicContent&quot;. Quando una sezione è impostata sul contenuto dinamico, crea sezioni dinamiche sottostanti all’interno della sezione del contenuto che ereditano tutte il tipo di base dell’elemento convertito. Ogni sezione dinamica eredita anche il contenuto dalla sezione convertita.

```
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

[Aggiornamento del contenuto](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) per ogni singolo segmento viene eseguito in base all’id del segmento.

```
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Una delle funzioni introdotte nelle pagine di destinazione guidate sono le variabili modificabili.  Le variabili contengono valori per gli elementi di una pagina di destinazione.  Le variabili possono essere modificate facilmente utilizzando l’editor di pagine di destinazione come mostrato di seguito:

![Variabili della pagina di destinazione](assets/landing-page-variables.png)

Le variabili sono definite come metatag all’interno di `<head>` di un modello di pagina di destinazione in modalità guidata. Sono disponibili tre tipi di variabili: String, Color e Boolean.  Di seguito è riportato un esempio di tre definizioni di variabili:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Per ulteriori informazioni, consulta la sezione &quot;Variabile modificabile&quot; in [Creare un modello di pagina di destinazione guidata](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template) documentazione.

### Query

Recupera le variabili per una pagina di destinazione guidata passando l’ID della pagina di destinazione all’endpoint &quot;Get Landing Page Variables&quot;.

```
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

In questo esempio, la pagina di destinazione guidata contiene 3 variabili: stringVar, colorVar, boolVar.

### Aggiornamento

Aggiorna una variabile per una pagina di destinazione guidata passando l’ID della pagina di destinazione, l’ID della variabile e il valore della variabile all’endpoint &quot;Update Landing Page Variables&quot;.

```
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

Marketo fornisce [Ottieni contenuto completo pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) endpoint per recuperare un’anteprima live di una pagina di destinazione così come verrebbe rappresentata in un browser. Esiste un parametro obbligatorio, il `id` parametro percorso che è l’id della pagina di destinazione da visualizzare in anteprima. Sono disponibili due parametri di query facoltativi aggiuntivi:

- segmentazione: accetta un array di oggetti JSON che contengono gli attributi segmentationId e segmentId. Con questa impostazione, visualizza in anteprima la pagina di destinazione come se fossi un lead corrispondente a tali segmenti.
- leadId: accetta l&#39;ID intero di un lead. Con questa impostazione, visualizza in anteprima la pagina di destinazione come se fosse visualizzata dal lead designato.

```
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
