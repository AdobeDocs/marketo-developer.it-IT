---
title: Importazione lead in blocco
feature: REST API
description: Creazione e monitoraggio delle importazioni asincrone di lead in blocco in Marketo con file CSV TSV o SSV.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
TQID: https://experienceleague.adobe.com/UamXYWis5J1ERqnp5lAnfUf3pFcgfSOLfKRXRB-Yg4I
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 623
ht-degree: 0%

---

# Importazione lead in blocco

[Riferimento endpoint importazione lead bulk](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads)

Utilizza l&#39;[API bulk](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) per importare un numero elevato di record di lead in modo asincrono. Fornisci i record in un file flat delimitato da virgole, tabulazioni o punti e virgola di dimensioni inferiori a 10 MB.

L&#39;importazione di lead in blocco supporta solo l&#39;operazione di inserimento o aggiornamento dei record.

## Limiti di elaborazione

Ogni richiesta di importazione in blocco viene aggiunta come processo a una coda FIFO (First-In, First-Out). Si applicano i seguenti limiti:

- È possibile elaborare contemporaneamente un massimo di due processi.
- Nella coda possono essere presenti al massimo 10 processi, inclusi i due in fase di elaborazione.

Se si supera il massimo di 10 processi, l&#39;API restituisce un errore `1016, Too many imports`.

## Importa file

La prima riga del file deve essere un’intestazione che elenca i campi REST API a cui corrispondono i valori in ogni riga. Un file tipico segue questo schema:

```csv
email,firstName,lastName
test@example.com,John,Doe
```

Utilizzare `externalCompanyId` per collegare un record lead a un record società. Utilizzare `externalSalesPersonId` per collegare un record lead a un record venditore.

Invia la richiesta utilizzando il tipo di contenuto `multipart/form-data`. Utilizza un’implementazione di libreria esistente per creare la richiesta multipart.

## Creazione di un processo

Per creare un processo di importazione in blocco, impostare il tipo di contenuto su `multipart/form-data` e includere i seguenti parametri:

- `file`: contenuto del file di importazione.
- `format`: formato del file. I valori validi sono `csv`, `tsv` e `ssv`.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Questo endpoint utilizza [dati multipart/modulo come tipo di contenuto](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Utilizza una libreria di supporto HTTP per la lingua preferita per creare correttamente la richiesta. Nell&#39;esempio seguente viene utilizzato cURL dalla riga di comando:

```bash
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

In questo esempio, il file di importazione `lead_data.csv` contiene i dati seguenti:

```text
firstName,lastName,email,company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

È inoltre possibile includere i seguenti parametri facoltativi:

- `lookupField`: seleziona il campo utilizzato per la deduplicazione e imposta il valore predefinito su `email`. Specificare `id` per eseguire un&#39;operazione di solo aggiornamento.
- `listId`: seleziona un elenco statico. I lead importati diventano membri di questo elenco, oltre a tutti i record creati o aggiornati dall&#39;importazione.
- `partitionName`: seleziona la partizione in cui importare. Per ulteriori informazioni, consulta la sezione Workspace e partizioni.

Poiché l&#39;API è asincrona, la risposta contiene `batchId` e `status` campi anziché singoli errori e operazioni riuscite. Lo stato può essere `Queued`, `Importing` o `Failed`.

Mantieni `batchId` per controllare lo stato del processo e recuperare gli errori o gli avvisi dopo il completamento. `batchId` rimane valido per sette giorni.

## Stato processo di polling

Utilizza l’API Get Import Lead Status per eseguire il polling del processo ogni 5-30 secondi, a seconda dei requisiti di latenza e delle limitazioni di chiamata API.

```http
GET /bulk/v1/leads/batch/{id}.json
```

```json
{
   "requestId":"8136#146daebc2ed",
   "success":true,
   "result":[
      {
         "batchId":1022,
         "status":"Complete",
         "numOfLeadsProcessed":2,
         "numOfRowsFailed":1,
         "numOfRowsWithWarning":0,
         "message":"Import completed with errors, 2 records imported (2 members), 1 failed"
      }
   ]
}
```

Questa risposta mostra un’importazione completata. Lo stato può corrispondere a uno dei seguenti valori:

- Completa
- In coda
- Importazione
- Operazione non riuscita

Al termine del processo, la risposta elenca il numero di righe elaborate, non riuscite ed elaborate con avvertenze. Il parametro `message` può anche fornire un messaggio di errore quando lo stato è `Failed`.

## Errori

L&#39;attributo `numOfRowsFailed` nella risposta Get Import Lead Status indica il numero di righe non riuscite. Un valore maggiore di zero indica che si sono verificati errori.

Per recuperare i record non riusciti e le relative cause, richiedere il file di errore:

```http
GET /bulk/v1/leads/batch/{id}/failures.json
```

L’API restituisce un file che identifica ogni riga con errori e spiega perché il record non è riuscito. Il file utilizza il formato specificato dal parametro `format` durante la creazione del processo. Un campo aggiuntivo in ciascun record descrive l’errore.

## Avvisi

L&#39;attributo `numOfRowsWithWarning` nella risposta Ottieni stato lead importazione indica il numero di righe con avvisi. Un valore maggiore di zero indica che si sono verificate delle avvertenze.

Per recuperare i record interessati e le relative cause, richiedere il file di avviso:

```http
GET /bulk/v1/leads/batch/{id}/warnings.json
```

L’API restituisce un file che identifica ogni riga con un avviso e spiega perché si è verificato l’avviso. Il file utilizza il formato specificato dal parametro `format` durante la creazione del processo. Un campo aggiuntivo in ciascun record descrive l’avviso.
