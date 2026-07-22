---
title: Lead
feature: REST API
description: Esplora le funzioni API REST dei lead di Marketo, tra cui Descrizione, query per ID o filtro, campi predefiniti, limiti e recupero degli ECID.
exl-id: 0a2f7c38-02ae-4d97-acfe-9dd108a1f733
TQID: https://experienceleague.adobe.com/jZ-ecWTmHwq9gvp4fMaeuuGba6cgwYx0QCCyfkrEDHQ
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 2728
ht-degree: 3%

---

# Lead

[Riferimento endpoint lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads)

L’API dei lead di Marketo supporta operazioni CRUD sui record dei lead. Puoi anche modificare l’appartenenza di un lead a elenchi e programmi statici e avviare l’elaborazione di Smart Campaign per i lead.

## Descrivere

Utilizza Descrivi lead per recuperare i campi disponibili tramite l’API REST e i metadati per ciascun campo:

- Tipo di dati
- Nome API REST
- Lunghezza, se applicabile
- Stato di sola lettura
- Etichetta intuitiva

Descrivi è la principale fonte di verità per la disponibilità dei campi e i metadati.

### Richiesta

```http
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

Le risposte effettive includono più campi nella matrice dei risultati. Ogni elemento rappresenta un campo disponibile nel record del lead e contiene almeno un ID, un displayName e un tipo di dati.

Gli altri oggetti secondari soap vengono visualizzati solo quando il campo è valido per l’API corrispondente. La proprietà `readOnly` indica se l&#39;API corrispondente può aggiornare il campo. Se presente, la proprietà length fornisce la lunghezza massima del campo e la proprietà dataType fornisce il tipo di dati del campo.

## Query

Utilizza uno dei due metodi principali per recuperare i lead:

- Ottieni lead per ID considera un ID lead come parametro di percorso e restituisce un record lead.
- Recupera lead per tipo di filtro trova i record il cui campo selezionato corrisponde a uno dei valori specificati.

Per Ottieni lead per ID, puoi facoltativamente trasmettere un parametro di campi con un elenco separato da virgole di nomi di campi da restituire. Se la richiesta omette i campi, la risposta include `email`, `updatedAt`, `createdAt`, `lastName`, `firstName` e `id`. Se un campo richiesto non viene restituito, il suo valore è implicitamente nullo.

### Richiesta

```http
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

Ottieni lead per ID restituisce sempre un record nella prima posizione della matrice dei risultati.

Recupera lead per tipo di filtro restituisce lo stesso tipo di record e può restituire fino a 300 record per pagina. I parametri di query `filterType` e `filterValues` sono obbligatori.

`filterType` accetta qualsiasi campo personalizzato e i campi più comunemente utilizzati. Chiamare l&#39;endpoint `Describe2` per recuperare i campi ricercabili consentiti per `filterType`. Durante la ricerca per campo personalizzato, i tipi di dati supportati sono `string`, `email` e `integer`. Utilizzare il metodo Describe per recuperare i dettagli del campo, ad esempio la descrizione e il tipo.

`filterValues` accetta fino a 300 valori separati da virgola. La chiamata restituisce record in cui il campo del lead selezionato corrisponde a uno di questi valori. Se più di 1.000 lead corrispondono al filtro, l’API restituisce &quot;1003, Troppi risultati corrispondono al filtro&quot;.

Se il totale della richiesta GET supera gli 8 KB, l’API restituisce &quot;414, URI troppo lungo&quot; con RFC 7231. Per aggirare questo limite, modifica GET in POST, aggiungi il parametro _method=GET e inserisci la stringa di query nel corpo della richiesta.

### Richiesta

```http
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

Questa chiamata restituisce record i cui ID corrispondono ai valori in `filterValues`.

Se nessun record corrisponde, la risposta indica il successo e contiene una matrice di risultati vuota.

### Risposta

```json
{
"requestId": "177a1#1578b643357",
"result": [],
"success": true
}
```

Sia Ottieni lead per ID che Ottieni lead per tipo di filtro accettano un parametro di query dei campi contenente un elenco separato da virgole di campi API. Quando i campi sono presenti, ogni record di risposta include i campi elencati. Se viene omesso, la risposta include `id`, `email`, `updatedAt`, `createdAt`, `firstName` e `lastName`.

## ADOBE ECID

Quando Adobe Experience Cloud Audience Sharing è abilitato, la sincronizzazione dei cookie associa i valori Adobe Experience Cloud ID (ECID) ai lead di Marketo. Per recuperare i valori ECID associati ai metodi di recupero dei lead precedenti, includere `ecids` nel parametro fields. Ad esempio, `&fields=email,firstName,lastName,ecids`.

## Crea e aggiorna

L’API Lead può creare, aggiornare ed eliminare record di lead. Le operazioni di creazione e aggiornamento utilizzano lo stesso endpoint, con il tipo di operazione definito nella richiesta. Una richiesta può creare o aggiornare fino a 300 record.

>[!NOTE]
>
> L&#39;aggiornamento dei campi società tramite l&#39;endpoint [Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST) non è supportato. Utilizza invece l&#39;endpoint [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST).

>[!NOTE]
>
> Durante la creazione o l’aggiornamento del valore e-mail in un record Persona, nel campo dell’indirizzo e-mail sono supportati solo i caratteri ASCII.

### Richiesta

```http
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

La richiesta utilizza due campi importanti:

- `action` specifica il tipo di operazione: `createOrUpdate`, `createOnly`, `updateOnly` o `createDuplicate`. Se omesso, verrà utilizzato il valore predefinito `createOrUpdate`.
- `lookupField` specifica la chiave quando action è `createOrUpdate` o `updateOnly`. Se omesso, verrà utilizzato il valore predefinito `email`.

Per impostazione predefinita, l&#39;operazione utilizza la partizione predefinita. Il parametro facoltativo `partitionName` funziona solo quando l&#39;azione è `createOnly` o `createOrUpdate`. Per utilizzare `partitionName` come criterio di deduplicazione aggiuntivo, includerlo nel tipo di origine per le regole di deduplicazione personalizzate.

Durante un aggiornamento, l’API restituisce un errore se il lead non esiste nella partizione specificata o se l’utente solo API non può accedere a tale partizione.

Poiché `id` è una chiave univoca gestita dal sistema, includerla solo con l&#39;azione `updateOnly`.

La richiesta deve includere un parametro `input` contenente una matrice di record lead. Ogni record lead è un oggetto JSON con un numero qualsiasi di campi lead. Le chiavi devono essere univoche all’interno di ogni record e tutte le stringhe JSON devono utilizzare la codifica UTF-8.

Utilizzare `externalCompanyId` per collegare un record lead a un record società. Utilizzare `externalSalesPersonId` per collegare un record lead a un record venditore.

Le richieste upsert simultanee o con tempistica ravvicinata possono creare record duplicati quando più richieste utilizzano lo stesso valore chiave prima della restituzione della prima richiesta. Per evitare duplicati, utilizzare `createOnly` o `updateOnly` come appropriato. In alternativa, metti in coda le chiamate e attendi la restituzione di ciascuna chiamata prima di inviare un altro upsert con la stessa chiave.

## Campi

L’oggetto lead contiene campi standard e campi personalizzati facoltativi. In ogni abbonamento a Marketo Engage sono presenti campi standard, mentre gli utenti creano campi personalizzati in base alle esigenze.

Ogni definizione di campo contiene attributi di metadati come il nome visualizzato, il nome API e il dataType.

Utilizzare gli endpoint seguenti per eseguire query, creare e aggiornare campi sull&#39;oggetto lead. Il ruolo dell&#39;utente API deve disporre dell&#39;autorizzazione Campo standard dello schema di lettura-scrittura, dell&#39;autorizzazione Campo personalizzato dello schema di lettura-scrittura o di entrambi.

## Campi query

Esegui la query di un campo lead per nome API o di tutti i campi lead. A seconda delle autorizzazioni del ruolo, la risposta può includere campi standard, campi personalizzati e campi nascosti.

## Per nome

L’endpoint &quot;Get Lead Field by Name&quot; recupera i metadati per un campo lead. Il parametro obbligatorio percorso fieldApiName specifica il nome API del campo.

La risposta è simile alla risposta Descrivi lead, ma include metadati aggiuntivi. L&#39;attributo isCustom, ad esempio, indica se il campo è personalizzato.

### Richiesta

```http
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

L&#39;endpoint Get Lead Fields recupera i metadati per tutti i campi sull&#39;oggetto lead. Per impostazione predefinita, restituisce un massimo di 300 record. Utilizzare il parametro di query `batchSize` per ridurre questo numero.

Se `moreResult` è true, sono disponibili altri risultati. Passa `nextPageToken` restituito in ogni chiamata successiva fino a quando `moreResult` non è false.

### Richiesta

```http
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

L’endpoint Create Lead Fields crea uno o più campi personalizzati sull’oggetto lead e fornisce funzionalità paragonabili a quelle dell’interfaccia utente di Marketo Engage. Con questo endpoint è possibile creare fino a 100 campi personalizzati.

Considera attentamente ogni campo prima di crearlo in un’istanza di produzione. Dopo aver creato un campo, è possibile nasconderlo ma non eliminarlo. I campi inutilizzati rendono l’istanza più complessa.

Il parametro di input richiesto è un array di oggetti campo lead. Ogni oggetto richiede i seguenti attributi:

- `displayName` è il nome visualizzato dell&#39;interfaccia utente del campo.
- `name` è il nome API del campo.
- `dataType` è il tipo di campo.

Gli attributi facoltativi sono `description`, `isHidden`, `isHtmlEncodingInEmail` e `isSensitive`.

L&#39;attributo name deve essere univoco, iniziare con una lettera e contenere solo lettere, numeri o trattini bassi. `displayName` deve essere univoco e non può contenere caratteri speciali.

Una convenzione comune applica la notazione Camel a `displayName` per produrre il nome. Ad esempio, un `displayName` di &quot;Campo personalizzato&quot; produce il nome &quot;myCustomField&quot;.

### Richiesta

```http
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

L’endpoint Update Lead Field aggiorna un campo personalizzato sull’oggetto lead. La maggior parte degli aggiornamenti dei campi disponibili nell’interfaccia utente di Marketo Engage sono disponibili anche tramite l’API. Nella tabella seguente sono riepilogate le differenze.

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

Il parametro di percorso `fieldApiName` richiesto specifica il nome API del campo da aggiornare. Il parametro di input richiesto è un array contenente un oggetto campo lead con uno o più attributi.

### Richiesta

```http
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

Il lead push è un’alternativa ai lead di sincronizzazione e fornisce più opzioni di attivazione, simili a quelle di un modulo Marketo. Oltre a sincronizzare i campi del lead, l’endpoint può associare un lead in base a un valore di cookie. Passa il valore `mkt_tok` generato da un clic da un&#39;e-mail di Marketo o passa un nome di programma nella chiamata.

L’endpoint crea anche un’attività attivabile associata a un programma Marketo, a una campagna o a entrambi. Utilizza questa attività per avviare flussi di lavoro da eventi di acquisizione lead attribuiti a una campagna o a un programma specifico.

Il lead push utilizza le stesse chiavi primarie e gli stessi nomi API di campo dei lead di sincronizzazione. Non ha alcun parametro di azione perché esegue sempre un upsert.

`programName` e i parametri di input sono obbligatori. Il parametro di input è un array di oggetti lead e l&#39;attività risultante viene attribuita al programma denominato. I parametri `lookupField`, `source` e `reason` sono facoltativi. Aggiungere stringhe arbitrarie in `source` e `reason` per includere tali valori nelle attività risultanti. Puoi utilizzare i valori come vincoli nei trigger corrispondenti (il lead viene inviato a Marketo) e nei filtri (il lead viene inviato a Marketo).

Per associare precedenti attività anonime a un lead appena creato, omettere l&#39;attributo dei cookie dall&#39;oggetto lead e chiamare Associa lead dopo il lead push. Per creare un lead senza cronologia attività, specifica l’attributo cookies nell’oggetto lead.

### Richiesta

```http
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
             "jobTitle": "Prime Minister"
         },
         {
             "email": "Justin.Trudeau@ottowa.gov.ca",
             "country": "canada",
             "firstName": "Justin",
             "website": "www.take-off-eh.com",
             "leadScore": 92,
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

Per passare il parametro `mkt_tok`, assegnarne il valore al membro mktToken in un record di lead all&#39;interno del parametro di input.

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

Invia modulo è un&#39;alternativa per la sincronizzazione dei lead e fornisce funzionalità equivalenti all&#39;invio di un Marketo Form. Utilizzala per avviare flussi di lavoro da eventi di acquisizione lead attribuiti a una campagna o a un programma specifico.

L’endpoint &quot;Invia modulo&quot; supporta le seguenti funzionalità:

- Inserisce un record cliente potenziale utilizzando il campo e-mail come chiave primaria.
- Crea un&#39;attività &quot;Compila modulo&quot; associata a un programma, una campagna o entrambi.
- Associa un lead in base a un valore di cookie.
- Convalida i campi modulo.

Inviare un modulo con il modello di database lead standard. Passa un record oggetto nel membro di input richiesto del corpo JSON della richiesta POST. Il membro `formId` richiesto contiene l&#39;ID del modulo Marketo di destinazione.

Utilizzare l&#39;elemento facoltativo `programId` per identificare il programma che riceve il lead, i campi personalizzati del membro del programma o entrambi. Se `programId` è presente, il lead viene aggiunto al programma insieme a qualsiasi campo membro del programma nel modulo. Il programma deve trovarsi nella stessa area di lavoro del modulo.

Se il modulo non contiene campi personalizzati per i membri del programma e `programId` viene omesso, il lead non verrà aggiunto a un programma. Se il modulo appartiene a un programma, contiene uno o più campi personalizzati del membro del programma e omette `programId`, l&#39;endpoint utilizza il programma del modulo.

L&#39;oggetto `leadFormFields` richiesto contiene una o più coppie nome/valore per i campi da compilare. Ogni campo deve essere definito nel modulo specificato e ogni nome deve essere il nome API REST del campo. Il campo `email` è obbligatorio.

L&#39;oggetto `visitorData` facoltativo contiene dati di visita pagina, inclusi `pageURL`, `queryString`, `leadClientIpAddress` e `userAgentString`. Utilizzala per compilare ulteriori campi di attività per filtri e attivatori.

Il membro cookie opzionale associa un cookie Munchkin a un record persona Marketo. Quando l’endpoint crea un lead, associa precedenti attività anonime a tale lead, a meno che il cookie non sia stato precedentemente associato a un altro record noto.

Se il cookie è stato associato in precedenza, le nuove attività vengono tracciate rispetto al nuovo record, ma le attività precedenti rimangono con il record noto esistente. Per creare un lead senza cronologia attività, ometti il membro cookie.

I nuovi lead vengono creati nella partizione primaria dell&#39;area di lavoro in cui si trova il modulo.

### Richiesta

```http
POST /rest/v1/leads/submitForm.json
```

### Intestazione

```text
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

L’immagine seguente mostra i dettagli corrispondenti dell’attività &quot;Compila modulo&quot; nell’interfaccia utente di Marketo Engage:

![Compila interfaccia utente modulo](assets/fill_out_form_activity_details.png)

## Unisci

>[!NOTE]
>
>A partire dal 31 marzo 2026, le chiamate che includono più di 25 ID nel parametro `leadIds` di una chiamata API Merge Leads genereranno un codice di errore 1080 e la chiamata verrà ignorata. I posti di lavoro che richiedono la fusione di più di 25 record in uno, dovrebbero essere suddivisi in più lavori per garantire il successo di tali chiamate.
>

Utilizza l’API Unisci lead per combinare i record duplicati in un unico record. Un’unione combina registri di attività, appartenenze a programmi, campagne ed elenchi, informazioni CRM e valori di campo.

Passa l’ID lead vincente come parametro del percorso. Passa un `leadId` come parametro di query o fino a 25 ID separati da virgole nel parametro `leadIds`.


### Richiesta

```http
POST /rest/v1/leads/{id}/merge.json?leadId=1324
```

### Risposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Il lead nel parametro percorso è il lead vincente. Quando i valori dei campi sono in conflitto, l&#39;unione utilizza il valore del vincitore a meno che tale valore non sia vuoto e il valore del record perdente non lo sia. I lead nel parametro `leadId` o `leadIds` sono i lead perdenti.

Per una sottoscrizione abilitata per la sincronizzazione di SFDC, utilizzare il parametro `mergeInCRM` per eseguire anche l&#39;unione nel CRM. Se entrambi i record sono in SFDC e uno è un lead del CRM mentre l&#39;altro è un contatto del CRM, il contatto del CRM vince indipendentemente dal vincitore specificato. Se un record si trova in SFDC e l&#39;altro esiste solo in Marketo, il lead di SFDC vince indipendentemente dal vincitore specificato.

## Associa attività web

Il tracciamento dei lead (Munchkin) registra le visite e i clic dei visitatori sul sito web e sulle pagine di destinazione di Marketo. Queste attività utilizzano una chiave che corrisponde al cookie &quot;_mkto_trk&quot; nel browser del lead, che consente a Marketo di tenere traccia delle attività della stessa persona.

L’associazione a un record di lead si verifica in genere quando un lead segue un collegamento da un’e-mail di Marketo o invia un modulo di Marketo. Per associare un lead dopo un altro tipo di evento, utilizzare l&#39;endpoint Associa lead. Passa l’ID del record lead noto come parametro di percorso e il valore del cookie &quot;_mkto_trk&quot; nel parametro di query del cookie.

### Richiesta

```http
POST /rest/v1/leads/{id}/associate.json?cookie=id:287-GTJ-838%26token:_mch-marketo.com-1396310362214-46169
```

### Risposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true
}
```

Se il cookie è già associato a un lead noto, l’utilizzo di questa API per un lead diverso registra la nuova attività web rispetto al nuovo record. L&#39;attività Web esistente non viene spostata nel nuovo record.
Iscrizione

Recuperare i record dei lead in base all&#39;appartenenza a un elenco o a un programma statico. Puoi anche recuperare tutti gli elenchi statici, i programmi o le campagne intelligenti che includono un lead specifico.

La struttura di risposta e i parametri facoltativi corrispondono a Get Leads by Filter Type, ma questa API non accetta `filterType` o `filterValues`.

Per trovare l’ID elenco nell’interfaccia utente di Marketo, passa all’elenco e ispezionane l’URL. In `https://app-****.marketo.com/#ST1001A1`, 1001 è l&#39;elenco `id`.

## Ottieni programmi per ID lead

### Richiesta

```http
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

## Ottieni elenchi per ID lead

L&#39;endpoint Get Lists by Lead Id accetta un parametro di percorso `id` del record di lead e restituisce ogni elenco statico che include il lead.

### Richiesta

```http
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

Recuperare l&#39;iscrizione al programma nello stesso modo dell&#39;iscrizione all&#39;elenco. L&#39;ID Get Leads by Program accetta gli stessi parametri di richiesta facoltativi e richiede il parametro di percorso `programId`.

Facoltativamente, passa un parametro fields contenente un elenco separato da virgole di nomi di campo. Se i campi vengono omessi, la risposta include `email`, `updatedAt`, `createdAt`, `lastName`, `firstName`, `membership` e `id`. Se un campo richiesto non viene restituito, il suo valore è implicitamente nullo.

Ogni elemento nella matrice dei risultati è un lead con un oggetto figlio denominato &quot;appartenenza&quot;. Questo oggetto descrive la relazione del lead con il programma richiesto e include sempre `progressionStatus`, `acquiredBy`, `reachedSuccess` e `membershipDate`.

Se il programma padre è un programma di coinvolgimento, l&#39;appartenenza include anche `stream`, `nurtureCadence` e `isExhausted` per descrivere la posizione e l&#39;attività del lead in tale programma.

### Richiesta

```http
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

L’endpoint &quot;Get Programs by Lead Id&quot; accetta un parametro di percorso dell’ID del record del lead e restituisce ogni programma che include il lead. Utilizzare i parametri facoltativi `filterType` e `filterValues` per filtrare in base all&#39;ID programma.

### Richiesta

```http
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

L’endpoint &quot;Get Smart Campaigns by Lead Id&quot; accetta un parametro di percorso per l’ID del record del lead e restituisce ogni campagna avanzata che include il lead.

### Richiesta

```http
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

Utilizzare l&#39;endpoint Elimina lead per rimuovere i record dei lead. Specifica gli ID lead nel corpo con gli attributi ID. Una richiesta può eliminare fino a 300 lead. Invia l’intestazione Content-Type: application/json.

### Richiesta

```http
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

- Aziende tramite il campo externalCompanyId nel record del lead
- SalesPersons tramite il campo externalSalesPersonId nel record lead
- Programmi tramite iscrizione al programma
- Elenchi tramite appartenenza a elenco
- Attività tramite il campo leadId nell’attività
- Segmentazione attraverso singoli campi segmento nel record del lead
- Partizioni nel campo leadPartitionId del record lead

## Timeout

Gli endpoint lead hanno un timeout di 30 secondi, ad eccezione dei seguenti endpoint:

- Lead di sincronizzazione: 90s
- Associa lead: anni 60
- Unisci lead: anni 180
- Aggiorna partizione lead: 60s
- Invia lead a Marketo: anni 90
- Ottieni lead per tipo di filtro: 60s
- Ottieni lead per ID elenco: 60s
