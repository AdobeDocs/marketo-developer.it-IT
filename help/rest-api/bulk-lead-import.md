---
title: Importazione lead in blocco
feature: REST API
description: Importazione batch dei dati del lead.
exl-id: 615f158b-35f9-425a-b568-0a7041262504
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%

---

# Importazione lead in blocco

[Riferimento endpoint importazione lead bulk](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Per grandi quantità di record di lead, è possibile importare i lead in modo asincrono con l&#39;[API bulk](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Questo consente di importare un elenco di record in Marketo utilizzando un file flat con i delimitatori (virgola, tabulazione o punto e virgola). Il file può contenere un numero qualsiasi di record, purché le dimensioni totali del file siano inferiori a 10 MB. L&#39;operazione di registrazione è solo &quot;insert or update&quot; (Inserisci o aggiorna).

## Limiti di elaborazione

È consentito inviare più di una richiesta di importazione in blocco, con limitazioni. Ogni richiesta viene aggiunta come processo a una coda FIFO da elaborare. Vengono elaborati al massimo due processi contemporaneamente. Sono consentiti fino a dieci processi nella coda in qualsiasi momento (compresi i due attualmente in fase di elaborazione). Se si supera il numero massimo di dieci processi, viene restituito un errore &quot;1016, Troppe importazioni&quot;.

## Importa file

La prima riga del file deve essere un’intestazione che elenca i campi API REST corrispondenti in cui mappare i valori di ogni riga. Un file tipico segue questo schema di base:

```
email,firstName,lastName
test@example.com,John,Doe
```

Il campo `externalCompanyId` può essere utilizzato per collegare il record del lead a un record della società. Il campo `externalSalesPersonId` può essere utilizzato per collegare il record del lead a un record del venditore.

La chiamata stessa viene effettuata utilizzando il tipo di contenuto `multipart/form-data`.

Questo tipo di richiesta può essere difficile da implementare, pertanto si consiglia vivamente di utilizzare un’implementazione di libreria esistente.

## Creazione di un processo

Per effettuare una richiesta di importazione in blocco, è necessario impostare l’intestazione del tipo di contenuto su &quot;multipart/form-data&quot; e includere almeno un parametro di file con il contenuto del file e un parametro di formato con il valore &quot;csv&quot;, &quot;tsv&quot; o &quot;ssv&quot; che indica il formato del file.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

FirstName,LastName,Email,Company
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

Questo endpoint utilizza [dati multipart/modulo come tipo di contenuto](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Per risolvere questo problema può essere difficile utilizzare una libreria di supporto HTTP per la lingua desiderata. Un modo semplice per eseguire questa operazione con cURL dalla riga di comando è simile al seguente:

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Dove il file di importazione &quot;lead_data.csv&quot; contiene quanto segue:

```
FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

Facoltativamente, puoi anche includere i parametri `lookupField`, `listId` e `partitionName` nella richiesta. `lookupField` ti consente di selezionare un campo specifico su cui eseguire la deduplicazione, proprio come Lead di sincronizzazione, e il valore predefinito è e-mail. È possibile specificare `id` come `lookupField` per indicare un&#39;operazione &quot;solo aggiornamento&quot;. `listId` consente di selezionare un elenco statico in cui importare l&#39;elenco di lead. In questo modo i lead nell&#39;elenco diventeranno membri di questo elenco statico, oltre a eventuali creazioni o aggiornamenti causati dall&#39;importazione. `partitionName` seleziona una partizione specifica in cui importare. Per ulteriori informazioni, consulta la sezione Workspace e partizioni.

Nella risposta alla chiamata di, osserva che non esiste un elenco di successi o errori come con i lead di sincronizzazione, ma un batchId e un campo di stato per il record nella matrice dei risultati. Questo perché questa API è asincrona e può restituire lo stato In coda, Importazione o Non riuscito. È necessario mantenere il batchId per ottenere lo stato del processo di importazione e per recuperare gli errori e/o gli avvisi al completamento. Il batchId rimane valido per sette giorni.

## Stato processo di polling

È consigliabile eseguire il polling del processo ogni 5-30 secondi, a seconda della latenza richiesta e delle limitazioni delle chiamate API, per visualizzare lo stato del processo di importazione. Puoi farlo con l’API Get Import Lead Status.

```
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

Questa risposta mostra un’importazione completata, ma lo stato può essere uno dei seguenti:

- Completo
- In coda
- Importazione
- Operazione non riuscita

Se il processo è stato completato, viene visualizzato un elenco del numero di righe elaborate, non riuscite, in quelle con avvisi. Il parametro message (Messaggio) può anche inviare il messaggio di errore se lo stato è Failed (Non riuscito).

## Errori

Gli errori sono indicati dall&#39;attributo &quot;numOfRowsFailed&quot; nella risposta Get Import Lead Status. Se &quot;numOfRowsFailed&quot; è maggiore di zero, tale valore indica il numero di errori che si sono verificati.

Per recuperare i record e le cause delle righe non riuscite, è necessario recuperare il file di errore:

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

L’API risponde con un file che indica quali righe non sono riuscite, insieme a un messaggio che indica il motivo per cui il record non è riuscito. Il formato del file è lo stesso specificato nel parametro &quot;format&quot; durante la creazione del processo. A ogni record viene aggiunto un campo aggiuntivo con una descrizione dell’errore.

## Avvisi

Gli avvisi sono indicati dall&#39;attributo &quot;numOfRowsWithWarning&quot; nella risposta Get Import Lead Status. Se &quot;numOfRowsWithWarning&quot; è maggiore di zero, tale valore indica il numero di avvisi che si sono verificati.

Per recuperare i record e le cause delle righe di avviso, recuperare il file di avviso:

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

L’API risponde con un file che indica quali righe hanno prodotto avvisi, insieme a un messaggio che indica il motivo per cui il record non è riuscito. Il formato del file è lo stesso specificato nel parametro &quot;format&quot; durante la creazione del processo. A ciascun record viene aggiunto un campo aggiuntivo con una descrizione dell’avviso.
