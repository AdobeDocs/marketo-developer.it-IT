---
title: Servizi personalizzati
feature: REST API
description: Crea servizi personalizzati Marketo, imposta ruoli e autorizzazioni solo API, ottieni ID client e segreto client in LaunchPoint e ottieni i token di accesso.
exl-id: 38b05c4c-4404-4c30-a7cb-d31b28a3a72e
TQID: https://experienceleague.adobe.com/lvT-8bYucf-K5LYxb5jQ7BHc137W71SvsGg7cWJlxEs
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 872
ht-degree: 0%

---

# Servizi personalizzati

Un servizio personalizzato fornisce le credenziali utilizzate per l&#39;autenticazione con Marketo e per ottenere un token di accesso dal servizio Marketo [Identity](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Ogni servizio personalizzato ha l’ambito di un utente per sola API e ne deriva le autorizzazioni.

## Ruoli

Prima di creare un servizio personalizzato, crea un ruolo da assegnare all’utente di sola API pertinente. Vai a **[!UICONTROL Admin]** > **[!UICONTROL Users & Roles]** > **[!UICONTROL Roles]**.

I ruoli contengono singole autorizzazioni che consentono o limitano l’accesso a funzioni specifiche. Nelle sottoscrizioni con le aree di lavoro e le partizioni abilitate, le autorizzazioni vengono assegnate per area di lavoro. Un utente può eseguire le azioni consentite solo nelle aree di lavoro in cui dispone di tali autorizzazioni.

Per creare un ruolo, selezionare **[!UICONTROL New Role]**.

![Utenti e ruoli](assets/admin-users-and-roles-roles.png)

Assegna al ruolo un nome descrittivo. Gli utenti solo API dispongono di un set specifico di autorizzazioni separate dalle autorizzazioni utente standard. Le autorizzazioni API vengono visualizzate nella rispettiva gerarchia nella struttura &quot;Access API&quot;.

![Autorizzazioni nuovo ruolo](assets/new-role-access-api-permissions.png)

### Autorizzazioni ruolo

Solo le autorizzazioni nel gruppo &quot;API di accesso&quot; sono applicabili agli utenti API. L’assegnazione di tutte le autorizzazioni di amministratore non concede le autorizzazioni API a un utente.

Quando si crea un ruolo, identificare le azioni che l&#39;applicazione deve eseguire. Assegna solo le autorizzazioni minime necessarie per tali azioni. Autorizzazioni non necessarie possono consentire alle integrazioni di eseguire azioni indesiderate nell’abbonamento.

Utilizza lo strumento [autorizzazioni](endpoint-reference.md) per determinare il set minimo di autorizzazioni. Vedi l&#39;elenco completo delle [autorizzazioni](#permission_list).

## Utenti

Dopo aver creato un ruolo, crea un utente &quot;Solo API&quot;. Altri utenti amministrano utenti solo API e gli utenti solo API non possono accedere a Marketo. Possono:

- Creare servizi personalizzati
- Autorizzazioni di ambito per tali servizi
- Accedere alle API REST

>[!MORELIKETHIS]
>
>Per creare un utente solo API, passare al menu **[!UICONTROL Admin]** > **[!UICONTROL Users & Roles]** > **[!UICONTROL Users]** e selezionare **[!UICONTROL Invite New User]**.

![Informazioni utente](assets/new-user-info.png)

Assegna all’utente un nome descrittivo e un indirizzo e-mail in base al servizio e all’applicazione che utilizzerà l’account. L&#39;indirizzo e-mail non deve essere valido. Completa i campi obbligatori, seleziona la casella di controllo **[!UICONTROL API Only]** e assegna all&#39;utente uno dei tuoi ruoli API. Questa azione assegna all&#39;utente il set di autorizzazioni del ruolo.

![Autorizzazioni per nuovi utenti](assets/new-user-permissions.png)

Selezionare **[!UICONTROL Send]** per creare l&#39;utente solo API.

Quando esegui il provisioning delle credenziali per una nuova applicazione, puoi creare un utente separato per il servizio, anche se un’altra integrazione utilizza lo stesso set di autorizzazioni. Le statistiche sull’utilizzo delle chiamate API e gli errori vengono tracciati per utente.

Un utente per ogni applicazione consente di isolare l’utilizzo e i problemi relativi ad applicazioni specifiche. Questa separazione è utile quando le integrazioni raggiungono i limiti di chiamata API giornalieri o generano errori API.

## Servizi personalizzati

I servizi personalizzati forniscono l’ID client e il segreto client necessari per l’autenticazione con un’istanza di Marketo. Per eseguire il provisioning di un servizio, passare a **[!UICONTROL Admin]** > **[!UICONTROL Integrations]** > **[!UICONTROL LaunchPoint]** e selezionare **[!UICONTROL New Service]**.

Assegna al servizio un nome descrittivo. Dall&#39;elenco &quot;Servizio&quot;, selezionare &quot;Personalizzato&quot;. Immettere una descrizione dettagliata, selezionare un utente appropriato dall&#39;elenco Utente solo API, quindi selezionare **[!UICONTROL Create]**.

![Nuovo servizio personalizzato](assets/admin-launchpoint-new-service.png)

Il servizio viene visualizzato nell&#39;elenco dei servizi LaunchPoint con l&#39;opzione &quot;Visualizza dettagli&quot;. Seleziona Visualizza dettagli per accedere all’ID client, al segreto client, all’utente proprietario e all’opzione Ottieni token.

Utilizza Ottieni token per test a breve termine. Il token ha la stessa durata dei token ottenuti dal servizio [Identity](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) ed è valido per 3.600 secondi dopo la creazione.

![Ottieni token](assets/get-token.png)

## Aree di lavoro e partizioni

Nelle sottoscrizioni con aree di lavoro e partizioni, le autorizzazioni del ruolo di un utente in un’area di lavoro determinano l’accesso a record e risorse. Ogni area di lavoro ha accesso a una o più partizioni e ogni lead appartiene a una partizione.

Se un utente con accesso solo API può leggere o scrivere record lead in un’area di lavoro, può accedere a tutti i record nelle partizioni disponibili in tale area di lavoro.

Assets appartiene alle aree di lavoro. Un utente può leggere o scrivere una risorsa quando dispone di un ruolo con l’autorizzazione richiesta nell’area di lavoro della risorsa.

## Elenco autorizzazioni

Nella tabella seguente sono elencate le autorizzazioni disponibili per gli utenti con sola API e l’accesso concesso da ciascuna autorizzazione.

| Autorizzazione per ruolo | Consente l&#39;accesso a... |
| --- | --- |
| Approva Assets | Approvare le risorse |
| Eseguire campagna | Richiedere o pianificare una campagna |
| Attività di sola lettura | Recupera attività lead |
| Metadati attività di sola lettura | Recuperare i metadati dell’attività del lead |
| Assets di sola lettura | Recuperare i dettagli della risorsa |
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
| Assets di lettura/scrittura | Recuperare, creare e aggiornare le risorse |
| Campagna di lettura/scrittura | Recuperare, creare e aggiornare campagne |
| Società di lettura/scrittura | Recuperare, creare e aggiornare le società |
| Oggetto personalizzato di lettura/scrittura | Recuperare, creare e aggiornare oggetti personalizzati |
| Lead di lettura/scrittura | Recuperare, creare e aggiornare i dettagli del lead |
| Account denominato di lettura-scrittura | Recupera, crea e aggiorna account denominati |
| Elenco account denominati di lettura-scrittura | Recupera, crea e aggiorna elenchi di account denominati |
| Opportunità di lettura/scrittura | Recuperare, creare e aggiornare le opportunità |
| Persona di vendita di lettura/scrittura | Recupera, crea e aggiorna i venditori |
