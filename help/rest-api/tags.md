---
title: Tag
feature: REST API, Tags
description: Esegui query sui tipi di tag, ottieni i valori consentiti per nome, aggiorna o elimina i tag dei programmi in Marketo tramite l’API REST Asset, con esempi di richieste.
exl-id: 64731d1a-a749-4d6f-b336-16c733d002f0
TQID: https://experienceleague.adobe.com/zjdyfoofVWytE0Q-K4lk598jmleTSFOD7tSRqeAHsjk
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 227
ht-degree: 2%

---

# Tag

[Riferimento endpoint tag](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags)

I tag sono campi definiti dall&#39;utente per i programmi. Un tag può essere applicato a uno o più tipi di programmi e può essere obbligatorio o facoltativo. Un tag può anche definire un elenco di valori consentiti tra cui gli utenti devono selezionare.

## Query

Effettua query sui tag con il modello di risorsa standard. I tag non hanno un endpoint By Id. Per recuperare i valori consentiti per un tag, esegui una query sul tag per nome.

### Ottieni tag

```http
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

```http
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

Utilizzare l&#39;endpoint [Aggiorna tag del programma](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST) per aggiornare il valore per un tipo di tag. Tutti i parametri sono obbligatori:

- Il parametro percorso `id` specifica l&#39;ID del programma.
- Il parametro percorso `tagType` specifica il tipo di tag da aggiornare.
- Il parametro di query `tagValue` specifica il nuovo valore.

```http
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

Per aggiornare più tag, utilizzare l&#39;endpoint [Aggiorna metadati programma](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST). Vedere l&#39;esempio nella sezione [Programmi aggiornati](programs.md#update).

## Elimina

Utilizzare l&#39;endpoint [Elimina tag del programma](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/deleteProgramUsingPOST) per eliminare un tipo di tag non richiesto. Il parametro percorso `id` specifica l&#39;ID del programma e il parametro percorso `tagType` specifica il tipo di tag da eliminare.

```http
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
