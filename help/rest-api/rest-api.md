---
title: "REST API"
feature: REST API
description: "Panoramica API REST"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 0%

---


# REST API

Marketo espone un’API REST che consente l’esecuzione remota di molte delle funzionalità del sistema. Dalla creazione di programmi all’importazione in blocco di lead, sono disponibili molte opzioni che consentono il controllo dettagliato di un’istanza di Marketo.

Queste API generalmente rientrano in due categorie generali: [Database lead](https://developer.adobe.com/marketo-apis/api/mapi/), e [Risorsa](https://developer.adobe.com/marketo-apis/api/asset/). Le API del database lead consentono il recupero e l’interazione con i record persona di Marketo e i tipi di oggetto associati, ad esempio Opportunità e Società. Le API Asset consentono l’interazione con materiale promozionale di marketing e record relativi al flusso di lavoro.

- **Quota giornaliera:** Agli abbonamenti vengono assegnate 50.000 chiamate API al giorno (che vengono ripristinate ogni giorno alle 00.00 CST). Puoi aumentare la tua quota giornaliera tramite il tuo account manager.
- **Limite tariffa:** L’accesso API per istanza è limitato a 100 chiamate per 20 secondi.
- **Limite concorrenza:**  Massimo dieci chiamate API simultanee.

La dimensione delle chiamate standard è limitata a una lunghezza URI di 8 KB e a una dimensione corpo di 1 MB, anche se il corpo può essere di 10 MB per le nostre API in blocco. Se si verifica un errore in durante la chiamata, in genere l’API restituisce il codice di stato 200, ma la risposta JSON conterrà un membro &quot;success&quot; con il valore `false`e un array di errori nel membro &quot;errors&quot;. Ulteriori informazioni sugli errori [qui](error-codes.md).

## Guida introduttiva

I passaggi seguenti richiedono i privilegi di amministratore nell’istanza Marketo.

Per la prima chiamata a Marketo, recupererai un record di lead. Per iniziare a utilizzare Marketo, devi ottenere le credenziali API per effettuare chiamate autenticate all’istanza. Accedi all’istanza e passa a **[!UICONTROL Admin]** -> **[!UICONTROL Users and Roles]**.

![Utenti e ruoli amministratore](assets/admin-users-and-roles.png)

Fai clic su [!UICONTROL Roles] , quindi Nuovo ruolo e assegnare almeno l&#39;autorizzazione &quot;Read-Only Lead&quot; (o &quot;Read-Only Person&quot;) al ruolo nel gruppo Access API. Assegna un nome descrittivo e fai clic su [!UICONTROL Create].

![Crea Ruolo](assets/new-role.png)

Ora torna alla scheda Utenti e fai clic su Invita nuovo utente. Assegna all’utente un nome descrittivo che indica che si tratta di un utente API, un Indirizzo e-mail e fai clic su **[!UICONTROL Next]**.

![Informazioni nuovo utente](assets/new-user-info.png)

Quindi, seleziona l’opzione Solo API e assegna all’utente il ruolo API creato e fai clic su **[!UICONTROL Next]**.

![Autorizzazioni per nuovi utenti](assets/new-user-permissions.png)

Per completare il processo di creazione utente, fai clic su **[!UICONTROL Send]**.

![Nuovo messaggio utente](assets/new-user-message.png)

Quindi, vai al menu Amministratore e fai clic su **[!UICONTROL LaunchPoint]**.

![Launchpoint](assets/admin-launchpoint.png)

Fai clic sul menu Nuovo e seleziona [!UICONTROL New Service]. Assegna un nome descrittivo al servizio e seleziona &quot;Personalizzato&quot; dal menu a discesa Servizio. Fornisci una descrizione, quindi seleziona il nuovo utente dal menu a discesa Solo utente API e fai clic su [!UICONTROL Create].

![Nuovo servizio Launchpoint](assets/admin-launchpoint-new-service.png)

Fai clic su Visualizza dettagli per il nuovo servizio per accedere all’ID client e al segreto client. Per il momento puoi fare clic su [!UICONTROL Get Token] per generare un token di accesso valido per un’ora. Per il momento salva il token in una nota.

![Ottieni token](assets/get-token.png)

Quindi, vai al menu Amministratore e poi a **[!UICONTROL Web Services]**.

![Servizi Web](assets/admin-web-services.png)

Per il momento, trova l’endpoint nella casella API REST e salvalo in una nota.

![Endpoint REST](assets/admin-web-services-rest-endpoint-1.png)

Apri una nuova scheda del browser e immetti quanto segue, utilizzando le informazioni appropriate per chiamare [Ottieni lead per tipo di filtro](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET):

```
<Your Endpoint URL>/rest/v1/leads.json?access_token=<Your Access Token>&filterType=email&filterValues=<Your Email Address>
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

Ciascuno degli utenti API viene segnalato singolarmente nel rapporto sull’utilizzo delle API, pertanto la suddivisione dei servizi web per utente consente di tenere facilmente conto dell’utilizzo di ciascuna integrazione. Se il numero di chiamate API all’istanza supera il limite e le chiamate successive non riescono, l’utilizzo di questa procedura ti consente di tenere conto del volume di ciascuno dei servizi e di valutare come risolvere il problema. Scopri come utilizzi andando in **[!UICONTROL Admin]** -> **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** e facendo clic sul numero di chiamate negli ultimi sette giorni.
