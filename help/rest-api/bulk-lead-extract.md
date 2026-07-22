---
title: Estrazione lead in blocco
feature: REST API
description: Scopri come utilizzare le API REST di Estrazione lead in blocco di Marketo per esportare in blocco i lead con filtri per data, elenco e elenchi avanzati, campi personalizzati e formati CSV/TSV.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
TQID: https://experienceleague.adobe.com/4eMJR87fHDdccrVid3wHtspvBVQmrBGHYMlIwFCSdEI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1037
ht-degree: 2%

---

# Estrazione lead in blocco

[Riferimento endpoint estrazione lead bulk](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads)

Le API REST Bulk Lead Extract recuperano set elevati di record di lead/persone da Marketo. Puoi anche recuperare i lead in modo incrementale in base alla data di creazione del record, all’aggiornamento più recente, all’iscrizione all’elenco statico o all’iscrizione all’elenco avanzato.

Utilizza Bulk Lead Extract per lo scambio continuo di dati tra Marketo e i sistemi esterni, inclusi ETL, data warehousing e flussi di lavoro di archiviazione.

## Autorizzazioni

L&#39;utente API proprietario del processo deve disporre di un ruolo con l&#39;autorizzazione Lead di sola lettura, l&#39;autorizzazione Lead di lettura/scrittura o entrambe le autorizzazioni.

## Filtri

I processi di esportazione dei lead supportano diversi tipi di filtro. Ogni processo di esportazione può utilizzare un solo tipo di filtro.

I filtri `updatedAt`, `smartListName` e `smartListId` richiedono un&#39;infrastruttura non disponibile in tutte le sottoscrizioni.

| Tipo di filtro | Tipo di dati | Note |
| --- | --- | --- |
| createdAt | Date Range | Un oggetto JSON con `startAt` e `endAt` membri. `startAt` è il valore di data/ora della filigrana bassa e `endAt` è il valore di data/ora della filigrana alta. Utilizzare i valori di data e ora ISO-8601 senza millisecondi. L’intervallo non può essere superiore a 31 giorni. Il job restituisce tutti i record accessibili creati all&#39;interno dell&#39;intervallo di date. |
| updateAt* | Date Range | Un oggetto JSON con `startAt` e `endAt` membri. `startAt` è il valore di data/ora della filigrana bassa e `endAt` è il valore di data/ora della filigrana alta. Utilizzare i valori di data e ora ISO-8601 senza millisecondi. L’intervallo non può essere superiore a 31 giorni. Questo filtro non utilizza il campo `updatedAt` visibile, che riflette gli aggiornamenti solo ai campi standard. Viene invece utilizzata l’ora dell’aggiornamento del campo più recente in un record principale. Il processo restituisce tutti i record accessibili aggiornati più di recente all’interno dell’intervallo di date. |
| staticListName | Stringa | Nome di un elenco statico. Il job restituisce tutti i record accessibili che sono membri dell&#39;elenco statico quando il job inizia l&#39;elaborazione. Recuperare i nomi di elenco statici utilizzando l&#39;endpoint Get Lists. |
| staticListId | Intero | ID di un elenco statico. Il job restituisce tutti i record accessibili che sono membri dell&#39;elenco statico quando il job inizia l&#39;elaborazione. Recupera gli ID di elenco statici utilizzando l’endpoint Get Lists. |
| smartListName* | Stringa | Nome di un elenco avanzato. Il processo restituisce tutti i record accessibili che sono membri dell&#39;elenco smart quando inizia l&#39;elaborazione del processo. Recuperare i nomi degli elenchi smart utilizzando l&#39;endpoint Ottieni elenchi smart. |
| smartListId* | Intero | ID di un elenco avanzato. Il processo restituisce tutti i record accessibili che sono membri dell&#39;elenco smart quando inizia l&#39;elaborazione del processo. Recupera gli ID degli elenchi avanzati utilizzando l’endpoint Ottieni elenchi avanzati. |

I tipi di filtro contrassegnati con un asterisco non sono disponibili per alcune sottoscrizioni. Se un tipo di filtro non è disponibile per la sottoscrizione, l’endpoint Crea processo lead di esportazione restituisce l’errore &quot;1035, tipo di filtro non supportato per la sottoscrizione di destinazione&quot;. Contatta il supporto Marketo per abilitare questa funzionalità per il tuo abbonamento.

## Opzioni

L’endpoint &quot;Crea processo lead di esportazione&quot; fornisce opzioni per selezionare i campi esportati, rinominare le intestazioni di colonna e impostare il formato del file.

| Parametro | Tipo di dati | Obbligatorio | Note |
| --- | --- | --- | --- |
| campi | Array[Stringa] | Sì | Array JSON di stringhe. Ogni stringa deve essere il nome REST API di un campo lead di Marketo. L&#39;esportazione include ogni campo elencato e utilizza il relativo nome API REST come intestazione di colonna, a meno che `columnHeaderNames` non lo sostituisca. Quando la funzione [!DNL Adobe Experience Cloud Audience Sharing] è abilitata, un processo di sincronizzazione dei cookie associa l&#39;ID [!DNL Adobe Experience Cloud] (ECID) ai lead di Marketo. Specificare il campo `ecids` per includere gli ECID nel file di esportazione. |
| columnHeaderNames | Oggetto | No | Oggetto JSON costituito da coppie chiave-valore di campo e intestazione di colonna. Ogni chiave deve essere il nome API di un campo incluso nel processo di esportazione. Recupera il nome API chiamando Descrivi lead. Ogni valore rappresenta l’intestazione della colonna esportata per quel campo. |
| formato | Stringa | No | Il formato del file di esportazione: CSV per i valori delimitati da virgole, TSV per i valori delimitati da tabulazioni o SSV per i valori delimitati da spazi. Il valore predefinito è CSV. |

## Creazione di un processo

Utilizza l&#39;endpoint [Crea processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST) per definire un processo di esportazione. Specificare `fields` da esportare, un tipo `filter` e i relativi parametri, il file `format` ed eventuali nomi di intestazione di colonna personalizzati.

```http
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
         "endAt": "2017-01-31T00:00:00Z"
      }
   }
}
```

Questa richiesta crea un processo di esportazione per i lead creati tra il 1° gennaio 2017 e il 31 gennaio 2017. L&#39;esportazione include i valori dei campi `firstName`, `lastName`, `id` e `email`.

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

La risposta conferma che il processo è stato creato ma non avviato. Per avviare il processo, chiama l&#39;endpoint [Accoda processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST) con `exportId` dalla risposta di creazione.

```http
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

La risposta di accodamento ha un `status` di &quot;In coda&quot;. Quando uno slot di esportazione diventa disponibile, lo stato cambia in &quot;Elaborazione&quot;.

## Stato processo di polling

Puoi recuperare lo stato solo per i processi creati dallo stesso utente API.

I processi di esportazione dei lead vengono eseguiti in modo asincrono. Esamina l&#39;endpoint [Ottieni stato processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) per tenere traccia dell&#39;avanzamento del processo.

Lo stato viene aggiornato una sola volta ogni 60 secondi. Non eseguire il polling con maggiore frequenza; nella maggior parte dei casi, tale intervallo è ancora eccessivo.

```http
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

Questa risposta indica che il processo è ancora in elaborazione, quindi il file non è disponibile. Quando lo stato del processo diventa &quot;Completato&quot;, il file è pronto per il download.

Il campo `status` può restituire uno dei seguenti valori:

- Creato
- In coda
- Elaborazione
- Annullato
- Completato
- Operazione non riuscita

## Recupero dei dati

Per recuperare un&#39;esportazione del lead completata, chiamare l&#39;endpoint [Get Export Lead File](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET) con `exportId`.

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

Il corpo della risposta contiene il file nel formato configurato per il processo.

Se un campo lead richiesto non contiene dati, il campo corrispondente nel file di esportazione contiene `null`. Nell’esempio seguente, il lead restituito ha un campo e-mail vuoto.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Per il recupero parziale o ripristinabile, l&#39;endpoint del file supporta l&#39;intestazione HTTP `Range` facoltativa con il tipo `bytes`. Se non imposti l’intestazione, l’endpoint restituisce tutto il contenuto. Ulteriori informazioni sull&#39;utilizzo dell&#39;intestazione `Range` con Marketo [Bulk Extract](bulk-extract.md).

## Annullamento di un processo

Per annullare un processo non configurato correttamente o non necessario, chiamare l&#39;endpoint [Annulla processo lead esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST).

```http
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

La risposta conferma l’annullamento del processo.
