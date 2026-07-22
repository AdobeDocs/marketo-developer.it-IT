---
title: Database lead
feature: REST API, Database
description: Guida alle API del database lead di Marketo che tratta gli oggetti, i metodi CRUD e Describe, i modelli di query, i limiti batch e le restrizioni di integrazione CRM.
exl-id: e62e381f-916b-4d56-bc3d-0046219b68d3
TQID: https://experienceleague.adobe.com/7lGbhE92lvIE-XkMyUIaK9GrreZVRdM-WVZTpHARhxE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1058
ht-degree: 1%

---

# Database lead

Le API del database lead di Marketo scambiano dati personali e relativi a persone con Marketo. Questi dati includono attività, opportunità e aziende.

## Oggetti

Il database lead include i seguenti oggetti:

- Lead
- Aziende/Account
- Account denominati
- Opportunità
- OpportunityRoles
- Venditori
- Oggetti personalizzati
- Attività
- Iscrizione a elenco e programma

La maggior parte degli oggetti del database lead supportano i metodi Create, Read, Update e Delete. Il metodo Describe fornisce i campi disponibili per ogni tipo di oggetto. Per gli oggetti non lead, identifica anche i campi utilizzati per la deduplicazione e i campi ricercabili durante il recupero dei record.

Gli oggetti lead supportano la più ampia serie di funzionalità, in quanto i lead hanno la più ampia varietà di utilizzi nelle applicazioni Marketo.

## API

Per un elenco completo degli endpoint API del database lead, dei parametri e delle informazioni di modellazione, vedere [Riferimento endpoint API del database lead](https://developer.adobe.com/marketo-apis/api/mapi).

Quando un&#39;istanza ha un&#39;integrazione Microsoft Dynamics nativa o CRM Salesforce.com, le API Società, Opportunità, Ruolo opportunità e Persona di vendita sono disabilitate. Il CRM gestisce questi record, pertanto non puoi accedervi o aggiornarli tramite le API di Marketo.

- Dimensione massima batch (standard): 300 record
- Dimensione batch massima (in blocco): 10 MB di file
- Dimensione batch predefinita: 300 record
- Intestazione content-type (standard): application/json
- Intestazione tipo di contenuto (in blocco): multipart/form-data

## Descrivere

L’API Describe è disponibile per lead, società, opportunità, ruoli, persone di vendita e oggetti personalizzati. Utilizzarlo per recuperare i metadati dell&#39;oggetto e i campi disponibili per gli aggiornamenti e le query.

Ad eccezione di Descrive i lead, ogni endpoint Descrive restituisce:

- `dedupeFields`: chiavi disponibili per la deduplicazione.
- `searchableFields`: chiavi disponibili per le query.

```http
GET /rest/v1/opportunities/roles/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunityRole",
         "displayName":"Opportunity Role",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId",
            "leadId",
            "role"
         ],
         "searchableFields":[
            [
               "externalOpportunityId",
               "leadId",
               "role"
            ],
            [
               "marketoGUID"
            ],
            [
               "leadId"
            ],
            [
               "externalOpportunityId"
            ]
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"leadId",
               "displayName":"Lead Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"role",
               "displayName":"Role",
               "dataType":"string",
               "length":50,
               "updateable":false
            },
            {
               "name":"isPrimary",
               "displayName":"Is Primary",
               "dataType":"boolean",
               "updateable":true
            },
            {
               "name":"externalCreatedDate",
               "displayName":"External Created Date",
               "dataType":"datetime",
               "updateable":true
            }
         ]
      }
   ]
}
```

In questo esempio, `dedupeFields` è una chiave composta. Quando si utilizza la modalità `dedupeFields` per le creazioni e gli aggiornamenti futuri, includere `externalOpportunityId`, `leadId` e `role` per ogni ruolo.

L&#39;array `searchableFields` elenca i campi disponibili per l&#39;esecuzione di query sui record dei ruoli. Questo elenco include la chiave composta di `externalOpportunityId`, `leadId` e `role`.

Il parametro di risposta `fields` fornisce le seguenti informazioni per ciascun campo:

- Nome.
- `displayName` come mostrato nell&#39;interfaccia utente di Marketo.
- Tipo di dati.
- Indica se il campo può essere aggiornato dopo la creazione.
- Lunghezza del campo, se applicabile.

## Query

Gli oggetti del database lead condividono un modello di query di base per le chiavi semplici che fanno riferimento a un campo.

```http
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Per tutti gli oggetti ad eccezione dei lead, selezionare `{field to query}` da `searchableFields` nella corrispondente risposta Describe. Fornisci un elenco separato da virgole contenente un massimo di 300 valori.

Puoi anche includere i seguenti parametri di query facoltativi:

- `batchSize`: numero intero che specifica il numero di risultati da restituire. Il valore predefinito e massimo è 300.
- `nextPageToken`: token restituito da una chiamata precedente per il paging. Per ulteriori informazioni, vedere [Token di paging](paging-tokens.md).
- `fields`: elenco di nomi di campo separati da virgole da restituire per ogni record. Per i campi validi, vedi la descrizione corrispondente. Se si richiede un campo che non viene restituito, il relativo valore sarà nullo.
- `_method`: invia le query utilizzando il metodo HTTP POST. Per informazioni sull&#39;utilizzo, vedere la sezione _method=GET.

L’esempio seguente interroga le opportunità:

```http
GET /rest/v1/opportunities.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

`filterType` in questa chiamata è &quot;idField&quot; e non &quot;marketoGUID&quot;. Sia &quot;idField&quot; che &quot;dedupeFields&quot; sono casi speciali che consentono di utilizzare un alias per il campo o i campi corrispondenti. Anche se la chiamata non imposta esplicitamente &quot;marketoGUID&quot;, rimane il campo di ricerca.

I campi o i set di campi identificati da `idField` e `dedupeFields` in una descrizione dell&#39;oggetto sono sempre validi `filterTypes` per una query. Questa chiamata restituisce record che corrispondono ai GUID in filterValues. Se nessun record corrisponde, la risposta indica che l&#39;operazione è riuscita e restituisce una matrice di risultati vuota.

Se il set di record corrispondente supera il valore 300 o `batchSize` specificato, a seconda di quale valore sia inferiore, la risposta include `moreResult` con valore true e `nextPageToken`. Includi il token in una chiamata successiva per recuperare più record. Per ulteriori informazioni, vedere [Token di paging](paging-tokens.md).

### URI lunghi

Un URI può superare il limite di 8 KB del servizio REST, ad esempio quando si esegue una query tramite GUID. In questo caso, utilizzare il metodo HTTP POST invece di GET e aggiungere il parametro di query `_method=GET`.

Passa i parametri di query rimanenti nel corpo POST come stringa &quot;application/x-www-form-urlencoded&quot;. Passa anche l’intestazione Content-type associata.

```http
POST /rest/v1/opportunities.json?_method=GET
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Il parametro `_method=GET` è necessario anche quando si esegue una query sulle chiavi composte.

### Tasti composti

Per eseguire una query su una chiave composta, invia una richiesta POST con un corpo JSON. Utilizzare questo modello solo quando `filterType` è un&#39;opzione `dedupeFields` con più campi.

Le chiavi composte sono attualmente utilizzate solo dai ruoli opportunità e da alcuni oggetti personalizzati. L&#39;esempio seguente esegue una query sui ruoli opportunità con la chiave composta di `dedupeFields`:

```http
POST /rest/v1/opportunities/roles.json?_method=GET
```

```json
{
   "filterType":"dedupeFields",
   "fields":[
      "marketoGuid",
      "externalOpportunityId",
      "leadId",
      "role"
   ],
   "input":[
      {
        "externalOpportunityId":"Opportunity1",
        "leadId": 1,
        "role": "Captain"
      },
      {
        "externalOpportunityId":"Opportunity2",
        "leadId": 1872,
        "role": "Commander"
      },
      {
        "externalOpportunityId":"Opportunity3",
        "leadId": 273891,
        "role": "Lieutenant Commander"
      }
   ]
}
```

L&#39;oggetto JSON accetta tutti i parametri di query utilizzati per le query con chiave semplice eccetto `filterValues`. Invece di `filterValues`, fornisci un array &quot;input&quot; di oggetti JSON. Ogni oggetto deve includere ogni campo nella chiave composta. In questo esempio, i campi sono `externalOpportunityId`, `leadId` e `role`.

La richiesta esegue query su `roles` in base agli input forniti e restituisce risultati corrispondenti. Se la risposta include `moreResult=true` e un `nextPageToken`, includi tutti gli input originali e `nextPageToken` nella richiesta successiva.

## Crea e aggiorna

Crea e aggiorna i record del database dei lead inviando richieste POST con corpi JSON. Opportunità, ruoli, oggetti personalizzati, società e addetti alle vendite utilizzano la stessa interfaccia. I lead utilizzano un’interfaccia diversa, descritta nella documentazione dei lead.

L&#39;unico parametro obbligatorio è `input`, un array di un massimo di 300 oggetti. Ogni oggetto contiene i campi da inserire o aggiornare.

È inoltre possibile includere i seguenti parametri facoltativi:

- `action`: accetta `createOnly`, `updateOnly` o `createOrUpdate`. Se omesso, la modalità predefinita sarà `createOrUpdate`.
- `dedupeBy`: accetta `idField` o `dedupeFields` quando l&#39;azione è impostata su createOnly o `createOrUpdate`. Se omesso, la modalità predefinita sarà `dedupeFields`.

Quando `dedupeBy` è `idField`, i `idField` elencati nella descrizione vengono utilizzati per la deduplicazione e devono essere inclusi in ogni record. Modalità `idField` non compatibile con la modalità `createOnly`.

Quando `dedupeBy` è `dedupeFields`, includere ogni campo `dedupeFields` elencato nella descrizione dell&#39;oggetto in ogni record.

Quando si passano i valori dei campi, il database scrive un valore di `null` o una stringa vuota come `null`.

```http
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

Ad eccezione dell&#39;API Leads, le chiamate di creazione e aggiornamento restituiscono un campo `seq` in ogni oggetto nell&#39;array `result`. Il numero corrisponde alla posizione del record aggiornato nella richiesta.

Ogni risultato restituisce anche il valore `idField` del tipo di oggetto e un `status` di &quot;creato&quot;, &quot;aggiornato&quot; o &quot;saltato&quot;. Se lo stato viene ignorato, il risultato include una matrice &quot;reason&quot;. Ogni oggetto motivo contiene un codice e un messaggio che spiegano perché il record è stato ignorato. Per ulteriori informazioni, vedere [codici di errore](error-codes.md).

### Elimina

Ad eccezione dei lead, gli oggetti del database lead utilizzano un&#39;interfaccia di eliminazione standard. Oltre all&#39;input, l&#39;unico parametro obbligatorio è `deleteBy,` che accetta idField o dedupeFields.

Nell&#39;esempio seguente vengono eliminati gli oggetti personalizzati:

```http
POST /rest/v1/customobjects/{name}/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "vin":"19UYA31581L000000"
      },
      {
         "vin":"29UYA31581L000000"
      },
      {
         "vin":"39UYA31581L000000"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq":1,
         "marketoGUID":"da42707c-4dc4-4fc1-9fef-f30a3017240a",
         "status": "deleted"
      },
      {
         "seq":2,
         "status": "skipped"
         "reasons":[
            {
               "code":"1013",
               "message":"Object not found"
            }
         ]
      }
   ]
}
```

La risposta include `seq`, `status` e `marketoGUID`. Per i record ignorati, include anche `reasons`.

Per informazioni dettagliate sulle operazioni CRUD per un tipo di oggetto specifico, vedere la documentazione relativa a tale oggetto.
