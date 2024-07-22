---
title: Prestazioni
feature: REST API
description: Suggerimenti sulle prestazioni per l’utilizzo dell’API Marketo.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Prestazioni

Questa pagina contiene un elenco di argomenti relativi alle prestazioni che puoi utilizzare per migliorare le prestazioni dell’integrazione.

## Compressione HTTP

L’API REST di Marketo supporta la compressione HTTP dei corpi di risposta utilizzando gli standard definiti dalla specifica HTTP 1.1.  L&#39;abilitazione della compressione è consigliata in quanto riduce l&#39;utilizzo della larghezza di banda e il tempo impiegato per recuperare i dati.

**Nota:**  I payload inferiori a 1024 byte non verranno compressi.

Per abilitare la compressione, includi la seguente intestazione HTTP nella richiesta:

```html
Accept-Encoding: gzip
```

L’API REST di Marketo comprimerà il corpo della risposta e includerà questa intestazione:

```html
Content-Encoding: gzip
```

Esempio di utilizzo di Curl per chiamare l&#39;endpoint [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) per recuperare 5 lead:

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
