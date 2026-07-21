---
title: Estrazione membro programma in blocco
feature: REST API
description: Utilizza le API REST di Estrai membri del programma in blocco Marketo per esportare record di membri di grandi dimensioni per ETL, data warehousing e archiviazione, con autorizzazioni e metadati di campo.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
TQID: https://experienceleague.adobe.com/w4qaVTKSe0EORaSiURB6WbJXi29JUdEgfkb2dnfuVFw
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1026
ht-degree: 2%

---

# Estrazione membro programma in blocco

[Riferimento endpoint estrazione membro programma in blocco](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members)

Le API REST Bulk Program Member Extract recuperano set di grandi dimensioni di record dei membri del programma da Marketo. Utilizza queste API per lo scambio continuo di dati tra Marketo e sistemi esterni, ETL, data warehousing e archiviazione.

## Autorizzazioni

L’utente API deve avere un ruolo con l’autorizzazione Lead di sola lettura, l’autorizzazione Lead di lettura/scrittura o entrambi.

## Descrivere

Utilizzare [Descrivi membro del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) per determinare quali campi sono disponibili e recuperare i relativi metadati. L&#39;attributo `name` contiene il nome del campo API REST.

```http
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Filtri

Le esportazioni dei membri del programma supportano più opzioni di filtro. Quando un processo specifica più tipi di filtro, l’API li combina con un’operazione AND.

Ogni processo deve specificare `programId` o `programIds`. Tutti gli altri filtri sono facoltativi. Il filtro `updatedAt` richiede un&#39;infrastruttura non disponibile in tutte le sottoscrizioni.

<table>
  <tbody>
    <tr>
      <td>Tipo di filtro</td>
      <td>Tipo di dati</td>
      <td>Note</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Intero</td>
      <td>Accetta l’ID di un programma. I job restituiscono tutti i record accessibili che sono membri del programma al momento in cui il job inizia l'elaborazione.Recupera gli ID del programma utilizzando l'endpoint <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Get Programs</a>.Impossibile utilizzare con il filtro programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[Integer]</td>
      <td>Accetta un array di un massimo di 10 ID di programma. I job restituiscono tutti i record accessibili che sono membri dei programmi al momento in cui il job inizia l'elaborazione.Come primo campo, al file di esportazione viene aggiunto un campo aggiuntivo "programId". Questo campo identifica il programma da cui è stato estratto un record di iscrizione al programma.Recupera gli ID del programma utilizzando l'endpoint <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs">Get Programs</a>.Non può essere utilizzato con il filtro programId.</td>
    </tr>
    <tr>
      <td>isExausted</td>
      <td>Booleano</td>
      <td>Accetta un valore booleano utilizzato per filtrare i record di appartenenza al programma per <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">persone con contenuto esaurito</a>.</td>
    </tr>
    <tr>
      <td>nurtureCadence</td>
      <td>Stringa</td>
      <td>Accetta una stringa utilizzata per filtrare i record di appartenenza al programma per una determinata cadenza di crescita. I valori consentiti sono:
        <ul>
          <li>pausa - la cadenza è in pausa</li>
          <li>norm - la cadenza è normale</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[Stringa]</td>
      <td>Accetta un array di nomi di stato dei membri del programma. I nomi di stato multipli sono associati a OR.I processi con questo tipo di filtro restituiscono tutti i record accessibili il cui stato membro del programma corrisponde a uno qualsiasi dei nomi di stato specificati. È possibile utilizzare sia nomi di stato predefiniti che nomi di stato definiti dall’utente.Se il filtro statusNames viene utilizzato con il filtro "programIds", ogni programma viene controllato per i record di appartenenza il cui stato corrisponde a uno qualsiasi dei nomi di stato. Se in uno dei programmi non viene trovato un nome di stato, viene restituito l'errore "1003, Invalid Data" (Dati non validi).
        <table>
          <tbody>
            <tr>
              <td>Partecipazione avvenuta</td>
              <td>Partecipazione on-demand</td>
              <td>Mancato recapito</td>
            </tr>
            <tr>
              <td>Clic</td>
              <td>Contattato</td>
              <td>Convertito</td>
            </tr>
            <tr>
              <td>Coinvolti</td>
              <td>Modulo compilato</td>
              <td>Influenzato</td>
            </tr>
            <tr>
              <td>Invitato</td>
              <td>Membro</td>
              <td>Nessuno spettacolo</td>
            </tr>
            <tr>
              <td>Non nel programma</td>
              <td>Su elenco</td>
              <td>Apertura</td>
            </tr>
            <tr>
              <td>Registrato</td>
              <td>Registrazione</td>
              <td>Errore di registrazione</td>
            </tr>
            <tr>
              <td>Inviato</td>
              <td>Iscritto</td>
              <td>Annulla l'iscrizione</td>
            </tr>
            <tr>
              <td>Visualizzato</td>
              <td>Visitato</td>
              <td>Stand visitato</td>
            </tr>
            <tr>
              <td>In lista d’attesa</td>
              <td>Contenuto web</td>
              <td></td>
            </tr>
          </tbody>
        </table></td>
    </tr>
    <tr>
      <td>updateAt*</td>
      <td>Date Range</td>
      <td>Accetta un oggetto JSON con i membri startAt e endAt. startAt accetta un valore datetime che rappresenta la filigrana bassa e endAt accetta un valore datetime che rappresenta la filigrana alta. L’intervallo non può essere superiore a 31 giorni. I valori di data devono essere in formato ISO-8601, senza millisecondi.I processi con questo tipo di filtro restituiscono tutti i record accessibili che sono stati aggiornati più di recente all’interno dell’intervallo di date.</td>
    </tr>
  </tbody>
</table>

Alcuni abbonamenti non supportano questo tipo di filtro. Se non è disponibile, l&#39;endpoint del processo del membro del programma Crea esportazione restituisce `1035, Unsupported filter type for target subscription`. Contatta il supporto Marketo per richiedere questa funzionalità per il tuo abbonamento.

## Opzioni

L&#39;endpoint del processo Crea membro del programma di esportazione fornisce le opzioni per:

- Specifica i campi da includere nel file di esportazione.
- Rinomina le intestazioni di colonna esportate.
- Specifica il formato del file di esportazione.

| Parametro | Tipo di dati | Obbligatorio | Note |
| --- | --- | --- | --- |
| campi | Array[Stringa] | Sì | Il parametro fields accetta un array JSON di stringhe. I campi elencati sono inclusi nel file esportato. È possibile esportare i seguenti tipi di campo:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Specifica un campo che utilizzi il nome API REST che può essere recuperato utilizzando gli endpoint Descrivi lead2 e/o Descrivi membri del programma. |
| columnHeaderNames | Oggetto | No | Oggetto JSON contenente coppie chiave-valore di nomi di intestazione di campo e colonna. La chiave deve essere il nome di un campo incluso nel processo di esportazione. Il valore corrisponde al nome dell&#39;intestazione di colonna esportata per il campo. |
| formato | Stringa | No | Accetta uno di: CSV, TSV, SSV. Il file esportato viene renderizzato rispettivamente come un file di valori separati da virgole, valori separati da tabulazioni o valori separati da spazi, se impostato. Se non impostato, viene impostato il valore predefinito CSV. |

## Creazione di un processo

Utilizza l&#39;endpoint [Crea processo membro del programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) per definire il processo di esportazione. Specificare un `filter` che contiene l&#39;ID del programma e il `fields` da esportare. È inoltre possibile specificare `format` e `columnHeaderNames`.

```http
POST /bulk/v1/program/members/export/create.json
```

```json
{
   "format": "CSV",
   "fields": [
        "firstName",
        "lastName",
        "email",
        "membershipDate",
        "program",
        "statusName",
        "leadId",
        "reachedSuccess",
        "leadCustomField01",
        "leadCustomField02",
        "pMCustomField01",
        "pMCustomField02"
   ],
   "filter": {
      "programId":1044
   }
}
```

```json
{
    "requestId": "4d44#16f92734f6e",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2020-01-11T02:33:48Z"
        }
    ],
    "success": true
}
```

La risposta conferma che il processo è stato creato, ma l’esportazione non si avvia automaticamente. Passa il `exportId` restituito all&#39;endpoint [Processo membro programma esportazione accodamento](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) per avviare il processo:

```http
POST /bulk/v1/program/members/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "d70b#16f9273ae32",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z"
        }
    ],
    "success": true
}
```

La risposta di accodamento restituisce inizialmente lo stato `Queued`. Quando uno slot di esportazione diventa disponibile, lo stato cambia in `Processing`.

## Stato processo di polling

Puoi recuperare lo stato solo per i processi creati dallo stesso utente API.

Poiché l&#39;esportazione viene eseguita in modo asincrono, utilizzare l&#39;endpoint [Ottieni stato processo membro programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) per eseguire il polling dell&#39;avanzamento. Lo stato viene aggiornato una sola volta ogni 60 secondi, quindi non eseguire il polling con maggiore frequenza.

Lo stato può essere `Created`, `Queued`, `Processing`, `Canceled`, `Completed` o `Failed`.

```http
GET /bulk/v1/program/members/export/{exportId}/status.json
```

```json
{
    "requestId": "9a40#16f9274d250",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
        }
    ],
    "success": true
}
```

Questa risposta indica che il processo è ancora in elaborazione, quindi il file non è disponibile. Quando lo stato del processo cambia in `Completed`, il file è pronto per il download.

```json
{
    "requestId": "11ad1#16f9ff6da23",
    "result": [
        {
            "exportId": "1118dc83-273b-4d44-becb-4d212fece550",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
            "finishedAt": "2020-01-11T02:36:12Z",
            "numberOfRecords": 13,
            "fileSize": 1752,
            "fileChecksum": "sha256:b3c8e70e6e501cf1025e345a66b409d4fd07364c7da773cfa68a2b68ce1a7212"
        }
    ],
    "success": true
}
```

## Recupero dei dati

Per recuperare un&#39;esportazione di un membro del programma completata, passare `exportId` all&#39;endpoint [Ottieni file membro del programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET).

L’endpoint restituisce il file nel formato configurato per il processo. Se un campo membro del programma richiesto non contiene dati, il campo di esportazione corrispondente contiene `null`.

```http
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```text
firstName,lastName,email,Member Date,Program,Status,Lead Id,Success,leadCustomField01,leadCustomField02,pMCustomField01,pMCustomField02
Meera,Reed,mree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1789,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jon,Umber,jumb@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1790,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Lyanna,Mormont,lmor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1791,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickon,Stark,rsta@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1792,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Hodor,null,hodor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1793,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Osha,null,osha@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1794,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jojen,Reed,Jree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1795,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickard,Karstark,rkar@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1796,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Maester,Luwin,mluw@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1797,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rodrik,Cassel,rcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1798,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jory,Cassel,jcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1799,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Septa,Mordane,smor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1800,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
```

Per il recupero parziale o ripristinabile, l&#39;endpoint del file supporta l&#39;intestazione HTTP `Range` facoltativa con un intervallo di tipo `bytes`. Se non si imposta l&#39;intestazione, l&#39;endpoint restituisce l&#39;intero file. Per ulteriori informazioni, vedere [Estrazione in blocco](bulk-extract.md).

## Annullamento di un processo

Per annullare un processo configurato in modo errato o non più necessario, chiamare l&#39;endpoint [Annulla processo membro del programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST):

```http
POST /bulk/v1/program/members/export/{exportId}/cancel.json
```

```json
{
    "requestId": "bb4f#16f86727f89",
    "result": [
        {
            "exportId": "f0d3520c-3a60-4568-9e71-2e619d3805a4",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2020-01-07T21:47:35Z"
        }
    ],
    "success": true
}
```

Lo stato della risposta indica che il processo è annullato.
