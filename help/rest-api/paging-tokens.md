---
title: Token di paging
feature: REST API
description: Utilizza i token di paging API REST di Marketo per recuperare attività e lead, che coprono token basati su data e posizione, ISO 8601 SinceDatetime ed errori 414.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
TQID: https://experienceleague.adobe.com/Ut05n-Y-qPJnvcNRs9liwE3NVBMbJlvaGyv-nExRsek
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 387
ht-degree: 0%

---

# Token di paging

Marketo fornisce token di paging per scorrere i risultati o recuperare dati aggiornati in base a una data specifica.

Alcune risposte restituiscono stringhe di token di paging lunghe, che possono causare un errore HTTP 414. Vedi le informazioni sulla gestione di questi [errori](error-codes.md).

Consulta la documentazione [Paging Token API](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET).

## Tipi di token

Marketo fornisce due tipi correlati ma distinti di token di paging:

- I token basati sulla data recuperano i record che si verificano dopo un datetime specificato.
- I token basati sulla posizione attraversano i record in un set di risultati.

## Basato su data

Un token di paging basato su data rappresenta un datetime. Utilizzala per recuperare attività, modifiche al valore dei dati e lead eliminati che si verificano dopo tale data e ora.

Genera un token basato su data chiamando l&#39;endpoint [Get Paging Token](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getActivitiesPagingTokenUsingGET) con un datetime:

```http
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

Il parametro `sinceDateTime` deve utilizzare la notazione di data standard [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Per risultati ottimali, fornisci una data e un’ora complete con un fuso orario.

Rappresenta il fuso orario come scostamento da GMT nel seguente formato:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

In alternativa, utilizzare una &quot;Z&quot; maiuscola per rappresentare UTC:

`yyyy-mm-ddThh:mm:ssZ`

Ad esempio:

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Poiché `sinceDateTime` è un parametro di query, il relativo valore viene codificato tramite URL.

Passa la stringa `nextPageToken` restituita a una chiamata [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET), [Get Lead Changes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) o [Get Deleted Leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). La chiamata recupera i record che si verificano dopo il datetime fornito all&#39;API Get Paging Token.

```http
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basato sulla posizione

Un token di paging basato sulla posizione può essere restituito da qualsiasi chiamata di recupero batch a un&#39;API del database lead. Il token funziona come un cursore del database e consente l’attraversamento dei record.

Ad esempio, una chiamata Get Leads By Filter Type può restituire un set di risultati più grande della dimensione batch richiesta, che in genere ha un valore massimo e predefinito di 300. Quando sono disponibili più risultati, la risposta imposta il campo moreResult su true e restituisce `nextPageToken`.

Per recuperare la pagina successiva, effettua un&#39;altra chiamata e passa il valore `nextPageToken` dalla risposta precedente. La risposta restituisce la pagina successiva nel set di risultati.
