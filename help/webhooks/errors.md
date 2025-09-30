---
title: Errori
feature: Webhooks
description: Scopri i codici di errore del webhook di Marketo, il motivo per cui sono necessarie risposte 2xx per aggiornare i campi del lead e come rilevare e gestire gli errori con Webhook è chiamato.
exl-id: adce40c3-87b1-4f31-8995-eb64e8a72b55
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 1%

---

# Errori

In questa pagina sono elencati i codici di risposta di errore per i webhook in Marketo.

1000 e 1001 sono generati da Marketo e da 2xx a 5xx sono errori restituiti dal sistema chiamato dal webhook di Marketo.

Affinché Marketo possa mappare nuovamente i valori in un campo, il codice di risposta del webhook deve essere del tipo 2xx. Se l’obiettivo del webhook è quello di modificare i valori nel record del lead di Marketo tramite la risposta, il servizio Web chiamato deve restituire 2xx, tutti gli altri codici di risposta si tradurranno nell’ignorare il webhook allo scopo di aggiornare i valori dei record del lead.

| Codice di risposta | Descrizione |
| --- | --- |
| 1000 | Questo indica che l’azione di flusso &quot;Chiama webhook&quot; si trova all’interno di una campagna batch. I webhook possono essere attivati solo da campagne trigger. |
| 1001 | Questo indica che il servizio web ha emesso un corpo di risposta vuoto. |

## Rilevamento di un errore del webhook

Gli errori dai webhook possono essere rilevati e gestiti dal trigger [!UICONTROL Webhook is Called]:

![Webhook chiamato](assets/webhook-called.png)

* Response - Response è il payload di risposta letterale ricevuto dalla richiesta.
* Tipo di errore: corrisponde alla frase-motivo del messaggio di stato HTTP.

Questi possono essere utilizzati per gestire errori ed eccezioni prevedibili e reagire ad essi. A seconda del servizio con cui si sta effettuando l’integrazione, è possibile ripristinare automaticamente alcune classi di errori, mentre è possibile creare avvisi per avvisare gli utenti di errori imprevisti.
