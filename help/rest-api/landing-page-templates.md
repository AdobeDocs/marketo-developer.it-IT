---
title: Modelli pagina di destinazione
feature: REST API, Landing Pages
description: Gestisci i modelli della pagina di destinazione di Marketo tramite endpoint API REST per moduli gratuiti e tipi guidati, query per ID o nome, creazione, aggiornamento di HTML, clone, Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
TQID: https://experienceleague.adobe.com/U9K1MG-q2gIgJMgfM3lt1S4olETt8ln9seOIKZUncBY
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 519
ht-degree: 2%

---

# Modelli pagina di destinazione

[Riferimento endpoint modello pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates)

I modelli di pagina di destinazione sono risorse principali per le pagine di destinazione di Marketo. Ogni pagina di destinazione deriva la propria struttura di contenuto iniziale dal modello principale.

## Tipi di modelli

Marketo fornisce modelli di pagina di destinazione guidati e in formato libero. I modelli in formato libero forniscono un’esperienza di modifica scarsamente strutturata. I modelli guidati possono limitare i tipi di elementi e le posizioni a livello di modello.

Per un confronto dettagliato, vedere [Informazioni sulle pagine di destinazione in formato libero e guidate](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Query

Eseguire una query sui modelli di pagina di destinazione [per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET) o per [navigazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Questi endpoint restituiscono i metadati del modello. Recupera il contenuto di HTML separatamente per ogni modello in base all’ID.

## Crea e aggiorna

I modelli vengono creati come risorse vuote con metadati. I parametri `name` e `folder` sono obbligatori. I parametri `description`, `templateType` e `enableMunchkin` sono facoltativi.

Il valore `templateType` può essere `freeform` o `guided` e il valore predefinito è `freeForm`. Il valore predefinito di `enableMunchkin` è `false`. Se questa opzione è abilitata, impedisce il tracciamento di Munchkin sulle pagine di destinazione secondarie del modello.

```http
POST /rest/asset/v1/landingPageTemplates.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

Aggiungi il contenuto del modello separatamente con l&#39;endpoint [Aggiorna contenuto modello pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST).

### Aggiorna metadati

Utilizza l&#39;endpoint [Aggiorna metadati modello pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST) per modificare il nome, la descrizione o l&#39;impostazione `enableMunchkin`.

### Aggiorna contenuto

L’aggiornamento del contenuto del modello sostituisce tutto il contenuto HTML esistente. Passa la sostituzione come `multipart/form-data` nel parametro `content`.

```http
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```html
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

```json
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

Clona un modello di pagina di destinazione con una richiesta POST `application/x-www-url-formencoded`.

Il parametro percorso `id` specifica il modello della pagina di destinazione di origine.

Il parametro `name` specifica il nome del nuovo modello di pagina di destinazione.

Il parametro `folder` specifica la cartella padre per il nuovo modello. Passarlo come oggetto JSON incorporato contenente `id` e `type`.

Il parametro facoltativo `description` descrive il nuovo modello.

```http
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

I modelli di pagina di destinazione utilizzano la bozza standard e il modello approvato. Gli aggiornamenti vengono applicati prima alla bozza e diventano attivi solo dopo l’approvazione del modello.

Prima dell’approvazione, un modello deve soddisfare i requisiti per il suo tipo guidato o in formato libero. Consulta queste risorse:

- [Modelli di pagina di destinazione in formato libero](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Modelli di pagina di destinazione in formato guidato](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Esempi di modelli guidati](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Elimina

Per eliminare un modello, assicurati che non sia approvato e che nessuna pagina di destinazione secondaria vi faccia riferimento. Non è possibile utilizzare questa API per eliminare i modelli di pagina di destinazione con pulsanti social incorporati.
