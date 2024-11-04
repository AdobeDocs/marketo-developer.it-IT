---
title: Attività
feature: REST API
description: API per la gestione delle attività del Marketo Engage.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 6baf62bc8881470eca597899e3228c377fb597d0
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 0%

---

# Attività

Marketo consente un&#39;ampia varietà di tipi di attività relativi ai record dei lead.  Quasi ogni modifica, azione o passaggio di flusso viene registrato rispetto al registro attività di un lead e può essere recuperato tramite l’API o utilizzato nei filtri e nei trigger di Smart Campaign e Smart List.  Le attività sono sempre correlate al record lead tramite il leadId, corrispondente al campo Id del record e dispone anche di un ID univoco.

Esistono molti tipi di attività potenziali, che possono variare da un abbonamento all’altro e che hanno definizioni univoche per ciascuno di essi. Anche se ogni attività ha il proprio `id`, `leadId` e `activityDate`, i valori `primaryAttributeValueId` e `primaryAttributeValue` variano nel loro significato.

Marketo consente inoltre la creazione di tipi di attività personalizzati tramite l’API di metadati delle attività personalizzate. L’aggiunta di attività personalizzate viene eseguita tramite l’API Add Custom Activities.

La maggior parte delle attività verrà eliminata dopo un certo periodo di tempo.

## Descrivere

Per recuperare un elenco dei tipi disponibili e delle relative definizioni per un&#39;istanza, è possibile utilizzare l&#39;endpoint [Get Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET).

```
GET /rest/v1/activities/types.json
```

```json
  "requestId": "6e78#148ad3b76f1",
  "success": true,
  "result": [
    {
      "id": 2,
      "name": "Fill Out Form",
      "description": "User fills out and submits form on web page",
      "primaryAttribute": {
        "name": "Webform ID",
        "dataType": "integer"
      },
      "attributes": [
        {
          "name": "Client IP Address",
          "dataType": "string"
        },
        {
          "name": "Form Fields",
          "dataType": "text"
        },
        {
          "name": "Query Parameters",
          "dataType": "string"
        },
        {
          "name": "Referrer URL",
          "dataType": "string"
        },
        {
          "name": "User Agent",
          "dataType": "string"
        },
        {
          "name": "Webpage ID",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

Le risposte dal mondo reale includono molte più definizioni. In questo esempio, il tipo visualizzato è &quot;Compila modulo&quot;, con un attributo principale di &quot;ID modulo web&quot; che fa riferimento all’ID Marketo del modulo compilato e può essere utilizzato per riferirsi a quella particolare risorsa in Marketo. Inoltre, esistono definizioni per ciascuno dei possibili attributi di un particolare record di attività di questo tipo e dei relativi tipi di dati. Tieni presente che se il campo è vuoto, quel particolare attributo viene omesso da un singolo record di attività.

## Query

Per recuperare le attività da Marketo, chiama l&#39;endpoint [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET). Devi innanzitutto recuperare un token di paging per il datetime da cui vuoi iniziare a recuperare le attività. Passare quindi il token di paging nel parametro di query `nextPageToken`. Inoltre, si passano fino a dieci ID del tipo di attività nel parametro di query `activityTypeIds` come elenco separato da virgole.

Facoltativamente è possibile includere un parametro di query listId per limitare la ricerca ai soli record inclusi in un elenco statico specifico oppure un parametro di query leadId e cercare le attività solo da un set specificato di lead. È possibile passare fino a 30 leadID come elenco separato da virgole.

```
GET /rest/v1/activities.json?activityTypeIds=1&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3I3LCWXH3Y6IIZ7YSGQLXHCPVE5Q====
```

```json
{
  "requestId": "24fd#15188a88d7f",
  "result": [
    {
      "id": 102988,
      "marketoGUID": "102988",
      "leadId": 1,
      "activityDate": "2023-01-16T23:32:19Z",
      "activityTypeId": 1,
      "primaryAttributeValueId": 71,
      "primaryAttributeValue": "localhost/munchkintest2.html",
      "attributes": [
        {
          "name": "Client IP Address",
          "value": "10.0.19.252"
        },
        {
          "name": "Query Parameters",
          "value": ""
        },
        {
          "name": "Referrer URL",
          "value": ""
        },
        {
          "name": "User Agent",
          "value": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
        },
        {
          "name": "Webpage URL",
          "value": "/munchkintest2.html"
        }
      ]
    }
  ],
  "success": true,
  "nextPageToken": "WQV2VQVPPCKHC6AQYVK7JDSA3J62DUSJ3EXJGDPTKPEBFW3SAVUA====",
  "moreResult": false
}
```

Per la prima chiamata, utilizza l&#39;API Get Paging Token per ottenere `nextPageToken`. Per le chiamate successive a questo endpoint, utilizza `nextPageToken returned` dalla risposta. Questo endpoint restituisce sempre `the nextPageToken`.

Se l&#39;attributo `moreResult` è true, significa che sono disponibili altri risultati. Continuare a chiamare questo endpoint fino a quando l&#39;attributo `moreResult` non restituisce false, ovvero non sono disponibili risultati. I `nextPageToken` restituiti da questa API devono essere sempre riutilizzati per la successiva iterazione di questa chiamata.

In alcuni casi, questa API potrebbe rispondere con meno di 300 elementi attività, ma anche avere l&#39;attributo `moreResult` impostato su true.  Ciò indica che è possibile restituire più attività e che è possibile eseguire una query sull&#39;endpoint per le attività più recenti includendo `nextPageToken` restituito in una chiamata successiva.

All&#39;interno di ogni elemento dell&#39;array dei risultati, l&#39;attributo intero `id` viene sostituito dall&#39;attributo stringa `marketoGUID` come identificatore univoco. 

### Modifiche al valore dei dati

Per le attività di modifica del valore dei dati, viene fornita una versione specializzata dell’API delle attività. L&#39;endpoint [Get Lead Changes](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) restituisce solo le attività dei record Data Value Change in campi lead. L’interfaccia è la stessa dell’API Get Lead Activities, con due differenze:

* Non esiste alcun parametro `activityTypeIds`, poiché l&#39;endpoint restituisce solo le attività Modifica valore dati e Nuovo lead.
* Il parametro di query `fields` è obbligatorio, dove è possibile trasmettere un elenco di campi separato da virgole per indicare per quali campi si desidera recuperare le modifiche.

```
GET /rest/v1/activities/leadchanges.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&fields=firstName,lastName,department
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 1078,
      "marketoGUID": "1078",
      "leadId": 775,
      "activityDate": "2014-09-17T22:31:49+0000",
      "activityTypeId": 13,
      "fields": [
        {
          "id": 48,
          "name": "firstName",
          "newValue": "FirstName_6176",
          "oldValue": "FirstName_4914"
        }
      ],
      "attributes": [
        {
          "name": "Reason",
          "value": "Web service API"
        },
        {
          "name": "Source",
          "value": "Web service API"
        },
        {
          "name": "Lead ID",
          "value": 775
        }
      ]
    }
  ]
}
```

Ogni attività nella risposta dispone di un array di campi, incluso un elenco di modifiche nell&#39;attività, che specificherà `id` e `name` del campo modificato, nonché i valori nuovi e vecchi relativi alla modifica.

All&#39;interno di ogni elemento dell&#39;array dei risultati, l&#39;attributo intero `id` viene sostituito dall&#39;attributo stringa `marketoGUID` come identificatore univoco.

### Lead eliminati

Esiste anche un endpoint speciale [Ottieni lead eliminati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) per recuperare le attività eliminate da Marketo.

```
GET /rest/v1/activities/deletedleads.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 2,
      "marketoGUID": "2",
      "leadId": 6,
      "activityDate": "2013-09-26T06:56:35+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 6,
      "primaryAttributeValue": "Owyliphys Iledil",
      "attributes": []
    },
    {
      "id": 3,
      "marketoGUID": "3",
      "leadId": 9,
      "activityDate": "2013-12-28T00:39:45+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 4,
      "primaryAttributeValue": "First Last",
      "attributes": []
    }
  ]
}
```

All&#39;interno di ogni elemento dell&#39;array dei risultati, l&#39;attributo intero `id` viene sostituito dall&#39;attributo stringa `marketoGUID` come identificatore univoco.

### Risultati pagina per pagina

Per impostazione predefinita, gli endpoint menzionati in questa sezione restituiscono 300 elementi attività alla volta.  Se l&#39;attributo `moreResult` è true, sono disponibili altri risultati. Chiamare l&#39;endpoint fino a quando l&#39;attributo `moreResult` non restituisce false, il che significa che non sono disponibili altri risultati. I `nextPageToken` restituiti da questo endpoint devono essere sempre riutilizzati per la successiva iterazione di questa chiamata.

In alcuni casi, questo endpoint può rispondere con meno di 300 elementi attività, ma con l&#39;attributo `moreResult` impostato su true.  Questo indica che ci sono attività aggiuntive che possono essere restituite e che è possibile eseguire una query sull&#39;endpoint per le attività più recenti includendo `nextPageToken` restituito in una chiamata successiva. Tieni presente che `nextPageToken` deve essere codificato nell&#39;URL nella richiesta.

## Tipi di attività personalizzati

Le attività personalizzate funzionano come le attività standard, tranne per il fatto che lo schema è gestito da terze parti e non da Marketo. Le istanze delle attività personalizzate sono collegate ai record dei lead tramite `leadId` proprio come le attività standard, ma sia gli attributi primari che secondari sono definiti arbitrariamente. Quando un tipo di attività personalizzato viene approvato, vengono creati un trigger di elenco avanzato e un filtro corrispondenti, in modo che i lead possano essere elaborati in base ai dati di attività personalizzati correnti o storici.

* Numero massimo di attività personalizzate: 10
* Numero massimo di attributi per attività personalizzata: 20

Il recupero dei dati delle attività personalizzate viene eseguito come le attività standard tramite l&#39;API [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET).

## Tipi di query

Oltre all&#39;endpoint standard Get Activity Types, gli endpoint [Get Custom Activity Types](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) e [Describe Custom Activity Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) restituiscono dettagli sui tipi di attività per i quali è stato eseguito il provisioning nell&#39;istanza di Marketo e metadati relativi agli attributi per un determinato tipo. I normali [Tipi di attività Get](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) restituiscono comunque i metadati relativi alle attività personalizzate, ma non indicano se un determinato tipo è personalizzato.

### Ottieni tipi

```
GET /rest/v1/activities/external/types.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved"
    }
  ]
}
```

### Descrivi tipi

Per le descrizioni dei tipi è necessario passare `apiName` come parametro di percorso. Per impostazione predefinita, si ottiene la versione approvata dell’attività. Facoltativamente, puoi passare il parametro `draft=true` per recuperare la versione bozza dell&#39;attività.

```
GET /rest/v1/activities/external/type/{apiName}/describe.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

## Crea tipo

Ogni tipo di attività personalizzata richiede un nome visualizzato, un nome API, un nome trigger, un nome filtro e un attributo primario.

Per garantire la coerenza dei tipi con le convenzioni di Marketo ed evitare conflitti, è importante seguire alcune linee guida durante la creazione dei tipi:

**Nome visualizzato:** Il nome visualizzato del tipo di attività deve descrivere brevemente ciò che rappresenta un record di attività, ad esempio &quot;Invia e-mail&quot; o &quot;Modifica valore dati&quot;. Questi nomi devono essere in genere nella forma infinita, ovvero &quot;Partecipa all’evento&quot;.  I nomi visualizzati accettano caratteri alfanumerici, spazi e trattini bassi. I nomi visualizzati devono contenere almeno una lettera.

**Nome API:** Il nome API è costituito da caratteri alfanumerici (lunghezza massima di 255). Se sei un partner LaunchPoint, devi anteporre uno spazio dei nomi rappresentativo ai nomi API del tipo di attività. In questo modo si evitano conflitti con i tipi con provisioning del cliente.  Per convenzione, utilizza tutte le lettere minuscole o CamelCase per distinguere altre stringhe di testo.

**Descrizione:** Per le attività che possono avere un comportamento non ovvio, includere una descrizione di ciò che il tipo di attività rappresenta in relazione al lead.

**Nome trigger:** Ogni tipo di attività deve avere un nome trigger univoco e leggibile. I nomi dei trigger devono essere nel tempo presente in terza persona, ad esempio &quot;Partecipa a un evento&quot;. I partner LaunchPoint devono includere nell’attività il nome della loro azienda, ad esempio &quot;Partecipa al webinar - Società Acme&quot;.

**Nome filtro:**  Ogni tipo di attività deve avere un nome di filtro univoco e leggibile. I nomi dei filtri devono rientrare nel tempo passato della terza persona, ad esempio &quot;Partecipazione a un evento&quot;. I partner LaunchPoint devono includere nell’attività il nome della loro azienda, ovvero &quot;Webinar Partecipato - Società Acme&quot;.

**Attributo primario:** L&#39;attributo primario di un&#39;attività personalizzata deve essere il campo più significativo per il tipo di attività. Ad esempio, per un’attività &quot;Evento partecipato&quot;, corrisponde al nome dell’evento. Gli attributi primari vengono inclusi come parametri per impostazione predefinita in ogni istanza di un trigger o di un filtro per quel tipo di attività e il valore viene visualizzato nel registro attività di un record persona senza richiedere un drill-down dell’attività.

Quando viene creata un’attività personalizzata, questa viene creata come bozza e deve essere approvata prima di poter essere utilizzata per aggiungere record di attività di quel tipo. Tutti gli aggiornamenti vengono applicati in modo implicito alla versione bozza del tipo. Per riflettere le modifiche nella versione live del tipo, è necessario approvarlo. Quando un tipo di attività personalizzato viene approvato e in uso, non è possibile apportare modifiche ai campi precedenti.

Durante la creazione di un tipo, il parametro di descrizione è facoltativo, mentre sono richiesti tutti i seguenti parametri: `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute`.

```
POST /rest/v1/activities/external/type.json
```

```json
{
  "apiName": "attendConference",
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attends Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Tipo di aggiornamento

L’aggiornamento di un tipo è molto simile, tranne per il fatto che apiName è l’unico parametro obbligatorio come parametro del percorso.

```
POST /rest/v1/activities/external/type/{apiName}.json
```

```json
{
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attend Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Approva tipo

I tipi possono essere gestiti con le opzioni Approva tipo di attività personalizzato, Elimina tipo di attività personalizzato bozza ed Elimina tipo di attività personalizzato, come avviene per le risorse Marketo standard.


## Attributi del tipo di attività personalizzato

Ogni tipo di attività personalizzata può avere da 0 a 20 attributi secondari. Gli attributi secondari possono avere qualsiasi tipo di campo valido per un campo Marketo. Vengono aggiunte, aggiornate e rimosse separatamente dal tipo principale, ma possono essere modificate mentre un tipo di attività è in uso e quindi approvate. Quando si modificano i campi su un tipo live, per tutte le attività di quel tipo create dopo l’approvazione viene impostato il nuovo attributo secondario. Le modifiche non saranno applicate retroattivamente alle attività esistenti che condividono tale tipo.

Presta attenzione alla rimozione degli attributi, in quanto questa ne influenzerà la disponibilità per l’utilizzo nei filtri corrispondenti.

Gli aggiornamenti apportati all’elenco degli attributi secondari utilizzano il nome API di ciascun attributo come chiave primaria. Il nome API di un attributo non può essere modificato, deve essere eliminato e aggiunto nuovamente con il nome API desiderato.

I tipi di dati validi per gli attributi sono: string, boolean, integer, float, link, e-mail, currency, date, datetime, phone, text.

Quando si modifica l&#39;attributo primario di un tipo di attività, tutti gli attributi primari esistenti devono essere abbassati di livello impostando prima `isPrimary` su false.

### Crea attributi

La creazione di un attributo richiede un parametro di percorso `apiName` obbligatorio. Sono richiesti anche i parametri `name` e `dataType`.I parametri ` The description and` `isPrimary` sono facoltativi.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/create.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendees",
      "name": "Number of Attendees",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Aggiorna attributi

Quando si eseguono aggiornamenti agli attributi, `apiName` dell&#39;attributo è la chiave primaria. Il parametro `apiName` deve esistere affinché l&#39;aggiornamento venga completato correttamente, ovvero non è possibile modificare il parametro `apiName` utilizzando l&#39;aggiornamento.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/update.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendee",
      "name": "Number of Attendee",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendee",
          "name": "Number of Attendee",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Elimina attributi

L&#39;eliminazione di un attributo richiede un parametro di percorso `apiName` obbligatorio corrispondente al nome API dell&#39;attività personalizzata.  È inoltre necessario un parametro di attributo che sia un array di oggetti attributo.  Ogni oggetto deve contenere un parametro `apiName` corrispondente al nome API del tipo di attività personalizzato.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/delete.json
```

```json
{ "attributes":[ { "apiName":"conferenceDate" }, { "apiName":"numberOfAttendees" } ] }
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Aggiungi attività personalizzate

Le attività personalizzate sono record in scrittura di attività storiche correlate ai record di singole persone in Marketo. Queste attività hanno uno schema gestito dagli amministratori di Marketo o in remoto tramite un’integrazione API. Le attività personalizzate vengono aggiunte ai record dei lead tramite l&#39;endpoint [Aggiungi attività personalizzate](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) e sono correlate a ciascun record dei lead tramite il relativo campo `leadId`. Le attività personalizzate possono essere visualizzate nell’interfaccia utente tramite il registro delle attività del lead oppure recuperate tramite l’endpoint &quot;Get Lead Activities&quot; specificando l’ID del tipo di attività personalizzata.

Le attività personalizzate sono appropriate per la registrazione di dati relativi a un singolo record di persona e che non devono essere aggiornati o sovrascritti. Un esempio potrebbe essere la registrazione di una persona che partecipa a un evento come attività &quot;Evento partecipato&quot;. Per i record relativi a una persona che potrebbe cambiare, come l’iscrizione degli studenti, è invece necessario utilizzare oggetti personalizzati, in quanto possono essere aggiornati, dove le attività personalizzate potrebbero non essere disponibili.

Il membro di input è un array di oggetti attività. È possibile inviare un massimo di 300 record di attività alla volta.

I membri `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` e gli attributi sono obbligatori. L&#39;array di attributi deve contenere l&#39;attributo non primario. Questo può essere specificato utilizzando nome (nome campo) o apiName (nome API) e un valore che corrisponde al valore che stai impostando.

```
POST /rest/v1/activities/external.json
```

```json
{
  "input": [
    {
      "leadId": 1001,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 1200,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 3000,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Contest Form",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 50,
      "marketoGUID": "50",
      "status": "added"
    },
    {
      "id": 51,
      "marketoGUID": "51",
      "status": "added"
    },
    {
      "status": "skipped",
      "errors": [
        {
          "code": "1004",
          "message": "Lead not found"
        }
      ]
    }
  ]
}
```

## Timeout

Gli endpoint delle attività hanno un timeout di 30 secondi, a meno che non sia indicato di seguito.

* Ottieni token di paging: 300s 
* Aggiungi attività personalizzata: anni 90
