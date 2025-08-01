---
title: Acquisizione dei dati
feature: REST API, Dynamic Content
description: Utilizzare i dati con le API di Marketo.
exl-id: 1d501916-53ac-42d8-a804-abb4ab01c7e8
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 10%

---

# API di acquisizione dati

L’API di acquisizione dati è un servizio ad alto volume, a bassa latenza e a disponibilità elevata progettato per gestire in modo efficiente e con ritardi minimi l’acquisizione di grandi quantità di dati relativi a persone e persone.

I dati vengono acquisiti inviando le richieste eseguite in modo asincrono. È possibile recuperare lo stato della richiesta sottoscrivendo eventi da [Marketo Observability Data Stream](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-observability-data-stream-setup).&#x200B;

Le interfacce sono disponibili per due tipi di oggetto: Persone, Oggetti personalizzati. L&#39;operazione di registrazione è solo &quot;insert or update&quot; (Inserisci o aggiorna).

L’API di acquisizione dati è attualmente in versione beta privata.  Gli invitati devono avere un diritto per il pacchetto [Marketo Engage Performance Tier](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Autenticazione

L&#39;API di acquisizione dati utilizza lo stesso metodo di autenticazione OAuth 2.0 dell&#39;API REST di Marketo per generare un token di accesso, ma il token di accesso deve essere passato tramite l&#39;intestazione HTTP `X-Mkto-User-Token`. Non è possibile passare il token di accesso tramite un parametro di query.

Esempio di token di accesso tramite intestazione:

`X-Mkto-User-Token: 11606815-aa7a-405a-80a1-f9683efa528b:ab`

## Autorizzazioni

L’acquisizione dei dati utilizza lo stesso modello di autorizzazioni dell’API REST di Marketo e non richiede alcuna autorizzazione speciale aggiuntiva da utilizzare, anche se sono necessarie autorizzazioni specifiche per ogni endpoint.

| Endpoint | Autorizzazione |
|-|-|
| Persone | Lead di lettura/scrittura |
| Oggetti personalizzati | Oggetto personalizzato di lettura/scrittura |

## Intestazioni

L’acquisizione dei dati utilizza le seguenti intestazioni HTTP personalizzate.

### Richiesta

| Chiave | Valore | Obbligatorio | Descrizione |
| - | - | - | - |
| X-Correlation-Id | Stringa arbitraria (lunghezza massima 255 caratteri). | No | Può essere utilizzato per tracciare le richieste attraverso il sistema.  Consulta Flusso di dati di osservabilità Marketo |
| X-Request-Source | Stringa arbitraria (lunghezza massima 50 caratteri). | No | Può essere utilizzato per tracciare l’origine delle richieste tramite il sistema.  Consulta Flusso di dati di osservabilità Marketo |

### Risposta

| Chiave | Valore | Obbligatorio |
| - | - | - |
| X-Request-Id | ID richiesta univoco. | Sì |

## Richieste

Utilizzare il metodo HTTP POST per inviare dati al server.

La rappresentazione dei dati è inclusa nel corpo della richiesta come application/json.

Nome di dominio: `mkto-ingestion-api.adobe.io`

Il percorso inizia con `/subscriptions/MunchkinId`, dove MunchkinId è specifico per l&#39;istanza Marketo. Puoi trovare il tuo Munchkin ID nell&#39;interfaccia utente di Marketo Engage in **Amministratore** > **Il mio account** > **Informazioni di supporto**.  Il resto del percorso viene utilizzato per specificare la risorsa di interesse.

URL di esempio per Persone:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/persons`

URL di esempio per oggetti personalizzati:

`https://mkto-ingestion-api.adobe.io/subscriptions/556-RJS-213/customobjects/purchases`

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
| - | - | - |
| 401 | 401013 | Token OAuth non valido |
| 403 | 403010 | Token OAuth mancante |
| 404 | 404040 | Risorsa non trovata |
| 429 | 429001 | Limite di utilizzo servizio raggiunto |

Di seguito sono riportati i codici di errore univoci dell’API di acquisizione dati, composti da 3 segmenti.  Le prime tre cifre sono lo stato (restituito da Adobe Developer Gateway), seguito da zero &quot;0&quot;, seguito da tre cifre.

| Codice di stato HTTP | error_code | messaggio |
| - | - | - |
| 400 | 4000801 | Richiesta non valida |
| 400 | 4000802 | Dati non validi |
| 403 | 4030801 | Non autorizzato |
| 429 | 4290801 | Quota giornaliera raggiunta |
| 500 | 5000801 | Errore interno del server |

## Nuovi tentativi

Quando viene rilevato un errore transitorio, il servizio ritenta l’operazione tre volte.  Il primo nuovo tentativo si verifica dopo un periodo di attesa di 5 minuti, il secondo dopo altri 30 minuti e infine il terzo dopo altri 30 minuti.  I tentativi si verificano per vari motivi, principalmente quando un servizio dipendente va in timeout o non è temporaneamente disponibile.

## Endpoint

Gli endpoint di acquisizione sono disponibili per Persone e Oggetti personalizzati.

### Persone

Endpoint utilizzato per eseguire l&#39;upsert dei record persona.

| Metodo | Percorso |
| - | - |
| POST | /subscriptions/{munchkinId}/people |

#### Intestazioni

| Chiave | Valore |
| - | - |
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

#### Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
| - | - | - | - | - |
| priorità | Stringa | No | Priorità della richiesta: normale o alta | normale |
| partitionName | Stringa | No | Nome della partizione della persona | Predefinito |
| dedupeFields | Oggetto | No | Attributi per la deduplicazione. Sono consentiti uno o due nomi di attributo. <br/> In un&#39;operazione AND vengono utilizzati due attributi. Se ad esempio si specificano sia `email` che `firstName`, verranno entrambi utilizzati per cercare una persona tramite l&#39;operazione AND. <br/>Attributi supportati: `id`, `email`, `sfdcAccountId`, `sfdcContactId`, `sfdcLeadId` `sfdcLeadOwnerId`, Attributi personalizzati (solo tipo &quot;stringa&quot; e &quot;numero intero&quot;), `email` |
| persone | Array di oggetti | Sì | Elenco delle coppie nome-valore dell’attributo della persona | - |

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

#### Risposta

`HTTP/1.1 202`
`X-Request-ID: WOUBf3fHJNU6sTmJqLL281lOmAEpMZFw`

### Oggetti personalizzati

Endpoint utilizzato per eseguire l&#39;upsert dei record oggetto personalizzati

| Metodo | Percorso |
| - | - |
| POST | `/subscriptions/{munchkinId}/customobjects/{customObjectAPIName}` |

#### Intestazioni

| Chiave | Valore |
| - | - |
| Content-Type | application/json |
| X-Mkto-User-Token | {accessToken} |

#### Corpo della richiesta

| Chiave | Tipo di dati | Obbligatorio | Valore | Valore predefinito |
| - |- | - | - | - |
| priorità | Stringa | No | Priorità della richiesta: normale, alta | normale |
| dedupeBy | Stringa | No | Attributi da deduplicare su: dedupeFields, marketoGUID | dedupeFields |
| customObjects | Array di oggetti | Sì | Elenco di coppie nome-valore dell&#39;attributo per l&#39;oggetto. | - |

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

## Limiti

Elenco di utilizzo dei guardrail:

* Dimensione massima della richiesta: 1 MB
* Massimo oggetti per richiesta per tipo di oggetto: 1.000
* Numero massimo di richieste al secondo per ID client: 5.000
* Massimo oggetti al giorno: 10.000.000

## API di acquisizione dati e API REST

Elenco delle differenze tra l’API di acquisizione dati e altre API REST di Marketo:

* Questa non è un&#39;interfaccia CRUD completa, ma supporta solo upsert
* Per eseguire l&#39;autenticazione, devi passare il token di accesso utilizzando l&#39;intestazione `X-Mkto-User-Token`
* Il nome di dominio URL è `mkto-ingestion-api.adobe.io`
* Il percorso URL inizia con `/subscriptions/MunchkinId`
* Nessun parametro di query
* Se la chiamata ha esito positivo, viene restituito lo stato 202 e il corpo della risposta è vuoto
* Se la chiamata non riesce, viene restituito uno stato non 202 e il corpo della risposta contiene `{ "error_code" : "Error Code", "message" : "Message" }`
* L&#39;ID della richiesta viene restituito tramite l&#39;intestazione `X-Request-Id`
