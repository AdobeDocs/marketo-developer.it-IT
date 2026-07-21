---
title: REST API
feature: REST API
description: Scopri come utilizzare l’API REST di Marketo, configurare gli utenti API e LaunchPoint, visualizzare quote e limiti, eseguire l’autenticazione con l’intestazione Autorizzazione e recuperare i lead.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
TQID: https://experienceleague.adobe.com/GqhWI816wWX-2zf89wWj-GXpg9i615HRFVl2ljdYVj0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 765
ht-degree: 2%

---

# REST API

L’API REST di Marketo fornisce l’accesso remoto a molte funzionalità di sistema. Puoi utilizzarlo per creare programmi, importare lead in blocco e controllare un’istanza di Marketo a un livello dettagliato.

Le API REST rientrano in due categorie generali:

- [Le API del database lead](https://developer.adobe.com/marketo-apis/api/mapi) recuperano e interagiscono con i record persona di Marketo e i tipi di oggetto associati, ad esempio Opportunità e Aziende.
- Le API di [Asset](https://developer.adobe.com/marketo-apis/api/asset) interagiscono con materiale promozionale di marketing e record relativi al flusso di lavoro.

>[!NOTE]
>
>L’API SOAP diventerà obsoleta e non sarà più disponibile dopo il 31 luglio 2026. Tutti i nuovi sviluppi devono essere eseguiti con Marketo [REST API](./rest-api.md) e i servizi esistenti devono essere migrati entro tale data per evitare interruzioni del servizio. Se si dispone di un servizio che utilizza l&#39;API SOAP, consultare la [Guida alla migrazione dell&#39;API SOAP](../soap-api/migration.md) per informazioni su come eseguire la migrazione.
>

>[!IMPORTANT]
>
>Consulta questo [Post nazione](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) sulla rimozione della doppia barra negli URL del gateway API.
>

- **Quota giornaliera:** A ogni abbonamento sono assegnate 50.000 chiamate API al giorno. La quota viene ripristinata ogni giorno alle 00:00 CST. Contatta il tuo account manager per aumentare la quota giornaliera.
- **Limite di frequenza:** Ogni istanza è limitata a 100 chiamate API in 20 secondi.
- **Limite di concorrenza:** Ogni istanza consente un massimo di dieci chiamate API simultanee.

Le chiamate API standard hanno una lunghezza massima dell’URI di 8 KB e una dimensione massima del corpo di 1 MB. Le chiamate API in blocco supportano una dimensione massima del corpo di 10 MB.

Quando una chiamata contiene un errore, in genere l’API restituisce ancora il codice di stato HTTP 200. La risposta JSON contiene un membro `success` con un valore di `false` e un array di errori nel membro `errors`. Ulteriori informazioni sugli errori sono disponibili [qui](error-codes.md).

## Guida introduttuva

Per completare la procedura seguente, è necessario disporre dei privilegi di amministratore nell’istanza Marketo. Questo flusso di lavoro crea le credenziali API e le utilizza per recuperare un record lead.

Innanzitutto, crea un utente API e ottieni le credenziali per le chiamate autenticate. Accedi all&#39;istanza e passa a **[!UICONTROL Admin]** > **[!UICONTROL Users and Roles]**.

![Utenti e ruoli amministratori](assets/admin-users-and-roles.png)

Selezionare la scheda **[!UICONTROL Roles]**, quindi selezionare Nuovo ruolo. Assegna al ruolo almeno l’autorizzazione &quot;Lead di sola lettura&quot; (o &quot;Persona di sola lettura&quot;) dal gruppo API di accesso. Assegna un nome descrittivo al ruolo e seleziona **[!UICONTROL Create]**.

![Nuovo ruolo](assets/new-role.png)

Torna alla scheda [!UICONTROL Users] e seleziona **[!UICONTROL Invite New User]**. Immettere un nome descrittivo che identifichi l&#39;utente come utente API, immettere un indirizzo e-mail e selezionare **[!UICONTROL Next]**.

![Informazioni nuovo utente](assets/new-user-info.png)

Selezionare l&#39;opzione [!UICONTROL API Only], assegnare il ruolo API creato e selezionare **[!UICONTROL Next]**.

![Autorizzazioni per nuovi utenti](assets/new-user-permissions.png)

Selezionare **[!UICONTROL Send]** per creare l&#39;utente.

![Nuovo messaggio utente](assets/new-user-message.png)

Quindi, passare al menu [!UICONTROL Admin] e selezionare **[!UICONTROL LaunchPoint]**.

![Punto di avvio](assets/admin-launchpoint.png)

Selezionare **[!UICONTROL New]** > **[!UICONTROL New Service]**. Immettere un nome e una descrizione descrittivi e selezionare **[!UICONTROL Custom]** dal menu [!UICONTROL Service]. Selezionare il nuovo utente dal menu [!UICONTROL API Only User], quindi selezionare **[!UICONTROL Create]**.

![Nuovo servizio Launchpoint](assets/admin-launchpoint-new-service.png)

Selezionare **[!UICONTROL View Details]** per il nuovo servizio per accedere all&#39;ID client e al segreto client. Selezionare **[!UICONTROL Get Token]** per generare un token di accesso valido per un&#39;ora. Salva il token per la prima chiamata API.

![Ottieni token](assets/get-token.png)

Vai a **[!UICONTROL Admin]** > **[!UICONTROL Web Services]**.

![Servizi Web](assets/admin-web-services.png)

Trova [!UICONTROL Endpoint] nella casella REST API e salvalo per la prima chiamata API.

![Endpoint REST](assets/admin-web-services-rest-endpoint-1.png)

Ogni chiamata API REST deve includere un token di accesso in un’intestazione HTTP.

```text
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>Il supporto per l&#39;autenticazione tramite il parametro di query **access_token** verrà rimosso il 30 giugno 2025. Se il progetto utilizza un parametro di query per passare il token di accesso, deve essere aggiornato per utilizzare l&#39;intestazione **Authorization** il prima possibile. Il nuovo sviluppo deve utilizzare esclusivamente l&#39;intestazione **Authorization**.

Apri una nuova scheda del browser e immetti il seguente URL. Sostituisci i segnaposto con l&#39;endpoint e l&#39;indirizzo e-mail dell&#39;istanza per chiamare [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadsByFilterUsingGET).

```text
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Se il database non contiene un record di lead con il tuo indirizzo e-mail, utilizza l’indirizzo e-mail di un lead esistente. Invia l’URL per ricevere una risposta JSON simile al seguente esempio:

```json
{
    "requestId":"c493#1511ca2b184",
    "result":[
       {
           "id":1,
           "updatedAt":"2015-08-24T20:17:23Z",
           "lastName":"Elkington",
           "email":"developerfeedback@marketo.com",
           "createdAt":"2013-02-19T23:17:04Z",
           "firstName":"Kenneth"
        }
    ],
    "success":true
}
```

## Utilizzo API

Il rapporto sull’utilizzo delle API tiene traccia di ogni utente API separatamente. L’assegnazione di un utente separato a ciascun servizio web consente di identificare l’utilizzo API di ogni integrazione.

Se le chiamate superano il limite di istanza e le chiamate successive non riescono, utilizza il rapporto per identificare il volume di chiamate da ciascun servizio. Vai a **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** e seleziona il numero di chiamate effettuate negli ultimi sette giorni.

Per gli endpoint REST che restituiscono le statistiche di utilizzo e di errore giornaliere e degli ultimi 7 giorni, vedere [Utilizzo](usage.md).
