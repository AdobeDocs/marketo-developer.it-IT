---
title: Token
feature: REST API, Tokens
description: Gestire i token in Marketo.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 2%

---

# Token

[Riferimento endpoint token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens)

I token in Marketo sono stringhe speciali simili ai codici brevi che vengono sostituiti da dati separati in fase di esecuzione. In Marketo sono disponibili diversi tipi di token, ma solo I miei token possono essere modificati tramite l’API. I miei token sono token secondari locali per una cartella o un programma specifico. I token possono essere letti, creati ed eliminati tramite l’API.

## Tipo di dati

I token possono essere creati con i seguenti tipi di dati:

| Tipo | Descrizione |
|---------------|----------------------------------------------------|
| data | Valore data del modulo &quot;aaaa-MM-gg&quot; |
| numero | Numero intero o a virgola mobile |
| testo RTF | Una stringa HTML |
| punteggio | Un numero intero a 32 bit con segno |
| campagna sfdc | Utilizzato nell’integrazione della gestione delle campagne Salesforce |
| text | Una stringa di testo |


Questi sono gli unici tipi di dati che possono essere utilizzati durante la creazione di un token tramite API.

## Query

[Ottieni token per ID cartella](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/getTokensByFolderIdUsingGET) accetta un `id` come parametro di percorso di un tipo di programma o cartella. Tipo specificato dal parametro `folderType`.

```curl
GET /rest/asset/v1/folder/{id}/tokens.json?folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4fbe#14e27fc9bbf",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "AprilFool - deverly",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Crea e aggiorna

L&#39;endpoint [Crea token](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/addTokenTOFolderUsingPOST) crea i token o, se esistono, li aggiorna con i valori inviati. I token vengono creati nel contesto di una cartella o di un programma. Il parametro di percorso `id` richiesto è l&#39;ID della cartella a cui verrà associato il token. `name`, `type`, `value` e `folderType` sono tutti parametri obbligatori del token. I dati vengono passati come POST x-www-form-urlencoded, non come JSON. Il campo `name` del token non può superare i 50 caratteri.

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&type=date&value=2015-04-01&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

## Elimina

[Elimina token per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Tokens/operation/deleteTokenByNameUsingPOST) accetta un ID come parametro di percorso di un tipo di programma o cartella. Tipo specificato dal parametro `folderType`. I token vengono eliminati in base alla cartella principale, `name`, e al `type` del token, ciascuno dei quali è obbligatorio. I dati vengono passati come POST x-www-form-urlencoded, non come JSON.

```
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=AprilFool - deverly&type=date&folderType=Program
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "12ed2#14e2800f89c",
    "result": [
        {
            "id": 416
        }
    ]
}
```
