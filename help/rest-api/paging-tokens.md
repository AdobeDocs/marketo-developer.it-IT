---
title: Token di paging
feature: REST API
description: Visualizzare i dati dei token di paging.
exl-id: 63fbbf03-8daf-4add-85b0-a8546c825e5b
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---

# Token di paging

Per esaminare i risultati o recuperare i dati aggiornati rispetto a un dato dato, Marketo fornisce token di paging.

In alcuni casi è possibile che vengano restituite stringhe di token di paging lunghe. Questo potrebbe causare l’errore HTTP 414. Ulteriori informazioni su come gestire questi [errori](error-codes.md).

## Tipi di token

Esistono due tipi correlati, ma distinti, di token di paging forniti da Marketo:

- Basato su data
- Basato sulla posizione

## Basato su data

Il primo è un token di paging che rappresenta una data. Vengono utilizzati per recuperare attività, modifiche al valore dei dati e lead eliminati che si sono verificati dopo la data rappresentata dal token di paging. Questo tipo di token di paging viene generato chiamando l&#39;endpoint [Get Paging Token](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET) e includendo un datetime.

```
GET /rest/v1/activities/pagingtoken.json?sinceDatetime=2014-10-06T13:22:17-08:00
```

```json
{
    "requestId": "1607c#14884f3e74e",
    "success": true,
    "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
}
```

Il formato del parametro `sinceDateTime` deve essere conforme alla notazione di data standard [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Per ottenere risultati ottimali, utilizza un datetime completo che includa il fuso orario. Il fuso orario può essere rappresentato come uno scostamento da GMT utilizzando il seguente formato:

`yyyy-mm-ddThh:mm:ss+|-hh:mm`

Oppure puoi usare una &quot;Z&quot; maiuscola come scorciatoia per rappresentare un UTC come questo:

`yyyy-mm-ddThh:mm:ssZ`

Esempi

`2016-09-15T15:53:00+05:00`

`2016-09-15T10:53:00Z`

Poiché `sinceDateTime` è un parametro di query, deve essere codificato nell&#39;URL.

La stringa `nextPageToken` viene quindi fornita a una chiamata [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET), [Get Lead Changes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) o [Get Deleted Leads](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) e le attività vengono recuperate da dopo il datetime fornito all&#39;API Get Paging Token.

```
GET /rest/v1/activities.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&activityTypeIds=1&activityTypeIds=12
```

## Basato sulla posizione

Il secondo tipo di token di paging può essere restituito da qualsiasi chiamata di recupero batch a un&#39;API del database lead. Questo tipo di token di paging ha un concetto simile a quello del cursore di un database che consente l’attraversamento dei record. Ad esempio, una chiamata Get Leads By Filter Type può rappresentare un set maggiore della dimensione batch specificata, in genere il valore massimo e predefinito di 300. Quando sono presenti altri risultati, il campo moreResult è true nella risposta e viene restituito `nextPageToken`. Per recuperare i record aggiuntivi nel set di risultati, una chiamata aggiuntiva che includa `nextPageToken` con il valore ricevuto dalla risposta precedente nella nuova chiamata. La risposta risultante restituirà la pagina successiva nel set di risultati.
