---
title: Guida introduttuva
description: Guida introduttiva alle API e al modello dati di Marketo Engage, inclusi lead, attività, programmi, tag, elenchi, linee guida REST e avvisi di rimozione da SOAP.
exl-id: 78c44c32-4e59-4d55-a45c-ef0d7dac814d
TQID: https://experienceleague.adobe.com/0lfzor5EQJ0VqIh4fqlK29OiPmRCy6fnEtncJ38r-OM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c954475c-8548-4e33-a0b8-6b550d956115
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1225
ht-degree: 2%

---

# Guida introduttuva

Marketo Engage è una piattaforma di automazione del marketing per la gestione di programmi e campagne multicanale personalizzati per clienti potenziali e potenziali. Puoi estendere la piattaforma attraverso i relativi punti di integrazione.

Questa pagina presenta le entità Marketo Engage di base e le relative relazioni.

>[!NOTE]
>
>L’API SOAP diventerà obsoleta e non sarà più disponibile dopo il 31 luglio 2026. Utilizza l&#39;API REST [REST](./rest-api/rest-api.md) di Marketo per tutti i nuovi sviluppi. Eseguire la migrazione dei servizi esistenti entro tale data per evitare interruzioni del servizio. Se un servizio utilizza l&#39;API SOAP, vedere la [Guida alla migrazione dell&#39;API SOAP](./soap-api/migration.md).
>

Quando la connessione SFDC nativa o MS Dynamics CRM è abilitata in un&#39;istanza di Marketo Engage, questi oggetti sono di sola lettura:

- Azienda
- Opportunità
- Ruolo opportunità
- Venditore

![Modello dati](assets/data_model.png)

## Persona (lead)

Le persone sono la base dell’automazione del marketing. Marketo fa riferimento a tutti i record di persone non di vendita come lead, indipendentemente dal fatto che le vendite li considerino lead, potenziali clienti, sospetti o contatti.

L’oggetto lead include campi standard come e-mail, nome e cognome. È possibile aggiungere campi per memorizzare altre informazioni e leggere e scrivere attributi personalizzati nello stesso modo dei campi standard. Trovare l&#39;elenco completo dei campi in **[!UICONTROL Admin]** > **[!UICONTROL Field Management]** in Marketo.

Marketo identifica i lead in modo univoco in base al campo id. È necessario applicare altre chiavi univoche all&#39;esterno del sistema.

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads), [JavaScript](javascript-api/lead-tracking.md#lead-tracking-api)

## Attività

I lead possono interagire con l’organizzazione in diversi modi, ad esempio visitando una pagina web, partecipando a una fiera o scaricando un white paper. Marketo acquisisce queste azioni come attività in modo che gli esperti di marketing possano capire cosa ha fatto e quando si è verificato un lead.

Le attività sono sempre correlate ai lead per leadId.

Puoi anche definire attività personalizzate. Dopo aver creato e pubblicato un’attività personalizzata, puoi aggiungerne delle istanze tramite l’API Marketo. Per ulteriori informazioni, vedere [Informazioni sulle attività personalizzate](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/marketo-custom-activities/understanding-custom-activities).

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities), [JavaScript](javascript-api/lead-tracking.md#munchkin-behavior)

## Programmi e campagne

Un programma organizza le attività di marketing di un addetto al marketing in un’unica posizione. Ad esempio, un’e-mail esplosiva può essere un programma.

Un lead può intraprendere più azioni o attività associate a un programma. Questo processo è noto come progressione del lead. Per un programma di e-mail blast, la progressione può registrare quando Marketo invia l’e-mail, quando la persona la apre e se fa clic su un collegamento.

Una campagna ha uno scopo e un obiettivo specifico all’interno di un programma. Ad esempio, una campagna può selezionare un gruppo di lead e inviare un messaggio e-mail esplosivo. Un’altra campagna può inviare una notifica a un rappresentante commerciale quando un lead fa clic su un collegamento nell’e-mail blast.

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Campaigns)

## Tag

Raggruppa i tag e categorizza i dati del programma per il reporting. Utilizza i tag per misurare l’efficacia del programma e il ROI.

In qualità di amministratore di Marketo, puoi creare tipi di tag obbligatori e facoltativi selezionati dagli utenti al momento della creazione di un programma. Puoi definire i possibili valori per ogni tipo di tag in base ai requisiti di reporting della tua azienda.

Ad esempio, crea un tipo di tag personalizzato &quot;Region&quot; con valori quali Nordest e Southeast per analizzare quale regione genera il maggior numero di lead. Crea un tipo di tag &quot;Proprietario&quot; per confrontare quali proprietari del programma, come Maria, David o John, hanno il maggiore impatto sulla creazione di lead e opportunità. Per ulteriori informazioni, vedere [Informazioni sui tag](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/core-marketo-concepts/programs/working-with-programs/understanding-tags).

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/asset)

## Elenchi

Elenca l&#39;organizzazione di raccolte di lead. Marketo offre due tipi di funzionalità:

- Un elenco statico è una raccolta fissa da cui un addetto marketing può aggiungere o rimuovere lead.
- Un elenco avanzato è una raccolta dinamica basata su caratteristiche definite.

Ad esempio, un elenco avanzato denominato &quot;Tutti i lead che hanno visitato la pagina dei prezzi sul nostro sito web&quot; continua a crescere man mano che altri lead visitano tale pagina. Per ulteriori informazioni, consulta la [documentazione di Marketo Engage](https://experienceleague.adobe.com/it/docs/marketo/using/home).

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/asset#tag/Static-Lists)

## Opportunità

Un&#39;opportunità rappresenta un potenziale affare di vendita che gli addetti al marketing consegnano alle vendite. In Marketo, un’opportunità è associata a un lead o a un contatto e a un’organizzazione.

Un ruolo opportunità collega un lead a un&#39;organizzazione e descrive la funzione del lead in tale organizzazione.

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Opportunities)

## Aziende

Un’organizzazione, a volte denominata account in Marketo, è l’organizzazione a cui appartiene una persona.

Per un’attribuzione precisa del ROI nei rapporti sul ROI di Marketo o nell’analisi del ciclo di fatturato (RCA), associa le persone alle loro organizzazioni e opportunità.

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

## Risorse

Assets include pagine di destinazione, e-mail, moduli e immagini utilizzati in un programma. Una risorsa può essere locale per un programma specifico o globale. Le risorse globali sono disponibili per ogni programma.

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/asset)

## Token

I token consentono agli addetti al marketing di personalizzare i messaggi con le risorse e aggiungere logica alle azioni di flusso. Marketo fornisce token per il sistema generale, i programmi, i lead e le aziende.

Inserire ad esempio il token lead `{{lead.First Name}}` in un messaggio e-mail per visualizzare il nome del lead.

I token definiti a livello di programma o cartella sono denominati &quot;I miei token&quot; in Marketo. I miei token hanno tre tipi:

- Locale: creato in una cartella di campagna specifica o in un programma e disponibile solo in tale cartella o programma.
- Ereditato: creato a livello di cartella della campagna e disponibile per tutti i programmi in tale cartella.
- Sostituito: modificato con un valore personalizzato a livello di programma senza modificare il valore padre Il mio token a livello di cartella del programma.

I miei token utilizzano la convenzione di denominazione `{{my.My Token}}`, con la parola &quot;my&quot; all&#39;inizio del nome del token. Ad esempio, un tipo di data My Token denominato EventDate ha il nome di token `{{my.EventDate}}`. Per ulteriori informazioni, vedere [Informazioni sui token in un programma](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program).

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens)

## Oggetti personalizzati

Un oggetto personalizzato di Marketo crea una relazione uno-a-molti o molti-a-molti (Edge-Bridge-Edge) tra i lead di Marketo e i record oggetto personalizzati.

Dopo aver creato e pubblicato un oggetto personalizzato di Marketo, puoi eseguirvi operazioni CRUD tramite l’API Marketo. Quando vengono aggiunti nuovi record, è possibile utilizzare un trigger di elenco avanzato per rispondere. È inoltre possibile utilizzare i dati oggetto personalizzati come filtro elenco avanzato per la segmentazione o nelle e-mail tramite [script e-mail](email-scripting.md). Per ulteriori informazioni sulla creazione di oggetti personalizzati, vedere la [documentazione di Marketo Engage](https://experienceleague.adobe.com/it/docs/marketo/using/home).

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects)

## Venditori

È possibile gestire i record Persona di vendita e le relazioni lead in Marketo quando non è abilitata alcuna integrazione CRM nativa. Questi record contengono informazioni quali Nome, E-mail e Qualifica. Quando un venditore è il proprietario di un lead, è possibile utilizzare queste informazioni per filtrare e token.

Gestire la relazione con un venditore a livello di lead tramite il campo &quot;externalSalesPersonId&quot;. Aggiorna questo campo tramite l&#39;API [Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST).

API correlate: [REST](https://developer.adobe.com/marketo-apis/api/mapi#tag/Sales-Persons)
