---
title: Lead
feature: REST API
description: Esplora le funzioni API REST dei lead di Marketo, tra cui Descrizione, query per ID o filtro, campi predefiniti, limiti e recupero degli ECID.
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '3351'
ht-degree: 2%

---

# Lead

[Riferimento endpoint lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads)

L’API del lead di Marketo offre un’ampia serie di funzionalità per semplici applicazioni CRUD rispetto ai record dei lead, nonché la possibilità di modificare l’appartenenza di un lead a elenchi e programmi statici e di avviare l’elaborazione di campagne intelligenti per i lead.

## Descrivere

Una delle funzionalità chiave dell’API Leads è il metodo Describe. Utilizza Descrivi lead per recuperare un elenco completo dei campi disponibili per l’interazione tramite l’API REST, nonché i metadati per ciascuno di essi:

* Tipo di dati
* Nomi API REST
* Lunghezza (se applicabile)
* Sola lettura
* Etichetta intuitiva

Descrivi è la principale fonte di verità per stabilire se i campi sono disponibili per l’uso e i metadati su di essi.

### Richiesta

```
GET /rest/v1/leads/describe.json
```

### Risposta

```json
{
   "requestId":"37ca#1475b74e276",
   "success":true,
   "result":[
      {
         "id":2,
         "displayName":"Company Name",
         "dataType":"string",
         "length":255,
         "rest":{
            "name":"company",
            "readOnly":false
         },
         "soap":{
            "name":"Company",
            "readOnly":false
         }
      }
}
```

Normalmente, le risposte includono un set molto più ampio di campi nell’array dei risultati, ma vengono omesse a scopo dimostrativo. Ogni elemento nella matrice dei risultati corrisponde a un campo disponibile nel record del lead e avrà almeno un ID, un displayName e un tipo di dati. Gli altri oggetti secondari soap e REST possono essere presenti o meno per un determinato campo e la loro presenza indicherà se il campo è valido per l’utilizzo nelle API REST o SOAP. La proprietà `readOnly` indica se il campo è di sola lettura tramite l&#39;API corrispondente (REST o SOAP). La proprietà length indica la lunghezza massima del campo, se presente. La proprietà dataType indica il tipo di dati del campo.

## Query

Esistono due metodi principali per il recupero dei lead: il metodo Get Lead per ID e il metodo Get Lead per tipo di filtro. Ottieni lead per ID considera un singolo ID lead come parametro di percorso e restituisce un singolo record lead.

Facoltativamente, puoi trasmettere un parametro di campi contenente un elenco separato da virgole di nomi di campi da restituire. Se il parametro fields non è incluso in questa richiesta, vengono restituiti i seguenti campi predefiniti: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` e `id`. Quando si richiede un elenco di campi, se un particolare campo viene richiesto ma non restituito, il valore deve essere nullo.

### Richiesta

```
GET /rest/v1/lead/{id}.json
```

### Risposta

```json
{
   "requestId": "10226#14d3049e51b",
   "success": true,
   "result": [
      {
         "id": 318581,
         "updatedAt":"2015-05-07T11:47:30-08:00"
         "lastName": "Doe",
         "email": "jdoe@marketo.com",
         "createdAt": "2015-05-01T16:47:30-08:00",
         "firstName": "John"
      }
   ]
}
```

Per questo metodo, ci sarà sempre un singolo record nella prima posizione della matrice dei risultati.

Ottieni lead per tipo di filtro restituirà lo stesso tipo di record, ma può restituire fino a 300 per pagina. Richiede i parametri di query `filterType` e `filterValues`.

`filterType` accetta qualsiasi campo personalizzato o la maggior parte dei campi comunemente utilizzati. Chiamare l&#39;endpoint `Describe2` per ottenere un elenco completo dei campi ricercabili consentiti in `filterType`. Durante la ricerca per campo personalizzato, sono supportati solo i seguenti tipi di dati: `string`, `email`, `integer`. È possibile ottenere i dettagli del campo (descrizione, tipo e così via) utilizzando il metodo Describe sopra indicato.

`filterValues` accetta fino a 300 valori in formato separato da virgole. La chiamata cerca i record in cui il campo del lead corrisponde a uno dei `filterValues` inclusi. Se il numero di lead che corrispondono al filtro lead è maggiore di 1.000, viene restituito un errore: &quot;1003, Troppi risultati corrispondono al filtro&quot;.

Se la lunghezza totale della richiesta GET supera gli 8 KB, viene restituito un errore HTTP: &quot;414, URI troppo lungo&quot; (per RFC 7231). Come soluzione alternativa, è possibile modificare il GET in POST, aggiungere il parametro _method=GET e inserire una stringa di query nel corpo della richiesta.

### Richiesta

```
GET /rest/v1/leads.json?filterType=id&filterValues=318581,318592
```

### Risposta

```json
{
    "requestId": "12951#15699db5c97",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2016-05-17T22:11:45Z",
            "lastName": "Lincoln",
            "email": "abe@usa.gov",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "Abraham"
        },
        {
            "id": 318592,
            "updatedAt": "2016-05-17T22:20:51Z",
            "lastName": "Washington",
            "email": "george@usa.gov",
            "createdAt": "2015-04-06T16:29:21Z",
            "firstName": "George"
        }
    ],
    "success": true
}
```

Questa chiamata cerca i record corrispondenti agli ID inclusi in `filterValues` e restituisce tutti i record corrispondenti.

Se non viene trovato alcun record, la risposta indica che l’operazione è riuscita, ma l’array dei risultati sarà vuoto.

### Risposta

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Sia il metodo Get Lead by Id che Get Leads by Filter Type accettano anche un parametro di query fields, che accetta un elenco separato da virgole di campi API. Se è incluso, ogni record nella risposta includerà i campi elencati.  Se viene omesso, verrà restituito un set predefinito di campi: `id`, `email`, `updatedAt`, `createdAt`, `firstName` e `lastName`.

## ADOBE ECID

Quando la funzione Condivisione pubblico di Adobe Experience Cloud è abilitata, si verifica un processo di sincronizzazione dei cookie che associa Adobe Experience Cloud ID (ECID) ai lead di Marketo.  I metodi di recupero dei lead sopra menzionati possono essere utilizzati per recuperare i valori ECID associati.  Per farlo, specifica `ecids` nel parametro fields. Ad esempio, `&fields=email,firstName,lastName,ecids`.

## Crea e aggiorna

Oltre a recuperare i dati dei lead, puoi creare, aggiornare ed eliminare i record dei lead tramite l’API. La creazione e l’aggiornamento dei lead condividono lo stesso endpoint con il tipo di operazione definito nella richiesta ed è possibile creare o aggiornare contemporaneamente fino a 300 record.

>[!NOTE]
>
> L&#39;aggiornamento dei campi società tramite l&#39;endpoint [Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) non è supportato. Utilizza invece l&#39;endpoint [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST).

>[!NOTE]
>
> Durante la creazione o l’aggiornamento del valore e-mail in un record Persona, nel campo dell’indirizzo e-mail sono supportati solo i caratteri ASCII.

### Richiesta

```
POST /rest/v1/leads.json
```

### Corpo

```json
{
   "action":"createOnly",
   "lookupField":"email",
   "input":[
      {
         "email":"kjashaedd-1@klooblept.com",
         "firstName":"Kataldar-1",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-2@klooblept.com",
         "firstName":"Kataldar-2",
         "postalCode":"04828"
      },
      {
         "email":"kjashaedd-3@klooblept.com",
         "firstName":"Kataldar-3",
         "postalCode":"04828"
      }
   ]
}
```

### Risposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "id":52,
         "status":"created"
      }
   ]
}
```

In questa richiesta sono presenti due campi importanti: `action` e `lookupField`.  `action` specifica il tipo di operazione della richiesta e può essere `createOrUpdate`, `createOnly`, `updateOnly` o `createDuplicate`. Se viene omesso, l&#39;impostazione predefinita sarà `createOrUpdate`.  Il parametro `lookupField` specifica la chiave da utilizzare quando l&#39;azione è `createOrUpdate` o `updateOnly`. Se `lookupField` viene omesso, la chiave predefinita è `email`.

Per impostazione predefinita, viene utilizzata la partizione predefinita. Facoltativamente, è possibile specificare il parametro `partitionName`, che funziona solo se l&#39;azione è `createOnly` o `createOrUpdate`. Affinché `partitionName` possa funzionare come criterio di deduplicazione aggiuntivo, deve far parte del tipo di origine nelle regole di deduplicazione personalizzate. Durante un&#39;operazione di aggiornamento, se nella partizione specificata non esiste un lead, viene restituito un errore. Se l’utente solo API non dispone dell’autorizzazione per accedere alla partizione specificata, viene restituito un errore.

Il campo `id` può essere incluso solo come parametro quando si utilizza l&#39;azione `updateOnly`, poiché `id` è una chiave univoca gestita dal sistema.

La richiesta deve inoltre avere un parametro `input`, che è una matrice di record lead. Ogni record lead è un oggetto JSON con un numero qualsiasi di campi lead. Le chiavi incluse in un record devono essere univoche per tale record e tutte le stringhe JSON devono avere la codifica UTF-8. Il campo `externalCompanyId` può essere utilizzato per collegare il record del lead a un record della società. Il campo `externalSalesPersonId` può essere utilizzato per collegare il record del lead a un record del venditore.

Nota: quando si eseguono richieste lead upsert simultaneamente o in rapida successione, è possibile che si verifichino record duplicati quando si eseguono più richieste con lo stesso valore chiave se viene effettuata una chiamata successiva con lo stesso valore prima della prima. È possibile evitare questo problema utilizzando `createOnly` o `updateOnly` come appropriato oppure accodando le chiamate e aspettando che venga restituita la chiamata prima di effettuare chiamate upsert successive con la stessa chiave.

## Campi

L’oggetto lead contiene campi standard e, facoltativamente, campi personalizzati. I campi standard sono presenti in ogni abbonamento a Marketo Engage, mentre i campi personalizzati vengono creati dall’utente in base alle esigenze. Ogni definizione di campo è composta da un insieme di attributi che descrivono il campo. Esempi di attributi sono nome visualizzato, nome API e dataType. Questi attributi sono noti collettivamente come metadati.

I seguenti endpoint consentono di eseguire query, creare e aggiornare campi sull&#39;oggetto lead. Queste API richiedono che l’utente API proprietario abbia un ruolo con una o entrambe le autorizzazioni Campo standard schema di lettura-scrittura o Campo personalizzato schema di lettura-scrittura.

## Campi query

La query dei campi lead è semplice. È possibile eseguire query su un singolo campo lead per nome API o sul set di tutti i campi lead. È possibile recuperare sia i campi standard che i campi personalizzati, a seconda delle autorizzazioni del ruolo utilizzate. Vengono recuperati anche i campi nascosti.

## Per nome

L’endpoint &quot;Get Lead Field by Name&quot; recupera i metadati per un singolo campo sull’oggetto lead. Il parametro di percorso fieldApiName obbligatorio specifica il nome API del campo. La risposta è simile all’endpoint Descrivi lead, ma contiene metadati aggiuntivi, come l’attributo isCustom, che indica se il campo è un campo personalizzato.

### Richiesta

```
GET /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Risposta

```json
{
    "requestId": "cd97#1793ee0fec4",
    "result": [
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        }
    ],
    "success": true
}
```

## Sfogliare

L’endpoint Get Lead Fields recupera i metadati per tutti i campi sull’oggetto lead, tra cui. Per impostazione predefinita, vengono restituiti al massimo 300 record. È possibile utilizzare il parametro di query `batchSize` per ridurre questo numero. Se l&#39;attributo `moreResult` è true, significa che sono disponibili altri risultati. Continuare a chiamare questo endpoint fino a quando l&#39;attributo `moreResult` non restituisce false, ovvero non sono disponibili risultati. I `nextPageToken` restituiti da questa API devono essere sempre riutilizzati per la successiva iterazione di questa chiamata.

### Richiesta

```
GET /rest/v1/leads/schema/fields.json
```

### Risposta (troncata)

```json
{
    "requestId": "142c3#1793eb976d8",
    "result": [
        {
            "displayName": "Salutation",
            "name": "salutation",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "First Name",
            "name": "firstName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Middle Name",
            "name": "middleName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Last Name",
            "name": "lastName",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Date of Birth",
            "name": "dateOfBirth",
            "description": null,
            "dataType": "date",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Email Address",
            "name": "email",
            "description": null,
            "dataType": "email",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Phone Number",
            "name": "phone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Mobile Phone Number",
            "name": "mobilePhone",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Fax Number",
            "name": "fax",
            "description": null,
            "dataType": "phone",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Job Title",
            "name": "title",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": true,
            "isCustom": false
        },
        {
            "displayName": "Unsubscribed",
            "name": "unsubscribed",
            "description": null,
            "dataType": "boolean",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": true,
            "isCustom": false
        },
        ...
    ],
    "success": true,
    "moreResult": false
}
```

## Crea campi

L&#39;endpoint Create Lead Fields crea uno o più campi personalizzati sull&#39;oggetto lead. Questo endpoint fornisce funzionalità paragonabili a quelle disponibili nell’interfaccia utente di Marketo Engage. Puoi creare un massimo di 100 campi personalizzati utilizzando questo endpoint.
Considera attentamente ogni campo creato nell’istanza di produzione di Marketo Engage utilizzando l’API.  Una volta creato un campo, non è possibile eliminarlo (è possibile solo nasconderlo). La proliferazione di campi inutilizzati è una pratica scorretta che aggiunge confusione all’istanza.

Il parametro di input richiesto è un array di oggetti campo lead. Ogni oggetto contiene uno o più attributi. Gli attributi richiesti sono `displayName`, `name` e `dataType`, che corrispondono rispettivamente al nome visualizzato dell&#39;interfaccia utente del campo, al nome API del campo e al tipo di campo.  Facoltativamente, è possibile specificare `description`, `isHidden`, `isHtmlEncodingInEmail` e `isSensitive`.

Esistono alcune regole associate al nome e alla denominazione di `displayName`. L&#39;attributo name deve essere univoco, iniziare con una lettera e contenere solo lettere, numeri o trattini bassi. `displayName` deve essere univoco e non può contenere caratteri speciali.  Una convenzione di denominazione comune consiste nell&#39;applicare Camel Case a `displayName` per produrre il nome. Ad esempio, un `displayName` di &quot;My Custom Field&quot; produrrebbe il nome &quot;myCustomField&quot;.

### Richiesta

```
POST /rest/v1/leads/schema/fields.json
```

### Corpo

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "name": "acmeAccessCode",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      },
      {
        "displayName": "Acme Mail Date",
        "name": "acmeMailDate",
        "description": "Acme Direct Mail Integration",
        "dataType": "string"
      }
  ]
}
```

### Risposta

```json
{
    "requestId": "d9f1#17943666811",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "created"
        },
        {
            "name": "acmeMailDate",
            "status": "created"
        }
    ],
    "success": true
}
```

## Aggiorna campo

L’endpoint Update Lead Field aggiorna un singolo campo personalizzato sull’oggetto lead. Nella maggior parte dei casi, le operazioni di aggiornamento dei campi eseguite utilizzando l’interfaccia utente di Marketo Engage sono ottenibili utilizzando l’API. Nella tabella seguente sono riepilogate alcune differenze.

<table>
<tbody>
<tr>
<td style="width: 26.5306%;" rowspan="2"><strong>Attributo</strong></td>
<td style="width: 35%;" colspan="2"><strong>Campo standard</strong></td>
<td style="width: 38.2654%;" colspan="2"><strong>Campo personalizzato</strong></td>
</tr>
<tr>
<td style="width: 17.449%;"><strong>Aggiornabile tramite API?</strong></td>
<td style="width: 17.551%;"><strong>Aggiornabile dall’interfaccia utente?</strong></td>
<td style="width: 19.3878%;"><strong>Aggiornabile tramite API?</strong></td>
<td style="width: 18.8776%;"><strong>Aggiornabile dall’interfaccia utente?</strong></td>
</tr>
<tr>
<td style="width: 26.5306%;">dataType</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">sì</td>
</tr>
<tr>
<td style="width: 26.5306%;">descrizione</td>
<td style="width: 17.449%;">sì</td>
<td style="width: 17.551%;">sì</td>
<td style="width: 19.3878%;">sì</td>
<td style="width: 18.8776%;">sì</td>
</tr>
<tr>
<td style="width: 26.5306%;">displayName</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">sì</td>
<td style="width: 18.8776%;">sì</td>
</tr>
<tr>
<td style="width: 26.5306%;">isCustom</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">no</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHidden</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">sì</td>
<td style="width: 19.3878%;">yes (se creato dall’API)</td>
<td style="width: 18.8776%;">sì</td>
</tr>
<tr>
<td style="width: 26.5306%;">isHtmlEncodingInEmail</td>
<td style="width: 17.449%;">sì</td>
<td style="width: 17.551%;">sì</td>
<td style="width: 19.3878%;">sì</td>
<td style="width: 18.8776%;">sì</td>
</tr>
<tr>
<td style="width: 26.5306%;">isSensitive</td>
<td style="width: 17.449%;">sì</td>
<td style="width: 17.551%;">sì</td>
<td style="width: 19.3878%;">sì</td>
<td style="width: 18.8776%;">sì</td>
</tr>
<tr>
<td style="width: 26.5306%;">length</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">no</td>
</tr>
<tr>
<td style="width: 26.5306%;">name</td>
<td style="width: 17.449%;">no</td>
<td style="width: 17.551%;">no</td>
<td style="width: 19.3878%;">no</td>
<td style="width: 18.8776%;">no</td>
</tr>
</tbody>
</table>

Il parametro di percorso `fieldApiName` richiesto specifica il nome API del campo da aggiornare. Il parametro di input richiesto è un array che contiene un singolo oggetto campo lead.  L&#39;oggetto field contiene uno o più attributi.

### Richiesta

```
POST /rest/v1/leads/schema/fields/{fieldApiName}.json
```

### Corpo

```json
{
  "input": [
      {
        "displayName": "Acme Access Code",
        "description": "Acme Direct Mail Integration",
        "isHtmlEncodingInEmail": true
      }
  ]
}
```

### Risposta

```json
{
    "requestId": "9f57#1794324f44c",
    "result": [
        {
            "name": "acmeAccessCode",
            "status": "updated"
        }
    ],
    "success": true
}
```

## Invia lead a Marketo

Il lead push è un’alternativa per la sincronizzazione dei lead con Marketo, progettata principalmente per consentire un livello maggiore di attivabilità rispetto ai lead di sincronizzazione standard (simile nell’utilizzo a un modulo Marketo). Oltre alla sincronizzazione dei campi lead, questo endpoint consente l’associazione dei lead in base ai valori dei cookie, che vengono passati all’endpoint. A tale scopo, si trasmette il valore `mkt_tok` generato facendo clic su un messaggio e-mail di Marketo o specificando il nome di un programma nella chiamata. Questo endpoint crea anche una singola attività attivabile, associata a un programma e/o a una campagna in Marketo. Questo consente di attivare su eventi di acquisizione lead attribuiti a una campagna o a un programma specifico per avviare i flussi di lavoro associati dall’interno di Marketo.

L’interfaccia del lead push è molto simile a quella dei lead di sincronizzazione. Tutte le stesse chiavi primarie sono valide e per i campi vengono utilizzati gli stessi nomi API (non esiste alcun parametro di azione, perché si tratta sempre di un’operazione upsert). I parametri `programName` e di input sono obbligatori e i parametri `lookupField`, `source` e `reason` sono facoltativi. Il parametro di input è un array di oggetti lead. L’attività risultante è attribuita al programma denominato corrispondente. I parametri `source` e `reason` sono campi stringa arbitrari che possono essere aggiunti alla richiesta di incorporare tali valori nelle attività risultanti. Questi possono essere utilizzati come vincoli nei trigger corrispondenti (il lead viene inviato a Marketo) e nei filtri (il lead viene inviato a Marketo).

Nota relativa alle attività anonime. Se desideri associare precedenti attività anonime al lead appena creato, non specificare l’attributo dei cookie nell’oggetto lead e chiama Associa lead dopo il lead push. Se desideri creare un nuovo lead senza cronologia attività, specifica semplicemente l’attributo cookies nell’oggetto lead.

### Richiesta

```
POST /rest/v1/leads/push.json
```

### Corpo

```json
{
    "programName": "Big Blue Thing Product Launch",
    "source": "Cool Sales Site",
    "reason": "Downloaded pricing sheet",
    "lookupField": "email",
    "input": [
        {
             "email": "Theresa.May@westminister.gov.uk",
             "country": "united kingdom",
             "firstName": "Theresa",
             "website": "www.brexit.com",
             "leadScore": 45,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/23434456",
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
             "marketoSocialFacebookProfileURL": "http://www.facebook.com/id/42434",
             "jobTitle": "Sonny"
         }
     ]
}
```

### Risposta

```json
{
    "requestId": "939079529805",
    "success": true,
    "warnings": [],
    "result": [
       {
           "id": 483894,
           "status": "created"
       },
       {
           "id": 1087425,
           "status": "updated"
       },
       {
           "id": 3525,
           "reasons": [
                    {
                        "code": "501",
                        "message": "Bad stuff happened"
                    }
           ]
       }
    ]
}
```

Per passare il parametro `mkt_tok`, assegnare il valore al membro mktToken all&#39;interno di un record di lead nel parametro di input nel modo seguente.

### Corpo

```json
{
  "programName": "Big Blue Thing Product Launch",
  "source": "Cool Sales Site",
  "reason": "Downloaded pricing sheet",
  "lookupField": "mktToken",
  "input" : [
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Thelma"
     },
     {
       "mktToken" : "<tokenValue>",
       "firstName" : "Louise"
     }
   ]
}
```

## Invia modulo

Invia modulo è un’alternativa per la sincronizzazione dei lead con Marketo ed è progettato per fornire funzionalità equivalenti all’invio di un Marketo Form. Questo consente di attivare su eventi di acquisizione lead attribuiti a una campagna o a un programma specifico per avviare i flussi di lavoro associati dall’interno di Marketo.

L’endpoint &quot;Invia modulo&quot; supporta le seguenti funzionalità:

* Aggiorna un record cliente potenziale utilizzando il campo e-mail come chiave primaria
* Crea un&#39;attività &quot;Compila modulo&quot; associata a un programma e/o a una campagna
* Consente l’associazione del lead in base al valore del cookie
* Esegue la convalida del campo modulo

L&#39;invio di un modulo segue il modello di database lead standard. Un singolo record di oggetto viene passato nel membro di input richiesto del corpo JSON di una richiesta POST. Il membro `formId` richiesto contiene l&#39;ID del modulo Marketo di destinazione.

L&#39;opzione `programId` può essere utilizzata per specificare il programma a cui aggiungere il lead e/o specificare il programma a cui aggiungere i campi personalizzati del membro del programma. Se viene fornito `programId`, il lead viene aggiunto al programma e vengono aggiunti anche tutti i campi dei membri del programma presenti nel modulo. Si noti che il programma specificato deve trovarsi nella stessa area di lavoro del modulo. Se il modulo non contiene campi personalizzati per i membri del programma e non viene fornito `programId`, il lead non verrà aggiunto a un programma. Se il modulo risiede in un programma e `programId` non viene fornito, tale programma viene utilizzato quando uno o più campi personalizzati del membro del programma sono presenti nel modulo.

Nel record di input è necessario specificare l&#39;oggetto `leadFormFields`. Questo oggetto contiene una o più coppie nome/valore corrispondenti ai campi modulo da compilare.  Tutti i campi specificati devono essere definiti all&#39;interno del modulo specificato. Il nome è il nome REST API per il campo. Il campo `email` è obbligatorio.

L&#39;oggetto membro `visitorData` è facoltativo e contiene coppie nome/valore corrispondenti ai dati di visita della pagina, inclusi `pageURL`, `queryString`, `leadClientIpAddress` e `userAgentString`. Può essere utilizzato per compilare campi di attività aggiuntivi a scopo di filtro e di attivazione.

La stringa del membro del cookie è facoltativa e consente di associare un cookie di Munchkin a un record persona in Marketo. Quando viene creato un nuovo lead, tutte le precedenti attività anonime vengono associate a quel lead, a meno che il valore del cookie non sia stato precedentemente associato a un altro record noto. Se il valore del cookie è stato associato in precedenza, vengono tracciate le nuove attività rispetto al record, ma le attività precedenti non verranno migrate dal record noto esistente. Per creare un nuovo lead senza cronologia attività, ometti semplicemente il membro cookie.

I nuovi lead vengono creati nella partizione primaria dell&#39;area di lavoro in cui si trova il modulo.

### Richiesta

```
POST /rest/v1/leads/submitForm.json
```

### Intestazione

```
Content-Type: application/json
```

### Corpo

```json
{
  "formId": 1029,
  "input": [
    {
      "leadFormFields": {
        "firstName": "Marge",
        "lastName": "Simpson",
        "email": "marge.simpson@fox.com",
        "pMCFField": "PMCF value"
      },
      "visitorData": {
        "pageURL": "https://na-sjst.marketo.com/lp/063-GJP-217/UnsubscribePage.html",
        "queryString": "Unsubscribed=yes",
        "leadClientIpAddress": "192.150.22.5",
        "userAgentString": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36"
      },
      "cookie": "id:063-GJP-217&token:_mch-marketo.com-1594662481190-60776"
    }
  ]
}
```

### Risposta

```json
{
  "requestId": "10667#173bc585ca5",
  "result": [
    {
      "id": 319174,
      "status": "updated"
    }
  ],
  "success": true
}
```

Qui possiamo vedere i corrispondenti dettagli dell’attività &quot;Compila modulo&quot; dall’interfaccia utente di Marketo Engage:

![Compila interfaccia utente modulo](assets/fill_out_form_activity_details.png)

## Unisci

A volte è necessario unire i record duplicati e Marketo lo facilita tramite l’API Unisci lead. L’unione dei lead combina i registri di attività, il programma, le iscrizioni alle campagne, le informazioni di gestione delle relazioni con i clienti e unisce tutti i valori dei campi in un unico record. Merge Leads prende un ID lead come parametro di percorso e un singolo `leadId` come parametro di query o un elenco di ID separati da virgole nel parametro `leadIds`.

### Richiesta

```
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Risposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Il lead specificato nel parametro path è il lead vincente, quindi se ci sono campi in conflitto tra i record da unire, verrà preso il valore del vincitore, a meno che il campo nel record vincente sia vuoto e il campo corrispondente nel record perdente non lo sia. I lead specificati nel parametro `leadId` o `leadIds` sono i lead perdenti.

Se si dispone di una sottoscrizione abilitata per la sincronizzazione con SFDC, è possibile utilizzare anche il parametro `mergeInCRM` nella richiesta. Se è impostato su true, verrà eseguita anche l’unione corrispondente nel CRM. Se entrambi i lead si trovano in SFDC e uno è un lead CRM e l’altro è un contatto CRM, allora il vincitore è il contatto CRM (indipendentemente dal lead specificato come vincitore). Se uno dei lead si trova in SFDC e l’altro è solo Marketo, il vincitore è il lead SFDC (indipendentemente dal lead specificato come vincitore).

## Associa attività web

Tramite il tracciamento dei lead (Munchkin), Marketo registra l’attività web dei visitatori sul sito web e sulle pagine di destinazione di Marketo. Queste attività, Visite e Clic, sono registrate con una chiave che corrisponde a un cookie &quot;_mkto_trk&quot; impostato nel browser del lead e Marketo lo utilizza per tenere traccia delle attività della stessa persona. In genere, l’associazione ai record dei lead si verifica quando un lead passa da un’e-mail di Marketo o compila un modulo di Marketo, ma a volte un’associazione può essere attivata da un tipo di evento diverso ed è possibile utilizzare l’endpoint Associa lead per farlo. L’endpoint considera l’ID del record lead noto come parametro di percorso e il valore del cookie &quot;_mkto_trk&quot; nel parametro di query del cookie.

### Richiesta

```
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Risposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Se un cookie è già associato a un record lead noto, l’utilizzo di questa API su un record lead diverso fa sì che la nuova attività web venga registrata su tale record, ma non sposterà alcuna attività web esistente nel nuovo record.
Iscrizione

È inoltre possibile recuperare i record dei lead in base all&#39;appartenenza a un elenco statico o a un programma. Inoltre, puoi recuperare tutti gli elenchi statici, i programmi o le campagne intelligenti di cui è membro un lead.

La struttura di risposta e i parametri facoltativi sono identici a quelli di Get Leads per Filter Type, anche se filterType e filterValues non possono essere utilizzati con questa API.
Per accedere all’ID elenco tramite l’interfaccia utente di Marketo, passa all’elenco. L&#39;elenco `id` si trova nell&#39;URL dell&#39;elenco statico, `https://app-****.marketo.com/#ST1001A1`. In questo esempio, 1001 è `id` per l&#39;elenco.

### Richiesta

```
GET /rest/v1/list/{listId}/leads.json?batchSize=3
```

### Risposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "nextPageToken":
"PS5VL5WD4UOWGOUCJR6VY7JQO2KUXL7BGBYXL4XH4BYZVPYSFBAONP4V4KQKN4SSBS55U4LEMAKE6===",
    "result":[
       {
            "id":50,
            "email":"kjashaedd@klooblept.com",
            "firstName":"Kataldar",
             "postalCode":"04828"
       },
       {
           "id":2343,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
           "postalCode":"04828"
       },
      {
           "id":88498,
           "email":"kjashaedd@klooblept.com",
           "firstName":"Kataldar",
         "postalCode":"04828"
         }
    ]
}
```

L&#39;endpoint Get Lists by Lead Id accetta un parametro di percorso `id` del record di lead e restituisce tutti i record di elenco statici di cui il lead è membro.

### Richiesta

```
GET /rest/v1/leads/{id}/listMembership.json?batchSize=3
```

### Risposta

```json
{
    "requestId": "1184b#1706f0ec23f",
    "result": [
        {
            "listId": 3379,
            "createdAt": "2016-05-17T19:32:44Z",
            "updatedAt": "2016-05-17T19:32:44Z"
        },
        {
            "listId": 2792,
            "createdAt": "2009-05-19T18:29:15Z",
            "updatedAt": "2009-05-19T18:29:15Z"
        },
        {
            "listId": 42,
            "createdAt": "2009-04-22T19:24:22Z",
            "updatedAt": "2009-04-22T19:24:22Z"
        }
    ],
    "success": true,
    "nextPageToken": "BFRV7OMVSNJWDVKVTUFS3XHT4E======",
    "moreResult": true
}
```

## Programmi

L’iscrizione al programma può essere recuperata in modo simile agli elenchi. Gli stessi parametri di richiesta facoltativi sono disponibili quando si chiama l&#39;endpoint Get Leads by Program Id e si passa il parametro di percorso `programId`.

Facoltativamente, puoi trasmettere un parametro di campi contenente un elenco separato da virgole di nomi di campi da restituire. Se il parametro fields non è incluso in questa richiesta, verranno restituiti i seguenti campi predefiniti: `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` e `id`. Quando si richiede un elenco di campi, se un particolare campo viene richiesto ma non restituito, il valore deve essere nullo.

La struttura di risposta è molto simile, in quanto ogni elemento nell’array dei risultati è un lead, con la differenza che ogni record ha anche un oggetto figlio denominato &quot;appartenenza&quot;. Questo oggetto di appartenenza include dati sulla relazione del lead con il programma indicato nella chiamata, mostrando sempre i relativi `progressionStatus`, `acquiredBy`, `reachedSuccess` e `membershipDate`. Se il programma padre è anche un programma di coinvolgimento, l&#39;appartenenza avrà membri `stream`, `nurtureCadence` e `isExhausted` per indicare la sua posizione e attività nel programma di coinvolgimento.

### Richiesta

```
GET /rest/v1/leads/programs/{programId}.json?batchSize=3
```

### Risposta

```json
{
    "requestId": "13ad4#1727b748a17",
    "result": [
        {
            "id": 319141,
            "firstName": "Meera",
            "lastName": "Reed",
            "email": "mree@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319142,
            "firstName": "Jon",
            "lastName": "Umber",
            "email": "jumb@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        },
        {
            "id": 319143,
            "firstName": "Lyanna",
            "lastName": "Mormont",
            "email": "lmor@housestark.com",
            "updatedAt": "2020-04-21T16:27:14Z",
            "createdAt": "2020-04-21T16:27:14Z",
            "membership": {
                "id": 1127,
                "progressionStatus": "Visited",
                "progressionStatusType": "Visited",
                "isExhausted": false,
                "acquiredBy": true,
                "reachedSuccess": false,
                "membershipDate": "2020-04-21T16:27:16Z",
                "updatedAt": "2020-04-21T16:27:16Z"
            }
        }
    ],
    "success": true,
    "nextPageToken": "SW3PTMBVFCNHSHJGZ7LQH3ZWNUOHKADJZ3MOQ2LOZZVNO3WEIUPDKPRTTHBSMW756KOCWURTOF2XS==="
}
```

L&#39;endpoint Get Programs by Lead Id accetta un parametro di percorso dell&#39;ID del record del lead e restituisce tutti i record del programma di cui il lead è membro. I parametri facoltativi `filterType` e `filterValues` consentono di filtrare in base all&#39;ID del programma.

### Richiesta

```
GET /rest/v1/leads/{id}/programMembership.json
```

### Risposta

```json
{
    "requestId": "12e84#1706f13a379",
    "result": [
        {
            "id": 1044,
            "progressionStatus": "Sent",
            "isExhausted": false,
            "acquiredBy": false,
            "reachedSuccess": false,
            "membershipDate": "2016-05-27T19:50:29Z",
            "updatedAt": "2016-05-27T19:50:29Z"
        }
    ],
    "success": true,
    "moreResult": false
}
```

## Campagne avanzate

L’endpoint &quot;Get Smart Campaigns by Lead Id&quot; accetta un parametro di percorso ID record lead e restituisce tutti i record smart campaign di cui il lead è membro.

### Richiesta

```
GET /rest/v1/leads/{id}/smartCampaignMembership.json?batchSize=3
```

### Risposta

```json
{
    "requestId": "e7b0#1706f163632",
    "result": [
        {
            "smartCampaignId": 3746,
            "createdAt": "2018-06-01T18:00:04Z",
            "updatedAt": "2018-06-01T18:00:06Z"
        },
        {
            "smartCampaignId": 3678,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:41Z"
        },
        {
            "smartCampaignId": 3680,
            "createdAt": "2015-04-06T18:37:30Z",
            "updatedAt": "2015-04-06T18:37:40Z"
        }
    ],
    "success": true,
    "nextPageToken": "TNGAH3NKDUFDHNXUVGTNBXJCQM======",
    "moreResult": true
}
```

## Elimina

La rimozione dei lead è semplice utilizzando l’endpoint Elimina lead.  Specifica gli ID lead da eliminare utilizzando gli attributi ID nel corpo.  Il massimo è di 300 lead per richiesta.  Utilizza Content-Type: intestazione application/json.

### Richiesta

```
POST /rest/v1/leads/delete.json
```

### Corpo

```json
{
   "input":[
      {
         "id": 235
      },
      {
         "id":766
      }
   ]
}
```

### Risposta

```json
{
  "requestId":"3608#16664333670",
  "result":[
    {
      "id":235,
      "status":"deleted"
    },
    {
      "id":766,
      "status":"deleted"
    }
  ],
  "success":true
}
```

## Relazioni

* Aziende tramite il campo externalCompanyId sul record del lead
* Campo SalesPersons tramite externalSalesPersonId nel record del lead
* Programmi tramite iscrizione al programma
* Elenchi tramite appartenenza a elenco
* Attività tramite il campo leadId nell’attività
* Segmentazione attraverso singoli campi segmento nel record del lead
* Partizioni tramite leadPartitionId nel record del lead

## Timeout

Gli endpoint lead hanno un timeout di 30 secondi, a meno che non sia indicato di seguito:

* Lead di sincronizzazione: 90s
* Associa lead: anni 60
* Unisci lead: anni 180
* Aggiorna partizione lead: 60s
* Invia lead a Marketo: anni 90
* Ottieni lead per tipo di filtro: 60s
* Ottieni lead per ID elenco: 60s
