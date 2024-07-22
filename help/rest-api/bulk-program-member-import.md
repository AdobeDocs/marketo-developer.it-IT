---
title: Importazione in blocco membri del programma
feature: REST API
description: Importazione in batch dei dati dei membri.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---

# Importazione in blocco membri del programma

[Riferimento all&#39;endpoint per l&#39;importazione di membri del programma in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Per grandi quantità di record di membri del programma, i membri del programma possono essere importati in modo asincrono con l&#39;[API in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Questo consente di importare un elenco di record in Marketo utilizzando un file flat con i delimitatori (virgola, tabulazione o punto e virgola). Il file può contenere un numero qualsiasi di record, purché le dimensioni totali del file siano inferiori a 10 MB. L&#39;operazione di registrazione è solo &quot;insert or update&quot; (Inserisci o aggiorna).

## Limiti di elaborazione

È consentito inviare più di una richiesta di importazione in blocco, con limitazioni. Ogni richiesta viene aggiunta come processo a una coda FIFO da elaborare. Vengono elaborati al massimo due processi contemporaneamente. Sono consentiti fino a dieci processi nella coda in qualsiasi momento (compresi i due attualmente in fase di elaborazione). Se si supera il numero massimo di dieci processi, viene restituito un errore &quot;1016, Troppe importazioni&quot;.

## Importa file

La prima riga del file deve essere un’intestazione che elenca i nomi API REST corrispondenti come campi in cui mappare i valori di ogni riga. I nomi REST API possono essere recuperati utilizzando [Descrivi lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) e/o [Descrivi endpoint membro programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET). I record possono contenere campi lead, campi lead personalizzati e campi membri del programma personalizzati.

Un file tipico segue questo schema di base:

```
email,firstName,lastName
test@example.com,John,Doe
```

La chiamata stessa viene effettuata utilizzando il tipo di contenuto `multipart/form-data`.

Questo tipo di richiesta può essere difficile da implementare, pertanto si consiglia vivamente di utilizzare un’implementazione di libreria esistente.

## Creazione di un processo

L&#39;endpoint [Importa membri del programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) legge un file contenente i record dei membri del programma e li aggiunge a un programma con uno stato specificato. I record possono contenere sia campi lead che campi personalizzati del membro del programma. Tutti i record devono includere il campo e-mail, utilizzato a scopo di deduplicazione.

Il parametro percorso `programId` specifica il programma a cui vengono aggiunti i membri.

Sono disponibili tre parametri di query obbligatori. Il parametro `format` specifica il formato del file di importazione (CSV, TSV o SSV), il parametro `programMemberStatus` specifica lo stato del programma per i membri aggiunti al programma e il parametro `file` contiene il nome del file di importazione che contiene i record dei membri del programma.

```
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```
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

Nella risposta alla nostra chiamata, noterai l&#39;esistenza di un campo `batchId` e un campo `status` per il record nella matrice dei risultati. Poiché questo endpoint è asincrono, può restituire lo stato In coda, Importazione o Non riuscito. È necessario mantenere `batchId` per ottenere lo stato del processo di importazione e recuperare gli errori e/o gli avvisi al completamento. `batchId` rimane valido per sette giorni.

Utilizzando l’esempio precedente, un modo semplice per chiamare l’endpoint è utilizzare cURL dalla riga di comando:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Il file di importazione &quot;Lead-House-Lannister.csv&quot; contiene quanto segue:

```
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

Una volta creato il processo di importazione, è necessario eseguire una query sul relativo stato. È consigliabile eseguire il polling del processo di importazione ogni 5-30 secondi. A tale scopo, passare il parametro del percorso `batchId` all&#39;endpoint [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET).

```
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

Questa risposta mostra un’importazione completata. Lo stato può essere Completato, In coda, Importazione, Non riuscito.

Se il processo è stato completato, viene visualizzato un elenco del numero di righe elaborate, non riuscite o con avvisi. Il parametro message (Messaggio) può anche inviare il messaggio di errore se lo stato è Failed (Non riuscito).

## Errori

Gli errori sono indicati dall&#39;attributo `numOfRowsFailed` nella risposta [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Se numOfRowsFailed è maggiore di zero, tale valore indica il numero di errori che si sono verificati.

Utilizzare l&#39;endpoint [Errori di membri del programma di importazione](http://TODO) per recuperare i record e le cause delle righe non riuscite passando il parametro di percorso `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

L&#39;endpoint risponde con un file che indica quali righe non sono riuscite, insieme a un messaggio che indica il motivo per cui il record non è riuscito. Il formato del file è uguale a quello specificato nel parametro `format` durante la creazione del processo. A ogni record viene aggiunto un campo aggiuntivo con una descrizione dell’errore.

Ad esempio, supponiamo di importare il seguente file con un punteggio di lead non valido:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Quando si controlla lo stato del processo, viene visualizzato il valore 1 di `numOfRowsFailed` che indica che si è verificato un errore:

```
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

Recuperare quindi il file degli errori per ulteriori dettagli sull&#39;errore:

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Avvisi

Gli avvisi sono indicati dall&#39;attributo `numOfRowsWithWarning` nella risposta [Get Import Program Member Status](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET). Se `numOfRowsWithWarning` è maggiore di zero, il valore indica il numero di avvisi che si sono verificati.

Utilizza l&#39;endpoint [Get Import Program Member Warnings](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) per recuperare i record e le cause delle righe di avviso passando il parametro di percorso `batchId`.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

L&#39;endpoint risponde con un file che indica quali righe hanno generato avvisi, insieme a un messaggio che indica il motivo per cui il record ha generato un avviso. Il formato del file è uguale a quello specificato nel parametro `format` durante la creazione del processo. A ciascun record viene aggiunto un campo aggiuntivo con una descrizione dell’avviso.

Ad esempio, supponiamo di importare il seguente file con un indirizzo e-mail non valido:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Quando si controlla lo stato del processo, viene visualizzato il valore 1 di `numOfRowsWithWarning` che indica che si è verificato un avviso:

```
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

Recuperate quindi il file delle avvertenze per ulteriori dettagli sull&#39;avvertenza:

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
