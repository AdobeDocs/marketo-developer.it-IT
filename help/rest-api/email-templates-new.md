---
title: Modelli e-mail
feature: REST API
description: Utilizza l’API REST di Marketo Asset per eseguire query, creare, aggiornare, clonare, eliminare, approvare e verificare le dipendenze per i modelli e-mail.
exl-id: 50bb0047-d6ea-4c94-a900-18c37b17a147
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 9%

---

# Modelli e-mail

[Riferimento endpoint modello e-mail](https://developer.adobe.com/marketo-apis/api/asset/#tag/Email-Templates)

I modelli e-mail definiscono la struttura e il layout riutilizzabile utilizzati per creare le e-mail.

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

Puoi recuperare i metadati del modello e-mail per ID risorsa o con l’endpoint del filtro.

### Per ID

#### Richiesta

```http
GET /rest/asset/v2/emailtemplate/{id}
```

#### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "14f9e#1900ab14001",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "description": "Core responsive template",
      "status": "draft"
    }
  ]
}
```

### Filtro

L’endpoint filtro supporta la ricerca all’interno di un’area di lavoro e la riduzione dei risultati con parametri di query aggiuntivi. `workspaceId` è obbligatorio.

I filtri supportati includono `folderId`, `folderIds` ripetuto, `status` ripetuto, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` e `includeArchived`.

#### Richiesta

```http
GET /rest/asset/v2/emailtemplate/filter?workspaceId=1001&name=Newsletter&pageIndex=0&pageSize=20
```

#### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "33c4#1900ab1402f",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Utilizzare il parametro `name` quando è necessario trovare un modello per nome.

## Creare

Crea un modello e-mail inviando un payload JSON. `name` e `appData` sono obbligatori. `appData` deve includere almeno `folderId` o `workspaceId`.

### Richiesta

```http
POST /rest/asset/v2/emailtemplate
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template",
  "description": "Core responsive template",
  "appData": {
    "workspaceId": "1001",
    "folderId": "15",
    "editorType": "emailTemplate"
  },
  "themeId": "42"
}
```

### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "a99f#1900ab1407e",
  "result": [
    {
      "id": "1022",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Il corpo della richiesta può includere anche `data`, `editorContext`, `appType` e `status`.

### Crea campi

`appData` identifica la posizione in cui è memorizzato il modello e la modalità di modifica.

È possibile utilizzare `themeId` per associare un tema al modello, se applicabile.

## Aggiornamento

Aggiorna un modello per ID risorsa.

### Richiesta

```http
POST /rest/asset/v2/emailtemplate/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template v2",
  "description": "Updated responsive template",
  "appData": {
    "folderId": "15"
  }
}
```

### Risposta

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "cf10#1900ab140b3",
  "result": [
    {
      "id": "1022"
    }
  ]
}
```

## Gestisci stato

I modelli e-mail utilizzano una bozza e un ciclo di vita approvato. Utilizza l’endpoint di transizione dello stato per approvare un modello, annullarne l’approvazione, scartare una bozza o crearne una nuova.

I valori `action` validi sono:

- `approve`
- `unapprove`
- `discard`
- `create_draft`

### Richiesta

```http
POST /rest/asset/v2/emailtemplate/state/transition
Content-Type: application/json
```

### Risposta

```json
{
  "contentId": "1022",
  "action": "approve"
}
```

## Duplica

Utilizza l’endpoint clone per creare una copia di un modello esistente.

### Richiesta

```http
POST /rest/asset/v2/emailtemplate/clone
Content-Type: application/json
```

### Risposta

```json
{
  "assetId": "1022",
  "newAsset": {
    "name": "Base Newsletter Template Copy",
    "description": "Cloned template"
  }
}
```

## Elimina

Elimina un modello per ID risorsa.

### Richiesta

```http
POST /rest/asset/v2/emailtemplate/{id}/delete
Content-Type: application/json
```

Questo endpoint accetta l’ID del modello nel percorso e non definisce il corpo di una richiesta.

## Utilizzato da

Utilizza l&#39;endpoint `usedby` per recuperare le risorse che fanno riferimento a un determinato modello.

### Richiesta

```http
POST /rest/asset/v2/emailtemplate/usedby
Content-Type: application/json
```

### Risposta

```json
{
  "assetId": "1022",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Note

Gli endpoint di query restituiscono i metadati della risorsa. Utilizza il riferimento all’endpoint per lo schema di risposta completo ed eventuali proprietà specifiche dell’ambiente.
