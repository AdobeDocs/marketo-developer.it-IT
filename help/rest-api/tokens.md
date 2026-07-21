---
title: Token
feature: REST API, Tokens
description: Gestisci i miei token di Marketo con l’API REST di Asset. Consulta i tipi di dati supportati, ottieni per cartella o programma, crea o aggiorna tramite POST codificato per modulo ed elimina per nome.
exl-id: 4f8d87d7-ba2a-4c90-8b39-4d20679d404a
TQID: https://experienceleague.adobe.com/uqOpu2vDuiQiZhILKuxZJQGadd0K14zwIaAdmNfK1-I
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 290
ht-degree: 3%

---

# Token

[Riferimento endpoint token](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

I token sono stringhe che Marketo sostituisce con altri dati in fase di esecuzione. L’API può modificare solo I miei token, che sono token secondari locali in una cartella o in un programma.

Utilizza l’API Tokens per leggere, creare, aggiornare ed eliminare i Miei Token.

## Tipo di dati

I token possono essere creati con i seguenti tipi di dati:

| Tipo | Descrizione |
| --- | --- |
| data | Valore data del modulo &quot;aaaa-MM-gg&quot; |
| numero | Numero intero o a virgola mobile |
| testo RTF | Una stringa HTML |
| punteggio | Un numero intero a 32 bit con segno |
| campagna sfdc | Utilizzato nell’integrazione di Salesforce Campaign Management |
| testo | Una stringa di testo |

L’API supporta solo questi tipi di dati durante la creazione di un token.

## Query

[Ottieni token per ID cartella](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/getTokensByFolderIdUsingGET) considera l&#39;ID di un programma o di una cartella come parametro di percorso. Utilizzare il parametro `folderType` per specificare il tipo.

```http
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

L&#39;endpoint [Crea token](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/addTokenTOFolderUsingPOST) crea un token o aggiorna un token esistente con i valori inviati. I token appartengono a una cartella o a un programma.

Il parametro di percorso `id` identifica la cartella padre. I parametri `name`, `type`, `value` e `folderType` sono obbligatori. Passare i dati come POST `x-www-form-urlencoded`, non come JSON. Il token `name` non può superare i 50 caratteri.

```http
POST /rest/asset/v1/folder/{id}/tokens.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

[Elimina token per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens/operation/deleteTokenByNameUsingPOST) considera l&#39;ID di un programma o di una cartella come parametro di percorso. Utilizzare `folderType` per specificare il tipo.

La cartella padre, il token `name` e il token `type` sono obbligatori. Passare i dati come POST `x-www-form-urlencoded`, non come JSON.

```http
POST /rest/asset/v1/folder/{id}/tokens/delete.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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
