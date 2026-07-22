---
title: Importazione in blocco membri del programma
feature: REST API
description: Scopri come importare i membri del programma in blocco tramite API REST di Marketo utilizzando file CSV TSV o SSV di dimensioni inferiori a 10 MB, limiti di coda, parametri richiesti e stato del processo di polling.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
TQID: https://experienceleague.adobe.com/T1PAzLN1mnp38kJ0jwh6kPv6r1Uvxc7-o9zeTHetIV0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 771
ht-degree: 0%

---

# Importazione in blocco membri del programma

[Riferimento endpoint importazione membri programma in blocco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members)

Utilizza l&#39;[API in blocco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members) per importare un numero elevato di record dei membri del programma in modo asincrono. Fornisci i record in un file flat delimitato da virgole, tabulazioni o punti e virgola di dimensioni inferiori a 10 MB.

L&#39;importazione in blocco dei membri del programma supporta solo l&#39;operazione di inserimento o aggiornamento dei record.

## Limiti di elaborazione

Ogni richiesta di importazione in blocco viene aggiunta come processo a una coda FIFO (First-In, First-Out). Si applicano i seguenti limiti:

- È possibile elaborare contemporaneamente un massimo di due processi.
- Nella coda possono essere presenti al massimo 10 processi, inclusi i due in fase di elaborazione.

Se si supera il massimo di 10 processi, l&#39;API restituisce un errore `1016, Too many imports`.

## Importa file

La prima riga del file deve essere un’intestazione che elenca i nomi dei campi API REST a cui corrispondono i valori in ogni riga. Recuperare questi nomi utilizzando gli endpoint [Descrivi lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) e [Descrivi membro del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeProgramMemberUsingGET).

I record possono contenere campi lead, campi lead personalizzati e campi membri del programma personalizzati.

Un file tipico segue questo schema:

```text
email,firstName,lastName
test@example.com,John,Doe
```

Invia la richiesta utilizzando il tipo di contenuto `multipart/form-data`. Utilizza un’implementazione di libreria esistente per creare la richiesta multipart.

## Creazione di un processo

L&#39;endpoint [Importa membri del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) legge i record dei membri del programma da un file e li aggiunge a un programma con uno stato specificato. I record possono contenere campi lead e campi membri del programma personalizzati.

Ogni record deve includere il campo e-mail, utilizzato per la deduplicazione.

Il parametro percorso `programId` specifica il programma a cui vengono aggiunti i membri.

La richiesta richiede tre parametri di query:

- `format`: formato del file di importazione (`CSV`, `TSV` o `SSV`).
- `programMemberStatus`: lo stato del programma assegnato ai membri importati.
- `file`: nome del file contenente i record dei membri del programma.

```http
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```text
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```text
----------------------------118046853683028616211319
Content-Disposition: form-data; name="file"; filename="Lead-House-Lannister.csv"
Content-Type: text/csv

firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0

----------------------------118046853683028616211319--
```

```json
{
    "requestId": "17f4a#16f87f87325",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Poiché l&#39;endpoint è asincrono, la risposta contiene `batchId` e `status` campi. Lo stato può essere `Queued`, `Importing` o `Failed`.

Mantieni `batchId` per controllare lo stato di importazione e recuperare gli errori o gli avvisi dopo il completamento. `batchId` rimane valido per sette giorni.

La seguente richiesta cURL della riga di comando invia il processo di esempio:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

In questo esempio, il file di importazione `Lead-House-Lannister.csv` contiene i dati seguenti:

```text
firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0
```

## Stato processo di polling

Dopo aver creato il processo di importazione, esegui il polling ogni 5-30 secondi. Passa il parametro del percorso `batchId` all&#39;endpoint [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "e0cb#16f87f8b177",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Complete",
            "numOfLeadsProcessed": 8,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 8 records imported (8 members)"
        }
    ],
    "success": true
}
```

Questa risposta mostra un’importazione completata. Lo stato può essere `Complete`, `Queued`, `Importing` o `Failed`.

Al termine del processo, la risposta elenca il numero di righe elaborate, non riuscite ed elaborate con avvertenze. Il parametro `message` può anche fornire un messaggio di errore quando lo stato è `Failed`.

## Errori

L&#39;attributo `numOfRowsFailed` nella risposta [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indica il numero di righe non riuscite. Un valore maggiore di zero indica che si sono verificati errori.

Passa il parametro del percorso `batchId` all&#39;endpoint Get Import Program Member Failures per recuperare i record non riusciti e le relative cause.

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

L’endpoint restituisce un file che identifica ogni riga con errori e spiega perché il record non è riuscito. Il file utilizza il formato specificato dal parametro `format` durante la creazione del processo. Un campo aggiuntivo in ciascun record descrive l’errore.

Ad esempio, supponiamo di importare il seguente file con un punteggio di lead non valido:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Lo stato del processo restituisce `numOfRowsFailed` come 1, a indicare che si è verificato un errore:

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "4c2d#16f8b32c8ef",
    "result": [
        {
            "batchId": 1046,
            "importId": "1046",
            "status": "Complete",
            "numOfLeadsProcessed": 0,
            "numOfRowsFailed": 1,
            "numOfRowsWithWarning": 0,
            "message": "Import completed with errors, 0 records imported (0 members), 1 failed"
        }
    ],
    "success": true
}
```

Recuperate il file di errore per ulteriori informazioni:

```http
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Avvisi

L&#39;attributo `numOfRowsWithWarning` nella risposta [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) indica il numero di righe con avvisi. Un valore maggiore di zero indica che si sono verificate delle avvertenze.

Passa il parametro del percorso `batchId` all&#39;endpoint [Get Import Program Member Warnings](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) per recuperare i record interessati e le relative cause.

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

L’endpoint restituisce un file che identifica ogni riga con un avviso e spiega perché si è verificato l’avviso. Il file utilizza il formato specificato dal parametro `format` durante la creazione del processo. Un campo aggiuntivo in ciascun record descrive l’avviso.

Ad esempio, supponiamo di importare il seguente file con un indirizzo e-mail non valido:

```text
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Lo stato del processo restituisce `numOfRowsWithWarning` come 1, a indicare che si è verificato un avviso:

```http
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
   "requestId":"4ca1#16f883c2003",
   "result":[
      {
         "batchId":1041,
         "importId":"1041",
         "status":"Complete",
         "numOfLeadsProcessed":1,
         "numOfRowsFailed":0,
         "numOfRowsWithWarning":1,
         "message":"Import succeeded, 1 records imported (1 members), 1 warning."
      }
   ],
   "success":true
}
```

Recuperare il file di avviso per ulteriori informazioni:

```http
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```text
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
