---
title: Attività
feature: REST API
description: Utilizza l’API REST delle attività di Marketo Engage per elencare i tipi di attività, recuperare le attività dei lead con i token di paging e gestire le modifiche ai valori personalizzati e dei dati.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
TQID: https://experienceleague.adobe.com/62keaj4uNoxIPCzr9AQzKrIsfuHBvC25knYisZRUvF4
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
source-wordcount: 1758
ht-degree: 0%

---

# Attività

Marketo supporta molti tipi di attività correlati ai record dei lead. Quasi ogni modifica, azione o passaggio di flusso viene registrato nel registro attività di un lead. Puoi recuperare queste attività tramite l’API o utilizzarle nei filtri e nei trigger degli elenchi avanzati e di Campaign.

Ogni attività ha un `id` univoco e si connette a un record lead tramite `leadId`, che corrisponde al campo ID del record. Ogni attività ha anche un `activityDate`.

I tipi di attività disponibili variano a seconda dell’abbonamento e ogni tipo ha una propria definizione. Il significato di `primaryAttributeValueId` e `primaryAttributeValue` dipende dal tipo di attività.

Utilizza l’API dei metadati delle attività personalizzate per creare tipi di attività personalizzati. Utilizza l’API Aggiungi attività personalizzate per aggiungere record di attività personalizzati.

La maggior parte delle attività verrà eliminata dopo un certo periodo di tempo.

## Descrivere

Utilizzare l&#39;endpoint [Ottieni tipi di attività](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) per recuperare i tipi di attività disponibili e le relative definizioni per un&#39;istanza.

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

Le risposte effettive includono più definizioni. Questo esempio mostra il tipo di attività &quot;Compila modulo&quot;. Il suo attributo principale, &quot;ID modulo web&quot;, fa riferimento all’ID Marketo del modulo inviato e collega l’attività a tale risorsa.

La risposta definisce anche ogni possibile attributo per il tipo di attività e il relativo tipo di dati. Se un campo è vuoto, tale attributo viene omesso dal record della singola attività.

## Query

Utilizza l&#39;endpoint [Ottieni attività lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) per recuperare le attività. Innanzitutto, recupera un token di paging per il datetime da cui deve iniziare il recupero dell&#39;attività. Passa il token nel parametro di query `nextPageToken`.

Passa fino a dieci ID di tipo attività come elenco separato da virgole nel parametro di query `activityTypeIds`.

Facoltativamente, restringi la query con uno dei seguenti parametri:

- `listId` limita i risultati ai record in un elenco statico specifico.
- `leadIds` limita i risultati alle attività per un massimo di 30 lead, forniti come elenco separato da virgole.

>[!CAUTION]
>
>A partire dal 2026-12-30, le chiamate agli endpoint `Get Lead Activities` e `Get Lead Changes` che includono il parametro `listId` non riusciranno (codice di errore 1003) se gli elenchi di destinazione contengono 10.000 o più lead. Per evitare interruzioni del servizio, assicurati che le chiamate abbiano un ambito corretto per evitare questo limite. Consulta la [Guida alla migrazione](migration.md).

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

Per la prima chiamata, utilizza l&#39;API Get Paging Token per ottenere `nextPageToken`. Per ogni chiamata successiva, passa `nextPageToken` restituito dalla risposta precedente. Questo endpoint restituisce sempre `nextPageToken`.

Se `moreResult` è true, sono disponibili altri risultati. Continuare a chiamare l&#39;endpoint con `nextPageToken` restituito fino a quando `moreResult` non è false.

L&#39;API può restituire meno di 300 elementi attività mentre si imposta `moreResult` su true. In questo caso, includi `nextPageToken` restituito in un&#39;altra chiamata per recuperare attività più recenti.

All&#39;interno di ogni elemento dell&#39;array dei risultati, l&#39;attributo stringa `marketoGUID` sostituisce l&#39;attributo intero `id` come identificatore univoco.

### Modifiche al valore dei dati

Utilizza l&#39;endpoint [Get Lead Changes](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) per recuperare i record Data Value Change per i campi lead. La sua interfaccia differisce dall’API Get Lead Activities in due modi:

- L&#39;endpoint non contiene il parametro `activityTypeIds` perché restituisce solo le attività Modifica valore dati e Nuovo lead.
- Il parametro di query `fields` obbligatorio accetta un elenco separato da virgole di campi di cui desideri recuperare le modifiche.

>[!CAUTION]
>
>A partire dal 2026-12-30, le chiamate agli endpoint `Get Lead Activities` e `Get Lead Changes` che includono il parametro `listId` non riusciranno (codice di errore 1003) se gli elenchi di destinazione contengono 10.000 o più lead. Per evitare interruzioni del servizio, assicurati che le chiamate abbiano un ambito corretto per evitare questo limite. Consulta la [Guida alla migrazione](migration.md).

```http
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

Ogni attività nella risposta ha una matrice di campi che elenca le relative modifiche. Ogni modifica specifica i valori `id` e `name` del campo, insieme ai valori nuovi e vecchi.

All&#39;interno di ogni elemento dell&#39;array dei risultati, l&#39;attributo stringa `marketoGUID` sostituisce l&#39;attributo intero `id` come identificatore univoco.

### Lead eliminati

Utilizza l&#39;endpoint [Get Deleted Leads](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET) per recuperare da Marketo le attività lead eliminate.

```http
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

All&#39;interno di ogni elemento dell&#39;array dei risultati, l&#39;attributo stringa `marketoGUID` sostituisce l&#39;attributo intero `id` come identificatore univoco.

### Risultati pagina per pagina

Per impostazione predefinita, gli endpoint in questa sezione restituiscono 300 elementi attività alla volta. Se `moreResult` è true, sono disponibili altri risultati. Passa `nextPageToken` restituito in ogni chiamata successiva fino a quando `moreResult` non è false.

Un endpoint può restituire meno di 300 elementi attività mentre si imposta `moreResult` su true. In questo caso, includi `nextPageToken` restituito in un&#39;altra chiamata per recuperare attività più recenti. Codifica URL `nextPageToken` nella richiesta.

## Tipi di attività personalizzati

Le attività personalizzate funzionano come le attività standard, ma terze parti gestiscono i loro schemi. I record di attività personalizzati sono collegati ai record dei lead tramite `leadId` e i relativi attributi primario e secondario sono definiti dall&#39;utente.

Quando un tipo di attività personalizzato viene approvato, Marketo crea un trigger e un filtro di elenco avanzato corrispondente. Puoi quindi elaborare i lead in base ai dati di attività personalizzati correnti o presenti nella cronologia.

- Attività personalizzate massime: 10
- Numero massimo di attributi per attività personalizzata: 20

Recupera i dati delle attività personalizzate tramite l&#39;API [Get Lead Activities](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET), nello stesso modo in cui recuperi le attività standard.

## Tipi di query

Utilizza [Ottieni tipi di attività personalizzati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getCustomActivityTypeUsingGET) per recuperare i dettagli sui tipi per i quali è stato eseguito il provisioning in un&#39;istanza di Marketo. Utilizza [Descrivi tipo di attività personalizzato](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/describeCustomActivityTypeUsingGET) per recuperare i metadati dell&#39;attributo per un tipo specifico.

Anche l&#39;endpoint standard [Ottieni tipi di attività](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) restituisce metadati di attività personalizzati, ma non identifica se un tipo è personalizzato.

### Ottieni tipi

```http
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

Per descrivere un tipo, passare `apiName` come parametro di percorso. Per impostazione predefinita, l’endpoint restituisce la versione approvata dell’attività. Per recuperare la bozza della versione, passare il parametro facoltativo `draft=true`.

```http
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

Ogni tipo di attività personalizzata richiede un nome visualizzato, un nome API, un nome trigger, un nome filtro e un attributo primario. Utilizza le seguenti linee guida per mantenere i tipi coerenti con le convenzioni di Marketo ed evitare conflitti di denominazione:

- **Nome visualizzato:** descrivere brevemente ciò che rappresenta un record di attività, ad esempio &quot;Invia e-mail&quot; o &quot;Modifica valore dati&quot;. Utilizza il modulo infinito, ad esempio &quot;Partecipa all’evento&quot;. I nomi visualizzati accettano caratteri alfanumerici, spazi e trattini bassi e devono contenere almeno una lettera.

- **Nome API:** Utilizza caratteri alfanumerici, con una lunghezza massima di 255. Se sei un partner LaunchPoint, anteponi uno spazio dei nomi rappresentativo ai nomi API dei tipi di attività per evitare conflitti con i tipi forniti dal cliente. Utilizza lettere minuscole o camelCase per distinguere i nomi API da altre stringhe.

- **Descrizione:** Per le attività con comportamento non ovvio, spiega cosa rappresenta il tipo di attività in relazione al lead.

- **Nome trigger:** Fornire un nome univoco leggibile nel contesto del presente in terza persona, ad esempio &quot;Partecipa a un evento&quot;. I partner LaunchPoint devono includere il nome della loro azienda, ad esempio &quot;Partecipa al webinar - Azienda Acme&quot;.

- **Nome filtro:** Specificare un nome univoco leggibile per la terza persona nel tempo passato, ad esempio &quot;Partecipazione a un evento&quot;. I partner LaunchPoint devono includere il nome della loro azienda, ad esempio &quot;Webinar Partecipato - Azienda Acme&quot;.

- **Attributo primario:** Selezionare il campo più significativo per il tipo di attività. Per un’attività &quot;Evento partecipato&quot;, questo campo è il nome dell’evento. L’attributo primario viene visualizzato per impostazione predefinita come parametro in ogni trigger o filtro per il tipo di attività. Il relativo valore viene visualizzato anche nel registro attività di una persona senza richiedere un drill-down dell’attività.

Un nuovo tipo di attività personalizzata viene creato come bozza. Approva il tipo prima di aggiungere record di attività di quel tipo. Gli aggiornamenti si applicano alla versione bozza e devono essere approvati prima di essere visualizzati nella versione live. Dopo l’approvazione e l’utilizzo di un tipo di attività personalizzato, i campi precedenti non possono essere modificati.

Durante la creazione di un tipo, il parametro description è facoltativo. I parametri richiesti sono `apiName`, `name`, `triggerName`, `filterName` e `primaryAttribute`.

```http
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

Per aggiornare un tipo, passa apiName richiesto come parametro di percorso. Nel corpo della richiesta è possibile specificare altri campi.

```http
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

Gestisci i tipi con Approva tipo di attività personalizzato, Elimina tipo di attività personalizzato bozza ed Elimina tipo di attività personalizzato, come faresti con le risorse Marketo standard.

## Attributi del tipo di attività personalizzato

Ogni tipo di attività personalizzata può avere 0-20 attributi secondari. Un attributo secondario può utilizzare qualsiasi tipo di campo Marketo valido. Aggiungere, aggiornare e rimuovere attributi secondari separatamente dal tipo padre.

Puoi modificare gli attributi mentre un tipo di attività è in uso, quindi approvare le modifiche. Le attività create dopo l’approvazione utilizzano la nuova serie di attributi secondari. Le modifiche non si applicano retroattivamente alle attività esistenti di questo tipo.

La rimozione degli attributi ne rimuove anche la disponibilità nei filtri corrispondenti.

Gli aggiornamenti all’elenco degli attributi secondari utilizzano il nome API di ciascun attributo come chiave primaria. Per modificare un nome API, elimina l’attributo e aggiungilo nuovamente con il nome API desiderato.

I tipi di dati validi per gli attributi sono: string, boolean, integer, float, link, e-mail, currency, date, datetime, phone, text.

Prima di modificare l&#39;attributo primario di un tipo di attività, abbassare di livello l&#39;attributo primario esistente impostando `isPrimary` su false.

### Crea attributi

Per creare un attributo, passare il parametro di percorso `apiName` richiesto. Sono necessari anche i parametri `name` e `dataType`. La descrizione e i parametri `isPrimary` sono facoltativi.

```http
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

Durante l&#39;aggiornamento degli attributi, l&#39;attributo `apiName` è la chiave primaria e deve esistere già. Impossibile modificare `apiName` con un aggiornamento.

```http
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

Per eliminare un attributo, passare il parametro di percorso `apiName` richiesto per l&#39;attività personalizzata. Inoltre, passa il parametro dell’attributo richiesto come array di oggetti attributo. Ogni oggetto deve contenere un parametro `apiName` per il tipo di attività personalizzato.

```http
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

Le attività personalizzate sono record di attività storiche di scrittura per record di singole persone. Gli amministratori di Marketo possono gestire il proprio schema in Marketo, oppure un’integrazione API può gestirlo in remoto.

Utilizza l&#39;endpoint [Aggiungi attività personalizzate](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/addCustomActivityUsingPOST) per aggiungere attività personalizzate ai record dei lead. Il campo `leadId` associa ogni attività a un lead. Visualizzare le attività personalizzate nel registro attività del lead o recuperarle tramite Ottieni attività lead specificando l’ID del tipo di attività personalizzato.

Utilizza le attività personalizzate per i dati relativi a una persona che non deve essere aggiornata o sovrascritta. Ad esempio, registra la partecipazione all’evento come attività &quot;Evento partecipato&quot;.

Utilizza oggetti personalizzati per i record relativi alla persona che possono essere modificati, ad esempio l’iscrizione degli studenti. È possibile aggiornare gli oggetti personalizzati, ma non le attività personalizzate.

Il membro di input è un array di oggetti attività. Puoi inviare un massimo di 300 record di attività alla volta.

I membri `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` e gli attributi sono obbligatori. L&#39;array di attributi deve contenere l&#39;attributo non primario. Specificalo con un nome (nome campo) o un apiName (nome API) e un valore per il valore da impostare.

```http
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

Gli endpoint attività hanno un timeout di 30 secondi, ad eccezione dei seguenti endpoint:

- Ottieni token di paging: 300s
- Aggiungi attività personalizzata: anni 90
