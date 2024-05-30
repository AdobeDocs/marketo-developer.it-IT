---
title: "Membri del programma"
feature: REST API
description: "Crea e gestisci i membri del programma."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# Membri del programma

[Riferimento endpoint membri programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members)

Marketo espone le API per la lettura, la creazione, l’aggiornamento e l’eliminazione dei record dei membri del programma. I record dei membri del programma sono correlati ai record dei lead tramite il campo ID lead. I record sono composti da un set di campi standard e facoltativamente da un massimo di 20 campi personalizzati aggiuntivi. I campi contengono dati specifici del programma per ogni membro e possono essere utilizzati in moduli, filtri, trigger e azioni di flusso. Questi dati sono visualizzabili nel [Scheda Membri](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) nell’interfaccia utente del Marketo Engage.

## Descrivere

Il [Descrivi membro programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) l&#39;endpoint segue il modello standard per gli oggetti di database lead. Il `searchableFields` matrice fornisce il set di campi validi per l&#39;esecuzione di query. Il `fields` L’array contiene metadati di campo che includono il nome API REST, il nome visualizzato e la possibilità di aggiornamento dei campi.

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

## Query

Il [Ottieni membri del programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMembersUsingGET) L&#39;endpoint consente di recuperare i membri di un programma. Richiede un `programId` parametro path e `filterType` e `filterValues` parametri di query.

`programId` viene utilizzato per specificare il programma in cui eseguire la ricerca.

`filterType` viene utilizzato per specificare quale campo utilizzare come filtro di ricerca. Accetta qualsiasi campo nell’elenco &quot;searchableFields&quot; restituito da [Descrivi membro programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) endpoint. Se si specifica un filterType come campo personalizzato, il valore dataType del campo personalizzato deve essere &quot;string&quot; o &quot;integer&quot;. Se si specifica un filterType diverso da &quot;leadId&quot;, la richiesta può elaborare un massimo di 100.000 record di membri del programma. A seconda della configurazione dell’istanza di Marketo, viene visualizzato uno dei seguenti errori:

- Se il numero totale di membri del programma supera i 100.000, viene restituito un errore: &quot;1003, dimensione totale appartenenza: 100.001 supera il limite consentito di 100.000 per il filtro&quot;.
- Se il numero totale di membri del programma _che corrispondono al filtro_ supera i 100.000, viene restituito un errore: &quot;1003, dimensioni appartenenza corrispondenti: 100.001 supera il limite consentito (100.000) per questa api&quot;.

Per eseguire una query su un programma il cui numero di iscrizioni supera il limite, utilizzare [API di estrazione membri programma in blocco](bulk-program-member-extract.md) invece.

`filterValues` viene utilizzato per specificare i valori da cercare e accetta fino a 300 valori in un formato separato da virgole. La chiamata cerca i record in cui il campo del membro del programma corrisponde a uno dei valori filterValues inclusi.

In alternativa, puoi filtrare per intervallo di date specificando `updatedAt` as filterType con `startAt` e `endAt` parametri datetime. L’intervallo non può essere superiore a sette giorni. I valori di data devono essere in formato ISO-8601, senza millisecondi.

L&#39;opzione `fields` Il parametro query accetta un elenco separato da virgole di nomi API di campo restituiti da [Descrivi membro programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) endpoint. Se incluso, ogni record nella risposta include i campi specificati. Se omesso, il set di campi predefinito restituito è `acquiredBy`, `leadId`, `membershipDate`, `programId`, e `reachedSuccess`.

Per impostazione predefinita, vengono restituiti al massimo 300 record. È possibile utilizzare `batchSize` parametro di query per ridurre questo numero. Se il **moreResult** è true, il che significa che sono disponibili più risultati. Continua a chiamare questo endpoint fino a quando l’attributo moreResult restituisce false, il che significa che non sono disponibili risultati. Il `nextPageToken` restituiti da questa API devono sempre essere riutilizzati per la successiva iterazione di questa chiamata.

Se la lunghezza totale della richiesta di GET supera gli 8 KB, viene restituito un errore HTTP: &quot;414, URI troppo lungo&quot; (per [RFC 7231](https://datatracker.ietf.org/doc/html/rfc72316.5.12)). Come soluzione alternativa, puoi cambiare il tuo GET in POST, aggiungere `_method=GET` e inserire la stringa di query nel corpo della richiesta.

```
GET /rest/v1/programs/{programId}/members.json?filterType=statusName&filterValues=Influenced
```

```json
{
    "requestId": "109da#17915eec072",
    "result": [
        {
            "seq": 0,
            "leadId": 1789,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 1,
            "leadId": 1790,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 2,
            "leadId": 1791,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 3,
            "leadId": 1792,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 4,
            "leadId": 1793,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 5,
            "leadId": 1794,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 6,
            "leadId": 1795,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 7,
            "leadId": 1796,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 8,
            "leadId": 1797,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 9,
            "leadId": 1798,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 10,
            "leadId": 1799,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        },
        {
            "seq": 11,
            "leadId": 1800,
            "reachedSuccess": true,
            "programId": 1044,
            "acquiredBy": true,
            "membershipDate": "2020-01-08T18:10:26Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Crea e aggiorna

Due endpoint supportano l&#39;operazione di creazione/aggiornamento per i membri del programma. Uno consente di aggiornare solo lo stato del membro del programma. L&#39;altro consente di aggiornare il set di campi dei membri del programma contrassegnati come &quot;aggiornabili&quot;. Entrambi gli endpoint consentono di modificare fino a 300 record dei membri del programma per chiamata.

### Stato membro programma

Il [Sincronizza stato membro programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) endpoint utilizzato per creare o aggiornare lo stato del programma per uno o più membri.

Il valore richiesto `programId` parametro path specifica il programma contenente i membri da creare o aggiornare.

Il valore richiesto `statusName` Il parametro specifica lo stato del programma da applicare a un elenco di lead. StatusName deve corrispondere a uno stato disponibile per il canale del programma. Gli stati validi possono essere recuperati utilizzando [Ottieni canali](https://developer.adobe.com/marketo-apis/api/asset/#tag/Channels/operation/getAllChannelsUsingGET) endpoint. Se lo stato di un lead ha un valore di incremento maggiore rispetto al valore statusName designato, il lead verrà ignorato.

Il valore richiesto `input` il parametro è un array di `leadId` che corrispondono ai membri del programma. Puoi inviare fino a 300 leadID per chiamata. Su ogni record viene eseguita un&#39;operazione upsert. Se il leadId è associato a un membro del programma, lo stato di appartenenza viene aggiornato. In caso contrario, viene creato un nuovo record membro del programma, il record viene associato al leadId e viene assegnato lo stato di appartenenza.

L’endpoint risponde con un `status` di &quot;updated&quot;, &quot;created&quot; o &quot;skipped&quot;. Se ignorato, un `reasons` sarà incluso anche l’array. L’endpoint risponderà anche con un `seq` che è un indice che può essere utilizzato per correlare i record inviati all’ordine della risposta.

Se la chiamata ha esito positivo, nel registro attività del lead viene scritta un’attività &quot;Modifica stato programma&quot;.

```
POST /rest/v1/programs/{programId}/members/status.json
```

```
Content-Type: application/json
```

```json
{
    "statusName":"Influenced",
    "input":[
        {
            "leadId": 1800
        },
        {
            "leadId": 1801
        },
        {
            "leadId": 1235
        }
    ]
}
```

```json
{
    "requestId": "14b2d#17916378ec5",
    "result": [
        {
            "seq": 0,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead skipped because it is already in or past this status"
                }
            ]
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1801
        },
        {
            "seq": 2,
            "status": "created",
            "leadId": 1235
        }
    ],
    "success": true
}
```

### Dati membro programma

Il [Sincronizza dati membro programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) l&#39;endpoint viene utilizzato per aggiornare i dati del campo del membro del programma per uno o più membri. È possibile modificare qualsiasi campo personalizzato o campo standard &quot;aggiornabile&quot; (vedere [Descrivi membro programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) endpoint).

Il valore richiesto `programId` parametro path specifica il programma contenente i membri da aggiornare.

Il valore richiesto `input` Il parametro è un array. Ogni elemento array contiene un `leadId` e uno o più campi da aggiornare (utilizzando il nome API). Su ogni record viene eseguita un&#39;operazione di aggiornamento. Il leadId deve essere associato a un membro del programma. I campi devono essere aggiornabili. Puoi inviare fino a 300 leadID per chiamata.

L’endpoint risponde con un `status` di &quot;aggiornato&quot; o &quot;saltato&quot;. Se ignorato, un `reasons` sarà incluso anche l’array. L’endpoint risponderà anche con un `seq` che è un indice che può essere utilizzato per correlare i record inviati all’ordine della risposta.

Se la chiamata ha esito positivo, nel registro attività del lead viene scritta un’attività &quot;Modifica dati membro programma&quot;.

```
POST /rest/v1/programs/{programId}/members.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1789,
            "registrationCode": "dcff5f12-a7c7-11eb-bcbc-0242ac130002"
        },
        {
            "leadId": 1790,
            "registrationCode": "c0404b78-d3fd-47bf-82c4-d16f3852ab3a"
        },
        {
            "leadId": 1003,
            "registrationCode": "aa880c57-75b8-426b-a33a-fbf6302d7cb4"
        }
    ]
}
```

```json
{
    "requestId": "edc3#1791659b8d2",
    "result": [
        {
            "seq": 0,
            "status": "updated",
            "leadId": 1789
        },
        {
            "seq": 1,
            "status": "updated",
            "leadId": 1790
        },
        {
            "seq": 2,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1013",
                    "message": "Membership not found"
                }
            ]
        }
    ],
    "success": true
}
```

## Campi

L&#39;oggetto membro del programma contiene campi standard e campi personalizzati facoltativi. I campi standard sono presenti in ogni abbonamento al Marketo Engage, mentre i campi personalizzati vengono creati dall’utente in base alle esigenze. Ogni definizione di campo è composta da un insieme di attributi che descrivono il campo. Esempi di attributi sono nome visualizzato, nome API e dataType. Questi attributi sono noti collettivamente come metadati.

I seguenti endpoint consentono di eseguire query, creare e aggiornare campi nell&#39;oggetto membro del programma. Queste API richiedono che l’utente API proprietario abbia un ruolo con una o entrambe le **Campo standard schema di lettura-scrittura** o **Campo personalizzato schema di lettura-scrittura** autorizzazioni.

### Campi query

La query dei campi dei membri del programma è semplice. È possibile eseguire una query su un singolo campo membro del programma in base al nome API o sul set di tutti i campi membri del programma. È possibile recuperare sia i campi standard che i campi personalizzati, a seconda delle autorizzazioni del ruolo utilizzate. Vengono recuperati anche i campi nascosti.

#### Per nome

Il [Ottieni campo membro programma per nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) l&#39;endpoint recupera i metadati per un singolo campo nell&#39;oggetto membro del programma. Il valore richiesto `fieldApiName` Il parametro path specifica il nome API del campo. La risposta è simile all’endpoint Descrivi membro del programma, ma contiene metadati aggiuntivi come `isCustom` attributo che indica se il campo è un campo personalizzato.

```
GET /rest/v1/programs/members/schema/fields/{fieldApiName}.json
```

```json
{
    "requestId": "15416#17e955554de",
    "result": [
        {
            "displayName": "Status",
            "name": "statusName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Sfogliare

Il [Recupera campi membri del programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) l&#39;endpoint recupera i metadati per tutti i campi nell&#39;oggetto membro del programma. Per impostazione predefinita, vengono restituiti al massimo 300 record. È possibile utilizzare `batchSize` parametro di query per ridurre questo numero. Se il `moreResult` è true, il che significa che sono disponibili più risultati. Continua a chiamare questo endpoint fino a quando l’attributo moreResult restituisce false, il che significa che non sono disponibili risultati. Il `nextPageToken` restituiti da questa API devono sempre essere riutilizzati per la successiva iterazione di questa chiamata.

```
GET /rest/v1/programs/members/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "102f6#17e9557f123",
    "result": [
        {
            "displayName": "Acquired By",
            "name": "acquiredBy",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Cadence",
            "name": "nurtureCadence",
            "description": null,
            "dataType": "string",
            "length": 4,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Nurture Exhausted",
            "name": "isExhausted",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Member Date",
            "name": "membershipDate",
            "description": null,
            "dataType": "datetime",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Program",
            "name": "program",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "BC7J6EPVLT6T4B5FKUU3APCYN4======",
    "moreResult": true
}
```

### Crea campi

Il [Crea campi membri del programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) endpoint crea uno o più campi personalizzati nell&#39;oggetto membro del programma. Questo endpoint fornisce funzionalità paragonabili a quelle [disponibile nell’interfaccia utente del Marketo Engage](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Puoi creare fino a 20 campi personalizzati utilizzando questo endpoint.

Considera attentamente ogni campo creato nell’istanza di produzione del Marketo Engage utilizzando l’API. Una volta creato un campo, non è più possibile eliminarlo ([puoi solo nasconderlo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo)). La proliferazione di campi inutilizzati è una pratica scorretta che aggiunge confusione all’istanza.

Il valore richiesto `input` Il parametro è un array di oggetti campo membro del programma. Ogni oggetto contiene uno o più attributi. Gli attributi richiesti sono `displayName`, `name`, e `dataType` che corrispondono rispettivamente al nome visualizzato dell’interfaccia utente del campo, al nome API del campo e al tipo di campo. Facoltativamente puoi specificare `description`, `isHidden`, `isHtmlEncodingInEmail`,e `isSensitive`.

Sono presenti alcune regole associate a `name` e `displayName` denominazione. Il `name` l&#39;attributo deve essere univoco, iniziare con una lettera e contenere solo lettere, numeri o trattini bassi. Il *`isplayName` deve essere univoco e non può contenere caratteri speciali. È necessario applicare una convenzione di denominazione comune [portamonete](https://en.wikipedia.org/wiki/Camel_case#) a `displayName` per produrre `name`. Ad esempio, un `displayName` di &quot;Campo personalizzato&quot; produrrebbe un `name` di &quot;myCustomField&quot;.

```
POST /rest/v1/programs/members/schema/fields.json
```

```json
{
  "input": [
    {
        "displayName": "PMCF Custom Field 03",
        "name": "pMCFCustomField03",
        "description": "My third custom field",
        "dataType": "string"
    }
  ]
}
```

```json
{
    "requestId": "13a7#17e955fcb44",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "created"
        }
    ],
    "success": true
}
```

### Aggiorna campo

Il [Aggiorna campo membro del programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) l&#39;endpoint aggiorna un singolo campo personalizzato sull&#39;oggetto membro del programma. In genere, le operazioni di aggiornamento dei campi eseguite utilizzando l’interfaccia utente del Marketo Engage sono ottenibili utilizzando l’API. Nella tabella seguente sono riepilogate alcune differenze.

| Attributo | Aggiornabile tramite API? | Aggiornabile dall’interfaccia utente? | Aggiornabile tramite API? | Aggiornabile dall’interfaccia utente? |
|---|---|---|---|---|
| dataType | no | no | no | sì |
| descrizione | sì | sì | sì | sì |
| displayName | no | no | sì | sì |
| isCustom | no | no | no | no |
| isHidden | no | sì | yes (se creato dall’API) | sì |
| isHtmlEncodingInEmail | sì | sì | sì | sì |
| isSensitive | sì | sì | sì | sì |
| length | no | no | no | no |
| name | no | no | no | no |

Il valore richiesto `fieldApiName` Il parametro path specifica il nome API del campo da aggiornare. Il valore richiesto `input` Il parametro è un array che contiene un singolo oggetto campo lead. L&#39;oggetto field contiene uno o più attributi.

```
POST /rest/v1/programs/members/schema/fields/pMCFCustomField03.json
```

```json
{
  "input": [
      {
        "displayName": "Lunch Preference",
        "description": "Attendee food preference",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

```json
{
    "requestId": "215f#17e95663955",
    "result": [
        {
            "name": "pMCFCustomField03",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Elimina

Il [Elimina membri del programma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/deleteProgramMemberUsingPOST) l&#39;endpoint viene utilizzato per eliminare i record dei membri del programma. Il valore richiesto `programId` parametro path specifica il programma contenente i membri da eliminare. Il corpo della richiesta contiene un `input` array di id lead. È consentito un massimo di 300 ID lead per chiamata.

L’endpoint risponde con un `status` di &quot;eliminato&quot; o &quot;saltato&quot;. Se ignorato, un `reasons` sarà incluso anche l’array. L’endpoint risponderà anche con un `seq` che è un indice che può essere utilizzato per correlare i record inviati all’ordine della risposta.

```
POST /rest/v1/programs/{programId}/members/delete.json
```

```
Content-Type: application/json
```

```json
{
    "input":[
        {
            "leadId": 1235
        },
        {
            "leadId": 77
        }
    ]
}
```

```json
{
    "requestId": "302a#17916619417",
    "result": [
        {
            "seq": 0,
            "status": "deleted",
            "leadId": 1235
        },
        {
            "seq": 1,
            "status": "skipped",
            "reasons": [
                {
                    "code": "1037",
                    "message": "Lead not in program"
                }
            ]
        }
    ],
    "success": true
}
```
