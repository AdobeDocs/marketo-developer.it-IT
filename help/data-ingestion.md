---
title: "Acquisizione dei dati"
description: "Panoramica dell’API di acquisizione dati"
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 9%

---


# Acquisizione dei dati

L’API di acquisizione dati è un servizio ad alto volume, a bassa latenza e a disponibilità elevata progettato per gestire l’acquisizione di grandi quantità di dati relativi a persone e persone in modo efficiente e con ritardi minimi. 

I dati vengono acquisiti inviando le richieste eseguite in modo asincrono. Lo stato della richiesta può essere recuperato sottoscrivendo eventi da [Flusso dati di osservabilità Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup/).&#x200B;

Le interfacce sono disponibili per due tipi di oggetto: Persone, Oggetti personalizzati. L&#39;operazione di registrazione è solo &quot;insert or update&quot; (Inserisci o aggiorna).

L’API di acquisizione dati è in versione beta privata. Gli invitati devono avere diritto a [Pacchetto livello prestazioni Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Autenticazione

Per generare un token di accesso, l’API di acquisizione dati utilizza lo stesso metodo di autenticazione OAuth 2.0 dell’API REST di Marketo, ma il token di accesso deve essere trasmesso tramite intestazione HTTP `X-Mkto-User-Token`. Non è possibile passare il token di accesso tramite un parametro di query.

Esempio di token di accesso tramite intestazione:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Autorizzazioni

L’acquisizione dei dati utilizza lo stesso modello di autorizzazioni dell’API REST di Marketo e non richiede autorizzazioni speciali aggiuntive da utilizzare, anche se sono necessarie autorizzazioni specifiche per ogni endpoint.

| Endpoint | Autorizzazione |
|---|---|
| Persone | Lead di lettura/scrittura |
| Oggetti personalizzati | Oggetto personalizzato di lettura/scrittura |

## Intestazioni

L’acquisizione dei dati utilizza le seguenti intestazioni HTTP personalizzate.

### Richiesta

| Chiave | Valore | Obbligatorio | Descrizione |
|---|---|---|---|
| X-Correlation-Id | Stringa arbitraria (lunghezza massima 255 caratteri). | No | Può essere utilizzato per tracciare le richieste tramite il sistema. Consulta Flusso di dati di osservabilità Marketo |
| X-Request-Source | Stringa arbitraria (lunghezza massima 50 caratteri). | No | Può essere utilizzato per tracciare l’origine delle richieste tramite il sistema. Consulta Flusso di dati di osservabilità Marketo |

### Risposta

| Chiave | Valore | Obbligatorio | Descrizione |
|---|---|---|---|
| X-Request-Id | ID richiesta univoco. | Sì | |

## Richieste

Utilizzare il metodo HTTP POST per inviare dati al server.

La rappresentazione dei dati è inclusa nel corpo della richiesta come application/json.

Il nome di dominio è: `mkto-ingestion-api.adobe.io`

Il percorso inizia con `/subscriptions/_MunchkinId_` dove `_MunchkinId_` è specifico per la tua istanza di Marketo. Puoi trovare il tuo ID Munchkin nell’interfaccia utente del Marketo Engage in **Amministratore** >**Il mio account** > **Informazioni di supporto**. Il resto del percorso viene utilizzato per specificare la risorsa di interesse.

URL di esempio per Persone:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

URL di esempio per oggetti personalizzati:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

## Risposte

Tutte le risposte restituiscono un ID richiesta univoco tramite `X-Request-Id` intestazione.

Esempio di ID richiesta tramite intestazione:

`X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Operazione riuscita

Quando una chiamata ha esito positivo, viene restituito lo stato 202. Nessun corpo di risposta restituito.

Esempio di risposta di successo:

`HTTP/1.1 202 Accepted` `X-Request-Id: e3d92152-0fb1-444a-8f8f-29d5a2338598` `Content-Length: 0` `Date: Wed, 18 Oct 2023 18:56:49 GMT`

### Errore

Quando una chiamata genera un errore, viene restituito uno stato non 202 insieme a un corpo della risposta con ulteriori dettagli sull’errore. Il corpo della risposta è application/json e contiene un singolo oggetto con membri `error_code` e `message`.

Di seguito sono riportati i codici di errore riutilizzati da Adobe Developer Gateway.

| Codice di stato HTTP | error_code | messaggio |
|--- |--- |--- |
| 401 | 401013 | Token OAuth non valido |
| 403 | 403010 | Token OAuth mancante |
| 404 | 404040 | Risorsa non trovata |
| 429 | 429001 | Limite di utilizzo servizio raggiunto |

Di seguito sono riportati i codici di errore univoci dell’API di acquisizione dati, composti da tre segmenti. Le prime tre cifre sono lo stato (restituito da Adobe IO Gateway), seguito da uno zero &quot;0&quot;, seguito da tre cifre.

| Codice di stato HTTP | error_code | messaggio |
|--- |--- |--- |
| 400 | 4000801 | Richiesta non valida |
| 400 | 4000802 | Dati non validi |
| 403 | 4030801 | Non autorizzato |
| 429 | 4290801 | Quota giornaliera raggiunta |
| 500 | 5000801 | Errore interno del server |

Esempio di risposta di errore:

`HTTP/1.1 403 Forbidden` `Content-Type: application/json` `Content-Length: 54` `Date: Wed, 18 Oct 2023 19:10:07 GMT` `X-Request-Id: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw` `{"error_code":"403010","message":"Oauth token is missing"}`

## Nuovi tentativi

Quando viene rilevato un errore transitorio, il servizio ritenta l’operazione tre volte. Il primo nuovo tentativo si verifica dopo un periodo di attesa di 5 minuti, il secondo dopo altri 30 minuti e infine il terzo dopo altri 30 minuti. I tentativi si verificano per vari motivi, principalmente quando un servizio dipendente va in timeout o non è temporaneamente disponibile.

## Endpoint

Gli endpoint di acquisizione sono disponibili per Persone e Oggetti personalizzati.

### Persone

Endpoint utilizzato per eseguire l&#39;upsert dei record persona.

| Metodo |
|---|
| POST |

| Percorso |
|---|
| `/subscriptions/{munchkinId}/persons` |

| HeadersKey | Valore |
|---|---|
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
|---|---|---|---|---|
| priorità | Stringa | No | Priorità della richiesta:normalhigh | normale |
| partitionName | Stringa | No | Nome della partizione della persona | Predefinito |
| dedupeFields | Oggetto | No | Attributi per la deduplicazione. Sono consentiti uno o due nomi di attributo. In un&#39;operazione AND vengono utilizzati due attributi. Ad esempio, se entrambi `email` e `firstName` vengono entrambi utilizzati per cercare una persona utilizzando l&#39;operazione AND. Gli attributi supportati sono:`idemail`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId`, `sfdcLeadOwnerIdCustom` attributi (solo tipo &quot;string&quot; e &quot;integer&quot;) | e-mail |
| persone | Array di oggetti | Sì | Elenco delle coppie nome-valore dell’attributo della persona | - |

| Autorizzazione |
|---|
| Lead di lettura/scrittura |

#### Esempio di persone

```
POST /subscriptions/{munchkinId}/persons
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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
         "lastName": "Parker"
      },
      {
         "email": "johnny.neal@yvu30.com",
         "firstName": "Johnny",
         "lastName": "Neal"
      }
   ]
}
```

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

### Oggetti personalizzati

Endpoint utilizzato per eseguire l&#39;upsert dei record oggetto personalizzati.

| Metodo |
|---|
| POST |

| Percorso |
|---|
| `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

Intestazioni

| Chiave | Valore |
|---|---|
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
|---|---|---|---|---|
| priorità | Stringa | No | Priorità della richiesta:normalhigh | normale |
| dedupeBy | Stringa | No | Attributi da deduplicare su:dedupeFieldMarketoGUID | dedupeFields |
| customObjects | Array di oggetti | Sì | Elenco di coppie nome-valore dell&#39;attributo per l&#39;oggetto. | - |

| Autorizzazione |
|---|
| Oggetto personalizzato di lettura/scrittura |

#### Persona non presente

Se nella richiesta è specificato un campo di collegamento a una persona e tale persona non esiste, si verificano diversi tentativi. Se la persona viene aggiunta durante la finestra dei nuovi tentativi (65 minuti), l’aggiornamento ha esito positivo. Ad esempio, se il campo del collegamento è `email` su Persona e Persona non esiste, quindi si verificano nuovi tentativi.

#### Esempio di oggetti personalizzati

```
POST /subscriptions/{munchkinId}/customobjects/{customObjectAPIName}
```

```
Content-Type: application/json
X-Mkto-User-Token: {accessToken}
```

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

```
HTTP/1.1 202
X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw
```

## Limiti

Elenco di utilizzo dei guardrail:

- Dimensione massima della richiesta: 1 MB
- Massimo oggetti per richiesta per tipo di oggetto: 1.000
- Numero massimo di richieste al secondo per ID client: 5.000
- Massimo oggetti al giorno: 10.000.000

## API di acquisizione dati e API REST

Elenco delle differenze tra l’API di acquisizione dati e altre API REST di Marketo:

- Questa non è un&#39;interfaccia CRUD completa, ma supporta solo &#39;upsert&#39;
- Per eseguire l’autenticazione, devi passare il token di accesso utilizzando `X-Mkto-User-Token` intestazione
- Il nome di dominio dell’URL è `mkto-ingestion-api.adobe.io`
- Il percorso URL inizia con `/subscriptions/_MunchkinId_`
- Nessun parametro di query
- Se la chiamata ha esito positivo, viene restituito lo stato 202 e il corpo della risposta è vuoto
- Se la chiamata non riesce, viene restituito uno stato non 202 e il corpo della risposta contiene `{ "error_code" : "_Error Code_", "message" : "_Message_" }`
- L’ID della richiesta viene restituito tramite `X-Request-Id` intestazione
