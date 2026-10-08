---
title: Ottieni aggiornamenti API lead
feature: REST API
description: Scopri le modifiche ai limiti per ottenere attività lead e endpoint per ottenere modifiche lead.
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
subfeature_v2:
  - id: cf1396d8-ab85-4e93-b35d-d9b573024abf
    internal-label: REST APIs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%
---
# Ottieni aggiornamenti API lead

A partire dal 30 settembre 2026, le chiamate agli endpoint [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) o [Get Lead Changes](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) che includono il parametro `listId` avranno esito negativo se gli elenchi di destinazione contengono 10.000 o più lead. Gli endpoint restituiranno un codice di errore 1003 che indica che l&#39;elenco statico di destinazione contiene troppi record.

Questa modifica influisce su una o più chiamate API recenti. Per evitare interruzioni del servizio, potrebbe essere necessario aggiornare la modalità di integrazione delle applicazioni con Marketo entro il 30 settembre 2026.

Queste query spesso creano ricerche che non hanno risultati potenziali o timeout prima di trovare risultati. Limitando le dimensioni del set si migliora la reattività delle query e si aiuta a completare le ricerche in modo tempestivo.

## Come posso sapere se sono interessato?

Questa modifica interessa solo un numero limitato di istanze di Marketo Engage. Gli amministratori degli abbonamenti interessati riceveranno una notifica in-application prima che la modifica venga applicata.

## Cosa devo fare?

Condividi questo documento con gli utenti o i team responsabili delle integrazioni Marketo Engage.

A seconda del caso d’uso, utilizza una delle seguenti opzioni di migrazione:

* Limita gli elenchi statici utilizzati per l’estrazione dell’attività a 10.000 membri. Dividi gli elenchi esistenti in elenchi più piccoli per continuare a polling dello stesso pubblico per le attività.
* Estrarre le attività o le modifiche al valore dei dati utilizzando Bulk Activity Extract o Data Streams. Unisci i risultati all&#39;appartenenza all&#39;elenco statico con [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) o [Bulk Lead Extract](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract).

## Cosa Succederà Se Non Faccio Niente?

Le integrazioni API potrebbero essere interrotte da errori non gestiti durante la query di attività da elenchi statici con un numero elevato di membri.
