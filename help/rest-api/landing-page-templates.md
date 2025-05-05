---
title: Modelli di pagina di destinazione
feature: REST API, Landing Pages
description: Creare e modificare modelli di pagina di destinazione.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---

# Modelli di pagina di destinazione

[Riferimento endpoint modello pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

I modelli di pagina di destinazione sono una risorsa principale e una dipendenza per le singole pagine di destinazione di Marketo. Le pagine di destinazione derivano l’ossatura del loro contenuto dal modello principale.

## Tipi di modelli

Marketo dispone di due tipi di modelli di pagina di destinazione, in formato libero e guidato. I modelli di pagina di destinazione in formato libero forniscono un’esperienza di modifica scarsamente strutturata per le pagine da essi derivate. I modelli guidati forniscono un’esperienza con una struttura molto ampia, in cui i tipi di elementi e le posizioni possono essere limitati a livello di modello. Per ulteriori informazioni sulle differenze, vedere [questo documento](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Query

I modelli di pagina di destinazione supportano i tipi di query standard per le risorse di [per id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) e [navigazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Questi endpoint restituiscono i metadati per i modelli. Il recupero del contenuto HTML dei modelli deve essere eseguito in base al modello tramite il relativo ID.

## Crea e aggiorna

I modelli vengono creati come risorse vuote con metadati associati. Durante la creazione di un modello, è necessario includere un nome e una cartella, oltre a una descrizione facoltativa, un parametro templateType e enableMunchkin. templateType può essere a forma libera o guidata e il valore predefinito è freeForm. Per informazioni sulle differenze tra i tipi, vedere la sezione Forma guidata e libera. enableMunchkin ha come impostazione predefinita false e, se attivato, impedisce l&#39;esecuzione del tracciamento Munchkin su qualsiasi pagina di destinazione figlio del modello.

```
POST /rest/asset/v1/landingPageTemplates.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=New LPT - PHP&folder={"id":12,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "11b7#14dfe1e3bcf",
    "result": [
        {
            "id": 286,
            "name": "assetAPITest",
            "description": "test",
            "createdAt": "2015-06-16T20:45:03Z+0000",
            "updatedAt": "2015-06-16T20:45:03Z+0000",
            "url": "https://app-devlocal1.marketo.com/#LT286B2ZN12",
            "folder": {
                "type": "Folder",
                "value": 12,
                "folderName": "Templates"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Il contenuto del modello deve essere compilato separatamente tramite l&#39;endpoint [Aggiorna contenuto modello pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST).

### Aggiorna metadati

I metadati per i modelli di pagina di destinazione possono essere aggiornati tramite l&#39;endpoint [Aggiorna metadati modello pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST). Nome, descrizione e impostazione enableMunchkin possono essere aggiornati in questo modo.

### Aggiorna contenuto

Il contenuto dei modelli di pagina di destinazione viene effettuato come aggiornamento distruttivo dell’intero contenuto di HTML. Il contenuto deve essere trasmesso come dati multipart/form, con l’unico parametro denominato content.

```
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```
content-type: multipart/form-data; boundary=--------------------------435851813185237176536801
----------------------------435851813185237176536801
Content-Disposition: form-data; name="content"; filename="content.txt"
Content-Type: text/plain

<html>
<head>
</head>
<body>
<div>Placeholder Content</div>
</body>
</html>
----------------------------435851813185237176536801--
```

```
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e0dc60bbc",
  "result": [
    {
      "id": 286
    }
  ]
}
```

## Duplica

Marketo fornisce un metodo semplice per clonare i modelli di una pagina di destinazione. Si tratta di una richiesta POST con codifica application/x-www-url-formencoded.

Il parametro percorso `id` specifica l&#39;ID del modello della pagina di destinazione di origine da clonare.

Il parametro `name` viene utilizzato per specificare il nome del nuovo modello della pagina di destinazione.

Il parametro `folder` viene utilizzato per specificare la cartella principale in cui risiederà il nuovo modello della pagina di destinazione. È un oggetto JSON incorporato contenente  `id` e `type`.

Il parametro facoltativo `description` viene utilizzato per descrivere il nuovo modello di pagina di destinazione.

```
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Standard Template Clone&folder={"type": "Folder", "id": 732}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "dee6#1683e9fd410",
    "warnings": [],
    "result": [
        {
            "id": 61,
            "name": "Standard Template Clone",
            "createdAt": "2019-01-11T20:34:48Z+0000",
            "updatedAt": "2019-01-11T20:34:48Z+0000",
            "url": "https://app-abm.marketo.com/#LT61B2ZN732",
            "folder": {
                "type": "Folder",
                "value": 732,
                "folderName": "Test LP Template Clone"
            },
            "status": "draft",
            "workspace": "Default",
            "templateType": "freeForm",
            "enableMunchkin": true
        }
    ]
}
```

## Approvazione

I modelli di pagina di destinazione seguono il modello standard approvato in bozza, in cui può essere presente una versione in bozza e/o una versione approvata. Ogni volta che si applicano aggiornamenti a un modello, questi vengono sempre applicati prima alla versione bozza e vengono visualizzati in tempo reale solo dopo l’approvazione del modello.

Per essere approvato, un modello deve essere conforme alle regole per il suo tipo, guidate o meno. Per ulteriori informazioni sui requisiti per la creazione e l’approvazione dei modelli dei rispettivi tipi, consulta i rispettivi documenti di creazione:

- [Modelli di pagina di destinazione in formato libero](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Modelli di pagina di destinazione guidata](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Esempi di modelli guidati](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Elimina

Per eliminare un modello, questo deve essere non utilizzato e non approvato, il che significa che nessuna pagina di destinazione secondaria può farvi riferimento.  Con questa API non è possibile eliminare i modelli di pagina di destinazione con pulsanti social incorporati.
