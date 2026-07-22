---
title: Utilizzo
feature: REST API
description: Monitora l’utilizzo e gli errori dell’API REST di Marketo con gli endpoint di stato giornalieri e degli ultimi 7 giorni, inclusi i conteggi per utente e i totali dei codici di errore.
exl-id: 935a00a4-1e1e-4b48-ae9c-72c5e578312a
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 8%

---

# Utilizzo

[Riferimento endpoint di utilizzo](https://developer.adobe.com/marketo-apis/api/mapi#tag/Usage)

Nelle API di utilizzo viene riepilogato il consumo e l’attività di errore REST API per la sottoscrizione. Utilizza questi endpoint per monitorare le integrazioni, monitorare il volume giornaliero delle chiamate e identificare le tendenze di errore.

I dati di utilizzo includono un conteggio totale di chiamate API e un raggruppamento per utente. I dati di errore includono un conteggio totale degli errori e una suddivisione per codice di errore.

Le API di utilizzo utilizzano lo stesso metodo di autenticazione delle altre API REST di Marketo. Passa il token di accesso nell&#39;intestazione `Authorization: Bearer {accessToken}`.

## Endpoint

| Metodo | Percorso | Descrizione |
| --- | --- | --- |
| GET | `/rest/v1/stats/usage.json` | Recupera l’utilizzo dell’API per il giorno corrente. |
| GET | `/rest/v1/stats/usage/last7days.json` | Recupera l’utilizzo API per gli ultimi 7 giorni. |
| GET | `/rest/v1/stats/errors.json` | Recupera gli errori API per il giorno corrente. |
| GET | `/rest/v1/stats/errors/last7days.json` | Recupera gli errori API per gli ultimi 7 giorni. |

## Utilizzo giornaliero

Recupera l’utilizzo dell’API per il giorno corrente.

```http
GET /rest/v1/stats/usage.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 1120,
         "users": [
            {
               "userId": "some.body@yahoo.com",
               "count": 200
            },
            {
               "userId": "some.body@marketo.com",
               "count": 200
            },
            {
               "userId": "some.body@gmail.com",
               "count": 720
            }
         ]
      }
   ]
}
```

Ogni oggetto nell&#39;array `result` contiene un giorno di totali di utilizzo e un raggruppamento per utente.

## Utilizzo ultimi 7 giorni

Recupera l’utilizzo API per gli ultimi 7 giorni. Ogni elemento nell&#39;array `result` rappresenta un giorno.

```http
GET /rest/v1/stats/usage/last7days.json
```

## Errori giornalieri

Recupera gli errori API per il giorno corrente.

```http
GET /rest/v1/stats/errors.json
```

```json
{
   "requestId": "5f7f#17d7d8f2b6f",
   "success": true,
   "result": [
      {
         "date": "2015-10-17",
         "total": 73,
         "errors": [
            {
               "errorCode": "604",
               "count": 1
            },
            {
               "errorCode": "609",
               "count": 56
            },
            {
               "errorCode": "610",
               "count": 16
            }
         ]
      }
   ]
}
```

Ogni oggetto nell&#39;array `result` contiene un giorno di totali di errore e una suddivisione per codice di errore.

## Errori degli ultimi 7 giorni

Recupera gli errori API per gli ultimi 7 giorni. Ogni elemento nell&#39;array `result` rappresenta un giorno.

```http
GET /rest/v1/stats/errors/last7days.json
```

## Membri risposta

### Oggetto risultato utilizzo

| Nome | Tipo di dati | Descrizione |
| --- | --- | --- |
| `date` | Stringa | La data per il riepilogo dell&#39;utilizzo nel formato `YYYY-MM-DD`. |
| `total` | Intero | Numero totale di chiamate API per quel giorno. |
| `users` | Array | Elenco dei conteggi di utilizzo per utente per quel giorno. |

### Oggetto utente di utilizzo

| Nome | Tipo di dati | Descrizione |
| --- | --- | --- |
| `userId` | Stringa | Identificatore utente API. |
| `count` | Intero | Numero di chiamate API effettuate dall’utente per la giornata. |

### Oggetto risultato errore

| Nome | Tipo di dati | Descrizione |
| --- | --- | --- |
| `date` | Stringa | Data del riepilogo degli errori nel formato `YYYY-MM-DD`. |
| `total` | Intero | Numero totale di errori API per quel giorno. |
| `errors` | Array | Elenco dei conteggi per codice di errore per quel giorno. |

### Oggetto Error

| Nome | Tipo di dati | Descrizione |
| --- | --- | --- |
| `errorCode` | Stringa | Codice di errore Marketo. |
| `count` | Intero | Numero di volte in cui si è verificato l’errore per il giorno. |

## Note

La risposta di utilizzo segnala ogni utente API separatamente. L’assegnazione di integrazioni a utenti API separati semplifica l’identificazione del servizio che consuma la quota e genera errori.
