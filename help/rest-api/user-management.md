---
title: Gestione utente
feature: REST API
description: Eseguire operazioni CRUD sui record utente.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
source-git-commit: 13a567be067a8a1272e981fad4e03b0a8519f132
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 0%

---

# Gestione utente

[Riferimento endpoint gestione utenti](https://developer.adobe.com/marketo-apis/api/user/)

Marketo fornisce una serie di endpoint per la gestione degli utenti che consentono di eseguire operazioni CRUD sui record degli utenti in Marketo. Gli utenti vengono creati inviando un invito a un utente, che imposta una password e ottiene l’accesso a Marketo per la prima volta.

A differenza di altre API REST di Marketo, quando si utilizzano le API User Management:

- Per inviare il token di accesso per l’autenticazione, è necessario utilizzare il metodo HTTP header. Impossibile passare il token di accesso come parametro della stringa di query. Ulteriori informazioni sull&#39;autenticazione sono disponibili [qui](authentication.md).
- È necessario selezionare un&#39;autorizzazione per il ruolo da due diversi gruppi durante la creazione del ruolo utente per [Servizio personalizzato](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) per API REST:
   1. Autorizzazione &quot;Accedi agli utenti&quot; dal gruppo [Accesso amministratore](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
   1. &quot;Accedere all&#39;API User Management&quot; dal gruppo [Access API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions)
- I corpi di risposta non contengono l’attributo booleano &quot;success&quot; che indica l’esito positivo o negativo di una chiamata. È invece necessario valutare il codice di stato della risposta HTTP. Se una chiamata ha esito positivo, viene restituito un codice di stato 200. Se una chiamata non riesce, viene restituito un codice di stato non di livello 200 e il corpo della risposta contiene l’array standard &quot;errors&quot; con codice di errore e messaggio di errore descrittivo.
- Il formato delle stringhe datetime è &quot;yyyyMMdd&#39;T&#39;HH:mm:ss.SSS&#39;t&#39;+|-hhmm&quot;. Questo vale per i seguenti attributi: createdAt, updatedAt, expiresAt.
- Come per gli altri endpoint, gli endpoint API per la gestione degli utenti non hanno il prefisso &quot;/rest&quot;.

## Query

Il supporto delle query per la gestione degli utenti include la possibilità di recuperare tutti gli utenti, i ruoli e le aree di lavoro. Inoltre, è possibile recuperare un singolo record utente per ID utente o un record ruolo/spazio di parola per ID utente.

### Utente per ID

L&#39;endpoint [Get User by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) accetta un singolo parametro di percorso `userid` e restituisce un singolo record utente per un utente che ha accettato l&#39;invito.

```
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

```
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

L&#39;endpoint [Get Roles and Workspaces by Id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) accetta un singolo parametro di percorso `userid` e restituisce un elenco di record di ruoli utente e area di lavoro. La risposta contiene un array con un oggetto che contiene l’ID del ruolo e dell’area di lavoro e il nome dell’utente specificato.

```
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

L&#39;endpoint [Get Users](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) restituisce un elenco di tutti i record utente. Il parametro facoltativo `pageSize` è un numero intero che specifica il numero massimo di voci da restituire. Il valore predefinito è 20. Il massimo è 200. Il parametro facoltativo `pageOffset` è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato con `pageSize`. Il valore predefinito è 0.

```
GET /userservice/management/v1/users/allusers.json
```

```json
[
  {
    "userid": "02226aae-9f54-45d1-bc26-8305c8f55ec7@adobe.com",
    "firstName": "Aparna",
    "lastName": "Ghosh",
    "emailAddress": "aparna.ghosh@ericsson.com",
    "id": 5222,
    "apiOnly": false
    },
    {
    "userid": "038e1cac-3f3e-4c05-b0b3-6265fd2abcd3@adobe.com",
    "firstName": "Timm",
    "lastName": "Rehse",
    "emailAddress": "timm.rehse@ericsson.com",
    "id": 7075,
    "apiOnly": false
    },
    {
    "userid": "0a855522-06c9-4a9e-93de-91a0d2cc2987@adobe.com",
    "firstName": "Dhinagaran",
    "lastName": "Swaminathan",
    "emailAddress": "dhinagaran.swaminathan@ericsson.com",
    "id": 6439,
    "apiOnly": false
    }
]
```

>[!NOTE]
>
>Nell&#39;esempio di codice riportato sopra, l&#39;elemento `userid` visualizzato è per un cliente che è stato trasferito ad Adobe IMS. I clienti che devono ancora effettuare la migrazione visualizzeranno un indirizzo e-mail regolare nel campo `userid`.

### Sfoglia ruoli

L&#39;endpoint [Get Roles](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) restituisce un elenco di tutti i record dei ruoli.

```
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

```
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

In [sottoscrizioni integrate in Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), questo endpoint supporta solo l&#39;invito di [utenti con sola API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Per invitare [utenti standard](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilizza l&#39;[API per la gestione degli utenti Adobe](https://developer.adobe.com/umapi/).

L&#39;endpoint [Invita utente](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) a inviare un invito e-mail di benvenuto a Marketo a un nuovo utente. Il corpo dell’e-mail contiene il collegamento &quot;Accedi a Marketo&quot;, che consente all’utente di accedere a Marketo per la prima volta. Per accettare l’invito, il destinatario dell’e-mail fa clic sul collegamento &quot;Accedi a Marketo&quot;, crea la propria password e ottiene l’accesso a Marketo. Fino al completamento del processo di accettazione, l’invito è &quot;in sospeso&quot; e il record utente non può essere modificato. Un invito in sospeso scade sette giorni dopo l’invio. Ulteriori informazioni sulla gestione degli utenti sono disponibili [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

I parametri vengono passati nel corpo della richiesta in formato `application/json`.

Sono necessari i seguenti parametri:  `emailAddress`, `firstName`, `lastName, userRoleWorkspaces`. Il parametro `userRoleWorkspaces` è un array di oggetti che contengono gli attributi `accessRoleId` e `workspaceId`.

Il parametro `userid` è un valore stringa univoco dell&#39;identificatore utente utilizzato per l&#39;accesso utente e deve essere formattato come indirizzo e-mail. Se non specificato nella richiesta, il valore di `userid` viene impostato automaticamente sul valore specificato nel parametro `emailAddress`.

Il parametro booleano `apiOnly` specifica se l&#39;utente è un [utente solo API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Il parametro `expiresAt` specifica quando scade l&#39;accesso utente e viene formattato utilizzando il formato W3C ISO-8601 (senza millisecondi). Se non viene fornito nella richiesta, l’utente non scade mai. Il parametro `reason` è una stringa che descrive il motivo dell&#39;invito utente.

In caso di esito positivo, l’endpoint restituisce il valore &quot;true&quot;; in caso contrario viene restituito un messaggio di errore.

```
POST /userservice/management/v1/users/invite.json
```

```
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

```
true
```

Di seguito è riportato un esempio dell&#39;invito e-mail di benvenuto in Marketo inviato al nuovo utente. L&#39;oggetto dell&#39;e-mail è &quot;Informazioni di accesso Marketo&quot;, il mittente è l&#39;indirizzo e-mail dell&#39;utente solo API associato al [servizio personalizzato API REST](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) e il destinatario è come specificato tramite i parametri firstName, lastName e emailAddress.

![Invita e-mail utente](assets/invite-user-email.png)

L’utente accetta l’invito e-mail immettendo la password due volte e facendo clic sul pulsante &quot;CREATE PASSWORD&quot; (CREA PASSWORD). Le viene quindi concesso l&#39;accesso a Marketo per la prima volta.

## Aggiorna utente

Il supporto per l’aggiornamento di utenti include la possibilità di aggiornare gli attributi utente o eliminare un utente. È possibile aggiornare solo gli utenti che hanno accettato l’invito. Gli attributi vengono trasmessi come parametri al corpo della richiesta in formato application/json.

### Aggiorna attributi utente

In [sottoscrizioni integrate in Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), questo endpoint supporta l&#39;aggiornamento degli attributi solo di [utenti API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Per aggiornare gli attributi per [utenti standard](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilizza l&#39;[API di gestione utenti Adobe](https://developer.adobe.com/umapi/).

L&#39;endpoint [Aggiorna attributi utente](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) accetta un singolo parametro di percorso `userid` e restituisce un singolo record utente. Il corpo della richiesta contiene uno o più attributi utente da aggiornare: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

```
POST /userservice/management/v1/users/{userid}/update.json
```

```
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

In [sottoscrizioni integrate in Adobe IMS](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), questo endpoint supporta l&#39;eliminazione solo di [utenti con sola API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). Per eliminare [utenti standard](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), utilizza invece [Adobe User Management API](https://developer.adobe.com/umapi/).

L&#39;endpoint [Elimina utente](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) accetta un singolo parametro di percorso `userid` ed elimina l&#39;utente corrispondente dall&#39;istanza. Questa è una cancellazione distruttiva e non può essere annullata. In caso di esito positivo, viene restituito il codice di stato 200, altrimenti viene restituito un messaggio di errore.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Elimina utente invitato

L&#39;endpoint [Elimina utente invitato](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) accetta un singolo parametro di percorso `userid` ed elimina l&#39;utente corrispondente &quot;in sospeso&quot; dall&#39;istanza (l&#39;utente non ha ancora accettato il proprio invito). Questa è una cancellazione distruttiva e non può essere annullata. In caso di esito positivo, viene restituito il codice di stato 200, altrimenti viene restituito un messaggio di errore.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Aggiorna Ruoli

Il supporto per l’aggiornamento dei ruoli include la possibilità di aggiungere ed eliminare ruoli. Gli attributi vengono trasmessi come parametri al corpo della richiesta in formato application/json.

## Aggiungi Ruoli

L&#39;endpoint [Aggiungi ruoli](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) accetta un singolo parametro di percorso `userid` e aggiunge uno o più ruoli utente all&#39;utente corrispondente. Il corpo della richiesta contiene un elenco di uno o più oggetti ciascuno contenente un  `accessRoleId` e un attributo `workspaceId`. In caso di esito positivo, viene restituito l&#39;intero elenco di `accessRoleId/workspaceId` coppie per l&#39;utente specificato.

```
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```
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

L&#39;endpoint [Elimina ruoli](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) accetta un singolo parametro di percorso `userid` ed elimina uno o più ruoli utente dall&#39;utente corrispondente. Il corpo della richiesta contiene un elenco di uno o più oggetti ciascuno contenente un  `accessRoleId` e un attributo `workspaceId`. In caso di esito positivo, viene restituito l’elenco rimanente di coppie accessRoleId/workspaceId per l’utente specificato.

```
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```
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
