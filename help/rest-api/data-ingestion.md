---
title: Acquisizione dati
feature: REST API, Dynamic Content
description: Utilizza l’API di acquisizione dati di Marketo per l’acquisizione di volumi elevati e a bassa latenza da parte di persone, oggetti personalizzati, aziende e membri del programma.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: 6145067629ce78175af3b7464807a0fa100c7b57
workflow-type: tm+mt
source-wordcount: '1786'
ht-degree: 13%

---

# API di acquisizione dati

L’API di acquisizione dati è un servizio ad alto volume, a bassa latenza e a disponibilità elevata progettato per gestire in modo efficiente e con ritardi minimi l’acquisizione di grandi quantità di dati relativi a persone e persone.

I dati vengono acquisiti inviando le richieste eseguite in modo asincrono. È possibile recuperare lo stato della richiesta sottoscrivendo eventi da [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup).

Le interfacce sono disponibili per quattro tipi di oggetto: Persone, Oggetti personalizzati, Aziende e Membri del programma. L&#39;operazione di registrazione è solo &quot;insert or update&quot; (Inserisci o aggiorna), ad eccezione dei membri del programma che supportano anche l&#39;eliminazione.

>[!NOTE]
>
>L&#39;accesso all&#39;API di acquisizione dati richiede l&#39;adesione al pacchetto [Marketo Engage Performance Tier](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Autenticazione

L&#39;API di acquisizione dati utilizza lo stesso metodo di autenticazione OAuth 2.0 dell&#39;API REST di Marketo per generare un token di accesso, ma il token di accesso deve essere passato tramite l&#39;intestazione HTTP `X-Mkto-User-Token`. Non è possibile passare il token di accesso tramite un parametro di query.

Esempio di token di accesso tramite intestazione:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Autorizzazioni

L’acquisizione dei dati utilizza lo stesso modello di autorizzazioni dell’API REST di Marketo e non richiede alcuna autorizzazione speciale aggiuntiva da utilizzare, anche se sono necessarie autorizzazioni specifiche per ogni endpoint.

| Endpoint | Autorizzazione |
| --- | --- |
| Persone | Lead di lettura/scrittura |
| Oggetti personalizzati | Oggetto personalizzato di lettura/scrittura |
| Aziende | Società di lettura/scrittura |
| Membri del programma | Lead di lettura/scrittura |

## Tipi di oggetto supportati

| Tipo di oggetto | Operazioni supportate |
| --- | --- |
| Persone | Upsert (inserire o aggiornare) |
| Oggetti personalizzati | Upsert (inserire o aggiornare) |
| Aziende | Sincronizzazione (`createOnly`, `updateOnly`, `createOrUpdate`) |
| Membri del programma | Sincronizza (stato upsert), Elimina (rimuovi dal programma) |

## Intestazioni

L’acquisizione dei dati utilizza le seguenti intestazioni HTTP personalizzate.

### Richiesta

| Chiave | Valore | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| `X-Correlation-Id` | Stringa arbitraria (lunghezza massima 255 caratteri). | No | Può essere utilizzato per tracciare le richieste attraverso il sistema. Consulta Flusso di dati di osservabilità Marketo |
| `X-Request-Source` | Stringa arbitraria (lunghezza massima 50 caratteri). | No | Può essere utilizzato per tracciare l’origine delle richieste tramite il sistema. Consulta Flusso di dati di osservabilità Marketo |

### Risposta

| Chiave | Valore | Obbligatorio |
| --- | --- | --- |
| `X-Request-Id` | ID richiesta univoco. | Sì |

## Richieste

Utilizzare il metodo HTTP POST per inviare dati al server.

La rappresentazione dei dati è inclusa nel corpo della richiesta come application/json.

Nome di dominio: `mkto-ingestion-api.adobe.io`

Il percorso inizia con `/subscriptions/MunchkinId`, dove MunchkinId è specifico per l&#39;istanza Marketo. Puoi trovare il tuo Munchkin ID nell&#39;interfaccia utente di Marketo Engage in **Amministratore** > **Il mio account** > **Informazioni di supporto**.  Il resto del percorso viene utilizzato per specificare la risorsa di interesse.

URL di esempio per Persone:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

URL di esempio per oggetti personalizzati:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

URL di esempio per le società:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/companies`

URL di esempio per i membri del programma:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/programmembers`

### Risposte

Tutte le risposte restituiscono un ID di richiesta univoco tramite l&#39;intestazione `X-Request-Id`.

Esempio di ID richiesta tramite intestazione:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Completato

Quando una chiamata ha esito positivo, viene restituito lo stato 202.  Nessun corpo di risposta restituito.

Esempio di risposta di successo:

```
HTTP/1.1 202 Accepted
X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598
Content-Length: 0
Date: Wed, 18 Oct 2023 18:56:49 GMT
```

### Errore

Quando una chiamata genera un errore, viene restituito uno stato non 202 insieme a un corpo della risposta con ulteriori dettagli sull’errore.  Il corpo della risposta è application/json e contiene un singolo oggetto con i membri error_code e message.

Di seguito sono riportati i codici di errore riutilizzati da Adobe Developer Gateway.

| Codice di stato HTTP | error_code | messaggio |
| --- | --- | --- |
| 401 | 401013 | Token OAuth non valido |
| 403 | 403010 | Token OAuth mancante |
| 404 | 404040 | Risorsa non trovata |
| 429 | 429001 | Limite di utilizzo servizio raggiunto |

Di seguito sono riportati i codici di errore univoci dell’API di acquisizione dati, composti da 3 segmenti.  Le prime tre cifre sono lo stato (restituito da Adobe Developer Gateway), seguito da zero &quot;0&quot;, seguito da tre cifre.

| Codice di stato HTTP | error_code | messaggio |
| --- | --- | --- |
| 400 | 4000801 | Richiesta non valida |
| 400 | 4000802 | Dati non validi |
| 403 | 4030801 | Non autorizzato |
| 429 | 4290801 | Quota giornaliera raggiunta |
| 500 | 5000801 | Errore interno del server |

## Nuovi tentativi

Quando viene rilevato un errore transitorio, il servizio ritenta l’operazione tre volte.  Il primo nuovo tentativo si verifica dopo un periodo di attesa di 5 minuti, il secondo dopo altri 30 minuti e infine il terzo dopo altri 30 minuti.  I tentativi si verificano per vari motivi, principalmente quando un servizio dipendente va in timeout o non è temporaneamente disponibile.

## Endpoint

Gli endpoint di acquisizione sono disponibili per Persone, Oggetti personalizzati, Aziende e Membri del programma.

### Persone

Endpoint utilizzato per eseguire l&#39;upsert dei record persona.

| Metodo | Percorso |
| --- | --- |
| POST | /subscriptions/{munchkinId}/people |

#### Intestazioni

| Chiave | Valore |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
| --- | --- | --- | --- | --- |
| `priority` | Stringa | No | Priorità della richiesta: normale o alta | normale |
| `partitionName` | Stringa | No | Nome della partizione della persona | Predefinito |
| `dedupeFields` | Oggetto | No | Attributi per la deduplicazione. Sono consentiti uno o due nomi di attributo. <br/> In un&#39;operazione AND vengono utilizzati due attributi. Se ad esempio si specificano sia `email` che `firstName`, verranno entrambi utilizzati per cercare una persona tramite l&#39;operazione AND. <br/>Gli attributi supportati sono: `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, attributi personalizzati (solo tipo &quot;stringa&quot; e &quot;intero&quot;), `email` |  |
| `persons` | Array di oggetti | Sì | Elenco delle coppie nome-valore dell’attributo della persona | - |

Le autorizzazioni richieste sono `Read-Write Lead`.

### Esempio di persone

#### Richiesta

`POST /subscriptions/{munchkinId}/persons`

#### Intestazioni

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "priority": "high",
   "partitionName": "EMEA",
   "dedupeFields": {
      "field1": "email",
      "field2": "firstName"
   },
   "persons":[
      {
         "email": "brooklyn.parker@karnv.com",
         "firstName": "Brooklyn",
         "lastName": "Parker",
         "company": "Karnv"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal",
         "company": "Acme Inc"
      }
   ]
}
```

#### Risposta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Oggetti personalizzati

Endpoint utilizzato per eseguire l&#39;upsert dei record oggetto personalizzati.

| Metodo | Percorso |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Intestazioni

| Chiave | Valore |
| --- | --- |
| `Content-Type` | application/json |
| `X-Mkto-User-Token` | {accessToken} |

#### Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
| --- | --- | --- | --- | --- |
| `priority` | Stringa | No | Priorità della richiesta: normale, alta | normale |
| `dedupeBy` | Stringa | No | Attributi da deduplicare su: dedupeFields, marketoGUID | dedupeFields |
| `customObjects` | Array di oggetti | Sì | Elenco di coppie nome-valore dell&#39;attributo per l&#39;oggetto. | - |

Le autorizzazioni necessarie sono `Read-Write Custom Object`.

Se nella richiesta è specificato un campo di collegamento a una persona e tale persona non esiste, si verificano diversi tentativi. Se la persona viene aggiunta durante la finestra dei nuovi tentativi (65 minuti), l’aggiornamento ha esito positivo. Ad esempio, se il campo del collegamento è e-mail su Persona e questa non esiste, si verifica un nuovo tentativo.

### Esempio di oggetti personalizzati

#### Richiesta

`POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}`

#### Intestazioni

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "dedupeBy": "dedupeFields",
   "priority": "high",
   "customObjects": [
      {
         "email": "brooklyn.parker@karnv.com",
         "vin": "20UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 330i",
         "year": 2003
      },
      {
         "email": "johnny.neal@yvu30.com",
         "vin": "19UYA31581L000000",
         "make": "BMW",
         "model": "3-Series 325i",
         "year": 1989
      }
   ]
}
```

#### Risposta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Aziende

Endpoint utilizzato per sincronizzare i record aziendali. Supporta le operazioni di creazione, aggiornamento e upsert con deduplicazione tramite ID società esterno o ID interno Marketo.

| Metodo | Percorso |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/companies` |

#### Intestazioni

| Chiave | Valore | Obbligatorio |
| --- | --- | --- |
| `Content-Type` | application/json | Sì |
| `X-Mkto-User-Token` | {accessToken} | Sì |
| `X-Correlation-Id` | Stringa arbitraria (lunghezza massima 255 caratteri) | No |
| `X-Request-Source` | Stringa arbitraria (lunghezza massima 50 caratteri) | No |

#### Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
| --- | --- | --- | --- | --- |
| `action` | Stringa | No | Azione di sincronizzazione: `createOnly`, `updateOnly` o `createOrUpdate` | `createOrUpdate` |
| `dedupeBy` | Stringa | No | Campo da deduplicare il: `dedupeFields` o `idField` (senza distinzione maiuscole/minuscole). Per `createOnly` e `createOrUpdate`, è consentito solo `dedupeFields`. Per `updateOnly`, entrambi sono consentiti. | `dedupeFields` |
| `input` | Array di oggetti | Sì | Elenco di coppie nome-valore di attributo della società. Accetta la chiave JSON `input` o `companies`. | - |

Ogni oggetto aziendale nell&#39;array `input` supporta i campi seguenti:

| Chiave | Tipo di dati | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| `externalCompanyId` | Stringa | Condizionale | Identificatore società esterno. Obbligatorio quando `dedupeBy` è `dedupeFields`. Non consentito quando `dedupeBy` è `idField`. |
| `id` | Lungo | Condizionale | ID società interno di Marketo. Obbligatorio quando `dedupeBy` è `idField` e `action` è `updateOnly`. Non consentito quando `dedupeBy` è `dedupeFields`. |
| `company` | Stringa | No | Nome dell’azienda. |
| qualsiasi campo | Any | No | Campi aziendali standard o personalizzati aggiuntivi definiti da [Descrivi società](companies.md). I nomi dei campi non fanno distinzione tra maiuscole e minuscole. |

Le autorizzazioni necessarie sono `Read-Write Company`.

### Esempio di società

#### Richiesta

`POST /subscriptions/{munchkinId}/companies`

#### Intestazioni

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "action": "createOrUpdate",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "externalCompanyId": "ext-company-001",
         "company": "Acme Corporation",
         "industry": "Technology",
         "numberOfEmployees": 5000,
         "annualRevenue": 100000000
      },
      {
         "externalCompanyId": "ext-company-002",
         "company": "Globex Industries",
         "industry": "Manufacturing",
         "numberOfEmployees": 1200
      }
   ]
}
```

#### Risposta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Esempio di aggiornamento società per ID

```json
{
   "action": "updateOnly",
   "dedupeBy": "idField",
   "input": [
      {
         "id": 12345,
         "company": "Acme Corporation (Renamed)",
         "numberOfEmployees": 5500
      }
   ]
}
```

### Regole di convalida società

| Regola | Dettaglio |
| --- | --- |
| action | Deve essere uno dei valori seguenti: `createOnly`, `updateOnly`, `createOrUpdate`. Maiuscole/minuscole. |
| dedupeBy | Deve essere `dedupeFields` o `idField` (senza distinzione maiuscole/minuscole). Impostazione predefinita: `dedupeFields`. |
| dedupeBy + azione | `createOnly` e `createOrUpdate` consentono solo `dedupeFields`. `updateOnly` consente sia `dedupeFields` che `idField`. |
| Quando `dedupeBy=dedupeFields` | Ogni società deve avere `externalCompanyId`. Il campo `id` non deve essere presente. |
| Quando `dedupeBy=idField` | Ogni società deve avere `id`. Il campo `externalCompanyId` non deve essere presente. |
| `input` / `companies` | Non può essere null o vuoto. |
| Numero massimo di oggetti per richiesta | 1,000 |

### Membri del programma (sincronizzazione)

Endpoint utilizzato per sincronizzare lo stato dei membri del programma, aggiungere lead ai programmi o aggiornarne lo stato.

| Metodo | Percorso |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers` |

#### Intestazioni

| Chiave | Valore | Obbligatorio |
| --- | --- | --- |
| Content-Type | application/json | Sì |
| X-Mkto-User-Token | {accessToken} | Sì |
| X-Correlation-Id | Stringa arbitraria (lunghezza massima 255 caratteri) | No |
| X-Request-Source | Stringa arbitraria (lunghezza massima 50 caratteri) | No |

#### Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
| --- | --- | --- | --- | --- |
| programmi | Array di oggetti | Sì | Elenco delle operazioni del programma. Ogni specifica un programma, uno stato di destinazione e i lead per la sincronizzazione. | - |

Ogni oggetto nell&#39;array `programs` contiene:

| Chiave | Tipo di dati | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| programId | Lungo | Sì | ID del programma Marketo. Deve essere un numero intero positivo. |
| stato | Stringa | Sì | Stato del membro del programma da impostare, ad esempio `"Member"` o `"Influenced"`. Accetta la chiave JSON `statusName` o `status`. Il valore non deve essere `"Not in Program"`. Utilizzare l&#39;endpoint di eliminazione. |
| membri | Array di oggetti | Sì | Elenco dei riferimenti di lead da aggiungere o aggiornare nel programma. Accetta la chiave JSON `input` o `members`. |

Ogni oggetto nell&#39;array `members` contiene:

| Chiave | Tipo di dati | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| leadId | Lungo | Sì | L’ID del lead di Marketo. |
| qualsiasi campo | Any | No | Campi aggiuntivi dei membri del programma personalizzati. I nomi dei campi non fanno distinzione tra maiuscole e minuscole. |

Le autorizzazioni necessarie sono `Read-Write Lead`.

### Esempio di sincronizzazione dei membri del programma

#### Richiesta

`POST /subscriptions/{munchkinId}/programmembers`

#### Intestazioni

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "programs": [
      {
         "programId": 1001,
         "status": "Member",
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "status": "Influenced",
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Risposta

`HTTP/1.1 202`
`X-Request-ID: e3d92152-0fb1-444a-8f8f-29d5a2338598`

### Regole di convalida sincronizzazione membri programma

| Regola | Dettaglio |
| --- | --- |
| programmi | Non può essere null o vuoto. |
| programId | Obbligatorio. Deve essere un numero intero positivo. |
| stato | Obbligatorio. Non deve essere vuoto. Non deve essere `"Not in Program"` (senza distinzione maiuscole/minuscole). Utilizza invece l’endpoint di eliminazione. |
| membri | Non può essere null o vuoto. |
| leadId | Obbligatorio per ogni membro nell&#39;array di input. |
| Numero massimo di lead per richiesta | 1.000 membri totali in tutti i programmi. |

### Membri del programma (eliminazione)

Endpoint utilizzato per rimuovere i lead dai programmi. Imposta lo stato di appartenenza del lead su `"Not in Program"` e rimuove il membro da tale programma.

>[!NOTE]
>
>Questo endpoint utilizza POST anziché DELETE perché la richiesta richiede un corpo JSON con dati strutturati.

| Metodo | Percorso |
| --- | --- |
| POST | `/subscriptions/{munchkinId}/programmembers/delete` |

#### Intestazioni

| Chiave | Valore | Obbligatorio |
| --- | --- | --- |
| Content-Type | application/json | Sì |
| X-Mkto-User-Token | {accessToken} | Sì |
| X-Correlation-Id | Stringa arbitraria (lunghezza massima 255 caratteri) | No |
| X-Request-Source | Stringa arbitraria (lunghezza massima 50 caratteri) | No |

#### Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
| --- | --- | --- | --- | --- |
| programmi | Array di oggetti | Sì | Elenco delle operazioni di eliminazione del programma. Ogni specifica un programma e i lead da rimuovere. | - |

Ogni oggetto nell&#39;array `programs` contiene:

| Chiave | Tipo di dati | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| programId | Lungo | Sì | ID del programma Marketo. Deve essere un numero intero positivo. |
| membri | Array di oggetti | Sì | Elenco dei riferimenti di lead da rimuovere dal programma. Accetta la chiave JSON `input` o `members`. |

Ogni oggetto nell&#39;array `members` contiene:

| Chiave | Tipo di dati | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| leadId | Lungo | Sì | L’ID del lead di Marketo. |

Le autorizzazioni necessarie sono `Read-Write Lead`.

### Esempio di eliminazione membri del programma

#### Richiesta

`POST /subscriptions/{munchkinId}/programmembers/delete`

#### Intestazioni

`Content-Type: application/json`
`X-Mkto-User-Token: {accessToken}`

#### Corpo

```json
{
   "programs": [
      {
         "programId": 1001,
         "members": [
            {
               "leadId": 10001
            },
            {
               "leadId": 10002
            }
         ]
      },
      {
         "programId": 1002,
         "members": [
            {
               "leadId": 10003
            }
         ]
      }
   ]
}
```

#### Risposta

`HTTP/1.1 202`
`X-Request-ID: a1b2c3d4-e5f6-7890-abcd-ef1234567890`

### I membri del programma eliminano le regole di convalida

| Regola | Dettaglio |
| --- | --- |
| programmi | Non può essere null o vuoto. |
| programId | Obbligatorio. Deve essere un numero intero positivo. |
| membri | Non può essere null o vuoto. |
| leadId | Obbligatorio per ogni membro nell&#39;array di input. |
| Numero massimo di lead per richiesta | 1.000 membri totali in tutti i programmi. |

## Limiti

Ecco un elenco aggiornato dei guardrail:

* Dimensione massima della richiesta: 1 MB
* Massimo oggetti per richiesta per tipo di oggetto: 1.000
* Numero massimo di richieste al secondo per ID client: 5.000
* Massimo oggetti al giorno: 10.000.000

Questi limiti si applicano in modo uniforme a Persone, Oggetti personalizzati, Aziende e Membri del programma. Per i membri del programma, &quot;oggetti per richiesta&quot; è il numero totale di riferimenti lead in tutti i programmi in una singola richiesta.

## API di acquisizione dati e API REST

Elenco delle differenze tra l’API di acquisizione dati e altre API REST di Marketo:

* Per eseguire l&#39;autenticazione, devi passare il token di accesso utilizzando l&#39;intestazione `X-Mkto-User-Token`
* Il nome di dominio URL è `mkto-ingestion-api.adobe.io`
* Il percorso URL inizia con `/subscriptions/MunchkinId`
* Nessun parametro di query
* Se la chiamata ha esito positivo, viene restituito lo stato 202 e il corpo della risposta è vuoto
* Se una chiamata non riesce, viene restituito uno stato non 202 e il corpo della risposta contiene `{ "error_code" : "Error Code", "message" : "Message" }`
* L&#39;ID della richiesta viene restituito tramite l&#39;intestazione `X-Request-Id`
