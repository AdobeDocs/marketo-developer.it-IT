---
title: Flussi di dati
description: Panoramica di Data Steams
exl-id: 5617b6a5-ebc8-4d97-a290-e3b87f83e360
source-git-commit: 43bcafd335a2fdc709e917ef74504500422c2889
workflow-type: tm+mt
source-wordcount: '1596'
ht-degree: 1%

---

# Flussi di dati

>[!NOTE]
> Le informazioni correnti sui flussi di dati sono ora disponibili in [Utilizzo dei flussi di dati](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/).
>

Le organizzazioni di marketing dei nostri clienti si affidano a campagne di marketing tempestive e mirate per rimanere al passo con la loro attività ed essere competitive. Al fine di supportare decisioni rapide e consentire cambiamenti strategici a velocità elevate, è importante disporre di dati per supportare e guidare quelle decisioni chiave che forniscono campagne mirate e mirate. Alcuni clienti, inoltre, svolgono attività di marketing a livello dei segmenti di clientela, sia all&#39;interno che all&#39;esterno di Marketo Engage. Per supportare queste diverse attività, Marketo ha creato la possibilità di acquisire grandi volumi di dati in tempo quasi reale attraverso i flussi di dati.

Oltre ai vantaggi derivanti dall&#39;utilizzo di dati quasi in tempo reale, esistono anche altri vantaggi correlati ai prodotti:

- Allevia il collo di bottiglia dei limiti API perché viene utilizzato lo streaming.
- Riduce lo scenario dei limiti API, generando meno messaggi di avviso.
- Non è necessario eseguire esportazioni in blocco per estrarre i dati a causa della funzionalità di streaming dei dati.

I flussi di dati sono disponibili per coloro che hanno acquistato un [pacchetto livello prestazioni Marketo Engage](https://nation.marketo.com/t5/product-documents/marketo-engage-performance-tiers/ta-p/328835).

## Panoramica del flusso di dati dell’attività del lead

Il flusso di dati dell’attività del lead fornisce uno streaming quasi in tempo reale delle attività del lead di tracciamento dei controlli, in cui grandi volumi di attività possono essere inviati al sistema esterno di un cliente. I flussi consentono ai clienti di controllare in modo efficace gli eventi correlati ai lead, i modelli di utilizzo, fornire visualizzazioni sulle modifiche apportate ai lead e attivare processi e flussi di lavoro in base ai diversi tipi di eventi lead. Esistono più di 144 tipi di attività che possono essere sottoscritti e inviati tramite il flusso.

Tipi di dati lead trasmessi:

1. Modifiche lead: tutte le modifiche su tutti i campi e sui nuovi lead
1. Attività lead: tutti i tipi di attività lead descritti nel documento
1. Lead eliminati
1. Tutti gli oggetti personalizzati sul lead (se richiesto). Al momento è tutto o niente.

Fornendo visualizzazioni sulle modifiche dei lead, i clienti possono prendere decisioni più rapide sulle strategie di marketing generali e creare campagne più mirate. Alcuni casi d’uso comuni sono:

- Avvisi personalizzati: se vengono trovati lead con condizioni non coerenti, è possibile aggiungerli all’elenco. I flussi di attività possono selezionarli e inviare l’attività &quot;Aggiungi all’elenco&quot; per i clienti a qualsiasi azione successiva.
- Potenziare i modelli ML: alcuni clienti pianificano di creare modelli di punteggio che utilizzano gli approfondimenti dell’attività e li inviano a Marketo o li utilizzano nei propri modelli di punteggio interni, a seconda delle esigenze. Assegnando un punteggio a un lead, i clienti possono informare Marketo se desiderano aggiungere nuovi clienti alle campagne Nurture per aumentarne il punteggio.

Elenco delle attività in streaming:

| AchieveGoalInReferral | ClickPredictiveContent | ReceivedForwardToFriendEmail |
|--- |--- |--- |
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

Se si desidera inviare in streaming gli oggetti personalizzati, è necessario che si tratti di tutti gli oggetti personalizzati correlati al lead. Al momento non è possibile selezionare quelli desiderati.

## Panoramica del flusso di dati di controllo dell’utente

Il flusso di dati di audit dell’utente fornisce il tracciamento in tempo reale delle modifiche apportate dagli utenti alle risorse&#x200B;. Questo consente a un cliente di controllare in modo efficace gli eventi delle risorse, fornire una panoramica delle modifiche apportate dagli utenti e attivare processi o flussi di lavoro in base a diversi tipi di eventi di audit. Le modifiche alle risorse quasi in tempo reale vengono inviate tramite eventi Adobe I/O a un endpoint configurabile. Gli eventi di audit sono suddivisi per tipo di risorsa e possono abbonarsi a eventi di audit che sono importanti per loro.

Un caso d’uso appropriato per l’abbonamento a questo flusso è:

- Tracciamento delle modifiche quando si utilizzano più sistemi di marketing: alcuni clienti eseguono anche un certo livello di attività di marketing in un altro sistema, ad esempio un sistema di gestione delle relazioni con i clienti come Salesforce, e quindi passano il lead a Marketo. Il lead a volte viene aggiornato e sincronizzato avanti e indietro, quindi è importante tenere traccia di quale sistema ha apportato modifiche recenti.

Elenco degli eventi di controllo degli utenti in streaming:

| COMPONENTE | ELENCO TIPI DI EVENTO |
|--- |--- |
| Programma predefinito | clona, crea, elimina, modifica canale, esporta, modifica configurazione programma, modifica token programma, rinomina |
| E-mail | approva, clona, crea, elimina, modifica, sposta, rinomina, annulla approvazione |
| Programma batch e-mail | approva, childUpdate, clona, crea, elimina, modifica, modifica canale, modifica pianificazione programma, modifica configurazione programma, modifica token programma, rinomina, annulla approvazione |
| Modello e-mail | approva, clona, crea, elimina, bozzaCrea, bozzaElimina, modifica, rinomina, annulla approvazione |
| Programma coinvolgimento | clona, crea, elimina, modifica canale, modifica configurazione programma, modifica flusso programma, modifica token programma, rinomina |
| Programma evento | clonare, creare, eliminare, modificare il canale, modificare la pianificazione del programma, modificare la configurazione del programma, modificare il token del programma, rinominare |
| Cartella | creare, eliminare, modificare, rinominare |
| Modulo | approvare, clonare, creare, eliminare, bozzaCreare, modificare, spostare, rinominare |
| Modulo -> Modulo pagina di destinazione | creare, clonare, modificare, eliminare, approvare, rinominare |
| Landing Page | approva, clona, crea, elimina, bozzaElimina, modifica, rinomina, annulla approvazione |
| Modello per pagina di destinazione | approva, clona, crea, elimina, bozzaCrea, bozzaElimina, modifica, rinomina, annulla approvazione |
| Elenco avanzato | clona, crea, elimina, modifica, esporta, modifica impostazione elenco avanzato, rinomina |
| Cartella di marketing | creare, modificare, eliminare |
| Programma di sviluppo | clona, crea, elimina, modifica canale, modifica configurazione programma, modifica flusso programma, modifica token programma, rinomina |
| Segmento | creare, eliminare, modificare, rinominare |
| Segmentazione | approvare, creare, eliminare, bozzaCreata, bozzaDiscardata, rinominare, non approvare |
| Campagna avanzata | interrompi, attiva, clona, crea, disattiva, elimina, modifica, modifica pianificazione campagne, modifica azione passaggio flusso, modifica impostazione elenco avanzato, sposta, rinomina |
| Frammento | approva, approva senza bozza, clona, crea, elimina, modifica, rinomina, annulla approvazione |
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

Attualmente, il centro notifiche in Marketo può essere configurato per inviare notifiche a un indirizzo e-mail. Flusso dati di notifica consente di inviare le notifiche direttamente a un endpoint configurabile tramite eventi di Adobe I/O. Le notifiche vengono fornite oggi tramite l’interfaccia utente e possono essere referenziate dalla campana arancione in alto a destra dello schermo e questo flusso prende tali notifiche e le invia tramite un flusso.

Elenco degli eventi di notifica:

| COMPONENTE | ELENCO TIPI DI EVENTO |
|--- |--- |
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

In questa sezione vengono fornite istruzioni su cosa è necessario, come connettersi e ricevere dati in streaming per ciascuno dei flussi. Per ciascuno di essi è necessario un certo livello di codifica e configurazione.

### Flusso dati attività lead

Il flusso Attività lead fornisce uno streaming quasi in tempo reale degli eventi Attività lead di Marketo e invia le modifiche del tipo di attività sottoscritte con attributi configurabili:

- Per impostazione predefinita, la frequenza dei dati viene inviata ogni 2 secondi.
- Batch da 100 a 500 per abbonamento.
- Il timeout per il servizio REST del cliente è di 20 secondi con 3 tentativi ogni 3 minuti e viene abilitato automaticamente in caso di esito positivo. In caso contrario, vengono messe in pausa. Una volta sospeso, il servizio tenta di riattivarlo ogni 3 minuti, a meno che non venga eseguito manualmente il deprovisioning.
- Conservazione dei dati in coda per un massimo di 7 giorni.

Per implementare il flusso di dati dell’attività del lead, i clienti devono seguire questi passaggi:

1. Esporta un endpoint HTTP che può ricevere richieste POST con un corpo JSON dalla rete Internet pubblica. Il flusso di dati push dell’attività invia richieste a:
1. Fornisci ad Adobe quanto segue:
   1. Marketo Munchkin ID per la sottoscrizione
   1. URL dell&#39;endpoint nel passaggio 1
   1. I tipi di attività che desiderano ricevere (elenco completo sopra riportato)
   1. Un mezzo di autenticazione che consenta al cliente di verificare la legittimità delle richieste. Oppure:
      1. Un URL del provider di identità, un ID client e un segreto client per OAuth [Autenticazione credenziali client](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/)
      1. Un token API, che può essere incluso nelle richieste inviate dallo stream di dati dell’attività lead in un’intestazione http di autorizzazione

Adobe abilita quindi il flusso di dati, nel qual caso i clienti iniziano a ricevere i dati.

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

Un esempio di codice per un&#39;applicazione che utilizza il flusso di dati dell&#39;attività lead di Marketo è disponibile [qui](https://github.com/ihgrant/activity-stream-consumer-example).

### Flusso dati di controllo utente e flusso dati di notifica

Gli eventi di User Audit vengono inviati ad Adobe IO e possono essere utilizzati effettuando l’accesso con un Adobe ID. Di seguito sono riportati i passaggi da seguire:

1. I clienti forniscono ad Adobe quanto segue:
   1. Adobe ID
   1. Marketo Munchkin ID per la sottoscrizione
1. Il cliente espone un endpoint REST per utilizzare gli eventi normalmente sotto forma di webhook.
1. Una volta fornito, Adobe abilita il flusso per l’abbonamento del cliente.
1. Il cliente imposta quindi il flusso in Adobe IO (istruzioni da fornire)
   1. Questo passaggio richiede un’organizzazione Adobe
   1. Richiede che l’utente dell’organizzazione Adobe abbia il ruolo di sviluppatore o amministratore di sistema

Per configurare Adobe IO, vedere [Configurazione dei flussi di dati di controllo degli utenti Marketo con Adobe IO](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-user-audit-data-stream-setup/) nella sezione Documentazione pubblica.

### Configurazione del flusso di dati di controllo degli utenti in Marketo

Il flusso di dati di audit utente è attualmente disponibile come parte dei pacchetti di prestazioni insieme agli altri 3 flussi di dati. Per ulteriori informazioni sui pacchetti, fare riferimento alla [pagina di descrizione del prodotto](https://helpx.adobe.com/legal/product-descriptions/adobe-marketo-engage---product-description.html) per i limiti e le funzionalità del prodotto.

### Configurazione di Adobe I/O

[Consulta la Guida introduttiva a Adobe I/O Events](https://developer.adobe.com/runtime/docs/guides/getting-started/)

Per istruzioni di base su questo caso d&#39;uso, a partire da [console.adobe.io](https://developer.adobe.com/console):

Quando richiesto, selezionare **[!UICONTROL Create New Project]** o **[!UICONTROL Add Event]**.

### Introduzione al nuovo progetto

Per iniziare a utilizzare i servizi Adobe, aggiungi un&#39;API, eventi o runtime, visualizza la [documentazione](https://developer.adobe.com/runtime/docs/).

## Documentazione pubblica

- [Flussi dati Marketo](https://developer.adobe.com/events/docs/guides/using/marketo/marketo-data-streams/)
- [Introduzione ad eventi I/O e webhook di Adobe](https://developer.adobe.com/events/docs/guides/)
- [Blog flussi dati](https://blog.developer.adobe.com/introducing-the-adobe-marketo-engage-data-streams-61198b567fbb)
