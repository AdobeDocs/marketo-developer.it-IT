---
title: "Database lead"
feature: REST API, Database
description: "Manipolare il database principale dei lead."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 0%

---


# Database lead

Le API del database lead di Marketo sono le API utilizzate più di frequente fornite da Marketo in quanto consentono lo scambio di dati personali e relativi a persone da Marketo, come attività, opportunità e aziende.

## Oggetti

Gli oggetti del database lead includono:

- Lead
- Aziende/Account
- Account denominati
- Opportunità
- OpportunityRoles
- Venditori
- Oggetti personalizzati
- Attività
- Iscrizione a elenco e programma

La maggior parte di questi oggetti include almeno i metodi Create, Read, Update e Delete. È inoltre incluso un metodo &quot;Descrivi&quot; che fornisce un elenco di campi disponibili per ciascun tipo, un elenco di campi utilizzati per la deduplicazione (per gli oggetti non lead) e quali campi possono essere cercati per recuperare i record. Il set più ricco viene fornito per i lead, in quanto dispongono della più ampia varietà di funzionalità all’interno delle applicazioni Marketo.

## API

Per un elenco completo degli endpoint API del database lead, inclusi i parametri e le informazioni di modellazione, vedere [Riferimento endpoint API database lead](https://developer.adobe.com/marketo-apis/api/mapi/).

Per le istanze con un’integrazione CRM nativa abilitata (Microsoft Dynamics o Salesforce.com), le API Società, Opportunità, Ruolo opportunità e Persona di vendita sono disabilitate. Se abilitati, i record vengono gestiti tramite il sistema di gestione delle relazioni con i clienti e non sono accessibili o aggiornabili tramite le API di Marketo.

- Dimensione massima batch (standard): 300 record
- Dimensione batch massima (in blocco): 10 MB di file
- Dimensione batch predefinita: 300 record
- Intestazione content-type (standard): application/json
- Intestazione tipo di contenuto (in blocco): multipart/form-data

## Descrivere

Per lead, società, opportunità, ruoli, persone di vendita e oggetti personalizzati, viene fornita un’API descrittiva. Con questa chiamata vengono recuperati i metadati per l’oggetto e un elenco di campi disponibili per l’aggiornamento e l’esecuzione di query. La descrizione è una parte fondamentale della progettazione di una corretta integrazione con Marketo. Fornisce metadati avanzati su come è possibile interagire con gli oggetti e non, nonché su come crearli, aggiornarli ed eseguire query. A parte Descrivi lead, ciascuno di questi restituisce un elenco di chiavi disponibili per `deduplication` nel `dedupeFields` parametro di risposta. Un elenco di campi è disponibile come chiavi per l’esecuzione di query nel `searchableFields` parametro di risposta.

```
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

In questo esempio, `dedupeFields` è una chiave composta. Ciò significa che in aggiornamenti e creazioni futuri, quando si utilizza `dedupeFields` modalità, è necessario includere tutti e tre `externalOpportunityId`, `leadId`, e `role` per ogni ruolo. Il `searchableFields` array, fornisce anche l’elenco dei campi disponibili per l’esecuzione di query sui record dei ruoli. Questo include anche la chiave composta di `externalOpportunityId`, `leadId`, e `role`.

Esiste anche un parametro di risposta dei campi, che fornirà il nome di ciascun campo, il `displayName` come appare nell’interfaccia utente di Marketo, il tipo di dati del campo, se può essere aggiornato dopo la creazione e la lunghezza del campo, se applicabile.

## Query

Tutti gli oggetti del database lead condividono lo schema di base per l&#39;esecuzione di query su chiavi semplici, in cui viene fatto riferimento a un solo campo.

```
GET /rest/v1/{type}.json?filterType={field to query}&filterValues={comma-separated list of possible values}
```

Per tutti gli oggetti ad eccezione dei lead, puoi selezionare il campo {field to query} dai searchableFields della chiamata di descrizione corrispondente e comporre un elenco separato da virgole con un massimo di 300 valori. Sono inoltre disponibili i seguenti parametri di query facoltativi:

- `batchSize` - Numero intero del numero di risultati da restituire. I valori predefiniti e massimi sono 300.
- `nextPageToken` - Token restituito da una chiamata precedente per il paging. Consulta [Token di paging](paging-tokens.md) per maggiori dettagli.
- `fields` : elenco separato da virgole di nomi di campo da restituire per ogni record. Per un elenco dei campi validi, vedi la descrizione corrispondente. Se un particolare campo viene richiesto ma non restituito, il valore deve essere nullo.
- `_method` : utilizzato per l’invio di query tramite il metodo HTTP POST. Per informazioni sull&#39;utilizzo, vedere la sezione _method=GET riportata di seguito.

Per un rapido esempio, esaminiamo le opportunità di query:

```
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

Il `filterType` specificato in questa chiamata è &quot;idField&quot; e non &quot;marketoGUID&quot;. Questo e &quot;dedupeFields&quot; sono entrambi casi speciali, in cui il campo corrispondente all’idField o a dedupeFields può essere alias in questo modo. Il &quot;marketoGUID&quot; è ancora il campo di ricerca risultante nella chiamata, ma non è impostato in modo esplicito nella chiamata. I campi e/o i set di campi indicati dalla `idField` e `dedupeFields` di una descrizione di oggetto sarà sempre valida `filterTypes` per una query. Questa chiamata cerca i record corrispondenti ai GUID inclusi in filterValues e restituisce tutti i record corrispondenti. Se non è stato trovato alcun record utilizzando questo metodo, la risposta continuerà a indicare il successo, tuttavia l’array dei risultati sarà vuoto, poiché la ricerca è stata eseguita correttamente, ma non sono presenti record da restituire.

Se il set di record nella query supera i 300 o il `batchSize` specificato, a seconda di quale valore sia inferiore, la risposta ha un membro `moreResult` con valore true e un valore `nextPageToken`, che può essere incluso in una chiamata successiva per recuperare una maggiore quantità del set. Consulta [Token di paging](paging-tokens.md) per ulteriori dettagli.

### URI lunghi

Talvolta, ad esempio quando si esegue una query tramite GUID, l’URI potrebbe essere lungo e superare gli 8 KB consentiti dal servizio REST. In questo caso, è necessario utilizzare il metodo HTTP POST anziché GET e aggiungere un parametro di query `_method=GET`. Inoltre, gli altri parametri di query devono essere passati nel corpo del POST come stringa &quot;application/x-www-form-urlencoded&quot; e passare l’intestazione Content-type associata.

```
POST /rest/v1/opportunities.json?_method=GET
```

```
Content-Type: application/x-www-form-urlencoded
```

```
filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb,544fb7f5-2ddf-4fca-ae32-7e6ef1415e9f,f1ba41a2-69d1-4a35-9807-0e159d66f2c9,f7521272-3331-4a89-a768-222baff2f894
```

Oltre agli URI lunghi, questo parametro è necessario anche quando si esegue una query sulle chiavi composte.

### Tasti composti

Il pattern per l’esecuzione di query sulle chiavi composte è diverso dalle chiavi semplici, in quanto richiede l’invio di un POST con un corpo JSON. Ciò non è necessario in tutti i casi, solo in quelli in cui `dedupeFields` con più campi viene utilizzata come `filterType`. Attualmente le chiavi composte vengono utilizzate solo dai Ruoli opportunità e da alcuni oggetti personalizzati. Vediamo un esempio di query per i ruoli opportunità con la chiave composta da `dedupeFields`:

```
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

La struttura dell’oggetto JSON è prevalentemente semplice e tutti i parametri di query per le query con chiavi semplici sono membri validi, ad eccezione di `filterValues`. Invece del valore di filtro, esiste un array &quot;input&quot; di oggetti JSON, ciascuno dei quali deve avere un membro per ciascuno dei campi nella chiave composta; in questo caso, sono `externalOpportunityId`, `leadId`, e `role`. Questa operazione esegue una query per `roles`, rispetto agli input forniti e restituisce i risultati corrispondenti. Se la risposta restituisce un parametro con `moreResult=true`, e un `nextPageToken`, è necessario includere tutti gli input originali e `nextPageToken` affinché la query possa essere eseguita correttamente.

## Crea e aggiorna

Crea e aggiorna i record del database dei lead, tutti eseguiti tramite POST con corpi JSON. L&#39;interfaccia per Opportunità, Ruoli, Oggetti personalizzati, Aziende e VenditePersone è la stessa. L&#39;interfaccia del lead è un po&#39; diversa, e potete saperne di più nello specifico lì.

L’unico parametro obbligatorio è un array denominato `input` contenente fino a 300 oggetti, ciascuno con i campi che si desidera inserire o aggiornare come membri. Facoltativamente, puoi anche includere `action` parametro che può essere uno dei seguenti: `createOnly`, `updateOnly`, o `createOrUpdate`. Se l’azione viene omessa, per impostazione predefinita la modalità è `createOrUpdate`. `dedupeBy` è un altro parametro facoltativo che può essere utilizzato quando l’azione è impostata su createOnly o `createOrUpdate`. ` dedupeBy` può essere `idField`, o `dedupeFields`. Se `idField` è selezionato, quindi `idField` elencati nella descrizione vengono utilizzati per la deduplicazione e devono essere inclusi in ogni record. `idField` non è compatibile con `createOnly` modalità. Se `dedupeFields` sono selezionati , quindi il `dedupeFields` elencati nella descrizione dell&#39;oggetto utilizzata e ciascuno di essi deve essere incluso in ogni record. Se il `dedupeBy` viene omesso, la modalità predefinita è `dedupeFields`.

Quando trasmetti un elenco di valori di campo, un valore di `null`o una stringa vuota, viene scritta nel database come `null`.

```
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

A parte l’API lead, le chiamate per creare o aggiornare gli oggetti di database lead restituiscono un `seq` in ogni oggetto nel `result` array. Il numero elencato corrisponde all’ordine del record aggiornato nella richiesta effettuata. Ogni elemento restituisce il valore del `idField` per il tipo di oggetto e un `status`. Il campo di stato indica uno tra &quot;creato&quot;, &quot;aggiornato&quot; o &quot;saltato&quot;.  Se lo stato viene ignorato, verrà inoltre visualizzato un array &quot;reason&quot; corrispondente con uno o più oggetti reason che includono un codice e un messaggio che indica il motivo per cui un record è stato ignorato. Consulta [codici di errore](error-codes.md) per ulteriori dettagli.

### Elimina

L&#39;interfaccia per le eliminazioni è standard per gli oggetti del database lead oltre ai lead. Oltre all’input, è necessario specificare un solo parametro `deleteBy,` che possono avere un valore idField o dedupeFields. Prendiamo in esame l’eliminazione di alcuni oggetti personalizzati.

```
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

Il `seq`, `status`, `marketoGUID`, e `reasons` Dovrebbe esserti già familiare.

Per ulteriori dettagli sull&#39;utilizzo delle operazioni CRUD per ogni singolo tipo di oggetto, consultare le relative pagine.
