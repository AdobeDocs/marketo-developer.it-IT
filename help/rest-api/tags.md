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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 258
ht-degree: 1%

---

# Tag

[Riferimento endpoint tag](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags)

I tag sono campi definiti dall&#39;utente per i programmi. Ogni tag può essere applicato a uno o più tipi di programmi e può essere obbligatorio o facoltativo, a seconda di come è stato definito il tag. I tag possono anche fornire un elenco di valori consentiti che devono essere selezionati da per l’uso.

## Query

I tag vengono interrogati con il modello di risorsa standard, ma non hanno un endpoint per Per ID. L’elenco dei valori consentiti per un tag viene restituito solo quando viene eseguita una query sul tag per nome.

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

L&#39;endpoint [Aggiorna tag del programma](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST) consente di aggiornare il valore per un determinato tipo di tag. L&#39;endpoint accetta un percorso `id` e `tagType` che specificano l&#39;ID del programma e il tipo di tag da aggiornare. Viene utilizzato un parametro di query `tagValue` per specificare il nuovo valore per il tipo di tag. Tutti i parametri sono obbligatori.

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

I tag possono essere aggiornati in massa utilizzando l&#39;endpoint [Aggiorna metadati programma](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/updateProgramUsingPOST). Un esempio è disponibile nella sezione [Programmi di aggiornamento](programs.md#update).

## Elimina

L&#39;endpoint [Elimina tag del programma](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/deleteProgramUsingPOST) consente di eliminare un tipo di tag non richiesto. L&#39;endpoint accetta `id` e `tagType` parametri di percorso che specificano l&#39;ID del programma e il tipo di tag da eliminare.

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
