---
title: "Servizi personalizzati"
feature: REST API
description: "Credenziali di autenticazione con Marketo."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '968'
ht-degree: 0%

---


# Servizi personalizzati

Un servizio personalizzato fornisce le credenziali per l’autenticazione con Marketo. Sono necessarie credenziali per ottenere un token di accesso da Marketo [Servizio identità](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Ogni servizio personalizzato ha l’ambito di competenza di un singolo utente di sola API da cui deriva le sue autorizzazioni.

## Ruoli

Il primo passaggio nella creazione di un servizio personalizzato consiste nel creare un ruolo che possa essere applicato all’utente pertinente e solo API. Questa operazione viene eseguita dal **[!UICONTROL Admin]** > **[!UICONTROL Users & Roles]** > **[!UICONTROL Roles]** menu.

I ruoli sono contenitori per singole autorizzazioni che consentono o limitano l’accesso a determinate funzioni. Nelle sottoscrizioni in cui sono abilitate le aree di lavoro e le partizioni, le autorizzazioni vengono assegnate in base all&#39;area di lavoro. Se un utente dispone di un’autorizzazione in un’area di lavoro ma non in un’altra, potrà eseguire solo le azioni consentite in tale area di lavoro. Per creare un ruolo, fare clic sul pulsante Nuovo ruolo.

![Utenti e ruoli](assets/admin-users-and-roles-roles.png)

Assegna al ruolo un nome descrittivo. Gli utenti solo API dispongono di un set specifico di autorizzazioni separate e distinte dalle normali autorizzazioni utente. Le autorizzazioni API esistono nella rispettiva gerarchia sotto la struttura &quot;Access API&quot;.

![Nuove autorizzazioni ruolo](assets/new-role-access-api-permissions.png)

### Autorizzazioni ruolo

Solo le autorizzazioni del gruppo &quot;API di accesso&quot; vengono applicate agli utenti API, ovvero il rilascio di tutte le autorizzazioni di amministratore non concederà alcuna autorizzazione API a un utente.

Durante la costruzione di un ruolo, è necessario considerare attentamente le azioni da eseguire nell&#39;applicazione. Concedi solo il set minimo di autorizzazioni necessarie per eseguire tali azioni. Consentendo un set di autorizzazioni inutilmente permissivo, le integrazioni possono eseguire azioni indesiderate nell’abbonamento. È possibile utilizzare [strumento autorizzazioni](endpoint-reference.md) per determinare il set minimo di autorizzazioni. Vedi l’elenco completo di [autorizzazioni](#permission_list).

## Utenti

Dopo aver creato un ruolo, devi creare un utente &quot;Solo API&quot;. Gli utenti con solo API sono un tipo speciale di utente in Marketo, in quanto sono amministrati da altri utenti e non possono essere utilizzati per accedere a Marketo. Gli utenti solo API possono:

- Creare servizi personalizzati
- Autorizzazioni di ambito per tali servizi
- Accedere alle API REST

>[!MORELIKETHIS]
>
>Per creare un utente solo API, vai al **[!UICONTROL Admin]** > **[!UICONTROL Users & Roles]** > **[!UICONTROL Users]** e fai clic su [!UICONTROL Invite New User].


![Informazioni nuovo utente](assets/new-user-info.png)

Assegna all’utente un nome descrittivo e un indirizzo e-mail (non deve essere valido) in base al servizio e all’applicazione per i quali verrà utilizzato. Compila i campi obbligatori nel menu di dialogo, fai clic sulla casella di controllo &quot;Solo API&quot; e assegna uno dei tuoi ruoli API all’utente. In questo modo le autorizzazioni del ruolo vengono assegnate all&#39;utente.

![Autorizzazioni per nuovi utenti](assets/new-user-permissions.png)

Infine, fai clic su &quot;Invia&quot; per creare l’utente solo API.

Quando esegui il provisioning di una nuova applicazione con credenziali, valuta seriamente la possibilità di creare un nuovo utente per il servizio anche se dispone dello stesso set di autorizzazioni di un’altra integrazione esistente. Le statistiche sull’utilizzo delle chiamate API e gli errori vengono tracciati in base all’utente, pertanto il provisioning di un utente per ogni applicazione può aiutarti a isolare l’utilizzo e i problemi in applicazioni specifiche. Questa funzione si rivela utile in caso di problemi con il superamento dei limiti di chiamata API giornalieri o errori derivanti da chiamate API effettuate da integrazioni.

## Servizi personalizzati

I servizi personalizzati forniscono le credenziali effettive, l’ID client e il segreto client, necessari per eseguire l’autenticazione con un’istanza di Marketo. Per effettuare il provisioning di uno, vai al tuo **[!UICONTROL Admin]** > **[!UICONTROL Integrations]** > **[!UICONTROL LaunchPoint]** e selezionare **[!UICONTROL New Service]**.

Assegna un nome descrittivo al servizio e, dall’elenco &quot;Servizio&quot;, seleziona &quot;Personalizzato&quot;. Fornisci al servizio una descrizione dettagliata e seleziona un utente appropriato dall’elenco Utente solo API, quindi fai clic su [!UICONTROL Create].

![Nuovo servizio personalizzato](assets/admin-launchpoint-new-service.png)

Questo aggiunge un nuovo servizio all’elenco dei servizi LaunchPoint e l’opzione &quot;Visualizza dettagli&quot;. Fai clic su &quot;Visualizza dettagli&quot; per ricevere l’ID client e il segreto client necessari per l’autenticazione, l’utente proprietario e un’opzione per ottenere un token a scopo di test a breve termine. Il token ottenuto da questa finestra di dialogo ha la stessa durata dei token ottenuti normalmente dalla [Servizio identità](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) ed è valido per 3.600 secondi dalla creazione.

![Ottieni token](assets/get-token.png)

## Aree di lavoro e partizioni

Nelle sottoscrizioni con aree di lavoro e partizioni, la possibilità di accedere a un determinato record o risorsa viene concessa in base alle autorizzazioni di cui dispone il ruolo di un utente in una determinata area di lavoro. A ogni area di lavoro viene concesso l&#39;accesso a una o più partizioni nel menu Aree di lavoro e partizioni e un lead appartiene a una singola partizione. Se l’utente con accesso solo API ha accesso ai record lead in lettura o scrittura in un’area di lavoro, può accedere a tutti i record nelle partizioni a cui tale area di lavoro ha accesso.

Le risorse appartengono alle aree di lavoro, pertanto la capacità di leggere o scrivere una risorsa dipende dal fatto che l’utente abbia o meno un ruolo nell’area di lavoro pertinente con autorizzazioni di lettura o scrittura di quel tipo di record di risorse nell’area di lavoro.

## Elenco autorizzazioni

Di seguito è riportato un elenco di tutte le autorizzazioni disponibili per gli utenti con sola API e di ciò che consentono a un utente con tale autorizzazione di fare.

| Autorizzazione per ruolo | Consente l&#39;accesso a... |
| --- | --- |
| Approva risorse | Approvare le risorse |
| Esegui campagna | Richiedere o pianificare una campagna |
| Attività di sola lettura | Recupera attività lead |
| Metadati attività di sola lettura | Recuperare i metadati dell’attività del lead |
| Risorse di sola lettura | Recuperare i dettagli della risorsa |
| Campagna di sola lettura | Recuperare i dettagli della campagna |
| Società di sola lettura | Recupera dettagli società |
| Oggetto personalizzato di sola lettura | Recupera dettagli oggetto personalizzato |
| Lead di sola lettura | Recupera dettagli lead |
| Account denominato di sola lettura | Recupera dettagli account denominati |
| Elenco di account denominati di sola lettura | Recuperare i dettagli dell’elenco account denominati |
| Opportunità di sola lettura | Recupera dettagli opportunità |
| Venditore di sola lettura | Recupera dettagli persona di vendita |
| Attività di lettura/scrittura | Recuperare e creare attività lead |
| Metadati attività di lettura/scrittura | Recuperare e creare metadati di attività lead |
| Risorse di lettura/scrittura | Recuperare, creare e aggiornare le risorse |
| Campagna di lettura/scrittura | Recuperare, creare e aggiornare campagne |
| Società di lettura/scrittura | Recuperare, creare e aggiornare le società |
| Oggetto personalizzato di lettura/scrittura | Recuperare, creare e aggiornare oggetti personalizzati |
| Lead di lettura/scrittura | Recuperare, creare e aggiornare i dettagli del lead |
| Account denominato di lettura-scrittura | Recupera, crea e aggiorna account denominati |
| Elenco account denominati di lettura-scrittura | Recupera, crea e aggiorna elenchi di account denominati |
| Opportunità di lettura/scrittura | Recuperare, creare e aggiornare le opportunità |
| Persona di vendita di lettura/scrittura | Recupera, crea e aggiorna i venditori |
