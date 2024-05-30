---
title: "Importazione in blocco"
feature: REST API
description: "Importazione in batch dei dati personali."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 1%

---


# Importazione in blocco

Marketo fornisce interfacce per l’inserimento di grandi set di dati relativi a persone e persone, denominati Importazione in blocco. Attualmente, le interfacce sono disponibili per tre tipi di oggetto:

- Lead (persone)
- Oggetti personalizzati
- Membri del programma

L’importazione in blocco viene eseguita creando un processo e attendendone il completamento durante la lettura di un file. Questi processi vengono eseguiti in modo asincrono e possono essere sottoposti a polling per recuperare lo stato dell’importazione. I file vengono caricati utilizzando dati modulo/multipart HTTP secondo RFC 2399.

Gli endpoint API in blocco non hanno il prefisso &quot;/rest&quot;, come gli altri endpoint.

## Autenticazione

Le API per l’importazione in blocco utilizzano lo stesso metodo di autenticazione OAuth 2.0 delle altre API REST di Marketo.  È necessario incorporare un token di accesso valido come parametro della stringa query `access_token={_AccessToken_}`, o come intestazione HTTP `Authorization: Bearer {_AccessToken_}`.

## Limiti

- Max processi di importazione simultanei: 2
- Numero massimo di processi di importazione in coda (inclusi i processi di importazione in corso): 10
- Dimensione massima del file di importazione: 10 MB

## Autorizzazioni

L’importazione in blocco utilizza lo stesso modello di autorizzazioni dell’API REST di Marketo e non richiede alcuna autorizzazione speciale aggiuntiva per utilizzare, anche se sono necessarie autorizzazioni specifiche per ogni set di endpoint.

## Operazioni record

L&#39;importazione in blocco è un&#39;operazione di inserimento o aggiornamento di record. Se nel database viene trovato un record corrispondente, questo viene aggiornato. In caso contrario, viene creato un nuovo record. La risposta dell’importazione in blocco non indica se un determinato record è stato aggiornato o inserito.

## Creazione di un processo

Le API di importazione in blocco di Marketo utilizzano il concetto di processo per l’esecuzione dell’importazione di dati. Vediamo come creare un semplice processo di importazione di lead utilizzando [Importa lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) endpoint.  Questo endpoint utilizza [multipart/form-data come tipo di contenuto](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Per risolvere questo problema può essere difficile utilizzare una libreria di supporto HTTP per la lingua desiderata.  Se si sta solo bagnando i piedi, si consiglia di utilizzare [ricciolo](https://curl.se/).

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Questa richiesta creerà un processo che importerà i valori contenuti nel file CSV denominato &quot;lead.csv&quot; con le intestazioni di colonna &quot;FirstName&quot;, &quot;LastName&quot;, &quot;Email&quot;, &quot;Company&quot;.

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

Quando inviamo il processo, questo restituirà un batchId che possiamo usare per controllarne lo stato.

### Parametri comuni

Ogni endpoint per la creazione di processi condivide alcuni parametri comuni per la configurazione del formato di file, dei nomi dei campi e del filtro di un processo di estrazione in blocco.  Ogni sottotipo di processo di estrazione può avere parametri aggiuntivi:

| Parametro | Tipo di dati | Note |
|---|---|---|
| formato | Stringa | Determina il formato di file dei dati importati con le opzioni per i valori separati da virgola, da tabulazione e da punto e virgola. Accetta uno di: CSV, SSV, TSV. Il formato predefinito è CSV. |
| file | Stringa | I dati vengono specificati tramite i dati modulo multipart nel file. |


## Stato processo di polling

Determinare lo stato del processo è semplice utilizzando [Ottieni stato lead di importazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET) endpoint.

```
GET /bulk/v1/leads/batch/{batchId}.json
```

```json
{
    "requestId": "1f63#15d6738fd15",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Complete",
            "numOfLeadsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Interno `status` Il membro indica l&#39;avanzamento del processo e può corrispondere a uno dei valori seguenti: In coda, Importazione, Completo, Non riuscito. In questo caso il nostro lavoro è stato completato, quindi possiamo interrompere il sondaggio.

## Errori

Gli errori sono indicati da `numOfRowsFailed` nella risposta Get Import Lead Status. Se `numOfRowsFailed` è maggiore di zero, quindi tale valore indica il numero di errori che si sono verificati.

Per recuperare i record e le cause delle righe non riuscite, è necessario recuperare il file di errore utilizzando [Recupera errori lead di importazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET) endpoint.

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Il file indica quali righe non sono riuscite, insieme a un messaggio che indica il motivo per cui il record non è riuscito.
