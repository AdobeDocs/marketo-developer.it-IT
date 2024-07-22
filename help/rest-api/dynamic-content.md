---
title: Contenuto dinamico
feature: REST API, Dynamic Content
description: Configurare i contenuti dinamici con le API di Marketo.
exl-id: 8ab97624-5fb5-4a41-911f-ec8616dd43c9
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 1%

---

# Contenuto dinamico

Marketo facilita l’utilizzo di contenuti dinamici tramite la segmentazione dei lead su più tipi di risorse:

- E-mail
- Pagine di destinazione
- Snippet

## Panoramica

Il contenuto dinamico viene implementato a livello di sezione, designando varianti specifiche di una sezione da distribuire a un lead in base alla loro qualifica in un segmento all’interno di una segmentazione scelta. Se un contenuto è configurato per fornire contenuto dinamico basato su una determinata segmentazione, a un lead che visualizza tale contenuto viene trasmessa la variante di contenuto che corrisponde al segmento in cui rientra oppure il contenuto predefinito, se non sono idonei per un segmento.

## Esempio

A dimostrazione di ciò, vediamo un esempio di e-mail in cui è presente una segmentazione per regione (Stati Uniti) e vogliamo mostrare una promozione evento solo per i lead che rientrano nel segmento Sud-ovest, che include i lead di California, Nevada, Utah, Colorado, Arizona e Nuovo Messico. A questo scopo, rendiamo una sezione modificabile nel nostro messaggio e-mail con ID &quot;Q1-promotion-banner&quot; in una sezione DynamicContent. Per eseguire questa operazione, è necessario utilizzare l&#39;endpoint [Aggiorna sezione contenuto e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST) per l&#39;e-mail. Il parametro `value` viene utilizzato per specificare l&#39;ID della segmentazione.

Nota: le e-mail e le pagine di destinazione seguono questo pattern. I snippet hanno un pattern diverso, descritto nella documentazione relativa all’API dei snippet.

L’esempio che segue imposta la sezione come una sezione Contenuto dinamico, segmentata dalla segmentazione 1001.

```
POST /rest/asset/v1/email/{id}/content/Q1-promotion-banner.json
```

```
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

Per aggiungere contenuto per singoli segmenti, è necessario chiamare l&#39;endpoint [Aggiorna sezione contenuto dinamico e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailDynamicContentUsingPOST) per la sezione specifica.

L&#39;esempio seguente imposta la sezione in modo da visualizzare l&#39;immagine speciale del banner per i lead nel segmento Sud-ovest invece del valore predefinito. Se volessimo creare più varianti per più segmenti, chiameremmo nuovamente questo endpoint per ciascun segmento e sezione.

```
POST /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json
```

```
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

La segmentazione è il nucleo del contenuto dinamico Marketo. Una segmentazione è un elenco definito dall’utente di singoli set di regole che vengono valutati dall’alto verso il basso rispetto all’intero database dei lead. Un lead può essere membro di un solo segmento in ogni segmentazione e sarà membro del primo segmento per il quale è qualificato in ogni segmentazione. Se non è idoneo per un segmento, sarà un membro del segmento Predefinito e riceverà il contenuto predefinito per qualsiasi dato contenuto dinamico utilizzando tale segmentazione.

### Elenco

Le segmentazioni hanno un endpoint di elenco che restituisce una risposta con un elenco di segmentazioni disponibili.

```
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

Le segmentazioni hanno anche un endpoint che restituisce una risposta con un elenco di segmenti da una segmentazione principale.

```
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
