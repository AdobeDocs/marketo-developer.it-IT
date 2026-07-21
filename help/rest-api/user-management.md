---
title: Gestione degli utenti
feature: REST API
description: Guida alle API User Management di Marketo per CRUD su utenti, autenticazione basata su intestazione, ruoli e aree di lavoro, gestione del codice di stato, formato datetime ed endpoint di query.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
TQID: https://experienceleague.adobe.com/V1NzpIl-peHBi9rqy8YwdJDh3O-dViIdF0cBsDSI-w8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: d65b4a73-87a3-4d56-b638-74e74d9939ce
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1440
ht-degree: 6%

---

# Gestione degli utenti

[Riferimento endpoint gestione utenti](https://developer.adobe.com/marketo-apis/api/user/)

Gli endpoint di gestione utenti di Marketo eseguono operazioni CRUD sui record utente. Per creare un utente, invia un invito. L’utente imposta quindi una password e accede a Marketo per la prima volta.

A differenza di altre API REST di Marketo, quando si utilizzano le API User Management:

- Invia il token di accesso in un’intestazione HTTP. Impossibile passare il token di accesso come parametro della stringa di query. Consulta la [Guida all&#39;autenticazione](authentication.md).
- Durante la creazione del ruolo utente per un servizio personalizzato [API REST](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api), selezionare un&#39;autorizzazione da ciascuno di questi gruppi:
  1. Autorizzazione &quot;Accedi agli utenti&quot; dal gruppo [Accesso amministratore](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
  1. &quot;Accedere all&#39;API User Management&quot; dal gruppo [Access API](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- Valuta il codice di stato della risposta HTTP perché i corpi di risposta non contengono l’attributo booleano &quot;success&quot;. Una chiamata di successo restituisce il codice di stato 200. Una chiamata non riuscita restituisce un codice di stato non 200 e l’array standard &quot;errors&quot; con un codice di errore e un messaggio descrittivo.
- Formattare le stringhe di data e ora come `yyyyMMdd'T'HH:mm:ss.SSS't'+|-hhmm`. Questo formato si applica a `createdAt`, `updatedAt` e `expiresAt`.
- Non aggiungere il prefisso &quot;/rest&quot; agli endpoint API di User Management.

## Query

Le query di Gestione utenti possono recuperare tutti gli utenti, i ruoli e le aree di lavoro. Possono anche recuperare un utente o i record dei ruoli e dell’area di lavoro associati per ID utente.

### Utente per ID

L&#39;endpoint [Get User by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) accetta un singolo parametro di percorso `userid` e restituisce un singolo record utente per un utente che ha accettato l&#39;invito.

```http
GET /userservice/management/v1/users/{userid}/user.json
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "Jamie",
  "lastName": "Lannister",
  "emailAddress": "jamie@lannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2020-12-31T08:00:00.000t+0000",
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

### Utente invitato per ID

L&#39;endpoint [Get Invited User by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) accetta un singolo parametro di percorso `userid` e restituisce un singolo record utente per un utente &quot;in sospeso&quot; (l&#39;invito non è stato ancora accettato).

```http
GET /userservice/management/v1/users/{userid}/invite.json
```

```json
{
    "id": 25112,
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@lannister.com",
    "userId": "jamie@lannister.com",
    "subscriptionId": 3381,
    "status": "pending",
    "expiresAt": "20200807T20:49:54.0t+0000",
    "createdAt": "20200731T20:49:54.0t+0000",
    "updatedAt": "20200731T20:49:54.0t+0000"
}
```

### Ruoli e aree di lavoro per ID

L&#39;endpoint [Get Roles and Workspaces by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) accetta un parametro di percorso `userid` e restituisce i record di ruolo e area di lavoro dell&#39;utente. Ogni oggetto nell’array di risposta contiene il ruolo e l’ID e il nome dell’area di lavoro.

```http
GET /userservice/management/v1/users/{userid}/roles.json
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

### Sfoglia utenti

L&#39;endpoint [Get Users](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) restituisce tutti i record utente. Supporta i seguenti parametri interi facoltativi:

- `pageSize` specifica il numero massimo di voci da restituire. Il valore predefinito è 20 e il massimo è 200.
- `pageOffset` specifica dove iniziare a recuperare le voci. Il valore predefinito è 0 e può essere utilizzato con `pageSize`.

```http
GET /userservice/management/v1/users/allusers.json
```

```json
[
  {
    "userid": "jamie@lannister.com",
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@houselannister.com",
    "id": 6785,
    "apiOnly": false
  },
  {
    "userid": "jeoffery@housebaratheon.com",
    "firstName": "Jeoffery",
    "lastName": "Baratheon",
    "emailAddress": "jeoffery@housebaratheon.com",
    "id": 7718,
    "apiOnly": false
  },
  {
    "userid": "rickon@housestark.com",
    "firstName": "Rickon",
    "lastName": "Stark",
    "emailAddress": "rickon@housestark.com",
    "id": 8612,
    "apiOnly": false
  }
]
```

>[!NOTE]
>
>Nell&#39;esempio di codice riportato sopra, l&#39;elemento `userid` visualizzato è per un cliente che è stato trasferito ad Adobe IMS. I clienti che devono ancora effettuare la migrazione visualizzeranno un indirizzo e-mail regolare nel campo `userid`.

### Sfoglia ruoli

L&#39;endpoint [Get Roles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) restituisce un elenco di tutti i record dei ruoli.

```http
GET /userservice/management/v1/users/roles.json
```

```json
[
    {
        "id": 1,
        "name": "Admin",
        "description": "All permissions",
        "type": "system",
        "hidden": false,
        "onlyAllZones": true,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 2,
        "name": "Standard User",
        "description": "All permissions except Admin",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 24,
        "name": "RTP Launcher",
        "description": "Role required for launcher in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 25,
        "name": "RTP Editor",
        "description": "Role required for editor in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 101,
        "name": "Analytics User",
        "description": "Has access to Analytics",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 102,
        "name": "Marketing User",
        "description": "All permissions except Admin",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 103,
        "name": "Web Designer",
        "description": "Has access to Design Studio except approval permission",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    }
]
```

### Sfoglia aree di lavoro

L&#39;endpoint [Get Workspaces](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) restituisce un elenco di tutti i record dell&#39;area di lavoro.

```http
GET /userservice/management/v1/users/workspaces.json
```

```json
[
  {
    "id": 1,
    "name": "Default",
    "description": "Initial workspace for Marketing Activities, Design Studio, and so on.",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20160910T23:08:05.0t+0000",
    "updatedAt": "20160910T23:08:05.0t+0000"
  },
  {
    "id": 1008,
    "name": "World",
    "description": "",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20181119T21:59:36.0t+0000",
    "updatedAt": "20181119T21:59:36.0t+0000"
  },
  {
    "id": 1009,
    "name": "Reproduction - US English - All Leads",
    "description": "A Workspace for recreating customer-reported problems.",
    "globalViz": 1,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190129T23:36:37.0t+0000",
    "updatedAt": "20190129T23:36:37.0t+0000"
  },
  {
    "id": 1010,
    "name": "US",
    "description": "United States - Qualified Leads",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190322T15:55:40.0t+0000",
    "updatedAt": "20190322T15:55:40.0t+0000"
  }
]
```

## Invita utente

In [sottoscrizioni integrate in Adobe IMS](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), questo endpoint supporta solo l&#39;invito di [utenti con sola API](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Per invitare [utenti standard](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilizza invece l&#39;[API di gestione utenti di Adobe](https://developer.adobe.com/umapi/).

L&#39;endpoint [Invita utente](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) invia un invito e-mail di benvenuto in Marketo a un nuovo utente. L’e-mail contiene il collegamento &quot;Accedi a Marketo&quot;. Il destinatario seleziona il collegamento, crea una password e ottiene l’accesso a Marketo.

Fino a quando il destinatario non accetta l’invito, il suo stato è &quot;In sospeso&quot; e il record utente non può essere modificato. Un invito in sospeso scade sette giorni dopo l’invio. Per ulteriori informazioni, consulta la [documentazione sulla gestione degli utenti di Marketo](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Trasmettere i parametri nel corpo della richiesta in formato `application/json`.

I parametri richiesti sono `emailAddress`, `firstName`, `lastName` e `userRoleWorkspaces`. Il parametro `userRoleWorkspaces` è un array di oggetti che contengono gli attributi `accessRoleId` e `workspaceId`.

Il parametro `userid` è l&#39;identificatore utente univoco utilizzato per l&#39;accesso e deve essere formattato come indirizzo e-mail. Se la richiesta omette `userid`, il valore predefinito sarà `emailAddress`.

Il parametro booleano `apiOnly` specifica se l&#39;utente è un [utente solo API](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Il parametro `expiresAt` specifica quando scade l&#39;accesso utente e utilizza il formato W3C ISO-8601 senza millisecondi. Se la richiesta omette `expiresAt`, l&#39;utente non scade mai. Il parametro `reason` descrive il motivo dell&#39;invito.

L’endpoint restituisce &quot;true&quot; quando l’invito ha esito positivo. In caso contrario, restituisce un messaggio di errore.

```http
POST /userservice/management/v1/users/invite.json
```

```text
Content-Type: application/json
```

```json
{
  "emailAddress": "daenerys@housetargaryen.com",
  "firstName": "Daenerys",
  "lastName": "Targaryen",
  "expiresAt": "2020-12-31T23:59:59-05:00",
  "reason": "Keeper of dragons",
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "workspaceId": 0
    }
  ]
}
```

```text
true
```

L’immagine seguente mostra l’e-mail di benvenuto in Marketo inviata al nuovo utente. L’oggetto è &quot;Informazioni di accesso a Marketo&quot;. Il mittente è l&#39;indirizzo di posta elettronica dell&#39;utente solo API associato al servizio personalizzato [REST API](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api). I parametri firstName, lastName e emailAddress specificano il destinatario.

![Invita e-mail utente](assets/invite-user-email.png)

L&#39;utente accetta l&#39;invito immettendo una password due volte e selezionando il pulsante &quot;CREA PASSWORD&quot;. L’utente riceve quindi l’accesso a Marketo.

## Aggiorna utente

È possibile aggiornare gli attributi utente o eliminare un utente dopo che ha accettato l&#39;invito. Passa gli attributi come parametri nel corpo della richiesta in formato application/json.

### Aggiorna attributi utente

In [sottoscrizioni integrate in Adobe IMS](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), questo endpoint supporta l&#39;aggiornamento degli attributi solo di [utenti API](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Per aggiornare gli attributi per [utenti standard](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilizza invece l&#39;[API Gestione utenti di Adobe](https://developer.adobe.com/umapi/).

L&#39;endpoint [Aggiorna attributi utente](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) accetta un singolo parametro di percorso `userid` e restituisce un singolo record utente. Il corpo della richiesta contiene uno o più attributi utente da aggiornare: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

```http
POST /userservice/management/v1/users/{userid}/update.json
```

```text
Content-Type: application/json
```

```json
{
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "expiresAt": "20211231T08:00:00.000t+0000"
}
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "emailAddress": "jamie@houselannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2021-12-31T08:00:00.000t+0000"
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

#### Elimina utente

In [sottoscrizioni integrate in Adobe IMS](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), questo endpoint supporta l&#39;eliminazione solo di [utenti con sola API](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Per eliminare [utenti standard](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilizza l&#39;[API di gestione utenti di Adobe](https://developer.adobe.com/umapi/).

L&#39;endpoint [Elimina utente](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) accetta un singolo parametro di percorso `userid` ed elimina l&#39;utente corrispondente dall&#39;istanza. Questa è una cancellazione distruttiva e non può essere annullata. In caso di esito positivo, viene restituito il codice di stato 200, altrimenti viene restituito un messaggio di errore.

```http
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Elimina utente invitato

L&#39;endpoint [Elimina utente invitato](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) accetta un singolo parametro di percorso `userid` ed elimina l&#39;utente corrispondente &quot;in sospeso&quot; dall&#39;istanza (l&#39;utente non ha ancora accettato il proprio invito). Questa è una cancellazione distruttiva e non può essere annullata. In caso di esito positivo, viene restituito il codice di stato 200, altrimenti viene restituito un messaggio di errore.

```http
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Aggiorna Ruoli

Puoi aggiungere o eliminare ruoli. Passa gli attributi come parametri nel corpo della richiesta in formato application/json.

## Aggiungi Ruoli

L&#39;endpoint [Aggiungi ruoli](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) accetta un singolo parametro di percorso `userid` e aggiunge uno o più ruoli utente all&#39;utente corrispondente. Il corpo della richiesta contiene un elenco di uno o più oggetti contenenti ciascuno un attributo `accessRoleId` e un attributo `workspaceId`. In caso di esito positivo, viene restituito l&#39;intero elenco di `accessRoleId/workspaceId` coppie per l&#39;utente specificato.

```http
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```text
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

## Elimina Ruoli

L&#39;endpoint [Elimina ruoli](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) accetta un singolo parametro di percorso `userid` ed elimina uno o più ruoli utente dall&#39;utente corrispondente. Il corpo della richiesta contiene un elenco di uno o più oggetti contenenti ciascuno un attributo `accessRoleId` e un attributo `workspaceId`. In caso di esito positivo, viene restituito l’elenco rimanente di coppie accessRoleId/workspaceId per l’utente specificato.

```http
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```text
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  }
]
```
