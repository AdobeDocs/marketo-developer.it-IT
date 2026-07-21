---
title: Importazione in blocco
feature: REST API
description: Importazione in blocco Marketo per il caricamento di lead, oggetti personalizzati e membri di programmi tramite caricamenti in più parti, la creazione di processi asincroni, lo stato di polling e la gestione di errori.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
TQID: https://experienceleague.adobe.com/lr9dyX-fY-oJ2LM5P0zE1m24HtFYKQYYbxMkVe--PkE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 538
ht-degree: 2%

---

# Importazione in blocco

Importazione in blocco fornisce interfacce per l&#39;inserimento di grandi set di dati relativi a persone e persone. È possibile importare tre tipi di oggetto:

- Lead (persone)
- Oggetti personalizzati
- Membri del programma

Per eseguire un’importazione in blocco, crea un processo che legga un file caricato. Il processo viene eseguito in modo asincrono, quindi esegui il polling per recuperare lo stato di importazione.

Carica i file utilizzando HTTP `multipart/form-data` per RFC 2399.

A differenza di altri endpoint, gli endpoint API in blocco non hanno il prefisso `/rest`.

## Autenticazione

Le API per l’importazione in blocco utilizzano lo stesso metodo di autenticazione OAuth 2.0 delle altre API REST di Marketo. Invia un token di accesso valido nell&#39;intestazione HTTP `Authorization: Bearer {_AccessToken_}`.

>[!IMPORTANT]
>
>Il supporto per l&#39;autenticazione tramite il parametro di query **access_token** verrà rimosso il 30 giugno 2025. Se il progetto utilizza un parametro di query per passare il token di accesso, deve essere aggiornato per utilizzare l&#39;intestazione **Authorization** il prima possibile. Il nuovo sviluppo deve utilizzare esclusivamente l&#39;intestazione **Authorization**.

## Limiti

- Numero massimo processi di importazione simultanei: 2
- Numero massimo di processi di importazione in coda, inclusi i processi in fase di importazione: 10
- Dimensione massima file di importazione: 10 MB

## Autorizzazioni

L’importazione in blocco utilizza lo stesso modello di autorizzazioni dell’API REST di Marketo. Non richiede autorizzazioni aggiuntive, ma ogni set di endpoint richiede autorizzazioni specifiche.

## Operazioni record

L&#39;importazione in blocco è un&#39;operazione di inserimento o aggiornamento di record. Se il database contiene un record corrispondente, l&#39;operazione lo aggiorna. In caso contrario, viene creato un record.

La risposta relativa all’importazione in blocco non indica se è stato aggiornato o inserito un singolo record.

## Creazione di un processo

Crea un processo di importazione lead chiamando l&#39;endpoint [Import Leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Questo endpoint utilizza [dati multipart/modulo come tipo di contenuto](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html).

Utilizza una libreria di supporto HTTP per la lingua preferita per creare la richiesta multipart. Puoi anche utilizzare [curl](https://curl.se/) per iniziare.

```http
POST /bulk/v1/leads.json?format=csv
```

```text
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```text
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Questa richiesta crea un processo che importa valori dal file CSV denominato `leads.csv`.

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

La risposta restituisce un `batchId`. Utilizzare questo valore per controllare lo stato del processo.

### Parametri comuni

Ogni endpoint di creazione del processo condivide i parametri per la configurazione del file di importazione. Un sottotipo di importazione può inoltre supportare parametri aggiuntivi.

| Parametro | Tipo di dati | Note |
| --- | --- | --- |
| formato | Stringa | Determina il formato di file dei dati importati con le opzioni per i valori separati da virgola, da tabulazione e da punto e virgola. Accetta uno di: CSV, SSV, TSV. Il formato predefinito è CSV. |
| file | Stringa | I dati vengono specificati tramite i dati modulo multipart nel file. |

## Stato processo di polling

Passa `batchId` all&#39;endpoint [Get Import Lead Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET) per recuperare lo stato del processo.

```http
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

Il membro `status` indica l&#39;avanzamento del processo. Il valore può essere `Queued`, `Importing`, `Complete` o `Failed`.

In questo esempio, il processo è completo, quindi il polling può essere interrotto.

## Errori

L&#39;attributo `numOfRowsFailed` nella risposta Get Import Lead Status indica il numero di righe non riuscite. Un valore maggiore di zero indica che si sono verificati errori.

Per recuperare i record non riusciti e le relative cause, utilizzare l&#39;endpoint [Get Import Lead Failures](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET).

```http
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Il file di errore identifica ogni riga non riuscita e spiega il motivo per cui il record non è riuscito.
