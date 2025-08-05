---
title: Estrazione oggetto personalizzato in blocco
feature: REST API, Custom Objects
description: Elaborazione in batch di oggetti Marketo personalizzati.
exl-id: 86cf02b0-90a3-4ec6-8abd-b4423cdd94eb
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1298'
ht-degree: 1%

---

# Estrazione oggetto personalizzato in blocco

[Riferimento all&#39;endpoint di estrazione oggetto personalizzato in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects)

Il set Bulk Custom Object Extract delle API REST fornisce un’interfaccia programmatica per recuperare set elevati di record di oggetti personalizzati da Marketo. Si tratta dell&#39;interfaccia consigliata per i casi d&#39;uso che richiedono l&#39;interscambio continuo di dati tra Marketo e uno o più sistemi esterni, a scopo di ETL, data warehousing e archiviazione.

Questa API supporta l’esportazione di record di oggetti personalizzati Marketo di primo livello collegati direttamente a un lead. Passa il nome dell’oggetto personalizzato e un elenco di lead a cui l’oggetto è collegato. Per ogni lead dell&#39;elenco, i record oggetto personalizzati collegati che corrispondono al nome oggetto personalizzato specificato vengono scritti come righe nel file di esportazione. I dati oggetto personalizzato sono visualizzabili nella scheda [Oggetto personalizzato della pagina dei dettagli del lead nell&#39;interfaccia utente di Marketo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects).

## Autorizzazioni

Le API Bulk Custom Object Extract richiedono che l’utente API abbia un ruolo con una o entrambe le autorizzazioni &quot;Read-Only Custom Object&quot; (Oggetto personalizzato di sola lettura) o &quot;Read-Write Custom Object&quot; (Oggetto personalizzato di lettura/scrittura).

## Filtri

L’estrazione dell’oggetto personalizzato supporta diverse opzioni di filtro utilizzate per specificare un elenco di lead collegati all’oggetto personalizzato. Se un lead nell&#39;elenco è collegato a record oggetto personalizzati che corrispondono a un determinato nome oggetto personalizzato, i record vengono scritti nel file di esportazione. È possibile specificare un solo tipo di filtro per processo di esportazione.

| Tipo di filtro | Tipo di dati | Note |
|---|---|---|
| `updatedAt` | Date Range | Accetta un oggetto JSON con i membri `startAt` e `endAt` &amp;nbsp.;`startAt` accetta un datetime che rappresenta la filigrana minima e `endAt` accetta un datetime che rappresenta la filigrana massima. L’intervallo non può essere superiore a 31 giorni. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono stati aggiornati entro l’intervallo di date. I valori di data devono essere in formato ISO-8601, senza millisecondi. |
| `staticListName` | Stringa | Accetta il nome di un elenco statico. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri dell&#39;elenco statico al momento dell&#39;inizio dell&#39;elaborazione del processo. Recuperare i nomi di elenco statici utilizzando l&#39;endpoint Get Lists. |
| `staticListId` | Intero | Accetta l’ID di un elenco statico. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri dell&#39;elenco statico al momento dell&#39;inizio dell&#39;elaborazione del processo. Recupera gli ID di elenco statici utilizzando l’endpoint Get Lists. |
| `smartListName`* | Stringa | Accetta il nome di un elenco avanzato. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri degli elenchi smart nel momento in cui il processo inizia l&#39;elaborazione. Recupera i nomi degli elenchi smart utilizzando l’endpoint Get Smart Lists. |
| `smartListId`* | Intero | Accetta l’ID di un elenco avanzato. I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono membri degli elenchi smart nel momento in cui il processo inizia l&#39;elaborazione. Recupera gli ID degli elenchi avanzati utilizzando l’endpoint Ottieni elenchi avanzati. |

Il tipo di filtro non è disponibile per alcune sottoscrizioni. Se non disponibile per la sottoscrizione, viene visualizzato un errore durante la chiamata dell’endpoint del processo Crea lead di esportazione (&quot;1035, tipo di filtro non supportato per la sottoscrizione di destinazione&quot;). I clienti possono contattare il supporto tecnico Marketo per richiedere che questa funzionalità sia abilitata nel loro abbonamento.

## Opzioni

L&#39;endpoint [Crea processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST) fornisce diverse opzioni di formattazione. Queste opzioni consentono all&#39;utente di:

- Specificare i campi da includere nel file esportato
- Rinomina le intestazioni di colonna di questi campi
- Specifica il formato del file esportato

| Parametro | Tipo di dati | Obbligatorio | Note |
|---|---|---|---|
| `fields` | Array[Stringa] | Sì | Array di stringhe contenenti il valore del nome dell&#39;attributo dell&#39;oggetto personalizzato restituito dall&#39;endpoint Describe Custom Object. I campi elencati sono inclusi nel file esportato. |
| `columnHeaderNames` | Oggetto | No | Oggetto JSON contenente coppie chiave-valore di nomi di intestazione di campo e colonna. La chiave deve essere il nome di un campo incluso nel processo di esportazione. Il valore corrisponde al nome dell&#39;intestazione di colonna esportata per il campo. |
| `format` | Stringa | No | Accetta uno di: CSV, TSV, SSV. Il file esportato viene renderizzato rispettivamente come un file di valori separati da virgole, valori separati da tabulazioni o valori separati da spazi, se impostato. Se non impostato, viene impostato il valore predefinito CSV. |

## Creazione di un processo

I parametri per il processo vengono definiti prima di avviare l&#39;esportazione utilizzando l&#39;endpoint [Crea processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST).

Il parametro di percorso `apiName` richiesto è il nome dell&#39;oggetto personalizzato restituito dall&#39;endpoint [Descrivi oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1). Specifica quale oggetto personalizzato Marketo esportare. Gli oggetti personalizzati CRM non sono consentiti. Il parametro `filter` richiesto contiene l&#39;elenco dei lead collegati all&#39;oggetto personalizzato. Può fare riferimento a un elenco statico o a un elenco avanzato. Il parametro `fields` obbligatorio contiene i nomi API degli attributi oggetto personalizzati da includere nel file di esportazione. Facoltativamente, è possibile definire `format` del file e `columnHeaderNames`.

Ad esempio, supponiamo di aver creato un oggetto personalizzato denominato &quot;Car&quot; con i seguenti campi: Color, Make, Model, VIN. Il campo del collegamento è l’ID del lead e il campo della deduplicazione è VIN.

Definizione oggetto personalizzato

![Oggetto personalizzato](assets/custom-object-car.png)

Campi oggetto personalizzati

![Campi Oggetto Personalizzati](assets/custom-object-car-fields.png)

È possibile chiamare [Descrivi oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/describeUsingGET_1) per controllare a livello di programmazione gli attributi dell&#39;oggetto personalizzato visualizzati nell&#39;attributo `fields` nella risposta.

```
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

Crea diversi record di oggetti personalizzati e collegali a un lead diverso utilizzando l&#39;endpoint [Sincronizza oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST). Un lead può essere collegato a molti record di oggetti personalizzati. Questa è nota come relazione &quot;uno a molti&quot;.

```
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

Ciascuno dei tre lead a cui si fa riferimento in precedenza appartiene a un elenco statico denominato &quot;Acquirenti auto&quot; il cui `id` è 1081, come illustrato di seguito, chiamando l&#39;endpoint [Ottieni lead per ID elenco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists/operation/getLeadsByListIdUsingGET_1).

```
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

Ora creiamo un processo di esportazione per recuperare questi record. Utilizzando l&#39;endpoint [Crea processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/createExportCustomObjectsUsingPOST), vengono specificati gli attributi dell&#39;oggetto personalizzato nel parametro `fields` e un ID di elenco statico nel parametro `filter`.

```
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

Nella risposta viene restituito uno stato che indica che il processo è stato creato. Il processo è stato definito e creato, ma non è ancora stato avviato. Per eseguire questa operazione, è necessario chiamare l&#39;endpoint [Processo oggetto personalizzato esportazione accodamento](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/enqueueExportCustomObjectsUsingPOST) utilizzando `apiName` e `exportId` dalla risposta dello stato di creazione.

```
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

Questo risponde con un `status` iniziale di &quot;In coda&quot; dopo il quale è impostato su &quot;Elaborazione&quot; quando è disponibile uno slot di esportazione.

## Stato processo di polling

Lo stato può essere recuperato solo per i processi creati dallo stesso utente API.

Poiché si tratta di un endpoint asincrono, dopo la creazione del processo è necessario eseguire il polling del relativo stato per determinarne l’avanzamento. Eseguire il polling utilizzando l&#39;endpoint [Ottieni stato processo di esportazione oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsStatusUsingGET). Lo stato viene aggiornato solo una volta ogni 60 secondi, pertanto non è consigliabile una frequenza di polling inferiore a questa, e in quasi tutti i casi è ancora eccessiva. Il campo di stato può rispondere con uno dei seguenti valori: Creato, In coda, Elaborazione, Annullato, Completato o Non riuscito.

```
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

L’endpoint di stato risponde indicando che il processo è ancora in elaborazione, pertanto il file non è ancora disponibile per il recupero. Quando il processo `status` diventa &quot;Completato&quot;, è disponibile per il download.

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

Per recuperare il file di un&#39;esportazione di oggetti personalizzati completata, è sufficiente chiamare l&#39;endpoint [Get Export Custom Object File](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingGET) con `apiName` e `exportId`.

La risposta contiene un file formattato nel modo in cui è stato configurato il processo. L’endpoint risponde con il contenuto del file. Se un attributo oggetto personalizzato richiesto è vuoto (non contiene dati), `null` viene inserito nel campo corrispondente nel file di esportazione.

```
GET /bulk/v1/customobjects/car_c/export/f2c03f1d-226f-47c1-a557-357af8c2b32a/file.json
```

```csv
leadId,color,make,model,vIN
11,Pearl White,Tesla,Model S,5YJSA1E41FF156789
12,Midnight Silver Metallic,Tesla,Model X,LRWXB2B41FF198765
13,Fusion Red,Tesla,Roadster,SFGRC3C41FF154321
```

Per supportare il recupero parziale e semplice dei dati estratti, l&#39;endpoint del file supporta facoltativamente l&#39;intestazione HTTP `Range` di tipo `bytes`. Se l’intestazione non è impostata, verrà restituito l’intero contenuto. Ulteriori informazioni sull&#39;utilizzo dell&#39;intestazione Range in Marketo [Bulk Extract](bulk-extract.md).

## Annullamento di un processo

Se un processo non è stato configurato correttamente o non è più necessario, può essere facilmente annullato utilizzando l&#39;endpoint [Annulla esportazione processo oggetto personalizzato](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Custom-Objects/operation/getExportCustomObjectsFileUsingPOST). Questo risponde con un `status` che indica che il processo è stato annullato.

```
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
