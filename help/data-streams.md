---
title: Flussi di dati
description: Panoramica dei flussi di dati Marketo Engage per attività lead quasi in tempo reale ed eventi di controllo degli utenti, con riduzione dei limiti API per i clienti di livello prestazioni
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
TQID: https://experienceleague.adobe.com/JnhN70HexjmNueZa9MAVrxjEhZ5yJatWqZiowl22quA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1342
ht-degree: 3%

---

# Flussi di dati

>[!NOTE]
>
>Le informazioni correnti sui flussi di dati sono ora disponibili in [Utilizzo dei flussi di dati](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams#).
>

I flussi di dati forniscono grandi volumi di dati Marketo Engage a sistemi esterni in tempo reale. Utilizza i dati in streaming per supportare decisioni tempestive, campagne mirate, processi di marketing esterni e auditing.

I flussi di dati offrono i seguenti vantaggi:

- Riduci la dipendenza da richieste API a velocità limitata.
- Riduci gli avvisi per limite API.
- Consegna dei dati senza eseguire esportazioni in blocco.

I flussi di dati sono disponibili per coloro che hanno acquistato un [pacchetto livello prestazioni Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Panoramica del flusso di dati dell’attività del lead

Il flusso di dati dell’attività del lead invia grandi volumi di dati dell’attività del lead a un sistema esterno in tempo reale. Utilizza il flusso per controllare gli eventi lead e i modelli di utilizzo, visualizzare le modifiche dei lead e attivare i flussi di lavoro dagli eventi lead.

Puoi iscriverti a più di 144 tipi di attività.

Il flusso può includere:

1. Modifiche a tutti i campi dei lead e dei nuovi lead creati.
1. Tutti i tipi di attività lead documentati.
1. Lead eliminati.
1. Tutti gli oggetti personalizzati del lead, se richiesto. Non è possibile selezionare singoli oggetti personalizzati.

I casi d’uso comuni includono:

- Avvisi personalizzati: aggiungi a un elenco i lead con condizioni incoerenti. Il flusso invia l’attività Aggiungi all’elenco a un processo di follow-up.
- Modelli di apprendimento automatico: utilizza gli approfondimenti dell’attività in modelli di punteggio esterni, quindi invia i punteggi a Marketo per influenzare le campagne di apprendimento o altri processi.

Elenco delle attività in streaming:

| AchieveGoalInReferral | ClickPredictiveContent | ReceivedForwardToFriendEmail |
| --- | --- | --- |
| AddToList | ClickRTPCallToAction | ReceiveSalesEmail |
| AddToNurture | ClickSalesEmail | Fai riferimento a SocialApp |
| AddToOpportunity | ClickSharedLink | RemoveFromList |
| AddToSalesCampaign | ConvertLead | RemoveFromOpportunity |
| Chiama webhook | DeleteLead | RequestCampaign |
| ChangeDataValue | SqualificareSweepstakes | SalesEmailBounded |
| ChangeLeadPartition | EarnEntryInSocialApp | InviaAvviso |
| ChangeNurtureCadence | E-mail non recapitata | InviaE-mail |
| ChangeNurtureTrack | EmailBoundedSoft | InviaE-mailVendite |
| ChangeOwner | EmailDelivered | SentForwardToFriendEmail |
| ChangeProgramData | EnrichWithDataDotCom | Attività SFDCA |
| ChangeProgramMemberData | EnterSweepstakes | ShareContent |
| ChangeRevenueStage | FillOutFacebookLeadAdsForm | AbbonatiPerOffertaRiferimenti |
| ChangeRevenueStageManualmente | ModuloCompilazione | SyncLeadToMicrosoft |
| ChangeScore | InterestingMoment | SyncLeadToSFDC |
| ChangeSegment | MergeLeads | Annulla iscrizione e-mail |
| ChangeStatusInProgression | NewLead | UpdateOpportunity |
| ChangeStatusInSalesCampaign | OpenEmail | VisitWebPage |
| ClickEmail | OpenSalesEmail | VoteInPoll |
| ClickLink | PushLeadToMarketo | WinSweepstakes |

Durante il flusso di oggetti personalizzati, includi tutti gli oggetti personalizzati relativi al lead. Non è possibile selezionare singoli oggetti personalizzati.

## Panoramica del flusso di dati di controllo dell’utente

Il flusso di dati di controllo dell’utente tiene traccia delle modifiche apportate dagli utenti alle risorse in tempo reale. Utilizzala per controllare gli eventi delle risorse, visualizzare le modifiche apportate dagli utenti e attivare i processi dagli eventi di audit.

Adobe I/O Events invia le modifiche a un endpoint configurabile. Iscriviti ai tipi di evento necessari per ogni tipo di risorsa.

Un caso d’uso è:

- Tracciamento delle modifiche tra i sistemi di marketing: quando un CRM o un altro sistema scambia lead con Marketo, utilizza gli eventi di audit per identificare quale sistema ha apportato la modifica più recente.

Elenco degli eventi di controllo degli utenti in streaming:

| COMPONENTE | ELENCO TIPI DI EVENTO |
| --- | --- |
| Programma predefinito | clona, crea, elimina, modifica canale, esporta, modifica configurazione programma, modifica token programma, rinomina |
| E-mail | approva, clona, crea, elimina, modifica, sposta, rinomina, annulla approvazione |
| Programma batch e-mail | approva, childUpdate, clona, crea, elimina, modifica, modifica canale, modifica pianificazione programma, modifica configurazione programma, modifica token programma, rinomina, annulla approvazione |
| Modello e-mail | approva, clona, crea, elimina, bozzaCrea, bozzaElimina, modifica, rinomina, annulla approvazione |
| Programma coinvolgimento | clona, crea, elimina, modifica canale, modifica configurazione programma, modifica flusso programma, modifica token programma, rinomina |
| Programma evento | clonare, creare, eliminare, modificare il canale, modificare la pianificazione del programma, modificare la configurazione del programma, modificare il token del programma, rinominare |
| Cartella | creare, eliminare, modificare, rinominare |
| Modulo | approvare, clonare, creare, eliminare, bozzaCreare, modificare, spostare, rinominare |
| Modulo -> Modulo pagina di destinazione | creare, clonare, modificare, eliminare, approvare, rinominare |
| Pagina di destinazione | approva, clona, crea, elimina, bozzaElimina, modifica, rinomina, annulla approvazione |
| Modello pagina di destinazione | approva, clona, crea, elimina, bozzaCrea, bozzaElimina, modifica, rinomina, annulla approvazione |
| Elenco avanzato | clona, crea, elimina, modifica, esporta, modifica impostazione elenco avanzato, rinomina |
| Cartella di marketing | creare, modificare, eliminare |
| Programma di nurturing | clona, crea, elimina, modifica canale, modifica configurazione programma, modifica flusso programma, modifica token programma, rinomina |
| Segmento | creare, eliminare, modificare, rinominare |
| Segmentazione | approvare, creare, eliminare, bozzaCreata, bozzaDiscardata, rinominare, non approvare |
| Campagna avanzata | interrompi, attiva, clona, crea, disattiva, elimina, modifica, modifica pianificazione campagne, modifica azione passaggio flusso, modifica impostazione elenco avanzato, sposta, rinomina |
| Snippet | approva, approva senza bozza, clona, crea, elimina, modifica, rinomina, annulla approvazione |
| Interfaccia utente amministratore -> Launchpoint -> Integrazione | creare, eliminare, modificare |
| Interfaccia utente amministratore -> Utente | Creare, modificare, eliminare (come per l’utente solo API) |
| Accesso amministratore -> Utente | accesso riuscito, accesso non riuscito |
| Programma -> Programma batch e-mail | Modifica (per modificare l’indirizzo e-mail selezionato) API risorse |
| Programma -> Programma di marketing | crea, clona |

Esempio di evento di controllo utente:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "id": "b77c743a-8e28-40f2-8aab-9541bbc85e68",
        "type": "com.adobe.platform.marketo.audit.user.email",
        "source": "https://www.marketo.com",
        "time": "2020-05-28T19:20:47.28Z",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentId": 232459,
            "componentType": "Email",
            "eventAction": "approve",
            "munchkinId": "123-ABC-456",
            "imsOrgId": "ADOBEORGID@AdobeOrg",
            "user": 253,
            "userId": "example@marketo.com"
        }
    }
}
```

## Panoramica del flusso di dati di notifica

Il flusso di dati per le notifiche è disponibile come parte delle offerte dei livelli di prestazioni di Marketo Engage.

Il Centro notifiche Marketo può inviare notifiche a un indirizzo e-mail. Il flusso di dati di notifica invia inoltre tali notifiche a un endpoint configurabile tramite Adobe I/O Events. Queste sono le stesse notifiche disponibili dall’icona a forma di campanello nell’interfaccia utente di Marketo.

Elenco degli eventi di notifica:

| COMPONENTE | ELENCO TIPI DI EVENTO |
| --- | --- |
| Notifica | interruzione campagna, errore campagna, sviluppo (programma esaurito), errore sincronizzazione salesforce, gruppo di test (risultato test A/B), servizi web (quota giornaliera) |

Esempio di evento di notifica:

```json
{
    "event_id": "a1b2c3d4-zyxw-9876-9z8y-a1b2c3d4e5f6",
    "event": {
        "specversion": "1.0",
        "type": "com.adobe.platform.marketo.notification.campaign_abort",
        "source": "https://www.marketo.com",
        "time": "2021-05-27T10:22:37.489-5:00",
        "datacontenttype": "application/json",
        "dataschema": "V1.0",
        "data": {
            "componentType": "campaign_abort",
            "subType": "user_campaign_abort",
            "eventAction": {
                "campaignId":1234,
                "userId":"example@marketo.com",
            }
            "imsOrgId":"ADOBEORGID@AdobeOrg",
            "munchkinId":"123-ABC-456"
        }
    }
}
```

## Dettagli tecnici

Le sezioni seguenti descrivono la configurazione necessaria per ricevere i dati da ciascun flusso. Ogni flusso richiede la configurazione dell’endpoint e il codice di integrazione.

### Flusso dati attività lead

Il flusso Attività lead invia gli eventi di attività lead sottoscritti con le seguenti caratteristiche del servizio:

- Per impostazione predefinita, i dati vengono inviati ogni due secondi.
- Ogni abbonamento utilizza batch di 100-500 record.
- Il servizio REST del cliente ha un timeout di 20 secondi e tre tentativi a intervalli di tre minuti. Un nuovo tentativo riuscito abilita automaticamente il servizio. Dopo tre errori, il servizio si interrompe e tenta ogni tre minuti, a meno che non venga eseguito manualmente il deprovisioning.
- I dati in coda vengono conservati per un massimo di sette giorni.

Per implementare il flusso di dati dell’attività del lead:

1. Esporta un endpoint HTTP che può ricevere richieste POST con un corpo JSON dalla rete Internet pubblica. Il flusso di dati push dell’attività invia richieste a:
1. Fornisci ad Adobe quanto segue:
   1. Marketo Munchkin ID per la sottoscrizione
   1. URL dell&#39;endpoint nel passaggio 1
   1. I tipi di attività che desiderano ricevere (elenco completo sopra riportato)
   1. Un mezzo di autenticazione che consenta al cliente di verificare la legittimità delle richieste. Oppure:
      1. Un URL del provider di identità, un ID client e un segreto client per OAuth [Autenticazione credenziali client](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Un token API, che può essere incluso nelle richieste inviate dallo stream di dati dell’attività lead in un’intestazione http di autorizzazione

Adobe abilita il flusso di dati dopo aver ricevuto le informazioni richieste. L’endpoint inizia quindi a ricevere i dati.

Diagramma UML di una chiamata tipica del flusso di dati dell’attività del lead:

![Diagramma flusso dati attività lead](assets/lead-activity-data-stream.png)

Esempio di creazione di un endpoint URL:

```javascript
/*
Copyright 2022 Adobe
All Rights Reserved.

NOTICE: Adobe permits you to use, modify, and distribute this file in
accordance with the terms of the Adobe license agreement accompanying
it.
*/
constexpress=require('express')
constwinston=require('winston');
constport=3000

constapp=express().use(express.json())

constlogger=winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: {service: 'activity-stream-consumer-example'},
  transports: [
    // - Write all logs with level `error` and below to `error.log`
    newwinston.transports.File({filename: 'error.log',level: 'error'}),
    // - Write all logs with level `info` and below to `combined.log`
    newwinston.transports.File({filename: 'combined.log'}),
    newwinston.transports.Console({format: winston.format.simple()})
  ],
});

app.get('/',(req,res)=>{
  logger.info(JSON.stringify(req.query))
  res.sendStatus(200)
})

app.post('/',(req,res)=>{
  logger.info(JSON.stringify(req.body))
  res.sendStatus(200)
})

app.listen(port,()=>{
  logger.info(`app listening on port ${port}`)
})
```

Per il codice dell&#39;applicazione di esempio, vedi l&#39;esempio del consumer [Flusso dati attività lead](https://github.com/ihgrant/activity-stream-consumer-example).

### Flusso dati di controllo utente e flusso dati di notifica

Gli eventi di User Audit vengono inviati tramite Adobe I/O. Per utilizzarli con un Adobe ID:

1. Fornisci ad Adobe le seguenti informazioni:
   1. Adobe ID
   1. Marketo Munchkin ID per la sottoscrizione
1. Esponi un endpoint REST, in genere un webhook, per utilizzare gli eventi.
1. Dopo aver ricevuto le informazioni sull’endpoint, Adobe abilita il flusso per la sottoscrizione.
1. Configura il flusso in Adobe I/O.
   1. Questo passaggio richiede un’organizzazione Adobe
   1. Richiede che l’utente dell’organizzazione Adobe abbia il ruolo di sviluppatore o amministratore di sistema

Per configurare Adobe I/O, vedere [Configurazione dei flussi di dati di controllo degli utenti di Marketo con Adobe I/O](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup#).

### Configurazione del flusso di dati di controllo degli utenti in Marketo

Il flusso di dati di audit utente è attualmente disponibile come parte dei pacchetti di prestazioni insieme agli altri 3 flussi di dati. Per ulteriori informazioni sui pacchetti, fare riferimento alla [pagina di descrizione del prodotto](https://helpx.adobe.com/it/legal/product-descriptions/adobe-marketo-engage---product-description.html) per i limiti e le funzionalità del prodotto.

### Configurazione di Adobe I/O

[Consulta Guida introduttiva a Adobe I/O Events](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Per istruzioni di base su questo caso d&#39;uso, a partire da [console.adobe.io](https://developer.adobe.com/console):

Quando richiesto, selezionare **[!UICONTROL Create New Project]** o **[!UICONTROL Add Event]**.

### Introduzione al nuovo progetto

Per iniziare a utilizzare i servizi Adobe, aggiungi un&#39;API, eventi o runtime, visualizza la [documentazione](https://developer.adobe.com/runtime/docs/).

## Documentazione pubblica

- [Flussi di dati Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Introduzione ad eventi IO e webhook di Adobe](https://developer.adobe.com/events/docs/guides/)
- [Blog sui flussi di dati](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
