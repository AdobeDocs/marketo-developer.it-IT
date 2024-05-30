---
title: "Importazione di oggetti personalizzati in blocco"
feature: Custom Objects
description: "Importazione in batch di oggetti personalizzati."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 0%

---


# Importazione di oggetti personalizzati in blocco

[Riferimento dell&#39;endpoint di importazione oggetti personalizzati in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects)

Quando disponi di molti record di oggetti personalizzati da importare, è consigliabile importarli in modo asincrono utilizzando l’API in blocco. Questa operazione viene eseguita importando un file flat contenente record delimitati (virgola, tabulazione o punto e virgola). Il file può contenere un numero qualsiasi di record, purché la dimensione sia inferiore a 10 MB (altrimenti viene restituito un codice di stato HTTP 413). Il contenuto del file dipende dalla definizione dell’oggetto personalizzato. La prima riga contiene sempre un’intestazione che elenca i campi in cui mappare i valori di ogni riga. Tutti i nomi dei campi nell’intestazione devono corrispondere a un nome API (come descritto di seguito). Le righe rimanenti contengono i dati da importare, un record per riga. L&#39;operazione di registrazione è solo &quot;insert or update&quot; (Inserisci o aggiorna).

## Limiti di elaborazione

È possibile inviare più richieste di importazione in blocco, entro i limiti consentiti. Ogni richiesta viene aggiunta come processo a una coda FIFO da elaborare. Vengono elaborati al massimo due processi contemporaneamente. Sono consentiti fino a dieci processi nella coda in qualsiasi momento (compresi i due attualmente in fase di elaborazione). Se si supera il numero massimo di dieci processi, viene restituito un errore &quot;1016, Troppe importazioni&quot;.

## Esempio di oggetto personalizzato

Prima di utilizzare l’API in blocco, è necessario utilizzare l’interfaccia utente di amministrazione di Marketo per [creare l&#39;oggetto personalizzato](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects). Ad esempio, supponiamo di aver creato un oggetto personalizzato &quot;Car&quot; con i campi &quot;Color&quot;, &quot;Make&quot;, &quot;Model&quot; e &quot;VIN&quot;. Di seguito sono riportate le schermate dell’interfaccia utente di amministrazione che mostrano l’oggetto personalizzato. Potete vedere che abbiamo usato il campo VIN per la deduplicazione. I nomi API vengono evidenziati perché devono essere utilizzati quando si chiamano endpoint correlati a API in blocco.

![Inserisci oggetto personalizzato](assets/bulk-insert-co-car-1.png)

Di seguito sono riportati i campi oggetto personalizzati come presentati nell’interfaccia utente di amministrazione.

![Inserisci campi oggetto personalizzati](assets/bulk-insert-co-car-fields.png)

### Nomi API

Puoi recuperare i nomi API a livello di programmazione passando il nome API dell’oggetto personalizzato alla [Descrivi oggetto personalizzato](#describe) endpoint.

```
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2017-02-22T19:55:51Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                }
            ]
        }
    ],
    "success": true
}
```

### Importa file

Ora supponiamo di voler importare tre record di oggetti personalizzati &quot;Car&quot;. Utilizzando il formato delimitato da virgole (CSV), il file potrebbe presentarsi così:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

La riga 1 è l&#39;intestazione e le righe 2-4 sono i record di dati oggetto personalizzati.

## Creazione di un processo

Per effettuare la richiesta di importazione in blocco, devi includere il nome API dell’oggetto personalizzato nel percorso di [Importa oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Identity/operation/identityUsingPOST) endpoint. È inoltre necessario includere un parametro &quot;file&quot; che faccia riferimento al nome del file di importazione e un parametro &quot;format&quot; che specifichi come il file di importazione è delimitato (&quot;csv&quot;, &quot;tsv&quot; o &quot;ssv&quot;).

```
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Disposition: form-data; name="file"; filename="custom_object_import.csv"
Content-Type: text/csv

color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo--
```

```json
{
    "requestId": "c015#15a68a23418",
    "result": [
        {
            "batchId": 1013,
            "status": "Queued",
            "objectApiName": "car_c"
        }
    ],
    "success": true
}
```

In questo esempio, abbiamo specificato il formato &quot;csv&quot; e denominato il file di importazione &quot;custom_object_import.csv&quot;.

Osserva che, nella risposta alla nostra chiamata, non esiste un elenco di successi o errori come si otterrebbe dall’endpoint Sincronizza oggetti personalizzati. Invece, riceverai un’ `batchId`. Questo perché la chiamata è asincrona e può restituire un `status` di &quot;In coda&quot;, &quot;Importazione&quot; o &quot;Non riuscita&quot;. È necessario conservare il batchId in modo da poter ottenere lo stato del processo di importazione o recuperare gli errori e/o gli avvisi al completamento. Il batchId rimane valido per sette giorni.

Un modo semplice per replicare la richiesta di importazione in blocco consiste nell’utilizzare curl dalla riga di comando:

```
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Dove il file di importazione &quot;custom_object_import.csv&quot; contiene quanto segue:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Stato processo di polling

Una volta creato il processo di importazione, è necessario eseguire una query sul relativo stato. È consigliabile eseguire il polling del processo di importazione ogni 5-30 secondi. A tale scopo, devi trasmettere il nome API dell’oggetto personalizzato e il `batchId` nel percorso del [Ottieni stato oggetto personalizzato importazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) endpoint.

```
GET /bulk/v1/customobjects/{apiName}/import/{batchId}/status.json
```

```json
{
    "requestId": "2a5#15a68dd9be1",
    "result": [
        {
            "batchId": 1013,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "importTime": "2 second(s)",
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Questa risposta mostra un’importazione completata, ma il `status` può essere uno dei seguenti: Complete, Queued, Importing, Failed. Se il processo è stato completato, viene visualizzato un elenco del numero di righe elaborate, con errori e avvisi. L’attributo message è inoltre utile per cercare informazioni aggiuntive sul processo.

## Errori

Gli errori sono indicati da `numOfRowsFailed` attributo in [Ottieni stato oggetto personalizzato importazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) risposta. Se numOfRowsFailed è maggiore di zero, tale valore indica il numero di errori che si sono verificati. Chiamata [Errori di importazione oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET) endpoint per ottenere un file con dettagli di errore. Di nuovo, devi passare il nome API dell’oggetto personalizzato e `batchId` nel percorso. Se non esiste alcun file con errori, viene restituito un codice di stato HTTP 404.

Continuando con l’esempio, possiamo forzare un errore modificando l’intestazione e cambiando &quot;vin&quot; in &quot; vin&quot; (aggiungendo uno spazio tra la virgola e &quot;vin&quot;).

```
color,make,model, vin
```

Quando importiamo di nuovo e controlliamo lo stato, vediamo questa risposta con `numRowsFailed`: 3. Questo indica tre errori.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/status.json
```

```json
{
    "requestId": "12260#15a68f491ed",
    "result": [
        {
            "batchId": 1016,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 0,
            "numOfRowsFailed": 3,
            "numOfRowsWithWarning": 0,
            "importTime": "1 second(s)",
            "message": "Import completed with errors, 0 records imported (0 members), 3 failed"
        }
    ],
    "success": true
}
```

Ora viene effettuata la chiamata dell’endpoint Get Import Custom Object Failures per ottenere ulteriori dettagli sull’errore:

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

E possiamo vedere che ci manca il nostro campo di deduplicazione `vin`.

## Avvisi

Le avvertenze sono indicate dalla `numOfRowsWithWarning` nella risposta Get Import Custom Object Status. Se numOfRowsWithWarning è maggiore di zero, tale valore indica il numero di avvisi che si sono verificati. Chiamata [Avvisi di importazione oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET) per ottenere un file con i dettagli di avviso. Di nuovo, devi passare il nome API dell’oggetto personalizzato e `batchId` nel percorso. Se non esiste alcun file di avviso, viene restituito un codice di stato HTTP 404.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
