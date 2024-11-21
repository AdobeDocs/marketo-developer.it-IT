---
title: Estrai in blocco
feature: REST API
description: Operazioni batch per l'estrazione dei dati Marketo.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: e7d893a81d3ed95e34eefac1ee8f1ddd6852f5cc
workflow-type: tm+mt
source-wordcount: '1683'
ht-degree: 0%

---

# Estrai in blocco

Marketo fornisce interfacce per il recupero di grandi set di dati relativi a persone e persone, denominati Bulk Extract. Attualmente, le interfacce sono disponibili per tre tipi di oggetto:

- Lead (persone)
- Attività
- Membri del programma
- Oggetti personalizzati

L’estrazione in blocco viene eseguita creando un processo, definendo il set di dati da recuperare, accodando il processo, aspettando che il processo completi la scrittura di un file e quindi recuperando il file tramite HTTP. Questi processi vengono eseguiti in modo asincrono e possono essere sottoposti a polling per recuperare lo stato dell’esportazione.

`Note:` endpoint API in blocco non hanno il prefisso &#39;/rest&#39; come gli altri endpoint.

## Autenticazione

Le API di estrazione in blocco utilizzano lo stesso metodo di autenticazione OAuth 2.0 delle altre API REST di Marketo. È necessario inviare un token di accesso valido come intestazione HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>Il supporto per l&#39;autenticazione tramite il parametro di query **access_token** verrà rimosso il 30 giugno 2025. Se il progetto utilizza un parametro di query per passare il token di accesso, deve essere aggiornato per utilizzare l&#39;intestazione **Authorization** il prima possibile. Il nuovo sviluppo deve utilizzare esclusivamente l&#39;intestazione **Authorization**.

## Limiti

- Numero massimo processi di esportazione simultanei: 2
- Numero massimo processi di esportazione in coda (inclusi i processi di esportazione correnti): 10
- Periodo di conservazione dei file: sette giorni
- Allocazione predefinita esportazione giornaliera: 500 MB (ripristinato ogni giorno alle 00:00 CST). Aumenti disponibili per l’acquisto.
- Intervallo di tempo massimo per il filtro dell’intervallo di date (createdAt o updatedAt): 31 giorni

I filtri Estrazione lead bulk per UpdatedAt e Smart List non sono disponibili per alcuni tipi di abbonamento. Se non disponibile, una chiamata all’endpoint Create Export Lead Job restituisce l’errore &quot;1035, Tipo di filtro non supportato per la sottoscrizione di destinazione&quot;. I clienti possono contattare il supporto tecnico Marketo per richiedere che questa funzionalità sia abilitata nel loro abbonamento.

### Coda

Le API di estrazione in blocco utilizzano una coda di processi (condivisa tra lead, attività, membri del programma e oggetti personalizzati). I processi di estrazione devono prima essere creati e quindi accodati chiamando gli endpoint Crea lead/attività di esportazione/membro del programma e Accoda lead esportazione/attività/membro del programma. Una volta accodati, i processi vengono estratti dalla coda e avviati quando le risorse di elaborazione diventano disponibili.

Il numero massimo di processi nella coda è 10. Se si tenta di accodare un processo quando la coda è piena, l&#39;endpoint del processo di esportazione accodamento restituisce l&#39;errore &quot;1029, Troppi processi in coda&quot;. È possibile eseguire contemporaneamente un massimo di due processi (lo stato è &quot;Elaborazione&quot;).

### Dimensione file

Le API di estrazione in blocco vengono misurate in base alle dimensioni su disco dei dati recuperati da un processo di estrazione in blocco. La dimensione esplicita in byte di un processo può essere determinata leggendo l&#39;attributo `fileSize` dalla risposta di stato completato di un processo di esportazione.

La quota giornaliera non può superare i 500 MB al giorno, condivisa tra lead, attività, membri del programma e oggetti personalizzati. Quando la quota viene superata, non è possibile creare o accodare un altro processo finché la quota giornaliera non viene ripristinata alla mezzanotte [ora centrale](https://en.wikipedia.org/wiki/Central_Time_Zone). Fino a quel momento, viene restituito l’errore &quot;1029, Quota giornaliera di esportazione superata&quot;. A parte la quota giornaliera, non esiste una dimensione massima per il file.

Una volta messo in coda o in elaborazione, il processo viene eseguito fino al completamento (salvo un errore o l’annullamento del processo). Se un processo non riesce per qualche motivo, è necessario ricrearlo. I file vengono scritti completamente solo quando un processo raggiunge lo stato completato (i file parziali non vengono mai scritti). Puoi verificare che un file sia stato scritto completamente calcolando l’hash SHA-256 e confrontandolo con il checksum restituito dagli endpoint di stato del processo.

È possibile determinare la quantità totale di disco utilizzato per il giorno corrente chiamando Processi Get Export Lead/Activity/Program Member. Questi endpoint restituiscono un elenco di tutti i processi degli ultimi sette giorni. È possibile filtrare l&#39;elenco in base ai soli processi completati nel giorno corrente (utilizzando gli attributi `status` e `finishedAt`). Quindi sommare le dimensioni dei file per quei processi per produrre la quantità totale utilizzata. Impossibile eliminare un file per recuperare spazio su disco.

## Autorizzazioni

L’estrazione in blocco utilizza lo stesso modello di autorizzazioni dell’API REST di Marketo e non richiede autorizzazioni speciali aggiuntive da utilizzare, anche se sono necessarie autorizzazioni specifiche per ogni set di endpoint.

I processi di estrazione in blocco sono accessibili solo all’utente API che li ha creati, inclusi il polling dello stato e il recupero del contenuto dei file.

Gli endpoint di estrazione in blocco non riconoscono le aree di lavoro di Marketo. Le richieste di estrazione includono sempre i dati in tutte le aree di lavoro, indipendentemente da come definisci l’API Solo utente per il servizio personalizzato.

## Creazione di un processo

Le API di estrazione in blocco di Marketo utilizzano il concetto di processo per avviare ed eseguire l’estrazione dei dati. Prendiamo in considerazione la creazione di un semplice processo di esportazione di lead.

```
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name"
   },
   "filter": {
      "createdAt": {
         "startAt": "2023-01-01T00:00:00Z",
         "endAt": "2023-01-31T00:00:00Z"
      }
   }
}
```

Questa semplice richiesta creerà un processo che restituirà i valori contenuti nei campi &quot;firstName&quot; e &quot;lastName&quot;, con le intestazioni di colonna &quot;First Name&quot; e &quot;Last Name&quot; come file CSV, contenenti ogni lead creato tra il 1° gennaio 2023 e il 31 gennaio 2023.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2023-01-21T11:47:30-08:00",
         "queuedAt": "2023-01-21T11:48:30-08:00",
         "format": "CSV",
      }
   ]
}
```

Quando creiamo il processo, questo restituisce un ID processo nell&#39;attributo `exportId`. Possiamo quindi utilizzare questo ID processo per accodare il processo, annullarlo, verificarne lo stato o recuperare il file completato.

### Parametri comuni

Ogni endpoint per la creazione di processi condivide alcuni parametri comuni per la configurazione del formato di file, dei nomi dei campi e del filtro di un processo di estrazione in blocco. Ogni sottotipo di processo di estrazione può avere parametri aggiuntivi:

| Parametro | Tipo di dati | Note |
|---|---|---|
| formato | Stringa | Determina il formato di file dei dati estratti con le opzioni per i valori separati da virgola, da tabulazione e da punto e virgola. Accetta uno di: CSV, SSV, TSV. Il formato predefinito è CSV. |
| columnHeaderNames | Oggetto | Consente di impostare i nomi delle intestazioni di colonna nel file restituito. Ogni chiave membro è il nome dell&#39;intestazione di colonna da rinominare e il valore è il nuovo nome dell&#39;intestazione di colonna. Ad esempio, &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filter | Oggetto | Filtro applicato al processo di estrazione. I tipi e le opzioni variano a seconda del tipo di processo. |


## Recupero processi

A volte, potrebbe essere necessario recuperare i processi recenti. Questa operazione può essere eseguita facilmente con Ottieni processi di esportazione per il tipo di oggetto corrispondente. Ogni endpoint Get Export Jobs supporta un campo filtro `status`, un  `batchSize` per limitare il numero di processi restituiti e `nextPageToken` per il paging attraverso set di risultati di grandi dimensioni. Il filtro di stato supporta ogni stato valido per un processo di esportazione: Creato, In coda, Elaborazione, Annullato, Completato e Non riuscito. La proprietà batchSize ha un valore massimo e predefinito di 300. Otteniamo l’elenco dei processi di esportazione lead:

```
GET /bulk/v1/leads/export.json?status=Completed,Failed
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
      ...
   ]
}
```

L&#39;endpoint risponde con la risposta `status` di ogni processo creato negli ultimi sette giorni per quel tipo di oggetto nell&#39;array dei risultati. La risposta includerà solo i risultati per i processi di proprietà dell’utente API che effettua la chiamata.

## Avvio di un processo

Con il nostro ID lavoro a portata di mano, iniziamo il lavoro:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

In questo modo viene avviata l’esecuzione del processo e viene restituita una risposta di stato. Poiché l’esportazione viene sempre eseguita in modo asincrono, è necessario eseguire il polling dello stato del processo per determinare se è stato completato. Lo stato di un determinato processo non viene aggiornato con una frequenza maggiore di una volta ogni 60 secondi, pertanto il polling dello stato non deve mai essere eseguito con una frequenza maggiore di tale intervallo. Tieni presente, tuttavia, che la maggior parte dei casi d’uso non deve mai richiedere un sondaggio più frequente di una volta ogni 5 minuti. I dati di ciascuna esportazione corretta vengono conservati per 10 giorni.

## Stato processo di polling

Determinare lo stato del processo è semplice.

È possibile eseguire il polling dello stato solo per i processi creati dallo stesso utente API che li ha creati.

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
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:d9c73f0b6960c71623c8bafe29603b3e8e20fd0e4eeaefd119c0227506ea9be4"
      }
   ]
}
```

Il membro interno `status` indica l&#39;avanzamento del processo e può corrispondere a uno dei valori seguenti: Created, Queued, Processing, Canceled, Completed, Failed. In questo caso il processo è stato completato, quindi è possibile interrompere il polling e continuare per recuperare il file. Una volta completato, il membro `fileSize` indica la lunghezza totale del file in byte e il membro `fileChecksum` contiene l&#39;hash SHA-256 del file. Lo stato del processo è disponibile per 30 giorni dopo il raggiungimento dello stato Completato o Non riuscito.

## Recupero dei dati

Una volta completato il processo, puoi recuperare facilmente il file.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

La risposta contiene un file formattato nel modo in cui è stato configurato il processo. L’endpoint risponde con il contenuto del file. Se un processo non è stato completato o viene passato un ID processo non valido, gli endpoint del file rispondono con uno stato 404 Non trovato e un messaggio di errore di testo normale come payload, a differenza della maggior parte degli altri endpoint REST di Marketo.

Per supportare il recupero parziale e semplice dei dati estratti, l&#39;endpoint del file supporta facoltativamente l&#39;intestazione HTTP `Range` di tipo `bytes` (per [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233)). Se l’intestazione non è impostata, verrà restituito l’intero contenuto. Per recuperare i primi 10.000 byte di un file, devi passare la seguente intestazione come parte della richiesta di GET all&#39;endpoint, a partire dal byte 0:

```
Range: bytes=0-9999
```

Quando recuperi il file parziale, l’endpoint risponde con il codice di stato 206 e restituisce le intestazioni Accept-ranges, Content-Length e Content-Range:

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Recupero parziale e ripresa

I file possono essere recuperati in parte o ripresi successivamente utilizzando l&#39;intestazione `Range`. L&#39;intervallo di un file inizia dal byte 0 e termina con il valore di `fileSize` meno 1. La lunghezza del file viene indicata anche come denominatore nel valore dell&#39;intestazione di risposta `Content-Range` quando si chiama un endpoint Get Export File. Se un recupero non riesce parzialmente, può essere ripreso in un secondo momento. Ad esempio, se tenti di recuperare un file lungo 1000 byte, ma sono stati ricevuti solo i primi 725 byte, il recupero può essere ritentato dal punto di errore chiamando nuovamente l’endpoint e passando un nuovo intervallo:

```
Range: bytes 724-999
```

Restituisce i restanti 275 byte del file.

#### Verifica integrità file

Gli endpoint di stato del processo restituiscono un checksum nell&#39;attributo `fileChecksum` quando `status` è &quot;Completato&quot;. Il checksum è un hash SHA-256 del file esportato. Puoi confrontare il checksum con l’hash SHA-256 del file recuperato per verificare che sia completo.

Di seguito è riportato un esempio di risposta contenente il checksum:

```json
{
    "exportId": "45547609-6732-418a-bb7b-17b0160b2317",
    "format": "CSV",
    "status": "Completed",
    "createdAt": "2019-06-04T23:13:12Z",
    "queuedAt": "2019-06-04T23:14:02Z",
    "startedAt": "2019-06-04T23:15:19Z",
    "finishedAt": "2019-06-04T23:36:40Z",
    "numberOfRecords": 1776,
    "fileSize": 400785,
    "fileChecksum": "sha256:83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6"
}
```

Di seguito è riportato un esempio di creazione dell&#39;hash SHA-256 di un file recuperato denominato &quot;bulk_lead_export.csv&quot; utilizzando l&#39;utility della riga di comando sha256sum:

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Annullamento di un processo

Se un processo non è stato configurato correttamente o diventa superfluo, può essere facilmente annullato:

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
         "format": "CSV",
      }
   ]
}
```

Questo risponde con uno stato che indica che il processo è stato annullato.
