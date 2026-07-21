---
title: Estrazione attività in blocco
feature: REST API
description: Marketo Bulk Activity Extract REST API per esportare dati di attività a volume elevato utilizzando un intervallo di date di 31 giorni, filtri di attività e attributi primari per ETL e CRM.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
TQID: https://experienceleague.adobe.com/lIlXNjatN-F77Dv3xsVkQ3hAWwLZ4wlSW0zKNkFJFMA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b0bb9048-d951-48d8-8232-45cf248a7e27id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1268
ht-degree: 4%

---

# Estrazione attività in blocco

[Riferimento endpoint estrazione attività in blocco](https://developer.adobe.com/marketo-apis/api/mapi)

Le API REST di Bulk Activity Extract recuperano grandi volumi di dati di attività da Marketo. Utilizzare queste API per i processi che non richiedono latenza ridotta, come l&#39;integrazione CRM, ETL, data warehousing e archiviazione dati.

## Autorizzazioni

L’utente API deve disporre dell’autorizzazione &quot;Attività di sola lettura&quot; o &quot;Attività di lettura/scrittura&quot;.

## Filtri

| Tipo di filtro | Tipo di dati | Obbligatorio | Note |
| --- | --- | --- | --- |
| `createdAt` | Date Range | Sì | Oggetto JSON contenente `startAt` e `endAt`. `startAt` è il valore di data/ora della filigrana bassa e `endAt` è il valore di data/ora della filigrana alta. L’intervallo non può essere superiore a 31 giorni. Il job restituisce tutti i record accessibili creati all&#39;interno dell&#39;intervallo di date. Utilizza valori datetime ISO-8601 senza millisecondi. |
| `activityTypeIds` | Array\[Intero\] | No | Matrice di numeri interi per i tipi di attività richiesti. L’attività &quot;Elimina lead&quot; non è supportata. Utilizza invece l&#39;endpoint [Get Deleted Leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET). Recupera gli ID dei tipi di attività con l&#39;endpoint [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [`primaryAttributeValueIds`](#primaryattributevalueids-options) | Array\[Intero\] | No | Array che accetta un massimo di 50 ID per gli attributi primari. Ogni ID identifica in modo univoco un campo o una risorsa lead. Recupera gli ID chiamando l’endpoint API REST appropriato. Ad esempio, per filtrare in base a un modulo specifico per l&#39;attività &quot;Compila modulo&quot;, passa il nome del modulo all&#39;endpoint [Ottieni modulo per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) per recuperare l&#39;ID modulo. Consulta [primaryAttributeValueIds options](#primaryattributevalueids-options) per i tipi di attività supportati. |
| [`primaryAttributeValues`](#primaryattributevalues-options) | Array\[Stringa\] | No | Matrice che accetta un massimo di 50 nomi per gli attributi primari. Ogni nome identifica in modo univoco un campo o una risorsa lead. Recupera i nomi chiamando l’endpoint API REST appropriato. Ad esempio, per filtrare in base a un modulo specifico per l&#39;attività &quot;Compila modulo&quot;, passa l&#39;ID modulo all&#39;endpoint [Ottieni modulo per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) per recuperare il nome del modulo. Consulta [primaryAttributeValues options](#primaryattributevalues-options) per i tipi di attività supportati. |

### opzioni primaryAttributeValueIds {#primaryattributevalueids-options}

| Tipo di attività | ID valore attributo principale | Endpoint di recupero | Gruppo risorse |
| --- | --- | --- | --- |
| Modifica valore dati | ID campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modificare punteggio | ID campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modifica stato in progressione | ID programma | [Ottieni programma per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByNameUsingGET) | Programma di marketing |
| Aggiungi all’elenco | ID elenco statico | [Ottieni elenco statico per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Elenco statico |
| Rimuovi dall’elenco | ID elenco statico | [Ottieni elenco statico per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Elenco statico |
| Compila modulo | ID modulo | [Ottieni modulo per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) | Modulo web |

Quando si utilizza `primaryAttributeValueIds`, è necessario includere anche il filtro `activityTypeIds`. Questo filtro può contenere solo ID attività che corrispondono al gruppo di risorse corrispondente. Ad esempio, quando si filtrano le risorse dei moduli Web, `activityTypeIds` può contenere solo l&#39;ID del tipo di attività &quot;Compila modulo&quot;.

La richiesta seguente include il filtro `primaryAttributeValueIds`:

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
| Modifica valore dati | Nome visualizzato campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modificare punteggio | Nome visualizzato campo lead | [Descrizione lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/describeUsingGET_2) | Nome attributo |
| Modifica stato in progressione | Nome del programma | [Ottieni programma per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs/operation/getProgramByIdUsingGET) | Programma di marketing |
| Aggiungi all’elenco | Nome elenco statico | [Ottieni elenco statico per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Elenco statico |
| Rimuovi dall’elenco | Nome elenco statico | [Ottieni elenco statico per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Elenco statico |
| Compila modulo | Nome del modulo | [Ottieni modulo per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET) | Modulo web |

Utilizza la notazione `&lt;program&gt;.&lt;asset&gt;` per specificare i nomi dei gruppi di risorse Programma di marketing, Elenco statico e Modulo web. Ad esempio, specificate la maschera &quot;MPS in uscita&quot; nel programma &quot;GL_OP_ALL_2021&quot; come &quot;GL_OP_ALL_2021.MPS in uscita&quot;.

La richiesta seguente include il filtro `primaryAttributeValues`:

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

Quando si utilizza `primaryAttributeValues`, è necessario includere anche il filtro `activityTypeIds`. Questo filtro può contenere solo ID attività che corrispondono al gruppo di risorse corrispondente. Ad esempio, quando si filtrano le risorse dei moduli Web, `activityTypeIds` può contenere solo l&#39;ID del tipo di attività &quot;Compila modulo&quot;.

Impossibile utilizzare `primaryAttributeValues` e `primaryAttributeValueIds` insieme.

## Opzioni

| Parametro | Tipo di dati | Obbligatorio | Note |
| --- | --- | --- | --- |
| `filter` | Oggetto | Sì | Oggetto contenente i filtri applicabili al set di attività accessibile. Includere esattamente un filtro `createdAt`. È inoltre possibile includere un filtro `activityTypeIds`. Il processo di esportazione restituisce il set di attività risultante. |
| `format` | Stringa | No | Il formato del file di esportazione: CSV, TSV o SSV. Questi valori generano rispettivamente valori separati da virgole, separati da tabulazioni o separati da spazi. Il valore predefinito è CSV. |
| `columnHeaderNames` | Oggetto | No | Oggetto JSON costituito da coppie chiave-valore di campo e intestazione di colonna. Ogni chiave deve assegnare un nome a un campo incluso nel processo di esportazione. Il relativo valore imposta l’intestazione della colonna esportata per tale campo. |
| `fields` | Array\[Stringa\] | No | Matrice di campi da includere nel file di esportazione. Per impostazione predefinita, la risposta include `marketoGUID`, `leadId`, `activityDate`, `activityTypeId`, `campaignId`, `primaryAttributeValueId`, `primaryAttributeValue` e `attributes`. Per restituire un sottoinsieme, specificare i campi di questo elenco, ad esempio `"fields": ["leadId", "activityDate", "activityTypeId"]`. È inoltre possibile specificare `actionResult` per includere l&#39;azione dell&#39;attività: `("succeeded", "skipped", or "failed")`. |

## Creazione di un processo

Creare un processo di esportazione per definire i record da recuperare. Utilizza l&#39;endpoint [Crea processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST).

Ogni processo richiede un filtro `createdAt`. I parametri datetime `startAt` e `endAt` definiscono le date di creazione dell&#39;attività consentite più recenti e meno recenti. Per escludere tipi di attività non rilevanti, includere anche il filtro `activityTypeIds` facoltativo.

La seguente richiesta crea un processo di esportazione CSV per i tipi di attività selezionati all’interno di un intervallo di date:

```http
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

La risposta restituisce un valore `exportId` e lo stato &quot;Creato&quot;. Un processo creato non è ancora nella coda di elaborazione.

Per aggiungere il processo alla coda, chiamare l&#39;endpoint [Attività di esportazione accodamento](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) con `exportId` dalla risposta di creazione.

```http
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

Lo stato della risposta ora è &quot;In coda&quot;. Quando un lavoratore diventa disponibile, lo stato cambia in &quot;Elaborazione&quot; e il processo inizia ad aggregare i record da Marketo.

## Stato processo di polling

Lo stato del processo può essere recuperato solo per i processi creati dallo stesso utente API.

L’estrazione dell’attività in blocco elabora i processi in modo asincrono. Esamina l&#39;endpoint [Ottieni stato processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) per determinare quando un processo è completo:

```http
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

Il campo `status` restituisce uno dei seguenti valori:

- `Created`
- `Queued`
- `Processing`
- `Canceled`
- `Completed`
- `Failed`

## Recupero dei dati

Quando lo stato del processo è &quot;Completato&quot;, recuperare i dati esportati con l&#39;endpoint [Ottieni file attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET):

```http
GET /bulk/v1/activities/export/{exportId}/file.json
```

Il corpo della risposta contiene il file nel formato configurato per il processo.

Se un campo attività richiesto non contiene dati, `null` viene visualizzato nel campo export-file corrispondente. L’esempio seguente mostra i dati di attività esportati:

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Per il recupero parziale o ripristinabile, l&#39;endpoint del file supporta l&#39;intestazione HTTP `Range` facoltativa con un intervallo `bytes`. Se ometti questa intestazione, l’endpoint restituisce l’intero file. Per ulteriori informazioni sull&#39;utilizzo dell&#39;intestazione `Range`, vedere [Estrazione in blocco](bulk-extract.md).

## Annullamento di un processo

Per interrompere un processo configurato in modo errato o non necessario, chiamare l&#39;endpoint [Annulla processo attività esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST):

```http
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

Lo stato della risposta indica che il processo è annullato.
