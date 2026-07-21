---
title: Contenuto dinamico
feature: REST API, Dynamic Content
description: Configurare contenuti dinamici Marketo a livello di sezione tramite API REST utilizzando segmentazioni per personalizzare e-mail, pagine di destinazione e snippet con endpoint ed esempi
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
TQID: https://experienceleague.adobe.com/MwfPxu74qk0bPZMr6yuxQi--e3gMvP1tXQZ5iMil02o
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 329
ht-degree: 3%

---

# Contenuto dinamico

Utilizza le segmentazioni dei lead per fornire contenuto dinamico in questi tipi di risorse:

- E-mail
- Pagine di destinazione
- Snippet

## Panoramica

Il contenuto dinamico funziona a livello di sezione. Ogni sezione può fornire varianti per i segmenti in una segmentazione selezionata.

Quando un lead visualizza la risorsa, Marketo mostra la variante per il segmento del lead. Se il lead non è idoneo per un segmento, Marketo visualizza il contenuto predefinito.

## Esempio

In questo esempio viene utilizzata una segmentazione Region (US) per visualizzare una promozione evento per i lead nel segmento Southwest. Il segmento include lead provenienti da California, Nevada, Utah, Colorado, Arizona e Nuovo Messico.

Utilizzare l&#39;endpoint [Aggiorna sezione contenuto e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailComponentContentUsingPOST) per modificare la sezione modificabile con ID `Q1-promotion-banner` in una sezione `DynamicContent`. Il parametro `value` specifica l&#39;ID di segmentazione.

Le e-mail e le pagine di destinazione seguono questo modello. I snippet utilizzano il diverso pattern descritto nella documentazione sull’API dei snippet.

L’esempio che segue imposta la sezione come una sezione Contenuto dinamico, segmentata dalla segmentazione 1001.

```http
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```text
type=DynamicContent&value=1001
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1909
    }
  ]
}
```

Chiamare l&#39;endpoint [sezione del contenuto dinamico dell&#39;e-mail di aggiornamento](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails/operation/updateEmailDynamicContentUsingPOST) per aggiungere contenuto per un segmento in una sezione specifica.

La richiesta seguente mostra un banner speciale invece del contenuto predefinito per i lead nel segmento Southwest. Per creare più varianti, chiama l’endpoint per ciascun segmento e sezione.

```http
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```text
segment=Southwest&type=HTML&value=<img src='//www.example.com/SuperSpecialBannerForAmericanSouthwestLeads.jpg'/>
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "891b#1729b34b9a5",
  "warnings": [],
  "result": [
    {
      "id": 1637
    }
  ]
}
```

## Segmentazione

Una segmentazione è un elenco definito dall’utente di set di regole che Marketo valuta dall’alto verso il basso rispetto al database principale. Un lead può appartenere a un solo segmento in ogni segmentazione. Il lead si unisce al primo segmento per il quale è qualificato.

Se il lead non è idoneo per un altro segmento, si unisce al segmento Predefinito e riceve il contenuto predefinito della segmentazione.

### Elenco

Utilizza l’endpoint di elenco per recuperare le segmentazioni disponibili.

```http
GET /rest/asset/v1/segmentation.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "78eb#14e9de95868",
  "result": [
    {
      "id": 1001,
      "name": "My Industry Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:10Z+0000",
      "url": "https://app-abm.marketo.com/#SG1001A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    },
    {
      "id": 1002,
      "name": "My Country Segmentation",
      "description": "",
      "createdAt": "2015-04-06T18:28:23Z+0000",
      "updatedAt": "2015-04-06T18:37:18Z+0000",
      "url": "https://app-abm.marketo.com/#SG1002A1",
      "folder": {
        "type": "Program",
        "value": 396,
        "folderName": null
      },
      "status": "approved",
      "workspace": "Default"
    }
  ]
}
```

Utilizza l’endpoint segmenti per recuperare i segmenti in una segmentazione principale.

```http
GET /rest/asset/v1/segmentation/1001/segments.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "2031#14e9df08796",
  "result": [
    {
      "id": 1001,
      "name": "Manufacturing",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1002,
      "name": "Healthcare",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769688A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1003,
      "name": "Financial",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769690A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1004,
      "name": "Technology",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769692A1",
      "status": "approved",
      "segmentationId": 1001
    },
    {
      "id": 1005,
      "name": "Default",
      "description": null,
      "createdAt": "2015-04-06T18:23:32Z+0000",
      "updatedAt": "2015-04-06T18:37:09Z+0000",
      "url": "https://app-abm.marketo.com/#SL769694A1",
      "status": "approved",
      "segmentationId": 1001
    }
  ]
}
```
