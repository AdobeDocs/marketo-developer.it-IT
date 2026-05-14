---
title: Ottieni aggiornamenti API lead
feature: REST API
description: Scopri le modifiche ai limiti per ottenere attività lead e endpoint per ottenere modifiche lead.
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 0%

---

# Ottieni aggiornamenti API lead

A partire dal 30 settembre 2026, le chiamate alle [attività Get Lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadActivitiesUsingGET) o [modifiche Get Lead](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadChangesUsingGET) endpoint che includono il parametro `listId` avranno esito negativo se gli elenchi di destinazione contengono 10.000 o più lead con un codice di errore 1003 che indica che l&#39;elenco statico di destinazione contiene troppi record. Di recente sono state effettuate una o più chiamate API che sarebbero interessate da questa modifica. Per evitare interruzioni del servizio, potrebbe essere necessario aggiornare il modo in cui le applicazioni si integrano con Marketo entro il 30 settembre 2026.

Questi tipi di query spesso creano ricerche che non hanno risultati potenziali o si interrompono prima di trovare eventuali risultati. Limitando le dimensioni del set di dati si migliora la reattività di questi tipi di query e si garantisce che una ricerca nel set di dati possa essere completata in modo tempestivo.

## Come posso sapere se sono interessato?

Questa modifica interesserà solo un numero limitato di istanze di Marketo Engage. Gli amministratori degli abbonamenti interessati verranno informati all’interno dell’applicazione prima che la modifica venga applicata.

## Cosa devo fare?

Condividi questo documento con i responsabili delle integrazioni Marketo Engage.

A seconda del caso d’uso, esistono due opzioni di base per migrare l’applicazione in:

* Limita gli elenchi statici da cui estrarre le attività a un massimo di 10.000 membri. Puoi suddividere uno qualsiasi degli elenchi esistenti in elenchi più piccoli per continuare a polling dello stesso pubblico per le attività.
* Estrai le attività o le modifiche al valore dei dati utilizzando Bulk Activity Extract o Data Streams e unisci tali risultati all&#39;iscrizione statica all&#39;elenco con [getLeadByListId](https://developer.adobe.com/marketo-apis/api/mapi#operation/getLeadsByListIdUsingGET_1) o [Bulk Lead Extract](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-lead-extract)

## Cosa Succederà Se Non Faccio Niente?

Potresti riscontrare interruzioni nella funzione delle integrazioni API a causa di errori non gestiti durante la query di attività da elenchi statici con un numero elevato di membri.
