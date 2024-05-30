---
title: "Tag"
feature: REST API, Tags
description: '"Gestisci i tag per i programmi in Marketo".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 1%

---


# Tag

[Riferimento endpoint tag](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tags)

I tag sono campi definiti dall&#39;utente per i programmi. Ogni tag può essere applicato a uno o più tipi di programmi e può essere obbligatorio o facoltativo, a seconda di come è stato definito il tag. I tag possono anche fornire un elenco di valori consentiti che devono essere selezionati da per l’uso.

## Query

I tag vengono interrogati con il modello di risorsa standard, ma non hanno un endpoint per Per ID. L’elenco dei valori consentiti per un tag viene restituito solo quando viene eseguita una query sul tag per nome.

### Ottieni tag

```
GET /rest/asset/v1/tagTypes.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1488a#1504ecfccf8",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true
        },
        {
            "tagType": "AAA2 Required Event Tag Type",
            "applicableProgramTypes": "[event]",
            "required": true
        },
        {
            "tagType": "AAA3 Not Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": false
        }
    ]
}
```

### Per nome

```
GET /rest/asset/v1/tagType/byName.json?name=AAA1 Required Tag Type
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "8a44#1504ed0da2f",
    "result": [
        {
            "tagType": "AAA1 Required Tag Type",
            "applicableProgramTypes": "[program,email_batch,nurture,event,webinar]",
            "required": true,
            "allowableValues": "[AAA1 RT1, AAA1 RT2, AAA1 RT3, AAA1 RT4]"
        }
    ]
}
```

## Aggiornamento

Il [Aggiorna tag programma](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) endpoint consente di aggiornare il valore per un determinato tipo di tag. L’endpoint richiede un `id` e `tagType` parametri del percorso che specificano l’id del programma e il tipo di tag da aggiornare. A `tagValue` il parametro query viene utilizzato per specificare il nuovo valore per il tipo di tag. Tutti i parametri sono obbligatori.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}.json?tagValue=David
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fd84#17f84a885a6",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```

I tag possono essere aggiornati in massa utilizzando [Aggiorna metadati programma](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/updateProgramUsingPOST) endpoint. Ecco un esempio [qui](programs.md#update).

## Elimina

Il [Elimina tag programma](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/deleteProgramUsingPOST) endpoint consente di eliminare un tipo di tag non richiesto. L’endpoint accetta `id` e `tagType` parametri di percorso che specificano l’id del programma e il tipo di tag da eliminare.

```
POST /rest/asset/v1/program/{id}/tag/{tagType}/delete.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d998#17f84ad36a7",
    "warnings": [],
    "result": [
        {
            "id": 1067
        }
    ]
}
```
