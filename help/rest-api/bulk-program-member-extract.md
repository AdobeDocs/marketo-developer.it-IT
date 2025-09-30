---
title: Estrazione membro programma in blocco
feature: REST API
description: Utilizza le API REST di Estrai membri del programma in blocco Marketo per esportare record di membri di grandi dimensioni per ETL, data warehousing e archiviazione, con autorizzazioni e metadati di campo.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1160'
ht-degree: 2%

---

# Estrazione membro programma in blocco

[Riferimento endpoint estrazione membro programma in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members)

Il set Bulk Program Member Extract delle API REST fornisce un’interfaccia programmatica per recuperare grandi set di record dei membri del programma da Marketo. Si tratta dell&#39;interfaccia consigliata per i casi d&#39;uso che richiedono l&#39;interscambio continuo di dati tra Marketo e uno o più sistemi esterni, a scopo di ETL, data warehousing e archiviazione.

## Autorizzazioni

Le API Bulk Program Member Extract richiedono che l&#39;utente API proprietario abbia un ruolo con una o entrambe le autorizzazioni Lead di sola lettura o Lead di lettura/scrittura.

## Descrivere

[Descrivi membro del programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) è l&#39;origine principale di verità per stabilire se i campi sono disponibili per l&#39;uso e i metadati relativi a tali campi. L&#39;attributo `name` contiene il nome API REST.

```
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

I membri del programma supportano varie opzioni di filtro. È possibile specificare più tipi di filtro per un processo, nel qual caso sono AND insieme. Specificare il filtro `programId` o `programIds`. Tutti gli altri filtri sono facoltativi. Il filtro `updatedAt` richiede componenti di infrastruttura aggiuntivi che non sono ancora stati distribuiti in tutte le sottoscrizioni.

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
      <td>Accetta l’ID di un programma. I processi restituiscono tutti i record accessibili che sono membri del programma al momento dell'inizio dell'elaborazione del processo.Recuperare gli ID del programma utilizzando l'endpoint <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Get Programs</a>.Non può essere utilizzato con il filtro programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[Integer]</td>
      <td>Accetta un array di un massimo di 10 ID di programma. I job restituiscono tutti i record accessibili che sono membri dei programmi al momento in cui il job inizia l'elaborazione.Un campo aggiuntivo "programId" viene aggiunto al file di esportazione come primo campo. Questo campo identifica il programma da cui è stato estratto un record di appartenenza al programma.Recuperare gli ID del programma utilizzando l'endpoint <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Get Programs</a>.Non può essere utilizzato con il filtro programId.</td>
    </tr>
    <tr>
      <td>isExausted</td>
      <td>Booleano</td>
      <td>Accetta un valore booleano utilizzato per filtrare i record di appartenenza al programma per <a href="https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">persone con contenuto esaurito</a>.</td>
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
              <td>Rifiutato</td>
            </tr>
            <tr>
              <td>Clic effettuato</td>
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
              <td>Aperto</td>
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
      <td>Accetta un oggetto JSON con i membri startAt e endAt. startAt accetta un valore datetime che rappresenta la filigrana bassa e endAt accetta un valore datetime che rappresenta la filigrana alta. L’intervallo non può essere superiore a 31 giorni. I valori di DataTime devono essere in formato ISO-8601, senza millisecondi.I processi con questo tipo di filtro restituiscono tutti i record accessibili più di recente aggiornati all'interno dell'intervallo di date.</td>
    </tr>
  </tbody>
</table>

Il tipo di filtro non è disponibile per alcune sottoscrizioni. Se non disponibile per l’abbonamento, viene visualizzato un errore durante la chiamata dell’endpoint Processo membro del programma di esportazione Crea (&quot;1035, Tipo di filtro non supportato per l’abbonamento di destinazione&quot;). I clienti possono contattare il supporto tecnico Marketo per richiedere che questa funzionalità sia abilitata nel loro abbonamento.

## Opzioni

L’endpoint del processo Crea membro del programma di esportazione offre diverse opzioni di formattazione. Queste opzioni consentono all&#39;utente di:

- Specificare i campi da includere nel file esportato
- Rinomina le intestazioni di colonna di questi campi
- Specifica il formato del file esportato

| Parametro | Tipo di dati | Obbligatorio | Note |
|---|---|---|---|
| campi | Array[Stringa] | Sì | Il parametro fields accetta un array JSON di stringhe. I campi elencati sono inclusi nel file esportato. È possibile esportare i seguenti tipi di campo:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Specifica un campo utilizzando il nome API REST che può essere recuperato utilizzando gli endpoint Descrivi lead2 e/o Descrivi membri del programma. |
| columnHeaderNames | Oggetto | No | Oggetto JSON contenente coppie chiave-valore di nomi di intestazione di campo e colonna. La chiave deve essere il nome di un campo incluso nel processo di esportazione. Il valore corrisponde al nome dell&#39;intestazione di colonna esportata per il campo. |
| formato | Stringa | No | Accetta uno di: CSV, TSV, SSV. Il file esportato viene renderizzato rispettivamente come un file di valori separati da virgole, valori separati da tabulazioni o valori separati da spazi, se impostato. Se non impostato, viene impostato il valore predefinito CSV. |

## Creazione di un processo

I parametri per il processo vengono definiti prima di avviare l&#39;esportazione utilizzando l&#39;endpoint [Crea processo membro del programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST). È necessario definire `filter` contenente l&#39;ID del programma e `fields` necessari per l&#39;esportazione. Facoltativamente, è possibile definire `format` del file e `columnHeaderNames`.

```
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

Restituisce una risposta di stato che indica che il processo è stato creato. Il processo è stato definito e creato, ma non è ancora stato avviato. Per eseguire questa operazione, l&#39;endpoint [Processo membro programma esportazione accodamento](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) deve essere chiamato utilizzando `exportId` dalla risposta dello stato di creazione:

```
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

Questo risponderà con un `status` iniziale di &quot;In coda&quot; dopo il quale sarà impostato su &quot;Elaborazione&quot; quando è disponibile uno slot di esportazione.

## Stato processo di polling

Nota: è possibile recuperare lo stato solo per i processi creati dallo stesso utente API.

Poiché si tratta di un endpoint asincrono, dopo la creazione del processo è necessario eseguire il polling del relativo stato per determinarne l’avanzamento. Effettua il polling utilizzando l&#39;endpoint [Ottieni stato processo membro del programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET). Lo stato viene aggiornato solo una volta ogni 60 secondi, pertanto non è consigliabile una frequenza di polling inferiore a questa, e in quasi tutti i casi è ancora eccessiva. Il campo di stato può rispondere con uno dei seguenti valori: Creato, In coda, Elaborazione, Annullato, Completato, Non riuscito.

```
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

L’endpoint di stato risponde indicando che il processo è ancora in elaborazione, pertanto il file non è ancora disponibile per il recupero. Quando il processo `status` diventa &quot;Completato&quot;, è disponibile per il download.

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

Per recuperare il file di un&#39;esportazione di un membro del programma completata, è sufficiente chiamare l&#39;endpoint [Ottieni file membro del programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET) con `exportId`.

La risposta contiene un file formattato nel modo in cui è stato configurato il processo. L’endpoint risponde con il contenuto del file. Se un campo del membro del programma richiesto è vuoto (non contiene dati), `null` viene inserito nel campo corrispondente nel file di esportazione.

```
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```
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

Per supportare il recupero parziale e intuitivo dei dati estratti, l’endpoint del file supporta facoltativamente l’intervallo di intestazioni HTTP dei byte di tipo. Se l’intestazione non è impostata, verrà restituito l’intero contenuto. Per ulteriori informazioni sull&#39;utilizzo dell&#39;intestazione Range con Marketo [Bulk Extract](bulk-extract.md).

## Annullamento di un processo

Se un processo non è stato configurato correttamente o non è più necessario, può essere facilmente annullato utilizzando l&#39;endpoint [Annulla processo membro del programma di esportazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST):

```
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

Questo risponde con un `status` che indica che il processo è stato annullato.
