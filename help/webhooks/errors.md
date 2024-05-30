---
title: "Errori"
feature: Webhooks
description: "Codici di errore per i webhook"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '232'
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

Gli errori dei webhook possono essere rilevati e gestiti dal [!UICONTROL Webhook is Called] trigger:

![Chiamata del webhook](assets/webhook-called.png)

* Response - Response è il payload di risposta letterale ricevuto dalla richiesta.
* Tipo di errore: corrisponde alla frase-motivo del messaggio di stato HTTP.

Questi possono essere utilizzati per gestire errori ed eccezioni prevedibili e reagire ad essi. A seconda del servizio con cui si sta effettuando l’integrazione, è possibile ripristinare automaticamente alcune classi di errori, mentre è possibile creare avvisi per avvisare gli utenti di errori imprevisti.
