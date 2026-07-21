---
title: Estrai in blocco
feature: REST API
description: Scopri come utilizzare l’API REST di Marketo Bulk Extract per esportare lead, attività, membri del programma e oggetti personalizzati, con OAuth, code di processi e limiti giornalieri di 500 MB.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
TQID: https://experienceleague.adobe.com/ECSchsjqp8fyxXbUGl5DgXHUkXuN0sIUc3yJfVaIe1E
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1549
ht-degree: 0%

---

# Estrai in blocco

Marketo Bulk Extract fornisce interfacce per il recupero di grandi set di dati relativi a persone e persone. Le interfacce sono attualmente disponibili per quattro tipi di oggetto:

- Lead (persone)
- Attività
- Membri del programma
- Oggetti personalizzati

Per eseguire un&#39;estrazione in blocco:

1. Crea un processo e definisci i dati da recuperare.
1. Accoda il processo.
1. Attendere che il processo finisca di scrivere il file.
1. Recupera il file tramite HTTP.

I processi di estrazione in blocco vengono eseguiti in modo asincrono. Eseguire il polling del processo per recuperare lo stato di esportazione.

`Note:` endpoint API in blocco non hanno il prefisso &#39;/rest&#39; come gli altri endpoint.

## Autenticazione

Le API di estrazione in blocco utilizzano lo stesso metodo di autenticazione OAuth 2.0 delle altre API REST di Marketo. Invia un token di accesso valido nell&#39;intestazione HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>Il supporto per l&#39;autenticazione tramite il parametro di query **access_token** verrà rimosso il 31 agosto 2026. Se il progetto utilizza un parametro di query per passare il token di accesso, deve essere aggiornato per utilizzare l&#39;intestazione **Authorization** il prima possibile. Il nuovo sviluppo deve utilizzare esclusivamente l&#39;intestazione **Authorization**.

## Limiti

- Numero massimo processi di esportazione simultanei: 2
- Numero massimo di processi di esportazione in coda, inclusi processi attualmente in fase di esportazione: 10
- Periodo di conservazione dei file: sette giorni
- Allocazione predefinita delle esportazioni giornaliere: 500 MB. L’allocazione viene ripristinata ogni giorno alle 00:00 CST. Sono disponibili aumenti per l’acquisto.
- Intervallo di tempo massimo per il filtro intervallo di date (`createdAt` o `updatedAt`): 31 giorni

I filtri Estrazione lead bulk per UpdatedAt e Smart List non sono disponibili per alcuni tipi di abbonamento. Se questi filtri non sono disponibili, l’endpoint Create Export Lead Job restituisce l’errore &quot;1035, Unsupported filter type for target subscription&quot;. Contatta il supporto Marketo per abilitare questa funzionalità per il tuo abbonamento.

### Coda

Le API di estrazione in blocco utilizzano una coda di processi condivisa tra lead, attività, membri del programma e oggetti personalizzati. Innanzitutto, chiama un endpoint Crea lead/attività di esportazione/processo membro del programma per creare un processo di estrazione. Quindi, chiamare l&#39;endpoint del processo membro attività/lead esportazione accodamento corrispondente per accodare il processo. Il processo inizia quando le risorse di calcolo diventano disponibili.

La coda può contenere un massimo di 10 processi. Se si tenta di accodare un processo quando la coda è piena, l&#39;endpoint del processo di esportazione accodamento restituisce l&#39;errore &quot;1029, Troppi processi in coda&quot;. Un massimo di due processi può avere lo stato &quot;Elaborazione&quot; ed essere eseguito contemporaneamente.

### Dimensione file

Le API di estrazione in blocco vengono misurate in base alla dimensione su disco dei dati recuperati da un processo di estrazione in blocco. Per determinare la dimensione del file in byte, leggere l&#39;attributo `fileSize` nella risposta di stato completato per un processo di esportazione.

La quota giornaliera è di 500 MB ed è condivisa tra lead, attività, membri del programma e oggetti personalizzati. Quando si supera la quota, non è possibile creare o accodare un altro processo finché la quota non viene ripristinata alla mezzanotte [ora centrale](https://en.wikipedia.org/wiki/Central_Time_Zone). Fino a quando non viene ripristinata, l’API restituisce l’errore &quot;1029, quota giornaliera di esportazione superata&quot;. A parte la quota giornaliera, non esiste una dimensione massima per il file.

Dopo essere stato messo in coda o elaborato, un processo viene eseguito fino al completamento, a meno che non si verifichi un errore o si annulli il processo. Se un processo non riesce, è necessario ricrearlo.

L’API scrive il file completo solo quando il processo raggiunge lo stato completato. Non scrive file parziali. Per verificare il file, calcolarne l’hash SHA-256 e confrontarlo con il checksum restituito dall’endpoint di stato del processo.

Per determinare lo spazio totale su disco utilizzato per il giorno corrente, chiamare un endpoint per ottenere il lead/attività di esportazione/processi membri del programma. Questi endpoint restituiscono tutti i processi degli ultimi sette giorni.

Filtra l&#39;elenco in base ai processi completati nel giorno corrente utilizzando gli attributi `status` e `finishedAt`. Quindi, aggiungere le dimensioni file per tali processi. Impossibile eliminare un file per recuperare spazio su disco.

## Autorizzazioni

L’estrazione in blocco utilizza lo stesso modello di autorizzazioni dell’API REST di Marketo. Non richiede autorizzazioni speciali aggiuntive, ma ogni set di endpoint richiede autorizzazioni specifiche.

Solo l’utente API che ha creato un processo di estrazione in blocco può accedervi, esaminarne lo stato o recuperarne il contenuto.

Gli endpoint di estrazione in blocco non riconoscono le aree di lavoro di Marketo. Le richieste di estrazione includono dati da tutte le aree di lavoro, indipendentemente da come definisci l’API Solo utente per il servizio personalizzato.

## Creazione di un processo

Le API di estrazione in blocco di Marketo utilizzano i processi per avviare ed eseguire estrazioni di dati. La richiesta seguente crea un processo di esportazione lead:

```http
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

Questa richiesta crea un processo che esporta ogni lead creato tra il 1° gennaio 2023 e il 31 gennaio 2023. Il file CSV contiene i valori dei campi &quot;firstName&quot; e &quot;lastName&quot; e utilizza le intestazioni di colonna &quot;First Name&quot; e &quot;Last Name&quot;.

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

La risposta restituisce l&#39;ID del processo nell&#39;attributo `exportId`. Utilizzare questo ID processo per accodare o annullare il processo, verificarne lo stato o recuperare il file completato.

### Parametri comuni

Ogni endpoint per la creazione di processi dispone di parametri comuni per la configurazione del formato di file, dei nomi dei campi e del filtro. Ciascun sottotipo di processo di estrazione può avere anche parametri aggiuntivi:

| Parametro | Tipo di dati | Note |
| --- | --- | --- |
| formato | Stringa | Determina il formato di file dei dati estratti con le opzioni per i valori separati da virgola, da tabulazione e da punto e virgola. Accetta uno di: CSV, SSV, TSV. Il formato predefinito è CSV. |
| columnHeaderNames | Oggetto | Consente di impostare i nomi delle intestazioni di colonna nel file restituito. Ogni chiave membro è il nome dell&#39;intestazione di colonna da rinominare e il valore è il nuovo nome dell&#39;intestazione di colonna. Ad esempio, &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filter | Oggetto | Filtro applicato al processo di estrazione. I tipi e le opzioni variano a seconda del tipo di processo. |

## Recupero processi

Utilizzare l&#39;endpoint Get Export Jobs per il tipo di oggetto corrispondente per recuperare i processi recenti. Ogni endpoint Get Export Jobs supporta i seguenti parametri:

- `status` filtra i processi per stato di esportazione. I valori validi sono Creato, In coda, Elaborazione, Annullato, Completato e Non riuscito.
- `batchSize` limita il numero di processi restituiti. Il valore predefinito e massimo è 300.
- `nextPageToken` pagine attraverso set di risultati di grandi dimensioni.

La richiesta seguente recupera i processi di esportazione lead con stato Completato o Non riuscito:

```http
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

La matrice dei risultati contiene la risposta di stato per ogni processo creato per il tipo di oggetto negli ultimi sette giorni. La risposta include solo i processi di cui è proprietario l’utente API che effettua la chiamata.

## Avvio di un processo

Dopo aver creato un processo, utilizzane l’ID per accodarlo e avviarlo:

```http
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

La richiesta avvia il processo e restituisce una risposta di stato. Poiché le esportazioni vengono eseguite in modo asincrono, eseguire il polling dello stato del processo per determinare quando l&#39;esportazione è completa.

## Stato processo di polling

Eseguire il polling dell&#39;endpoint di stato per determinare l&#39;avanzamento di un processo. Solo l’utente API che ha creato un processo può effettuare il polling del relativo stato.

Lo stato di un processo non viene aggiornato più di una volta ogni 60 secondi. Non eseguire sondaggi con una frequenza superiore a quella indicata. Per la maggior parte dei casi d’uso è sufficiente eseguire il polling una volta ogni 5 minuti. I dati di ciascuna esportazione corretta vengono conservati per 10 giorni.

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

Il membro interno `status` indica l&#39;avanzamento del processo. Il relativo valore può essere Creato, In coda, Elaborazione, Annullato, Completato o Non riuscito.

In questo esempio, il processo è completo, quindi puoi interrompere il polling e recuperare il file. Per un processo completato, il membro `fileSize` indica la lunghezza totale del file in byte e il membro `fileChecksum` contiene l&#39;hash SHA-256 del file. Lo stato del processo è disponibile per 30 giorni dopo che il processo ha raggiunto lo stato Completato o Non riuscito.

## Recupero dei dati

Al termine del processo, recupera il file esportato:

```http
GET /bulk/v1/leads/export/{exportId}/file.json
```

La risposta contiene il file nel formato configurato per il processo. Se il processo è incompleto o la richiesta contiene un ID di processo non valido, l’endpoint del file restituisce lo stato 404 Non trovato e un messaggio di errore di testo normale. Questa risposta è diversa dalla maggior parte delle altre risposte dell’endpoint REST di Marketo.

Per supportare il recupero parziale e ripristinabile, l&#39;endpoint del file supporta l&#39;intestazione HTTP `Range` facoltativa con il tipo `bytes`, come definito in [RFC 7233](https://datatracker.ietf.org/doc/html/rfc7233). Se non si imposta l&#39;intestazione, l&#39;endpoint restituisce l&#39;intero file.

Per recuperare i primi 10.000 byte di un file, passa la seguente intestazione nella richiesta GET. L&#39;intervallo inizia dal byte 0:

```text
Range: bytes=0-9999
```

Per un file parziale, l’endpoint restituisce il codice di stato 206 e le intestazioni Accept-ranges, Content-Length e Content-Range:

```text
Accept-Ranges: bytes
Content-Length: 10000
Content-Range: bytes 0-9999/123424
```

### Recupero parziale e ripresa

Utilizzare l&#39;intestazione `Range` per recuperare parte di un file o riprendere un recupero. L&#39;intervallo di file inizia dal byte 0 e termina con il valore di `fileSize` meno 1. L&#39;endpoint Get Export File riporta inoltre la lunghezza del file come denominatore nell&#39;intestazione di risposta `Content-Range`.

Se un recupero non riesce parzialmente, potete riattivarlo. Ad esempio, se tenti di recuperare un file da 1000 byte ma ricevi solo i primi 725 byte, chiama nuovamente l’endpoint e passa un nuovo intervallo:

```text
Range: bytes=725-999
```

Questa richiesta restituisce i restanti 275 byte del file.

#### Verifica integrità file

Quando `status` è &quot;Completato&quot;, gli endpoint di stato del processo restituiscono un checksum nell&#39;attributo `fileChecksum`. Il checksum è l’hash SHA-256 del file esportato. Confrontalo con l’hash SHA-256 del file recuperato per verificare che il file sia completo.

La seguente risposta contiene un checksum:

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

Nell&#39;esempio seguente viene utilizzata l&#39;utilità della riga di comando sha256sum per creare l&#39;hash SHA-256 di un file recuperato denominato &quot;bulk_lead_export.csv&quot;:

```bash
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Annullamento di un processo

Se un processo non è configurato correttamente o non è più necessario, annullarlo:

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
         "format": "CSV",
      }
   ]
}
```

Lo stato della risposta indica che il processo è stato annullato.
