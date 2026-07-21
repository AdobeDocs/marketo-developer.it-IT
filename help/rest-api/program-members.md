---
title: Membri del programma
feature: REST API
description: Utilizza l’API REST di Marketo per leggere, creare, aggiornare ed eliminare i membri del programma, gestire i campi standard e personalizzati ed eseguire query utilizzando campi ricercabili.
exl-id: 22f29a42-2a30-4dce-a571-d7776374cf43
TQID: https://experienceleague.adobe.com/scEHyXYq9C7cCS1kIX810wG7ahT9fsa448NwIfBmzQM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1670
ht-degree: 2%

---

# Membri del programma

[Riferimento endpoint membri programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members)

Marketo fornisce API per la lettura, la creazione, l’aggiornamento e l’eliminazione di record di membri del programma. Il campo ID lead collega i record dei membri del programma ai record dei lead.

Ogni record contiene campi standard e può contenere fino a 20 campi personalizzati. In questi campi vengono memorizzati i dati dei membri specifici del programma da utilizzare in moduli, filtri, trigger e azioni di flusso. Puoi visualizzare questi dati nella [scheda Membri](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/manage-and-view-members) del programma nell&#39;interfaccia utente di Marketo Engage.

## Descrivere

L&#39;endpoint [Descrivi membro del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) segue il modello standard per gli oggetti del database lead.

- L&#39;array `searchableFields` identifica i campi validi per le query.
- L&#39;array `fields` contiene metadati quali il nome API REST, il nome visualizzato e se il campo è aggiornabile.

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

## Query

Utilizzare l&#39;endpoint [Recupera membri del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMembersUsingGET) per recuperare i membri di un programma. La richiesta richiede un parametro di percorso `programId` e `filterType` e `filterValues` parametri di query.

`programId` specifica il programma da cercare.

`filterType` specifica il campo da utilizzare come filtro di ricerca. Accetta qualsiasi campo nell&#39;elenco &quot;searchableFields&quot; restituito dall&#39;endpoint [Descrivi membro del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2). Per un campo personalizzato, dataType deve essere &quot;string&quot; o &quot;integer&quot;.

Se filterType non è &quot;leadId&quot;, la richiesta può elaborare un massimo di 100.000 record di membri del programma. A seconda della configurazione dell’istanza di Marketo, viene visualizzato uno dei seguenti errori:

- Se il numero totale di membri del programma supera i 100.000, viene restituito un errore: &quot;1003, dimensione totale appartenenza: 100.001 supera il limite consentito di 100.000 per il filtro&quot;.
- Se il numero totale di membri del programma _che corrispondono al filtro_ supera i 100.000, viene restituito un errore: &quot;1003, dimensione appartenenza corrispondente: 100.001 supera il limite consentito (100.000) per questa API&quot;.

Per eseguire una query su un programma il cui numero di iscrizioni supera il limite, utilizzare l&#39;[API Estrazione membri programma in blocco](bulk-program-member-extract.md).

`filterValues` specifica i valori da cercare e accetta fino a 300 valori separati da virgola. La chiamata cerca i record in cui il campo del membro del programma corrisponde a uno dei valori filterValues inclusi.

In alternativa, filtrare per intervallo di date specificando `updatedAt` come filterType e fornendo i parametri datetime `startAt` e `endAt`. L’intervallo non può essere superiore a sette giorni. Utilizza il formato ISO-8601 senza millisecondi per i valori datetime.

Il parametro di query `fields` facoltativo accetta un elenco separato da virgole di nomi API di campo restituiti dall&#39;endpoint [Descrivi membro del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2). Se incluso, ogni record di risposta contiene i campi specificati. Se omessa, la risposta restituisce `acquiredBy`, `leadId`, `membershipDate`, `programId` e `reachedSuccess` per impostazione predefinita.

Per impostazione predefinita, l&#39;endpoint restituisce un massimo di 300 record. Utilizzare il parametro di query `batchSize` per ridurre questo numero.

Se l&#39;attributo **moreResult** è true, sono disponibili altri risultati. Continuare a chiamare l&#39;endpoint con `nextPageToken` restituito finché moreResult non è false.

Se la lunghezza totale della richiesta GET supera gli 8 KB, l’endpoint restituisce l’errore HTTP &quot;414, URI troppo lungo&quot;. Per aggirare questo limite, modifica la richiesta da GET a POST, aggiungi il parametro `_method=GET` e inserisci la stringa di query nel corpo della richiesta.

```http
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

Due endpoint supportano le operazioni di creazione e aggiornamento per i membri del programma:

- Un endpoint aggiorna solo lo stato del membro del programma.
- Un endpoint aggiorna i campi dei membri del programma contrassegnati come &quot;aggiornabili&quot;.

Ogni endpoint può modificare fino a 300 record di membri del programma per chiamata.

### Stato membro programma

Utilizzare l&#39;endpoint [Stato membro programma di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberStatusUsingPOST) per creare o aggiornare lo stato del programma per uno o più membri.

I parametri richiesti sono:

- `programId`: parametro di percorso che specifica il programma contenente i membri da creare o aggiornare.
- `statusName`: specifica lo stato del programma da applicare a un elenco di lead. StatusName deve corrispondere a uno stato disponibile per il canale del programma. Recupera gli stati validi con l&#39;endpoint [Get Channels](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels/operation/getAllChannelsUsingGET). Se lo stato di un lead ha un valore di incremento maggiore rispetto al valore statusName designato, la richiesta ignora tale lead.
- `input`: matrice di `leadId` valori che corrispondono ai membri del programma. Puoi inviare fino a 300 leadID per chiamata.

L’endpoint esegue un upsert su ciascun record. Se il leadId è associato a un membro del programma, l&#39;endpoint aggiorna lo stato di appartenenza. In caso contrario, crea un record membro del programma, associa il record al leadId e assegna lo stato di appartenenza.

La risposta include `status` di &quot;aggiornato&quot;, &quot;creato&quot; o &quot;saltato&quot;. Un risultato ignorato include anche un array `reasons`. Il campo `seq` è un indice che mette in correlazione ogni record inviato con l&#39;ordine di risposta.

Se la chiamata ha esito positivo, nel registro attività del lead viene scritta un’attività &quot;Modifica stato programma&quot;.

```http
POST /rest/v1/programs/{programId}/members/status.json
```

```text
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

Utilizza l&#39;endpoint [Sincronizza dati membro programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/syncProgramMemberDataUsingPOST) per aggiornare i dati del campo membro programma per uno o più membri. Puoi modificare qualsiasi campo personalizzato o qualsiasi campo standard contrassegnato come &quot;aggiornabile&quot; dall&#39;endpoint [Descrivi membro del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2).

I parametri richiesti sono:

- `programId`: parametro di percorso che specifica il programma contenente i membri da aggiornare.
- `input`: array i cui elementi contengono `leadId` e uno o più campi da aggiornare in base al nome API. Puoi inviare fino a 300 leadID per chiamata.

L’endpoint aggiorna ogni record. Il leadId deve essere associato a un membro del programma e ogni campo deve essere aggiornabile.

La risposta include `status` di &quot;aggiornato&quot; o &quot;ignorato&quot;. Un risultato ignorato include anche un array `reasons`. Il campo `seq` è un indice che mette in correlazione ogni record inviato con l&#39;ordine di risposta.

Se la chiamata ha esito positivo, nel registro attività del lead viene scritta un’attività &quot;Modifica dati membro programma&quot;.

```http
POST /rest/v1/programs/{programId}/members.json
```

```text
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

L&#39;oggetto membro del programma contiene campi standard e campi personalizzati facoltativi. I campi standard sono presenti in ogni abbonamento a Marketo Engage, mentre gli utenti creano campi personalizzati in base alle esigenze.

Ogni campo è definito da attributi quali il nome visualizzato, il nome API e il dataType. Insieme, questi attributi sono denominati metadati.

I seguenti endpoint eseguono query, creano e aggiornano campi sull&#39;oggetto membro del programma. L&#39;utente API deve avere un ruolo con l&#39;autorizzazione **Campo standard dello schema di lettura-scrittura**, l&#39;autorizzazione **Campo personalizzato dello schema di lettura-scrittura** o entrambi.

### Campi query

Eseguire una query su un campo membro del programma in base al nome API o recuperare tutti i campi membri del programma. Le autorizzazioni del ruolo determinano se la risposta può includere campi standard, campi personalizzati o entrambi. La risposta include anche campi nascosti.

#### Per nome

L&#39;endpoint [Recupera campo membro del programma per nome](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldByNameUsingGET) recupera i metadati per un campo nell&#39;oggetto membro del programma. Il parametro di percorso `fieldApiName` richiesto specifica il nome API del campo.

La risposta è simile alla risposta Descrivi membro del programma, ma include metadati aggiuntivi. Ad esempio, l&#39;attributo `isCustom` indica se il campo è personalizzato.

```http
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

#### Sfoglia

L&#39;endpoint [Get Program Member Fields](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/getProgramMemberFieldsUsingGET) recupera i metadati per tutti i campi nell&#39;oggetto membro del programma. Per impostazione predefinita, restituisce un massimo di 300 record. Utilizzare il parametro di query `batchSize` per ridurre questo numero.

Se l&#39;attributo `moreResult` è true, sono disponibili altri risultati. Continuare a chiamare l&#39;endpoint con `nextPageToken` restituito finché moreResult non è false.

```http
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

L&#39;endpoint [Crea campi membri del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/createProgramMemberFieldUsingPOST) crea campi personalizzati sull&#39;oggetto membro del programma. Offre funzionalità paragonabili a quelle della [interfaccia utente di Marketo Engage](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/program-member-custom-fields). Con questo endpoint è possibile creare fino a 20 campi personalizzati.

Considera attentamente ogni campo prima di crearlo in un’istanza Marketo Engage di produzione. Dopo aver creato un campo, non puoi eliminarlo; [puoi solo nasconderlo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/delete-a-custom-field-in-marketo). I campi inutilizzati rendono l’istanza più complessa.

Il parametro obbligatorio `input` è un array di oggetti campo membro del programma. Ogni oggetto contiene uno o più attributi.

- Gli attributi richiesti sono `displayName`, `name` e `dataType`. Corrispondono rispettivamente al nome visualizzato dell’interfaccia utente, al nome API e al tipo di campo.
- Gli attributi facoltativi sono `description`, `isHidden`, `isHtmlEncodingInEmail` e `isSensitive`.

Gli attributi `name` e `displayName` hanno le seguenti regole di denominazione:

- L&#39;attributo `name` deve essere univoco, iniziare con una lettera e contenere solo lettere, numeri o trattini bassi.
- *`isplayName` deve essere univoco e non può contenere caratteri speciali.

Una convenzione comune consiste nell&#39;applicare [camel case](https://en.wikipedia.org/wiki/Camel_case#) a `displayName` per produrre `name`. Ad esempio, un `displayName` di &quot;Campo personalizzato&quot; produce un `name` di &quot;myCustomField&quot;.

```http
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

L&#39;endpoint [Aggiorna campo membro del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/updateProgramMemberFieldUsingPOST) aggiorna un campo personalizzato nell&#39;oggetto membro del programma. La maggior parte degli aggiornamenti dei campi disponibili nell’interfaccia utente di Marketo Engage sono disponibili anche tramite l’API. Nella tabella seguente sono riepilogate le differenze.

| Attributo | Aggiornabile tramite API? | Aggiornabile dall’interfaccia utente? | Aggiornabile tramite API? | Aggiornabile dall’interfaccia utente? |
| --- | --- | --- | --- | --- |
| dataType | no | no | no | sì |
| descrizione | sì | sì | sì | sì |
| displayName | no | no | sì | sì |
| isCustom | no | no | no | no |
| isHidden | no | sì | yes (se creato dall’API) | sì |
| isHtmlEncodingInEmail | sì | sì | sì | sì |
| isSensitive | sì | sì | sì | sì |
| length | no | no | no | no |
| name | no | no | no | no |

La richiesta richiede i seguenti parametri:

- `fieldApiName`: parametro di percorso che specifica il nome API del campo da aggiornare.
- `input`: array contenente un oggetto campo lead con uno o più attributi.

```http
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

Utilizza l&#39;endpoint [Elimina membri del programma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/deleteProgramMemberUsingPOST) per eliminare i record dei membri del programma. Il parametro di percorso `programId` richiesto specifica il programma contenente i membri da eliminare.

Il corpo della richiesta contiene una matrice `input` di ID lead. Ogni chiamata consente un massimo di 300 ID lead.

La risposta include `status` di &quot;eliminato&quot; o &quot;ignorato&quot;. Un risultato ignorato include anche un array `reasons`. Il campo `seq` è un indice che mette in correlazione ogni record inviato con l&#39;ordine di risposta.

```http
POST /rest/v1/programs/{programId}/members/delete.json
```

```text
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
