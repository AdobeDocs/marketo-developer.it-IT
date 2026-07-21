---
title: Estrazione oggetto personalizzato in blocco
feature: REST API, Custom Objects
description: Guida alle API REST per l’estrazione di oggetti personalizzati in blocco da Marketo per l’esportazione di oggetti personalizzati collegati al lead con filtri di tipo updateAt ed list, campi selezionati e...
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
TQID: https://experienceleague.adobe.com/KAT-vab2uZq8FrRbZLy30PCJNfq01znDDuSSWuIu7WE
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
source-wordcount: 1231
ht-degree: 1%

---

# Estrazione oggetto personalizzato in blocco

[Riferimento dell&#39;endpoint di estrazione oggetto personalizzato bulk](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects)

Le API REST Bulk Custom Object Extract recuperano set elevati di record di oggetti personalizzati da Marketo. Utilizza queste API per lo scambio continuo di dati tra Marketo e sistemi esterni, ETL, data warehousing e archiviazione.

L’API esporta record di oggetti personalizzati Marketo di primo livello collegati direttamente ai lead. Specifica il nome dell’oggetto personalizzato e un elenco di lead collegati. Per ogni lead, l’API scrive i record oggetto personalizzati collegati corrispondenti come righe nel file di esportazione.

Puoi visualizzare i dati oggetto personalizzato nella scheda [Oggetto personalizzato della pagina dei dettagli del lead nell&#39;interfaccia utente di Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Autorizzazioni

L&#39;utente API deve disporre di un ruolo con l&#39;autorizzazione Oggetto personalizzato di sola lettura, l&#39;autorizzazione Oggetto personalizzato di lettura/scrittura o entrambi.

## Filtri

I filtri di estrazione dell’oggetto personalizzato specificano un elenco di lead collegati all’oggetto personalizzato. Se un lead elencato è collegato a record che corrispondono al nome di oggetto personalizzato specificato, l’API scrive tali record nel file di esportazione.

Specifica un solo tipo di filtro per processo di esportazione.

| Tipo di filtro | Tipo di dati | Note |
| --- | --- | --- |
| `updatedAt` | Date Range | Accetta un oggetto JSON con i membri `startAt` e `endAt` &nbsp;`startAt` accetta un datetime che rappresenta la filigrana bassa e `endAt` accetta un datetime che rappresenta la filigrana alta. L’intervallo non può essere superiore a 31 giorni. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono stati aggiornati entro l’intervallo di date. I valori di data devono essere in formato ISO-8601, senza millisecondi. |
| `staticListName` | Stringa | Accetta il nome di un elenco statico. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri dell&#39;elenco statico al momento dell&#39;inizio dell&#39;elaborazione del processo. Recuperare i nomi di elenco statici utilizzando l&#39;endpoint Get Lists. |
| `staticListId` | Intero | Accetta l’ID di un elenco statico. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri dell&#39;elenco statico al momento dell&#39;inizio dell&#39;elaborazione del processo. Recupera gli ID di elenco statici utilizzando l’endpoint Get Lists. |
| `smartListName`* | Stringa | Accetta il nome di un elenco avanzato. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri degli elenchi smart nel momento in cui il processo inizia l&#39;elaborazione. Recupera i nomi degli elenchi smart utilizzando l’endpoint Get Smart Lists. |
| `smartListId`* | Intero | Accetta l’ID di un elenco avanzato. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri degli elenchi smart nel momento in cui il processo inizia l&#39;elaborazione. Recupera gli ID degli elenchi avanzati utilizzando l’endpoint Ottieni elenchi avanzati. |

Alcuni abbonamenti non supportano questo tipo di filtro. Se non è disponibile, l&#39;endpoint del processo di creazione del lead di esportazione restituisce `1035, Unsupported filter type for target subscription`. Contatta il supporto Marketo per richiedere questa funzionalità per il tuo abbonamento.

## Opzioni

L&#39;endpoint [Crea processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) fornisce le opzioni per:

- Specifica i campi da includere nel file di esportazione.
- Rinomina le intestazioni di colonna esportate.
- Specifica il formato del file di esportazione.

| Parametro | Tipo di dati | Obbligatorio | Note |
| --- | --- | --- | --- |
| `fields` | Array[Stringa] | Sì | Array di stringhe contenenti il valore del nome dell&#39;attributo dell&#39;oggetto personalizzato restituito dall&#39;endpoint Describe Custom Object. I campi elencati sono inclusi nel file esportato. |
| `columnHeaderNames` | Oggetto | No | Oggetto JSON contenente coppie chiave-valore di nomi di intestazione di campo e colonna. La chiave deve essere il nome di un campo incluso nel processo di esportazione. Il valore corrisponde al nome dell&#39;intestazione di colonna esportata per il campo. |
| `format` | Stringa | No | Accetta uno di: CSV, TSV, SSV. Il file esportato viene renderizzato rispettivamente come un file di valori separati da virgole, valori separati da tabulazioni o valori separati da spazi, se impostato. Se non impostato, viene impostato il valore predefinito CSV. |

## Creazione di un processo

Utilizza l&#39;endpoint [Crea processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) per definire il processo di esportazione.

La richiesta utilizza i seguenti parametri:

- `apiName`: parametro di percorso obbligatorio. Specifica l&#39;oggetto personalizzato Marketo da esportare, utilizzando il nome restituito dall&#39;endpoint [Descrivi oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1). Gli oggetti personalizzati CRM non sono consentiti.
- `filter`: obbligatorio. Specifica i lead collegati facendo riferimento a un elenco statico o a un elenco avanzato.
- `fields`: obbligatorio. Specifica i nomi API degli attributi oggetto personalizzati da includere nel file di esportazione.
- `format`: facoltativo. Specifica il formato del file di esportazione.
- `columnHeaderNames`: facoltativo. Specifica i nomi delle intestazioni di colonna sostitutive.

In questo esempio viene utilizzato un oggetto personalizzato `Car` con campi `Color`, `Make`, `Model` e `VIN`. Il campo del collegamento è l’ID del lead e il campo della deduplicazione è VIN.

Definizione oggetto personalizzato

![Oggetto personalizzato](assets/custom-object-car.png)

Campi oggetto personalizzati

![Campi Oggetto Personalizzati](assets/custom-object-car-fields.png)

Chiamare [Descrivi oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/describeUsingGET_1) per controllare gli attributi dell&#39;oggetto personalizzato a livello di programmazione. La risposta restituisce gli attributi in `fields`.

```http
GET /rest/v1/customobjects/car_c/describe.json
```

```json
{
    "requestId": "148ef#1793e00f64f",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2021-05-05T16:14:41Z",
            "updatedAt": "2021-05-05T16:14:42Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vIN"
            ],
            "searchableFields": [
                [
                    "vIN"
                ],
                [
                    "marketoGUID"
                ],
                [
                    "leadID"
                ]
            ],
            "relationships": [
                {
                    "field": "leadID",
                    "type": "child",
                    "relatedTo": {
                        "name": "Lead",
                        "field": "Id"
                    }
                }
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "leadID",
                    "displayName": "Lead ID",
                    "dataType": "integer",
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "vIN",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

Utilizzare l&#39;endpoint [Sincronizza oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST) per creare record di oggetti personalizzati e collegare ciascuno di essi a un lead. Un lead può essere collegato a più record di oggetti personalizzati, creando una relazione uno-a-molti.

```http
POST /rest/v1/customobjects/car_c.json
```

```json
{
   "action":"createOrUpdate",
   "input":[
       {
           "leadId": 11,
           "color": "Pearl White",
           "make": "Tesla",
           "model": "Model S",
           "vIN": "5YJSA1E41FF156789"
       },
       {
           "leadId": 12,
           "color": "Midnight Silver Metallic",
           "make": "Tesla",
           "model": "Model X",
           "vIN": "LRWXB2B41FF198765"
       },
       {
           "leadId": 13,
           "color": "Fusion Red",
           "make": "Tesla",
           "model": "Roadster",
           "vIN": "SFGRC3C41FF154321"
       }
    ]
}
```

```json
{
    "requestId": "50d9#1793e066088",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "d911eaa1-fd0b-4a99-9b71-c6a7233c782c",
            "status": "created"
        },
        {
            "seq": 1,
            "marketoGUID": "20d04ffb-51f0-4336-924c-c783b9bb4215",
            "status": "created"
        },
        {
            "seq": 2,
            "marketoGUID": "e7da4331-8e7a-473b-85c8-047638eb6c7f",
            "status": "created"
        }
    ],
    "success": true
}
```

I tre lead in questo esempio appartengono all&#39;elenco statico `Car Buyers`, che ha un `id` di 1081. Chiamare l&#39;endpoint [Get Leads by List Id](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1) per recuperare i membri dell&#39;elenco.

```http
GET /rest/v1/lists/1081/leads.json
```

```json
{
    "requestId": "d023#1793e1e982b",
    "result": [
        {
            "id": 11,
            "firstName": "Hanna",
            "lastName": "Crawford",
            "email": "208161Hanna.Crawford@pookmail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 12,
            "firstName": "Bertha",
            "lastName": "Fulton",
            "email": "208160Bertha.Fulton@trashymail.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        },
        {
            "id": 13,
            "firstName": "Faith",
            "lastName": "England",
            "email": "208159Faith.England@dodgit.com",
            "updatedAt": "2020-01-16T02:38:22Z",
            "createdAt": "2017-07-27T01:38:42Z"
        }
    ],
    "success": true
}
```

Per recuperare questi record, chiamare l&#39;endpoint [Crea processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST). Specificare gli attributi oggetto personalizzati in `fields` e l&#39;ID elenco statico in `filter`.

```http
POST /bulk/v1/customobjects/car_c/export/create.json
```

```json
{
    "fields": [
        "leadId",
        "color",
        "make",
        "model",
        "vIN"
    ],
    "filter": {
        "staticListId": 1081
    }
}
```

```json
{
    "requestId": "8d2f#1793e289e87",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2021-05-05T20:12:01Z"
        }
    ],
    "success": true
}
```

La risposta conferma che il processo è stato creato, ma l’esportazione non si avvia automaticamente. Passa `apiName` e il `exportId` restituito all&#39;endpoint [Accoda processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) per avviare il processo.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/enqueue.json
```

```json
{
    "requestId": "cfaf#1793e2a0762",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z"
        }
    ],
    "success": true
}
```

La risposta di accodamento restituisce inizialmente lo stato `Queued`. Quando uno slot di esportazione diventa disponibile, lo stato cambia in `Processing`.

## Stato processo di polling

Puoi recuperare lo stato solo per i processi creati dallo stesso utente API.

Poiché l&#39;esportazione viene eseguita in modo asincrono, utilizzare l&#39;endpoint [Get Export Custom Object Job Status](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET) per eseguire il polling dell&#39;avanzamento. Lo stato viene aggiornato una sola volta ogni 60 secondi, quindi non eseguire il polling con maggiore frequenza.

Lo stato può essere `Created`, `Queued`, `Processing`, `Canceled`, `Completed` o `Failed`.

```http
GET /bulk/v1/customobjects/{apiName}/export/{exportId}/status.json
```

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z"
        }
    ],
    "success": true
}
```

Questa risposta indica che il processo è ancora in elaborazione, quindi il file non è disponibile. Quando lo stato del processo cambia in `Completed`, il file è pronto per il download.

```json
{
    "requestId": "14daa#1793e2cf9de",
    "result": [
        {
            "exportId": "f2c03f1d-226f-47c1-a557-357af8c2b32a",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2021-05-05T20:12:01Z",
            "queuedAt": "2021-05-05T20:13:32Z",
            "startedAt": "2021-05-05T20:14:15Z",
            "finishedAt": "2021-05-05T20:14:28Z",
            "numberOfRecords": 3,
            "fileSize": 182,
            "fileChecksum": "sha256:fac0cabc2352229c12e18b2fde03d1f24178bc71e9e926f520ae8d61bbe98c01"
        }
    ],
    "success": true
}
```

## Recupero dei dati

Per recuperare un&#39;esportazione di oggetti personalizzati completata, passare `apiName` e `exportId` all&#39;endpoint [Get Export Custom Object File](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET).

L’endpoint restituisce il file nel formato configurato per il processo. Se un attributo oggetto personalizzato richiesto non contiene dati, il campo di esportazione corrispondente contiene `null`.

```http
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Per il recupero parziale o ripristinabile, l&#39;endpoint del file supporta l&#39;intestazione HTTP `Range` facoltativa con un intervallo di tipo `bytes`. Se non si imposta l&#39;intestazione, l&#39;endpoint restituisce l&#39;intero file. Per ulteriori informazioni, vedere [Estrazione in blocco](bulk-extract.md).

## Annullamento di un processo

Per annullare un processo configurato in modo errato o non più necessario, chiamare l&#39;endpoint [Annulla esportazione processo oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST). Lo stato della risposta indica che il processo è annullato.

```http
POST /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/cancel.json
```

```json
{
    "requestId": "e5f9#179391286a7",
    "result": [
        {
            "exportId": "4a8cdd80-0d16-4dd6-9923-6ec97e30e91b",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2021-05-04T20:24:33Z"
        }
    ],
    "success": true
}
```
