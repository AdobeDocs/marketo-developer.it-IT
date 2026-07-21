---
title: Best practice per l’integrazione di Marketo
feature: REST API
description: Best practice per le integrazioni API di Marketo che riguardano quote, limiti di tasso e di concorrenza, batch, importazione ed esportazione in blocco, caching e pianificazione della latenza.
exl-id: 1e418008-a36b-4366-a044-dfa9fe4b5f82
TQID: https://experienceleague.adobe.com/Ld-rmFCwKSx-0W2-ceYICu0FQHK8BKAC1QgqtiOWDn4
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: df401a2a-327d-468c-a5e4-b7b7ccd071a0
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 882
ht-degree: 0%

---

# Best practice per l’integrazione di Marketo

Progetta integrazioni in base ai limiti dell’API condivisa per l’istanza Marketo. Utilizza la gestione degli hit in batch, la memorizzazione in cache e tassi di richiesta prudenti per migliorare la velocità effettiva e l’affidabilità.

## Limiti API

- **Quota giornaliera:** alla maggior parte degli abbonamenti sono assegnate 50.000 chiamate API al giorno. La quota viene ripristinata ogni giorno alle 00:00 CST. Contatta il tuo account manager per aumentare la quota giornaliera.
- **Limite di frequenza:** Ogni istanza è limitata a 100 chiamate API in 20 secondi.
- **Limite di concorrenza:** Ogni istanza consente un massimo di dieci chiamate API simultanee.
- **Dimensione batch:** il database lead supporta 300 record; Asset Query supporta 200 record.
- **Dimensione payload REST API:** 1 MB.
- **Dimensione file di importazione in blocco:** 10 MB.
- **Dimensione batch massima SOAP:** 300 record.
- **Processi di estrazione in blocco:** due in esecuzione e dieci in coda, inclusi.

## Suggerimenti

- Impostare limiti di utilizzo prudenti perché l&#39;applicazione condivide le risorse relative a quote, tassi e concorrenza con altre applicazioni.
- Se disponibili, utilizza i metodi batch e di massa di Marketo. Utilizza le chiamate a record singolo o risultato singolo solo quando necessario.
- Utilizza [backoff esponenziale](https://en.wikipedia.org/wiki/Exponential_backoff) per ritentare le chiamate API che non riescono a causa dei limiti di frequenza o concorrenza.
- Evita le chiamate API simultanee a meno che non vadano a beneficio del tuo caso d’uso.

## Batch

Per gli inserimenti e gli aggiornamenti, raggruppare i record nel minor numero possibile di transazioni. Quando si recuperano record da un archivio dati, aggregarli prima dell’invio anziché inviare una richiesta per ogni modifica.

## Latenza accettabile

Definisci la latenza accettabile (il tempo massimo prima di inviare una chiamata API) durante la progettazione di un’integrazione. Questa scelta determina i metodi e le opzioni di configurazione di Marketo adatti al caso d’uso.

Ad esempio, un’integrazione in tempo reale che invia una notifica al venditore all’avvio di una versione di prova potrebbe inviare batch di una quando è necessario un follow-up immediato. La maggior parte dei casi d’uso tollera una maggiore latenza e funziona in modo più efficiente accodando e batch le chiamate.

| Latenza accettabile | Metodi preferiti | Note |
| --- | --- | --- |
| Basso (&lt;10 secondi) | API sincrone (in batch o non in batch) | Assicurati che il tuo caso d’uso lo richieda. L’invio di chiamate immediate e sincrone per casi di utilizzo di volumi elevati può assorbire rapidamente una quota API giornaliera e potenzialmente superare i limiti di velocità e concorrenza. |
| Medium (10s - 60m) | API sincrone (in batch) | Per le integrazioni di dati in entrata in Marketo, si consiglia vivamente di utilizzare una coda con un limite di età e un limite di dimensioni. Quando viene raggiunto uno di questi limiti, svuota la coda e invia la richiesta API con i record accumulati. Si tratta di un compromesso importante tra velocità ed efficienza, che garantisce che le richieste si verifichino alla frequenza richiesta, mantenendo in batch il numero di record consentito dall’età della coda. |
| Alta(>60 m) | Importazione/esportazione in blocco (se supportata) | Per le integrazioni di dati in entrata, i record devono essere messi in coda e inviati tramite API Marketo Bulk, se disponibili. |

## Limiti giornalieri

Ogni istanza di Marketo abilitata per le API dispone di un’allocazione giornaliera di almeno 10.000 chiamate REST API, anche se 50.000 o più sono comuni. Ogni istanza dispone anche di 500 MB o più di capacità di estrazione in blocco. È possibile acquistare ulteriore capacità giornaliera come parte di un abbonamento Marketo, ma le progettazioni delle applicazioni devono tenere conto dei limiti comuni di abbonamento.

La capacità è condivisa da tutti i servizi API e dagli utenti in un’istanza. Eliminare le chiamate ridondanti e i record batch nel minor numero possibile di chiamate.

Il metodo di importazione più efficiente per le chiamate è l&#39;API di importazione in blocco Marketo, disponibile per [lead/persone](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) e [oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi#tag/Snippets/operation/createSnippetUsingPOST). Marketo fornisce anche l&#39;estrazione in blocco per [lead](bulk-lead-extract.md) e [attività](bulk-activity-extract.md).

### Memorizzazione in cache

I risultati delle seguenti operazioni possono in genere essere memorizzati nella cache lato client per un giorno o più, in quanto vengono modificati raramente:

- Risultati delle operazioni Describe
- [Tipi di attività](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partizioni](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/getLeadPartitionsUsingGET)

Per casi d’uso come l’arricchimento dei dati di lead o attività, puoi anche memorizzare in cache tipi di risorse come programmi, e-mail e cartelle.

## Limite di velocità

Ogni istanza di Marketo ha un limite di velocità condivisa di 100 chiamate ogni 20 secondi in tutti i servizi API di terze parti. Se le chiamate superano questo limite, l’API restituisce un codice di errore 606.

In generale, limita ogni integrazione di terze parti a 50 chiamate per 20 secondi o meno, in modo che più integrazioni API e utenti possano condividere la capacità disponibile. Per alcuni casi di utilizzo potrebbe essere necessario il limite completo. Tuttavia, le applicazioni che utilizzano la gestione in batch e il throughput di destinazione sono generalmente più reattive e coerenti, con un lieve aumento della latenza.

## Limite concorrenza

Ogni istanza di Marketo ha un limite condiviso di dieci chiamate API REST in esecuzione simultanea. Non presumere che l’applicazione sia l’unico consumatore di questo limite.

Marketo conta le chiamate in elaborazione che non sono ancora state restituite. Quando viene restituita una chiamata, questa non viene più conteggiata nel limite di concorrenza.

La maggior parte delle integrazioni non beneficia di chiamate simultanee. Se implementi la concorrenza, limita inizialmente l’applicazione a cinque o meno richieste simultanee. Aumentare il limite solo dopo aver stabilito che l&#39;applicazione richiede di più.

## Errori

Tranne in rari casi, le richieste API restituiscono il codice di stato HTTP 200. Anche gli errori di logica di business restituiscono 200, ma includono i dettagli nel corpo della risposta. Per ulteriori informazioni, vedere [Codici errore](error-codes.md).

Non valutare la frase del motivo HTTP perché è facoltativa e soggetta a modifiche.
