---
title: Frammenti
feature: REST API
description: Utilizza l’API REST di Marketo Asset per eseguire query, creare, aggiornare, clonare, eliminare, approvare e verificare le dipendenze dei frammenti.
exl-id: 9dd532d1-1dd7-4581-86dd-1943fab66cbb
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 9%

---

# Frammenti

I frammenti sono risorse di contenuto riutilizzabili a cui possono fare riferimento altre risorse, ad esempio le e-mail.

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

Puoi recuperare i metadati del frammento per ID risorsa o con l’endpoint del filtro.

### Per ID

#### Richiesta

```http
GET /rest/asset/v2/fragment/{id}
```

#### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "fa0f#1900ab15001",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "description": "Reusable hero block",
      "status": "approved"
    }
  ]
}
```

### Filtro

L’endpoint filtro supporta la ricerca all’interno di un’area di lavoro e la riduzione dei risultati con parametri di query aggiuntivi. `workspaceId` è obbligatorio.

todo: trasformare questa tabella in tabella
I filtri supportati includono `folderId`, `folderIds` ripetuto, `status` ripetuto, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `fragmentType`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` e `includeArchived`.

#### Richiesta

```http
GET /rest/asset/v2/fragment/filter?workspaceId=1001&fragmentType=email&pageIndex=0&pageSize=20
```

#### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "f9cc#1900ab1504a",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "status": "approved"
    }
  ]
}
```

Utilizza il parametro `name` quando devi trovare un frammento per nome.

## Creare

Crea un frammento inviando un payload JSON. `name`, `appData` e `settings` sono obbligatori. `settings` deve includere `fragmentType` e `supportedChannels`.

### Richiesta

```http
POST /rest/asset/v2/fragment
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment",
  "description": "Reusable hero block",
  "appData": {
    "workspaceId": "1001",
    "folderId": "395",
    "editorType": "fragment"
  },
  "settings": {
    "fragmentType": "email",
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "bd57#1900ab1509d",
  "result": [
    {
      "id": "13",
      "name": "Hero Banner Fragment",
      "status": "draft"
    }
  ]
}
```

Il corpo della richiesta può includere anche `data`, `editorContext`, `themeId`, `appType` e `status`.

### Crea campi

* `appData` identifica la posizione in cui è memorizzato il frammento e la modalità di modifica.
* `settings.fragmentType` identifica la categoria del frammento, ad esempio un frammento di posta elettronica.
* `settings.fragmentSubType` può essere utilizzato per definire ulteriormente il formato del frammento.
* `settings.supportedChannels` elenca i canali in cui è possibile utilizzare il frammento.

## Aggiornamento

Aggiorna un frammento per ID risorsa.

### Richiesta

```http
POST /rest/asset/v2/fragment/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment v2",
  "description": "Updated reusable hero block",
  "settings": {
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "73d9#1900ab150f0",
  "result": [
    {
      "id": "13"
    }
  ]
}
```

## Gestisci stato

I frammenti utilizzano una bozza e un ciclo di vita approvato. Utilizza l’endpoint di transizione dello stato per approvare un frammento, annullarne l’approvazione, scartare una bozza o crearne una nuova.

I valori `action` validi sono:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Richiesta

```http
POST /rest/asset/v2/fragment/state/transition
Content-Type: application/json
```

### Risposta

```json
{
  "contentId": "13",
  "action": "approve"
}
```

## Duplica

Utilizza l’endpoint clone per creare una copia di un frammento esistente.

### Richiesta

```http
POST /rest/asset/v2/fragment/clone
Content-Type: application/json
```

### Risposta

```json
{
  "assetId": "13",
  "newAsset": {
    "name": "Hero Banner Fragment Copy",
    "description": "Cloned fragment"
  }
}
```

## Elimina

Elimina un frammento per ID risorsa.

### Richiesta

```http
POST /rest/asset/v2/fragment/{id}/delete
Content-Type: application/json
```

Questo endpoint accetta l’ID del frammento nel percorso e non definisce il corpo di una richiesta.

## Utilizzato da

Utilizza l&#39;endpoint `usedby` per recuperare le risorse che fanno riferimento a un frammento specifico.

### Richiesta

```http
POST /rest/asset/v2/fragment/usedby
Content-Type: application/json
```

### Risposta

```json
{
  "assetId": "13",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```
