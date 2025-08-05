---
title: Estrazione attività in blocco
feature: REST API
description: Elaborazione in batch dei dati dell’attività da Marketo.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1332'
ht-degree: 3%

---

# Estrazione attività in blocco

[Riferimento endpoint estrazione attività in blocco](https://developer.adobe.com/marketo-apis/api/mapi/)

Il set di API REST per l’estrazione di attività in blocco fornisce un’interfaccia programmatica per recuperare grandi quantità di dati di attività da Marketo.  Per i casi che non richiedono bassa latenza e che devono trasferire volumi significativi di dati di attività al di fuori di Marketo, come l’integrazione CRM, ETL, il data warehousing e l’archiviazione dei dati.

## Autorizzazioni

Le API di estrazione attività in blocco richiedono che l’utente API disponga delle autorizzazioni &quot;Attività di sola lettura&quot; o &quot;Attività di lettura/scrittura&quot;.

## Filtri

| Tipo di filtro | Tipo di dati | Obbligatorio | Note |
| --- | --- | --- | --- |
| createdAt | Date Range | Sì | Accetta un oggetto JSON con i membri `startAt` e `endAt`. `startAt` accetta un datetime che rappresenta la filigrana bassa e `endAt` accetta un datetime che rappresenta la filigrana alta. L’intervallo non può essere superiore a 31 giorni. I processi con questo tipo di filtro restituiscono tutti i record accessibili creati entro l&#39;intervallo di date. I valori di data devono essere in formato ISO-8601, senza millisecondi. |
| activityTypeIds | Array\[Intero\] | No | Accetta un oggetto JSON con un membro, `activityTypeIds`. Il valore deve essere una matrice di numeri interi corrispondenti ai tipi di attività desiderati. L&#39;attività &quot;Elimina lead&quot; non è supportata (utilizzare l&#39;endpoint [Ottieni lead eliminati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET)). Recupera gli ID dei tipi di attività utilizzando l&#39;endpoint [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [primaryAttributeValueIds](#primaryattributevalueids-options) | Array\[Intero\] | No | Accetta un oggetto JSON con un membro, `primaryAttributeValueIds`. Il valore è una matrice di ID che specificano gli attributi primari su cui filtrare. È possibile specificare un massimo di 50 ID. Gli ID sono l’identificatore univoco di un campo lead o di una risorsa e possono essere recuperati chiamando l’endpoint API REST appropriato. Ad esempio, per filtrare in base a un modulo specifico per l&#39;attività &quot;Compila modulo&quot;, passa il nome del modulo all&#39;endpoint [Ottieni modulo per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) per recuperare l&#39;ID modulo. Di seguito è riportato un elenco di tipi di attività in cui è supportato il filtro degli attributi primari. |
| [primaryAttributeValues](#primaryattributevalues-options) | Array\[Stringa\] | No | Accetta un oggetto JSON con un membro, `primaryAttributeValues`. Il valore è un array di nomi che specificano gli attributi principali su cui filtrare. È possibile specificare un massimo di 50 nomi. I nomi sono l’identificatore univoco di un campo lead o di una risorsa e possono essere recuperati chiamando l’endpoint API REST appropriato. Ad esempio, per filtrare in base a un modulo specifico per l&#39;attività &quot;Compila modulo&quot;, passa l&#39;ID modulo all&#39;endpoint [Ottieni modulo per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) per recuperare il nome del modulo. Di seguito è riportato un elenco di tipi di attività in cui è supportato il filtro degli attributi primari. |

### opzioni primaryAttributeValueIds {#primaryattributevalueids-options}

| Tipo di attività | ID valore attributo principale | Endpoint di recupero | Gruppo risorse |
| --- | --- | --- | --- |
| Modifica valore dati | ID campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modifica punteggio | ID campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modifica stato in progressione | ID programma | [Ottieni programma per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) | Programma di marketing |
| Aggiungi all’elenco | ID elenco statico | [Ottieni elenco statico per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Elenco statico |
| Rimuovi dall’elenco | ID elenco statico | [Ottieni elenco statico per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Elenco statico |
| Compila modulo | ID modulo | [Ottieni modulo per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) | Modulo web |

Quando si utilizza `primaryAttributeValueIds`, il filtro `activityTypeIds` deve essere presente e contenere solo ID attività che corrispondono al gruppo di risorse corrispondente. Se ad esempio si applica un filtro alle risorse di un modulo Web, in `activityTypeIds` è consentito solo l&#39;ID del tipo di attività &quot;Compila modulo&quot;.

Esempio di corpo della richiesta:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValueIds": [
      16,102,95,8
    ]
  }
}
```

Impossibile utilizzare `primaryAttributeValueIds` e `primaryAttributeValues` insieme.

### opzioni primaryAttributeValues {#primaryattributevalues-options}

| Tipo di attività | Valore attributo principale | Endpoint di recupero | Gruppo risorse |
| --- | --- | --- | --- |
| Modifica valore dati | Nome visualizzato campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modifica punteggio | Nome visualizzato campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modifica stato in progressione | Nome del programma | [Ottieni programma per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) | Programma di marketing |
| Aggiungi all’elenco | Nome elenco statico | [Ottieni elenco statico per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Elenco statico |
| Rimuovi dall’elenco | Nome elenco statico | [Ottieni elenco statico per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Elenco statico |
| Compila modulo | Nome modulo | [Ottieni modulo per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) | Modulo web |

Devi usare &quot;&lt;<em>programma</em>>.Notazione &lt;<em>asset</em>>&quot; per specificare il nome per i seguenti gruppi di risorse: Programma di marketing, Elenco statico, Modulo Web. Ad esempio, una maschera con il nome &quot;MPS in uscita&quot; che si trova sotto un programma con il nome &quot;GL_OP_ALL_2021&quot; viene specificata come &quot;GL_OP_ALL_2021.MPS in uscita&quot;.

Esempio di corpo della richiesta:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValues": [
      "GL_OP_ALL_2021.MPS Outbound"
    ]
  }
}
```

Quando si utilizza `primaryAttributeValues`, il filtro `activityTypeIds` deve essere presente e contenere solo ID attività che corrispondono al gruppo di risorse corrispondente. Se ad esempio si applica un filtro alle risorse di un modulo Web, in `activityTypeIds` è consentito solo l&#39;ID del tipo di attività &quot;Compila modulo&quot;. Impossibile utilizzare `primaryAttributeValues` e `primaryAttributeValueIds` insieme.

## Opzioni

| Parametro | Tipo di dati | Obbligatorio | Note |
|---|---|---|---|
| filter | Array[Oggetto] | Sì | Accetta un array di filtri. Nell&#39;array deve essere incluso esattamente un filtro `createdAt`. È possibile includere un filtro `activityTypeIds` facoltativo. I filtri vengono applicati al set di attività accessibile e il set di attività risultante viene restituito dal processo di esportazione. |
| formato | Stringa | No | Accetta uno dei seguenti file: CSV, TSV, SSV Il file esportato viene renderizzato rispettivamente come file di valori separati da virgola, valori separati da tabulazioni o valori separati da spazi, se impostati. Se non impostato, viene impostato il valore predefinito CSV. |
| columnHeaderNames | Oggetto | No | Oggetto JSON contenente coppie chiave-valore di nomi di intestazione di campo e colonna. La chiave deve essere il nome di un campo incluso nel processo di esportazione. Il valore corrisponde al nome dell&#39;intestazione di colonna esportata per il campo. |
| campi | Array[Stringa] | No | Matrice facoltativa di stringhe contenenti valori di campo. I campi elencati sono inclusi nel file esportato. Per impostazione predefinita, vengono restituiti i campi seguenti: <ul><li>`marketoGUIDleadId`</li><li> `activityDate` </li><li>`activityTypeId` </li><li>`campaignId`</li><li> `primaryAttributeValueId` </li><li>`primaryAttributeValue`</li><li> `attributes`</li></ul>. Questo parametro può essere utilizzato per ridurre il numero di campi restituiti specificando un sottoinsieme dall&#39;elenco precedente:`"fields": ["leadId", "activityDate", "activityTypeId"]`. È possibile specificare un campo aggiuntivo `actionResult` per includere l&#39;azione dell&#39;attività: `("succeeded", "skipped", or "failed")`. |

## Creazione di un processo

Per esportare i record, è innanzitutto necessario definire il job e il set di record che si desidera recuperare.  Crea il processo utilizzando l&#39;endpoint [Crea processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).  Durante l&#39;esportazione delle attività è possibile applicare due filtri primari: `createdAt`, che è sempre obbligatorio, e `activityTypeIds`, che è facoltativo.  Il filtro `createdAt` viene utilizzato per definire un intervallo di date in cui sono state create le attività, utilizzando i parametri `startAt` e `endAt`, che sono entrambi campi datetime e rappresentano rispettivamente la prima data di creazione consentita e l&#39;ultima data di creazione consentita.  Facoltativamente, è inoltre possibile filtrare solo alcuni tipi di attività utilizzando il filtro `activityTypeIds`.  Ciò è utile per rimuovere risultati che non sono rilevanti per il tuo caso d’uso.

```
POST /bulk/v1/activities/export/create.json
```

```json
{
   "format": "CSV",
   "filter": {
      "createdAt": {
         "startAt": "2017-07-01T23:59:59-00:00",
         "endAt": "2017-07-31T23:59:59-00:00"
      },
      "activityTypeIds": [
         1,
         12,
         13
      ]
   }
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Il processo ora ha lo stato &quot;Creato&quot;, ma non è ancora nella coda di elaborazione.  Per metterlo in coda in modo che possa iniziare l&#39;elaborazione, chiamare l&#39;endpoint [Processo attività di esportazione accodamento](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) utilizzando il valore exportId dalla risposta dello stato di creazione.

```
POST /bulk/v1/activities/export/{exportId}/enqueue.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Queued",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Ora lo stato segnala che il processo è stato messo in coda.  Quando un processo di lavoro diventa disponibile per questo processo, lo stato viene impostato su &quot;Elaborazione&quot; e il processo inizia ad aggregare i record da Marketo.

## Stato processo di polling

Lo stato del processo può essere recuperato solo per i processi creati dallo stesso utente API.

L’estrazione dell’attività in blocco di Marketo è un endpoint asincrono, pertanto è necessario eseguire il polling dello stato del processo per determinare quando è stato completato.  Effettua il polling utilizzando l&#39;endpoint [Ottieni stato processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) come segue:

```
GET /bulk/v1/activities/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 15423,
         "fileSize": 12342,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
   ]
}
```

Il campo di stato può rispondere con uno dei seguenti valori:

- Creata
- In coda
- Elaborazione
- Annullato
- Completato
- Operazione non riuscita

## Recupero dei dati

Una volta completato il processo, recuperare i dati utilizzando l&#39;endpoint [Get Export Activity File](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET).

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

La risposta contiene un file formattato nel modo in cui è stato configurato il processo. L’endpoint risponde con il contenuto del file.

Se un campo lead richiesto è vuoto (non contiene dati), `then null` viene inserito nel campo corrispondente nel file di esportazione.  Nell&#39;esempio seguente, il campo `campaignId` per l&#39;attività restituita è vuoto.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Per supportare il recupero parziale e semplice dei dati estratti, l&#39;endpoint del file supporta facoltativamente l&#39;intestazione HTTP `Range` di tipo `bytes`.  Se l’intestazione non è impostata, viene restituito l’intero contenuto.  Per ulteriori informazioni sull&#39;utilizzo dell&#39;intestazione Range con Marketo [Bulk Extract](bulk-extract.md).

## Annullamento di un processo

Se un processo non è stato configurato correttamente o non è più necessario, può essere facilmente annullato utilizzando l&#39;endpoint [Annulla processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST):

```
POST /bulk/v1/activities/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV"
      }
   ]
}
```

La risposta presenta uno stato che indica che il processo è stato annullato.
