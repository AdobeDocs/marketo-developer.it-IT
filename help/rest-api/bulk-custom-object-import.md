---
title: Importazione di oggetti personalizzati in blocco
feature: Custom Objects
description: Scopri come importare in blocco oggetti personalizzati Marketo tramite REST utilizzando file CSV, TSV o SSV.
exl-id: e795476c-14bc-4e8c-b611-1f0941a65825
TQID: https://experienceleague.adobe.com/C1LKLZDEvv95XXH3AEoxIXsLK55tgKTrvyxvs4LnYWw
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 736
ht-degree: 0%

---

# Importazione di oggetti personalizzati in blocco

[Riferimento dell&#39;endpoint di importazione oggetti personalizzati in blocco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects)

Utilizza l’API bulk per importare in modo asincrono un numero elevato di record di oggetti personalizzati. Fornisci i record in un file flat delimitato da virgole, tabulazioni o punti e virgola di dimensioni inferiori a 10 MB. Se il file è più grande, l’API restituisce un codice di stato HTTP 413.

Il contenuto del file dipende dalla definizione dell&#39;oggetto personalizzato. La prima riga deve essere un’intestazione e ogni campo dell’intestazione deve corrispondere a un nome API. Ogni riga rimanente contiene un record.

L&#39;importazione di oggetti personalizzati in blocco supporta solo l&#39;operazione di inserimento o aggiornamento dei record.

## Limiti di elaborazione

Ogni richiesta di importazione in blocco viene aggiunta come processo a una coda FIFO (First-In, First-Out). Si applicano i seguenti limiti:

- È possibile elaborare contemporaneamente un massimo di due processi.
- Nella coda possono essere presenti al massimo 10 processi, inclusi i due in fase di elaborazione.

Se si supera il massimo di 10 processi, l&#39;API restituisce un errore `1016, Too many imports`.

## Esempio di oggetto personalizzato

Prima di utilizzare l&#39;API in blocco, utilizza l&#39;interfaccia utente di amministrazione di Marketo per [creare l&#39;oggetto personalizzato](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects).

In questo esempio viene utilizzato un oggetto personalizzato `Car` con campi `Color`, `Make`, `Model` e `VIN`. Il campo VIN viene utilizzato per la deduplicazione. Le schermate dell’interfaccia utente di amministrazione evidenziano i nomi API richiesti dagli endpoint API in blocco.

![Inserisci oggetto personalizzato](assets/bulk-insert-co-car-1.png)

Di seguito sono riportati i campi oggetto personalizzati come presentati nell’interfaccia utente di amministrazione.

![Inserisci campi oggetto personalizzato](assets/bulk-insert-co-car-fields.png)

### Nomi API

Per recuperare i nomi API a livello di programmazione, passare il nome API dell&#39;oggetto personalizzato all&#39;endpoint [Descrizione oggetto personalizzato](#describe).

```text
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It is a car.",
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

Il seguente file CSV contiene tre `Car` record oggetto personalizzati:

```text
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

La prima riga è l’intestazione. Le righe da 2 a 4 contengono i record di dati oggetto personalizzati.

## Creazione di un processo

Per creare il processo di importazione in blocco, includere il nome API dell&#39;oggetto personalizzato nel percorso dell&#39;endpoint [Importa oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Identity/operation/identityUsingPOST). Includi questi parametri:

- `file`: nome del file di importazione.
- `format`: formato delimitatore file (`csv`, `tsv` o `ssv`).

```http
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```text
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```text
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

In questo esempio viene specificato il formato `csv` e viene denominato il file di importazione `custom_object_import.csv`.

Poiché la chiamata è asincrona, la risposta contiene `batchId` anziché i singoli errori e operazioni riuscite restituiti dall&#39;endpoint Sync Custom Objects. `status` può essere `Queued`, `Importing` o `Failed`.

Mantieni `batchId` per controllare lo stato di importazione e recuperare gli errori o gli avvisi dopo il completamento. `batchId` rimane valido per sette giorni.

La seguente richiesta cURL della riga di comando invia il processo di esempio:

```bash
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

In questo esempio, il file `custom_object_import.csv` contiene i dati seguenti:

```text
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Stato processo di polling

Dopo aver creato il processo di importazione, esegui il polling ogni 5-30 secondi. Passa il nome API dell&#39;oggetto personalizzato e `batchId` nel percorso dell&#39;endpoint [Get Import Custom Object Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET).

```http
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

Questa risposta mostra un’importazione completata. `status` può essere `Complete`, `Queued`, `Importing` o `Failed`.

Al termine del processo, la risposta elenca il numero di righe elaborate, non riuscite ed elaborate con avvertenze. L&#39;attributo `message` può fornire informazioni aggiuntive sul processo.

## Errori

L&#39;attributo `numOfRowsFailed` nella risposta [Get Import Custom Object Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) indica il numero di righe non riuscite. Un valore maggiore di zero indica che si sono verificati errori.

Passa il nome API dell&#39;oggetto personalizzato e `batchId` nel percorso dell&#39;endpoint [Get Import Custom Object Failures](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET). L’endpoint restituisce un file con dettagli di errore. Se non esiste alcun file con errori, restituisce il codice di stato HTTP 404.

Per dimostrare l&#39;errore, modificare l&#39;intestazione cambiando `vin` in ` vin`, aggiungendo uno spazio tra la virgola e `vin`.

```text
color,make,model, vin
```

Dopo la reimportazione del file, la risposta di stato mostra `numRowsFailed`: 3, che indica tre errori.

```http
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

Chiamare l&#39;endpoint Get Import Custom Object Failures per ulteriori informazioni:

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```text
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

La risposta indica che manca il campo di deduplicazione `vin`.

## Avvisi

L&#39;attributo `numOfRowsWithWarning` nella risposta Get Import Custom Object Status indica il numero di righe con avvisi. Un valore maggiore di zero indica che si sono verificate delle avvertenze.

Passa il nome API dell&#39;oggetto personalizzato e `batchId` nel percorso dell&#39;endpoint [Get Import Custom Object Warnings](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET). L’endpoint restituisce un file con i dettagli di avviso. Se non esiste alcun file di avviso, restituisce un codice di stato HTTP 404.

```http
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
