---
title: E-mail
feature: REST API
description: Utilizza l’API REST di Marketo Asset per eseguire query, creare, aggiornare, clonare, eliminare, approvare e verificare le dipendenze per le risorse e-mail.
exl-id: b41a3ae5-2b25-4103-84b4-320fc2c44bd6
source-git-commit: 0e0a3e5a08e81f349044cbc327d1aba963ab30e4
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 5%

---

# E-mail

[Riferimento endpoint e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails_New)

Le e-mail sono record di risorse che definiscono i metadati dei messaggi, la configurazione del contenuto, le impostazioni e lo stato di approvazione.

## Accesso

Gli endpoint descritti in questo articolo richiedono un token di accesso:

```text
?access_token=<access_token>
```

Le richieste richiedono anche l&#39;intestazione `x-app-type`:

```text
x-app-type: <app-type>
```

## Query

È possibile recuperare i metadati delle e-mail per risorsa `id` o con l&#39;endpoint del filtro.

### Per ID

#### Richiesta

```text
GET /rest/asset/v2/email/{id}
```

#### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7b3c#1900ab12cd3",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "description": "Main announcement email",
      "status": "draft"
    }
  ]
}
```

### Filtro

L’endpoint filtro supporta la ricerca all’interno di un’area di lavoro e la riduzione dei risultati con parametri di query aggiuntivi.

`workspaceId` è obbligatorio.

Parametri di query supportati:

| Parametro | Descrizione |
| - | - |
| `workspaceId` | Identificatore dell’area di lavoro richiesto utilizzato per definire l’ambito della richiesta di filtro. |
| `folderId` | Filtra i risultati in una singola cartella. |
| `folderIds` | Parametro ripetuto che filtra i risultati in più cartelle. |
| `status` | Parametro ripetuto che filtra per uno o più stati e-mail. |
| `pageIndex` | Indice di pagina a base zero per risultati impaginati. |
| `pageSize` | Numero massimo di risultati da restituire per pagina. |
| `createdBy` | Filtra i risultati in base all’utente che crea. |
| `createdAtStart` | Restituisce le risorse create in corrispondenza o dopo questa marca temporale. |
| `createdAtEnd` | Restituisce le risorse create in corrispondenza o prima di questa marca temporale. |
| `modifiedBy` | Filtra i risultati in base all&#39;ultimo utente che ha apportato modifiche. |
| `modifiedAtStart` | Restituisce le risorse modificate in corrispondenza o dopo questa marca temporale. |
| `modifiedAtEnd` | Restituisce le risorse modificate in corrispondenza o prima di questa marca temporale. |
| `name` | Filtra i risultati per nome e-mail. |
| `sortKey` | Seleziona il campo utilizzato per ordinare i risultati. |
| `sortOrder` | Imposta la direzione di ordinamento. |
| `isCreatedByMe` | Limita i risultati alle risorse create dall&#39;utente corrente. |
| `isModifiedByMe` | Limita i risultati all&#39;ultima modifica apportata dall&#39;utente corrente alle risorse. |
| `templateId` | Filtra i risultati in base all’identificatore del modello. |
| `scriptEngine` | Filtra i risultati in base alla configurazione del motore di script. |
| `isValueNonNullable` | Filtri basati sul fatto che il valore non sia nullable. |
| `includeArchived` | Include nei risultati le risorse archiviate. |

#### Richiesta

```text
GET /rest/asset/v2/email/filter?workspaceId=1001&name=Spring%20Launch&status=draft&status=approved&pageIndex=0&pageSize=20
```

#### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "5dd1#1900ab13011",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Utilizzare il parametro `name` quando è necessario trovare un&#39;e-mail per nome.

## Creare

Crea un’e-mail inviando un payload JSON. `name`, `appData` e `headers` sono obbligatori. `headers.subject` è obbligatorio e `appData` deve includere almeno uno tra `folderId`, `workspaceId` o `programId`.

### Richiesta

```text
POST /rest/asset/v2/email
Content-Type: application/json
```

```json
{
  "name": "Spring Launch Email",
  "description": "Main announcement email",
  "appData": {
    "workspaceId": "1001",
    "folderId": "2002",
    "editorType": "email"
  },
  "headers": {
    "subject": "Introducing the Spring Launch",
    "fromName": "Marketing Team",
    "fromEmail": "marketing@example.com",
    "replyEmail": "reply@example.com",
    "preheader": "See what changed this quarter",
    "ccEmails": [
      "owner@example.com"
    ]
  },
  "settings": {
    "isOperational": false,
    "isTextOnly": false,
    "isWebPageView": true,
    "enableUrlTracking": true
  },
  "templateId": "3003"
}
```

### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "238c#1900ab1313f",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Il corpo della richiesta può includere anche `data`, `editorContext`, `themeId`, `appType` e `status`.

### Crea campi

* `appData` identifica dove viene creato il messaggio e-mail e come viene modificato.
* `headers` contiene i valori di intestazione del messaggio, inclusi l&#39;oggetto, il mittente, l&#39;indirizzo di risposta, il preheader e i destinatari CC facoltativi.
* `settings` controlla il comportamento operativo e di rendering, ad esempio la modalità di solo testo, la visualizzazione della pagina Web e il tracciamento degli URL.
* `templateId` identifica il modello e-mail utilizzato al momento della creazione dell&#39;e-mail.

## Aggiornamento

Aggiorna un’e-mail per ID risorsa. Il corpo della richiesta utilizza lo schema `UpdateEmailRequest` e tutte le proprietà sono facoltative, pertanto puoi inviare solo i campi che desideri modificare.

### Richiesta

```text
POST /rest/asset/v2/email/{id}/update
Content-Type: application/json
```

```json
{
  "description": "Updated announcement email",
  "headers": {
    "subject": "Spring Launch Is Live",
    "preheader": "Read the latest release notes"
  },
  "settings": {
    "enableUrlTracking": true,
    "isWebPageView": true
  }
}
```

### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "9fd3#1900ab13210",
  "result": [
    {
      "id": "1017"
    }
  ]
}
```

## Gestisci stato

Le e-mail utilizzano una bozza e un ciclo di vita approvato. Utilizza l’endpoint di transizione dello stato per approvare un’e-mail, annullarne l’approvazione, scartare una bozza o crearne una nuova.

I valori `action` validi sono:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Richiesta

```text
POST /rest/asset/v2/email/state/transition
Content-Type: application/json
```

### Risposta

```json
{
  "contentId": "1017",
  "action": "approve"
}
```

## Duplica

Utilizza l’endpoint clone per creare una copia di un’e-mail esistente.

### Richiesta

```text
POST /rest/asset/v2/email/clone
Content-Type: application/json
```

### Risposta

```json
{
  "assetId": "1017",
  "newAsset": {
    "name": "Spring Launch Email Copy",
    "description": "Cloned from Spring Launch Email"
  }
}
```

## Elimina

Elimina un’e-mail per ID risorsa.

### Richiesta

```text
POST /rest/asset/v2/email/{id}/delete
Content-Type: application/json
```

Questo endpoint accetta la risorsa `id` nel percorso e non definisce un corpo di richiesta.

## Utilizzato da

Utilizza l&#39;endpoint `usedby` per recuperare le risorse che fanno riferimento a un determinato messaggio e-mail.

### Richiesta

```text
POST /rest/asset/v2/email/usedby
Content-Type: application/json
```

### Risposta

```json
{
  "assetId": "1017",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Note

Gli endpoint di query restituiscono i metadati della risorsa. Utilizza il riferimento all’endpoint per lo schema completo dei campi disponibili e per tutte le proprietà specifiche dell’ambiente.
