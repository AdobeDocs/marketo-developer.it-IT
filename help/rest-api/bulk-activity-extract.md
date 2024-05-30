---
title: "Estrazione attività in blocco"
feature: REST API
description: "Elaborazione in batch dei dati dell’attività da Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1381'
ht-degree: 1%

---


# Estrazione attività in blocco

[Riferimento endpoint estrazione attività in blocco](https://developer.adobe.com/marketo-apis/api/mapi/)

Il set di API REST per l’estrazione di attività in blocco fornisce un’interfaccia programmatica per recuperare grandi quantità di dati di attività da Marketo.  Per i casi che non richiedono bassa latenza e che devono trasferire volumi significativi di dati di attività fuori da Marketo, come l’integrazione CRM, ETL, il data warehousing e l’archiviazione dei dati.

## Autorizzazioni

Le API di estrazione attività in blocco richiedono che l’utente API disponga delle autorizzazioni &quot;Attività di sola lettura&quot; o &quot;Attività di lettura/scrittura&quot;.

## Filtri

<table>
  <tbody>
    <tr>
      <td>Tipo di filtro</td>
      <td>Tipo di dati</td>
      <td>Obbligatorio</td>
      <td>Note</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>Intervallo date</td>
      <td>Sì</td>
      <td>Accetta un oggetto JSON con i membri startAt e endAt. startAt accetta un valore datetime che rappresenta la filigrana bassa e endAt accetta un valore datetime che rappresenta la filigrana alta. L’intervallo deve essere inferiore o pari a 31 giorni.I processi con questo tipo di filtro restituiscono tutti i record accessibili creati all’interno dell’intervallo di date.I valori di data devono essere in formato ISO-8601, senza millisecondi.</td>
    </tr>
    <tr>
      <td>activityTypeIds</td>
      <td>Array[Integer]</td>
      <td>No</td>
      <td>Accetta un oggetto JSON con un membro, activityTypeIds. Il valore deve essere una matrice di numeri interi corrispondenti ai tipi di attività desiderati. L’attività "Elimina lead" non è supportata (utilizzare <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET">Ottieni lead eliminati</a>endpoint al suo posto).Recupera gli ID del tipo di attività utilizzando<a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET">Ottieni tipi di attività</a>endpoint.</td>
    </tr>
    <tr>
      <td>primaryAttributeValueIds</td>
      <td>Array[Integer]</td>
      <td>No</td>
      <td>Accetta un oggetto JSON con un membro, primaryAttributeValueIds. Il valore è una matrice di ID che specificano gli attributi primari su cui filtrare. È possibile specificare un massimo di 50 ID. Gli ID sono l’identificatore univoco di un campo lead o di una risorsa e possono essere recuperati chiamando l’endpoint API REST appropriato. Ad esempio, per filtrare in base a un modulo specifico per l’attività "Compila modulo", passa il nome del modulo a <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Ottieni modulo per nome</a> endpoint per recuperare l'ID modulo.Di seguito è riportato un elenco di tipi di attività in cui è supportato il filtro degli attributi primari.
        <table>
          <tbody>
            <tr>
              <td>Tipo di attività</td>
              <td>ID valore attributo principale</td>
              <td>Endpoint di recupero</td>
              <td>Gruppo risorse</td>
            </tr>
            <tr>
              <td>Modifica valore dati</td>
              <td>ID campo lead</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrivi lead</a></td>
              <td>Nome attributo</td>
            </tr>
            <tr>
              <td>Modifica punteggio</td>
              <td>ID campo lead</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrivi lead</a></td>
              <td>Nome attributo</td>
            </tr>
            <tr>
              <td>Modifica stato in progressione</td>
              <td>ID programma</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET">Ottieni programma per nome</a></td>
              <td>Programma di marketing</td>
            </tr>
            <tr>
              <td>Aggiungi all'elenco</td>
              <td>ID elenco statico</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Ottieni elenco statico per nome</a></td>
              <td>Elenco statico</td>
            </tr>
            <tr>
              <td>Rimuovi dall’elenco</td>
              <td>ID elenco statico</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Ottieni elenco statico per nome</a></td>
              <td>Elenco statico</td>
            </tr>
            <tr>
              <td>Compila modulo</td>
              <td>ID modulo</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Ottieni modulo per nome</a></td>
              <td>Modulo web</td>
            </tr>
          </tbody>
        </table>
        Quando si utilizzano primaryAttributeValueIds, il filtro activityTypeIds deve essere presente e contenere solo ID attività che corrispondono al gruppo di risorse corrispondente.Esempio:Se ad esempio si filtrano risorse di moduli Web, in activityTypeIds è consentito solo l'ID del tipo di attività "Compila modulo".Corpo della richiesta di esempio:{"filter":{"createdAt":{"startAt": "2021-07-01T23:59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValueIds" : [16,102,95,8]}}primaryAttributeValueIds e primaryAttributeValues non possono essere utilizzati insieme.</td>
    </tr>
    <tr>
      <td>primaryAttributeValues</td>
      <td>Array[Stringa]</td>
      <td>No</td>
      <td>Accetta un oggetto JSON con un membro, primaryAttributeValues. Il valore è una matrice di nomi che specificano gli attributi principali su cui filtrare. È possibile specificare un massimo di 50 nomi. I nomi sono l’identificatore univoco di un campo lead o di una risorsa e possono essere recuperati chiamando l’endpoint API REST appropriato. Ad esempio, per filtrare in base a un modulo specifico l’attività "Compila modulo", passa l’ID modulo a <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Ottieni modulo per ID</a> endpoint per recuperare il nome del modulo.Di seguito è riportato un elenco di tipi di attività in cui è supportato il filtro degli attributi primari.
        <table>
          <tbody>
            <tr>
              <td>Tipo di attività</td>
              <td>Valore attributo principale</td>
              <td>Endpoint di recupero</td>
              <td>Gruppo risorse</td>
            </tr>
            <tr>
              <td>Modifica valore dati</td>
              <td>Nome visualizzato campo lead</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrivi lead</a></td>
              <td>Nome attributo</td>
            </tr>
            <tr>
              <td>Modifica punteggio</td>
              <td>Nome visualizzato campo lead</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Descrivi lead</a></td>
              <td>Nome attributo</td>
            </tr>
            <tr>
              <td>Modifica stato in progressione</td>
              <td>Nome del programma</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET">Ottieni programma per ID</a></td>
              <td>Programma di marketing</td>
            </tr>
            <tr>
              <td>Aggiungi all'elenco</td>
              <td>Nome elenco statico</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Ottieni elenco statico per ID</a></td>
              <td>Elenco statico</td>
            </tr>
            <tr>
              <td>Rimuovi dall’elenco</td>
              <td>Nome elenco statico</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Ottieni elenco statico per ID</a></td>
              <td>Elenco statico</td>
            </tr>
            <tr>
              <td>Compila modulo</td>
              <td>Nome modulo</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Ottieni modulo per ID</a></td>
              <td>Modulo web</td>
            </tr>
          </tbody>
        </table>
        Si noti che è necessario utilizzare "&lt;<em>programma</em>&gt;.&lt;<em>risorsa</em>Notazione &gt;" per specificare in modo univoco il nome per i seguenti gruppi di risorse: Programma di marketing, Elenco statico, Modulo web.Esempio:Ad esempio, un modulo con nome "MPS in uscita" che risiede sotto il programma con nome "GL_OP_ALL_2021" verrebbe specificato come "GL_OP_ALL_2021.MPS in uscita".Corpo della richiesta di esempio:{"filter":{"createdAt":{"startAt": "2021-07-01T23":59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValues":["GL_OP_ALL_2021.MPS In uscita"]}}Quando si utilizza primaryAttributeValues, il filtro activityTypeIds deve essere presente e contenere solo ID attività che corrispondono al gruppo di risorse corrispondente. Ad esempio, se si applica un filtro alle risorse di un modulo web, in activityTypeIds.primaryAttributeValues e primaryAttributeValueIds non è possibile utilizzare contemporaneamente solo l’ID del tipo di attività "Compila modulo".</td>
    </tr>
  </tbody>
</table>

## Opzioni

| Parametro | Tipo di dati | Obbligatorio | Note |
|---|---|---|---|
| filter | Array[Oggetto] | Sì | Accetta un array di filtri. Nell’array deve essere incluso esattamente un filtro createdAt. È possibile includere un filtro activityTypeIds facoltativo. I filtri vengono applicati al set di attività accessibile e il set di attività risultante viene restituito dal processo di esportazione. |
| formato | Stringa | No | Accetta uno dei seguenti file: CSV, TSV, SSV Il file esportato viene renderizzato come file di valori separati da virgole, valori separati da tabulazioni o valori separati da spazi, rispettivamente se impostato. Se non impostato, il valore predefinito è CSV. |
| columnHeaderNames | Oggetto | No | Oggetto JSON contenente coppie chiave-valore di nomi di intestazione di campo e colonna. La chiave deve essere il nome di un campo incluso nel processo di esportazione. Il valore corrisponde al nome dell&#39;intestazione di colonna esportata per il campo. |
| campi | Array[Stringa] | No | Matrice facoltativa di stringhe contenenti valori di campo. I campi elencati sono inclusi nel file esportato.Per impostazione predefinita, vengono restituiti i campi seguenti: `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`,Questo parametro può essere utilizzato per ridurre il numero di campi restituiti specificando un sottoinsieme dall&#39;elenco precedente.Esempio:&quot;fields&quot;: [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]È possibile specificare un campo aggiuntivo &quot;actionResult&quot; per includere l&#39;azione dell&#39;attività (&quot;success&quot;, &quot;skipped&quot; o &quot;failed&quot;). |


## Creazione di un processo

Per esportare i record, è innanzitutto necessario definire il job e il set di record che si desidera recuperare.  Crea il processo utilizzando [Crea processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST) endpoint.  Durante l’esportazione delle attività è possibile applicare due filtri principali: `createdAt`, che è sempre obbligatorio, e `activityTypeIds`, che è facoltativo.  Il filtro createdAt viene utilizzato per definire un intervallo di date in cui sono state create le attività, utilizzando `startAt` e `endAt` parametri, entrambi campi datetime, che rappresentano rispettivamente la prima data di creazione consentita e l&#39;ultima data di creazione consentita.  Facoltativamente, puoi anche filtrare solo alcuni tipi di attività utilizzando `activityTypeIds` filtro.  Ciò è utile per rimuovere i risultati che non sono rilevanti per il tuo caso d’uso.

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

Il processo ora ha lo stato &quot;Creato&quot;, ma non è ancora nella coda di elaborazione.  Per metterlo in coda in modo che possa iniziare l’elaborazione, è necessario chiamare il [Accoda processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) utilizzando l’exportId dalla risposta dello stato di creazione.

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

Ora lo stato segnala che il processo è stato messo in coda.  Quando un processo di lavoro diventa disponibile per questo processo, lo stato viene impostato su &quot;Elaborazione&quot; e il processo inizierà l’aggregazione dei record da Marketo.

## Stato processo di polling

Lo stato del processo può essere recuperato solo per i processi creati dallo stesso utente API.

L’estrazione dell’attività in blocco di Marketo è un endpoint asincrono, pertanto è necessario eseguire il polling dello stato del processo per determinare quando è stato completato.  Effettua il polling utilizzando [Ottieni stato processo attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) endpoint come segue:

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

- Creato
- In coda
- Elaborazione
- Annullato
- Completato
- Operazione non riuscita

## Recupero dei dati

Una volta completato il processo, recupera i dati utilizzando [Ottieni file attività di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) endpoint.

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

La risposta contiene un file formattato nel modo in cui è stato configurato il processo. L’endpoint risponde con il contenuto del file.

Se un campo lead richiesto è vuoto (non contiene dati), `then null` viene inserito nel campo corrispondente nel file di esportazione.  Nell’esempio seguente, il campo campaignId per l’attività restituita è vuoto.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Per supportare il recupero parziale e semplice dei dati estratti, l’endpoint del file supporta facoltativamente l’intestazione HTTP `Range` del tipo `bytes`.  Se l’intestazione non è impostata, verrà restituito l’intero contenuto.  Per ulteriori informazioni, consulta Utilizzo dell’intestazione Intervallo con Marketo [Estrai in blocco](bulk-extract.md).

## Annullamento di un processo

Se un processo non è stato configurato correttamente o diventa superfluo, può essere facilmente annullato utilizzando [Annulla processo attività esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) endpoint:

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
