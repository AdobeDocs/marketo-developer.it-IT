---
title: "Best practice per l’integrazione di Marketo"
feature: REST API
description: "Best practice per l’utilizzo delle API di Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---


# Best practice per l’integrazione di Marketo

## Limiti API

- **Quota giornaliera:** Alla maggior parte degli abbonamenti vengono assegnate 50.000 chiamate API al giorno (che vengono ripristinate ogni giorno alle 00.00 CST). Puoi aumentare la tua quota giornaliera tramite il tuo account manager.
- **Limite tariffa:** L’accesso API per istanza è limitato a 100 chiamate per 20 secondi.
- **Limite concorrenza:**  Massimo dieci chiamate API simultanee.
- **Dimensione batch:** DB lead - 300 record; Query risorse - 200 record
- **Dimensione payload REST API:** 1 MB
- **Dimensione file importazione in blocco:** 10 MB
- **Dimensione massima batch SOAP:** 300 record
- **Processi estrazione in blocco:** 2 in esecuzione; 10 in coda (incluso)

## Suggerimenti

- Si supponga che l&#39;applicazione concorrerà per le risorse di quota, tasso e concorrenza con altre applicazioni e che vengano impostati limiti di utilizzo prudenti.
- Utilizza i metodi in blocco e batch di Marketo quando disponibili e appropriati. Se necessario, utilizza solo un record singolo o singole chiamate di risultati.
- Utilizzare [backoff esponenziale](https://en.wikipedia.org/wiki/Exponential_backoff) per ritentare le chiamate API che non riescono a causa di limiti di frequenza o concorrenza.
- Evita di effettuare chiamate API simultanee se il tuo caso d’uso non ne trae vantaggio.

## Batch

Per garantire le migliori prestazioni per le integrazioni, durante l&#39;esecuzione di inserimenti o aggiornamenti, i record devono essere raggruppati nel minor numero possibile di transazioni. Quando si recuperano record da un archivio dati per l&#39;invio, è sempre necessario aggregare i record prima dell&#39;invio, anziché inviare una richiesta per ogni singola modifica.

## Latenza accettabile

Determinando le tolleranze di latenza, o il tempo massimo che può trascorrere prima di inviare una chiamata API, informerai molte, se non la maggior parte, delle decisioni che prendi durante la progettazione dell’integrazione in Marketo. Marketo offre diversi metodi e opzioni di configurazione adatti a casi d’uso diversi e a diverse classi di latenza. Ad esempio, un’integrazione in tempo reale per notificare a un venditore che un utente si è iscritto a una versione di prova potrebbe inviare batch di uno solo se è necessario un follow-up immediato. Tuttavia, la maggior parte dei casi non lo richiede e può tollerare una latenza aggiuntiva e può essere gestita in modo più efficiente tramite chiamate in coda e in batch.

| Latenza accettabile | Metodi preferiti | Note |
|---|---|---|
| Basso (&lt;10 secondi) | API sincrone (in batch o non in batch) | Assicurati che il tuo caso d’uso lo richieda. L’invio di chiamate immediate e sincrone per casi di utilizzo di volumi elevati può assorbire rapidamente una quota API giornaliera e potenzialmente superare i limiti di velocità e concorrenza. |
| Medio (10s - 60m) | API sincrone (in batch) | Per le integrazioni di dati in entrata in Marketo, si consiglia vivamente di utilizzare una coda con un limite di età e un limite di dimensioni. Quando viene raggiunto uno di questi limiti, svuota la coda e invia la richiesta API con i record accumulati. Si tratta di un compromesso importante tra velocità ed efficienza, che garantisce che le richieste si verifichino alla frequenza richiesta, mantenendo in batch il numero di record consentito dall’età della coda. |
| Alta(>60 m) | Importazione/esportazione in blocco (se supportata) | Per le integrazioni di dati in entrata, i record devono essere messi in coda e inviati tramite API Marketo Bulk, se disponibili. |

## Limiti giornalieri

Ogni istanza di Marketo abilitata per le API dispone di un’allocazione giornaliera di almeno 10.000 chiamate REST API al giorno, ma più comunemente 50.000 o più, e 500 MB o più di capacità Bulk Extract. Anche se la capacità giornaliera aggiuntiva può essere acquistata come parte di un abbonamento a Marketo, la progettazione dell’applicazione deve tenere conto dei limiti comuni degli abbonamenti a Marketo.

Poiché in un’istanza la capacità è condivisa tra tutti i servizi API e gli utenti, la best practice prevede di eliminare le chiamate ridondanti e di raggruppare i record nel minor numero possibile di chiamate. Il modo più efficiente per importare i record consiste nell’utilizzare le API di importazione in blocco di Marketo, disponibili per [Lead/Persone](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) e [Oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST). Marketo fornisce anche l’estrazione in blocco per [Lead](bulk-lead-extract.md) e [Attività](bulk-activity-extract.md).

### Memorizzazione in cache

I risultati delle seguenti operazioni possono in genere essere memorizzati nella cache lato client per un giorno o più, in quanto vengono modificati raramente:

- Risultati delle operazioni Describe
- [Tipi di attività](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partizioni](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

La memorizzazione in cache di alcuni tipi di risorse, come programmi, e-mail e cartelle, è appropriata anche per alcuni casi d’uso, come l’arricchimento dei dati per i record di lead o attività.

## Limite di velocità

Ogni istanza di Marketo ha un limite di frequenza di 100 chiamate per 20 secondi, che viene condiviso tra tutti i servizi API di terze parti. Se questo limite viene superato, l’API risponde con un codice di errore 606 che indica che il limite di velocità è stato superato. In generale, le integrazioni di terze parti devono limitare l’utilizzo a 50 chiamate per 20 secondi o meno per consentire un utilizzo corretto dei limiti di velocità da parte di più integrazioni API e utenti. Anche se può essere opportuno saturare questo limite in alcuni casi, in generale, le applicazioni che utilizzano la gestione in batch e indirizzano il loro throughput al di sotto di questo limite sono più reattive e coerenti nel loro funzionamento, con un costo ridotto di maggiore latenza.

## Limite concorrenza

Ogni istanza di Marketo ha un limite condiviso di dieci chiamate API REST in esecuzione simultanea. Analogamente ai limiti giornalieri di quota e tasso, è condiviso, pertanto non si deve presumere che l&#39;applicazione sarà il consumatore esclusivo di questo limite. Marketo conta il numero di chiamate simultanee come quelle che stanno elaborando e che non sono ancora state restituite, quindi quando viene restituita una chiamata, non viene più conteggiato rispetto al limite di chiamate simultanee.

La maggior parte dei casi di utilizzo di integrazione non trae vantaggio dall’effettuare chiamate simultanee, pertanto considera se l’applicazione offre vantaggi prima di decidere di inviare richieste simultanee a Marketo. Se desideri implementare la concorrenza, devi limitare il numero di richieste simultanee a cinque o meno nella progettazione iniziale e aumentarlo solo dopo aver stabilito che l’applicazione richiede di più.

## Errori

Ad eccezione di alcuni rari casi, le richieste API restituiscono il codice di stato HTTP 200. Anche gli errori della logica di business restituiscono un valore 200, ma contengono informazioni dettagliate nel corpo della risposta. Consulta [Codici errore](error-codes.md) per una spiegazione dettagliata. La frase relativa al motivo HTTP non deve essere valutata in quanto è facoltativa e soggetta a modifiche.
