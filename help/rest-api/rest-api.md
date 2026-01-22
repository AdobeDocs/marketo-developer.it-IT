---
title: REST API
feature: REST API
description: Scopri come utilizzare l’API REST di Marketo, configurare gli utenti API e LaunchPoint, visualizzare quote e limiti, eseguire l’autenticazione con l’intestazione Autorizzazione e recuperare i lead.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 5881ab969eca3a37d19f56b6570e42828994eff3
workflow-type: tm+mt
source-wordcount: '806'
ht-degree: 1%

---

# REST API

Marketo espone un’API REST che consente l’esecuzione remota di molte delle funzionalità del sistema. Dalla creazione di programmi all’importazione in blocco di lead, sono disponibili molte opzioni che consentono il controllo dettagliato di un’istanza di Marketo.

Queste API in genere rientrano in due categorie generali: [Lead Database](https://developer.adobe.com/marketo-apis/api/mapi/) e [Asset](https://developer.adobe.com/marketo-apis/api/asset/). Le API del database lead consentono il recupero e l’interazione con i record persona di Marketo e i tipi di oggetto associati, ad esempio Opportunità e Società. Le API Asset consentono l’interazione con materiale promozionale di marketing e record relativi al flusso di lavoro.

>[!NOTE]
>L’API SOAP diventerà obsoleta e non sarà più disponibile dopo il 31 marzo 2026. Tutti i nuovi sviluppi devono essere eseguiti con Marketo [REST API](./rest-api.md) e i servizi esistenti devono essere migrati entro tale data per evitare interruzioni del servizio. Se si dispone di un servizio che utilizza l&#39;API SOAP, consultare la [Guida alla migrazione dell&#39;API SOAP](../soap-api/migration.md) per informazioni su come eseguire la migrazione.
>

>[!IMPORTANT]
>Consulta questo [Post nazione](https://nation.marketo.com/t5/product-blogs/rest-api-double-slash-deprecation/ba-p/358616) sulla rimozione della doppia barra negli URL del gateway API.
>

- **Quota giornaliera:** agli abbonamenti vengono assegnate 50.000 chiamate API al giorno (che vengono ripristinate ogni giorno a 12:00AM CST). Puoi aumentare la tua quota giornaliera tramite il tuo account manager.
- **Limite di frequenza:** l&#39;accesso API per istanza è limitato a 100 chiamate per 20 secondi.
- **Limite concorrenza:**  Massimo dieci chiamate API simultanee.

La dimensione delle chiamate standard è limitata a una lunghezza URI di 8 KB e a una dimensione corpo di 1 MB, anche se il corpo può essere di 10 MB per le nostre API in blocco. Se si verifica un errore in con la chiamata, in genere l&#39;API restituisce il codice di stato 200, ma la risposta JSON conterrà un membro &quot;success&quot; con un valore di `false` e una matrice di errori nel membro &quot;errors&quot;. Ulteriori informazioni sugli errori [qui](error-codes.md).

## Guida introduttuva

I passaggi seguenti richiedono i privilegi di amministratore nell’istanza Marketo.

Per la prima chiamata a Marketo, recupera un record di lead. Per iniziare a utilizzare Marketo, devi ottenere le credenziali API per effettuare chiamate autenticate all’istanza. Accedi all&#39;istanza e passa a **[!UICONTROL Admin]** -> **[!UICONTROL Users and Roles]**.

![Utenti e ruoli amministratori](assets/admin-users-and-roles.png)

Fare clic sulla scheda **[!UICONTROL Roles]**, quindi su Nuovo ruolo e assegnare almeno l&#39;autorizzazione &quot;Lead di sola lettura&quot; (o &quot;Persona di sola lettura&quot;) al ruolo nel gruppo API di accesso. Assicurarsi di assegnare un nome descrittivo e fare clic su **[!UICONTROL Create]**.

![Nuovo ruolo](assets/new-role.png)

Tornare alla scheda [!UICONTROL Users] e fare clic su **[!UICONTROL Invite New User]**. Assegnare all&#39;utente un nome descrittivo che indichi che si tratta di un utente API, un indirizzo di posta elettronica e fare clic su **[!UICONTROL Next]**.

![Informazioni nuovo utente](assets/new-user-info.png)

Quindi, seleziona l&#39;opzione [!UICONTROL API Only] e assegna all&#39;utente il ruolo API creato, quindi fai clic su **[!UICONTROL Next]**.

![Autorizzazioni per nuovi utenti](assets/new-user-permissions.png)

Per completare il processo di creazione utente, fare clic su **[!UICONTROL Send]**.

![Nuovo messaggio utente](assets/new-user-message.png)

Passare quindi al menu [!UICONTROL Admin] e fare clic su **[!UICONTROL LaunchPoint]**.

![Punto di avvio](assets/admin-launchpoint.png)

Fare clic sul menu **[!UICONTROL New]** e selezionare **[!UICONTROL New Service]**. Assegna un nome descrittivo al servizio e seleziona **[!UICONTROL Custom]** dal menu a discesa [!UICONTROL Service]. Fornisci una descrizione, quindi seleziona il nuovo utente dal menu a discesa [!UICONTROL API Only User] e fai clic su **[!UICONTROL Create]**.

![Nuovo servizio Launchpoint](assets/admin-launchpoint-new-service.png)

Fare clic su **[!UICONTROL View Details]** per il nuovo servizio per accedere all&#39;ID client e al segreto client. Per il momento puoi fare clic sul pulsante **[!UICONTROL Get Token]** per generare un token di accesso valido per un&#39;ora. Per il momento salva il token in una nota.

![Ottieni token](assets/get-token.png)

Passare quindi al menu **[!UICONTROL Admin]**, quindi a **[!UICONTROL Web Services]**.

![Servizi Web](assets/admin-web-services.png)

Trova [!UICONTROL Endpoint] nella casella API REST e salva per il momento in una nota.

![Endpoint REST](assets/admin-web-services-rest-endpoint-1.png)

Quando si effettuano chiamate ai metodi API REST, per garantire la riuscita della chiamata è necessario includere un token di accesso in ogni chiamata. Il token di accesso deve essere inviato come intestazione HTTP.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>Il supporto per l&#39;autenticazione tramite il parametro di query **access_token** verrà rimosso il 30 giugno 2025. Se il progetto utilizza un parametro di query per passare il token di accesso, deve essere aggiornato per utilizzare l&#39;intestazione **Authorization** il prima possibile. Il nuovo sviluppo deve utilizzare esclusivamente l&#39;intestazione **Authorization**.

Apri una nuova scheda del browser e immetti quanto segue, utilizzando le informazioni appropriate per chiamare [Get Leads by Filter Type](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

```
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Se nel database non è presente un record di lead con l&#39;indirizzo di posta elettronica, sostituirlo con uno che si sa essere presente. Premi Invio nella barra URL e dovresti ricevere una risposta JSON simile alla seguente:

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

Ciascuno degli utenti API viene segnalato singolarmente nel rapporto sull’utilizzo delle API, pertanto la suddivisione dei servizi web per utente consente di tenere facilmente conto dell’utilizzo di ciascuna integrazione. Se il numero di chiamate API all’istanza supera il limite e le chiamate successive non riescono, l’utilizzo di questa procedura ti consente di tenere conto del volume di ciascuno dei servizi e di valutare come risolvere il problema. Per visualizzare il tuo utilizzo, vai a **[!UICONTROL Admin]** -> **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** e fai clic sul numero di chiamate negli ultimi sette giorni.
