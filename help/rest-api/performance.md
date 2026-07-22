---
title: Prestazioni
feature: REST API
description: Migliora le prestazioni dell’API REST di Marketo con la compressione HTTP. Consenti a gzip di tagliare la larghezza di banda; API in blocco non supportate e inferiori a 1024 byte non compresse.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
TQID: https://experienceleague.adobe.com/foJCTd890HZtL-UzWx2cjRXwTxqgW56A79sB7FPEWis
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 131
ht-degree: 1%

---

# Prestazioni

Utilizza le opzioni relative alle prestazioni in questa pagina per migliorare l’efficienza dell’integrazione.

## Compressione HTTP

L’API REST di Marketo supporta la compressione HTTP response-body come definito dalla specifica HTTP 1.1. Abilita la compressione per ridurre l’utilizzo della larghezza di banda e i tempi di recupero dei dati.

>[!NOTE]
>
>I payload inferiori a 1024 byte non sono compressi e le API in blocco non supportano la compressione.

Per abilitare la compressione, includi la seguente intestazione HTTP nella richiesta:

```html
Accept-Encoding: gzip
```

L’API REST di Marketo comprime il corpo della risposta e include la seguente intestazione:

```html
Content-Encoding: gzip
```

L&#39;esempio di cURL seguente chiama l&#39;endpoint [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET) per recuperare cinque lead:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
