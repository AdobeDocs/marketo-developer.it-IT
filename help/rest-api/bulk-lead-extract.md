---
title: Estrazione lead in blocco
feature: REST API
description: Scopri come utilizzare le API REST di Estrazione lead in blocco di Marketo per esportare in blocco i lead con filtri per data, elenco e elenchi avanzati, campi personalizzati e formati CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 2%

---

# Estrazione lead in blocco

[Riferimento endpoint estrazione lead bulk](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

Il set Bulk Lead Extract delle API REST fornisce un’interfaccia programmatica per recuperare set elevati di record di lead/persone da Marketo. Inoltre, può essere utilizzato per recuperare i lead in modo incrementale in base alla data di creazione del record, all’aggiornamento più recente, all’iscrizione all’elenco statico o all’iscrizione all’elenco avanzato. L&#39;interfaccia consigliata per i casi d&#39;uso che richiedono l&#39;interscambio continuo di dati tra Marketo e uno o più sistemi esterni, a scopo di ETL, data warehousing e archiviazione.

## Autorizzazioni

Le API Bulk Lead Extract richiedono che l’utente API proprietario abbia un ruolo con una o entrambe le autorizzazioni Read-Only Lead o Read-Write Lead.

## Filtri

I lead supportano varie opzioni di filtro. Alcuni filtri, tra cui `updatedAt`, `smartListName` e `smartListId`, richiedono componenti di infrastruttura aggiuntivi non ancora implementati in tutte le sottoscrizioni. È possibile specificare un solo tipo di filtro per processo di esportazione.

| Tipo di filtro | Tipo di dati | Note |
|---|---|---|
| createdAt | Date Range | Accetta un oggetto JSON con i membri `startAt` e `endAt`. `startAt` accetta un datetime che rappresenta la filigrana bassa e `endAt` accetta un datetime che rappresenta la filigrana alta. L’intervallo non può essere superiore a 31 giorni. I valori di data devono essere in formato ISO-8601, senza millisecondi. I processi con questo tipo di filtro restituiscono tutti i record accessibili creati entro l&#39;intervallo di date. |
| updateAt* | Date Range | Accetta un oggetto JSON con i membri `startAt` e `endAt`. `startAt` accetta un datetime che rappresenta la filigrana bassa e `endAt` accetta un datetime che rappresenta la filigrana alta. L’intervallo non può essere superiore a 31 giorni. I valori di data devono essere in formato ISO-8601, senza millisecondi. Nota: questo filtro non filtra il campo visibile &quot;updatedAt&quot; che riflette solo gli aggiornamenti ai campi standard. Filtra in base a quando è stato effettuato l’aggiornamento più recente del campo a un record leadJobs con questo tipo di filtro restituisce tutti i record accessibili più di recente aggiornati all’interno dell’intervallo di date. |
| staticListName | Stringa | Accetta il nome di un elenco statico. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri dell&#39;elenco statico al momento dell&#39;inizio dell&#39;elaborazione del processo. Recuperare i nomi di elenco statici utilizzando l&#39;endpoint Get Lists. |
| staticListId | Intero | Accetta l’ID di un elenco statico. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri dell&#39;elenco statico al momento dell&#39;inizio dell&#39;elaborazione del processo. Recupera gli ID di elenco statici utilizzando l’endpoint Get Lists. |
| smartListName* | Stringa | Accetta il nome di un elenco avanzato. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri degli elenchi smart nel momento in cui il processo inizia l&#39;elaborazione. Recuperare i nomi degli elenchi smart utilizzando l&#39;endpoint Get Smart Lists. |
| smartListId* | Intero | Accetta l’ID di un elenco avanzato. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri degli elenchi smart nel momento in cui il processo inizia l&#39;elaborazione. Recupera gli ID degli elenchi avanzati utilizzando l’endpoint &quot;Get Smart Lists&quot;. |

Il tipo di filtro non è disponibile per alcune sottoscrizioni. Se non disponibile per la sottoscrizione, viene visualizzato un errore durante la chiamata dell’endpoint del processo Crea lead di esportazione (&quot;1035, tipo di filtro non supportato per la sottoscrizione di destinazione&quot;). I clienti possono contattare il supporto tecnico Marketo per richiedere che questa funzionalità sia abilitata nel loro abbonamento.

## Opzioni

L’endpoint del processo Crea lead di esportazione offre diverse opzioni di formattazione, consentendo all’utente di includere campi particolari all’interno del file esportato, la possibilità di rinominare le intestazioni di colonna di tali campi e il formato del file esportato.

| Parametro | Tipo di dati | Obbligatorio | Note |
|---|---|---|---|
| campi | Array[Stringa] | Sì | Il parametro fields accetta un array JSON di stringhe. Ogni stringa deve essere il nome REST API di un campo lead di Marketo. I campi elencati sono inclusi nel file esportato. L’intestazione di colonna per ciascun campo corrisponderà al nome API REST di ciascun campo, a meno che non venga sostituita da columnHeader. Nota: quando la funzionalità [!DNL Adobe Experience Cloud Audience Sharing] è abilitata, si verifica un processo di sincronizzazione dei cookie che associa l&#39;ID [!DNL Adobe Experience Cloud] (ECID) ai lead di Marketo. Puoi specificare il campo &quot;ecid&quot; per includere gli ECID nel file di esportazione. |
| columnHeaderNames | Oggetto | No | Oggetto JSON contenente coppie chiave-valore di nomi di intestazione di campo e colonna. La chiave deve essere il nome di un campo incluso nel processo di esportazione. Questo è il nome API del campo che può essere recuperato chiamando Descrivi lead. Il valore corrisponde al nome dell&#39;intestazione di colonna esportata per il campo. |
| formato | Stringa | No | Accetta uno di: CSV, TSV, SSV. Il file esportato viene renderizzato rispettivamente come un file di valori separati da virgole, valori separati da tabulazioni o valori separati da spazi, se impostato. Se non impostato, viene impostato il valore predefinito CSV. |

## Creazione di un processo

I parametri per il processo vengono definiti prima di avviare l&#39;esportazione utilizzando l&#39;endpoint [Crea processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST). È necessario definire `fields` necessari per l&#39;esportazione, il tipo di parametri di `filter`, `format` del file e gli eventuali nomi di intestazione di colonna.

```
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName",
      "id",
      "email"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "id": "Marketo Id",
      "email": "Email Address"
   },
   "filter": {
      "createdAt": {
         "startAt": "2017-01-01T00:00:00Z",
         "`endAt`": "2017-01-31T00:00:00Z"
      }
   }
}
```

Questa richiesta inizierà l&#39;esportazione di un set di lead creati tra il 1° gennaio 2017 e il 31 gennaio 2017, inclusi i valori dei campi `firstName`, `lastName`, `id` e `email` corrispondenti.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Restituisce una risposta di stato che indica che il processo è stato creato. Il processo è stato definito e creato, ma non è ancora stato avviato. A tale scopo, l&#39;endpoint [Processo lead esportazione accodamento](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) deve essere chiamato utilizzando il valore exportId dalla risposta dello stato di creazione:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "147e4#16b24d9b913",
    "result": [
        {
            "exportId": "fad2cd1b-e822-4025-be1e-9caa9cf1d4b8",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2019-06-04T23:35:43Z",
            "queuedAt": "2019-06-04T23:36:17Z"
        }
    ],
    "success": true
}
```

Questo risponde con un `status` di &quot;In coda&quot; dopo il quale verrà impostato su &quot;Elaborazione&quot; quando è disponibile uno slot di esportazione.

## Stato processo di polling

È possibile recuperare lo stato `Note:` solo per i processi creati dallo stesso utente API.

Poiché si tratta di un endpoint asincrono, dopo la creazione del processo è necessario eseguire un polling del relativo stato per determinarne l’avanzamento. Eseguire il polling utilizzando l&#39;endpoint [Ottieni stato processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). Lo stato viene aggiornato solo una volta ogni 60 secondi, pertanto non è consigliabile una frequenza di polling inferiore a questa, e in quasi tutti i casi è ancora eccessiva. Diamo un&#39;occhiata ai sondaggi.

```
GET /bulk/v1/leads/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

L’endpoint di stato risponde indicando che il processo è ancora in elaborazione, pertanto il file non è ancora disponibile per il recupero. Una volta che lo stato del processo cambia in &quot;Completato&quot;, si è preparato per il download.

Il campo di stato può rispondere con uno qualsiasi dei seguenti elementi:

- Creata
- In coda
- Elaborazione
- Annullato
- Completato
- Operazione non riuscita

## Recupero dei dati

Per recuperare il file di un&#39;esportazione lead completata, è sufficiente chiamare l&#39;endpoint [Ottieni file lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) con `exportId`.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La risposta contiene un file formattato nel modo in cui è stato configurato il processo. L’endpoint risponde con il contenuto del file.

Se un campo lead richiesto è vuoto (non contiene dati), `null` viene inserito nel campo corrispondente nel file di esportazione. Nell’esempio seguente, il campo e-mail per il lead restituito è vuoto.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Per supportare il recupero parziale e intuitivo dei dati estratti, l’endpoint del file supporta facoltativamente l’intervallo di intestazioni HTTP dei byte di tipo. Se l’intestazione non è impostata, viene restituito l’intero contenuto. Ulteriori informazioni sull&#39;utilizzo dell&#39;intestazione Range con Marketo [Bulk Extract](bulk-extract.md).

## Annullamento di un processo

Se un processo non è stato configurato correttamente o non è più necessario, può essere facilmente annullato utilizzando l&#39;endpoint [Annulla processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST):

```
POST /bulk/v1/leads/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Questo risponde con uno stato che indica che il processo è stato annullato.
