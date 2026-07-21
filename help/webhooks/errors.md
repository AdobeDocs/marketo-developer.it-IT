---
title: Errori
feature: Webhooks
description: Scopri i codici di errore del webhook di Marketo, il motivo per cui sono necessarie risposte 2xx per aggiornare i campi del lead e come rilevare e gestire gli errori con Webhook è chiamato.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
TQID: https://experienceleague.adobe.com/N2jNA4EUMMTUFL9uJHZhOor6Tlz4-EXWciwoXrPml48
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 210
ht-degree: 2%

---

# Errori

Questa pagina descrive i codici di risposta di errore per i webhook di Marketo e spiega come gestire gli errori dei webhook.

Marketo genera i codici di errore 1000 e 1001. Il sistema chiamato dal webhook Marketo restituisce codici di risposta da 2xx a 5xx.

Marketo mappa i valori di risposta su un campo solo quando il servizio web restituisce un codice di risposta 2xx. Se una risposta del webhook ha lo scopo di modificare i valori in un record lead di Marketo, tutti gli altri codici di risposta fanno in modo che Marketo ignori la risposta per gli aggiornamenti dei campi.

| Codice di risposta | Descrizione |
| --- | --- |
| 1000 | Questo indica che l’azione di flusso &quot;Chiama webhook&quot; si trova all’interno di una campagna batch. I webhook possono essere attivati solo da campagne trigger. |
| 1001 | Questo indica che il servizio web ha emesso un corpo di risposta vuoto. |

## Rilevamento di un errore del webhook

Utilizza il trigger **[!UICONTROL Webhook is Called]** per rilevare e gestire gli errori del webhook:

![Webhook chiamato](assets/webhook-called.png)

* **Risposta** - Payload di risposta letterale ricevuto dalla richiesta.
* **Tipo di errore**: la frase-motivo del messaggio di stato HTTP.

Utilizzare questi valori per rispondere a errori ed eccezioni prevedibili. A seconda del servizio integrato, è possibile ripristinare automaticamente da alcune classi di errore e creare avvisi in caso di errori imprevisti.
