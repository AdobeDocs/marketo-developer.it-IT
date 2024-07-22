---
title: Tipi di campi
feature: REST API
description: Elenco di tipi di campi di Marketo
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 7%

---

# Tipi di campi

Segue una descrizione dei tipi di campo in Marketo. Ulteriori informazioni sui tipi di campo sono disponibili [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Ulteriori informazioni sui limiti dei tipi di campo sono disponibili [qui](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents).

| Tipo di campo | Descrizione | Esempio |
| --- | --- | --- |
| Data e ora | Utilizzato per immettere una data e un’ora. Segue il formato [W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). La best practice prevede di includere sempre lo scostamento del fuso orario. Data completa più ore e minuti: YYYY-MM-DDThh:mm:ssTZD dove TZD è &quot;+hh:mm&quot; o &quot;-hh:mm&quot; Nota: alcune API di Asset restituiscono &quot;Z+0000&quot; come TZD per updatedAt e createdAt. | 05/05/2010 T15:41:32-05:00 |
| E-mail | Campo stringa che accetta indirizzi e-mail | example@example.com |
| Mobile | Campo numerico che contiene numeri reali e può utilizzare una cifra decimale. | 10,4 |
| Intero | Numero intero | 10 |
| Formula | Campi i i cui valori vengono generati manipolando i dati di altri campi presenti in un record lead. Non vengono esportati e non possono essere utilizzati in una campagna avanzata. | Vedi questo [articolo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Percentuale | Una percentuale espressa come numero intero | 30 |
| URL | Campo di testo che limita l’input agli URL, incluso il protocollo dell’URL. | http://example.com/ |
| Telefono | Numero di telefono | 111-111-1111 |
| Area di testo | Testo più lungo. | Supporta fino a 30.000 byte. I caratteri ASCII standard utilizzano 1 byte per carattere (fino a 30.000 caratteri). I caratteri Unicode possono utilizzare fino a 4 byte per carattere (riducendo il  numero di caratteri consentito inferiore a 30.000). |
| Stringa | Testo più breve (fino a 255 caratteri) | Lorem ipsum dolor sit amet |
| Punteggio | Un campo intero che può essere manipolato con il passaggio di flusso Change Score | 10 |
| Booleano (precedentemente casella di controllo) | Consente agli utenti di selezionare un valore True (selezionato) o False (non selezionato). | Vero |
| Valuta | Campo float che rappresenta il tipo di valuta predefinito selezionato per l’abbonamento Marketo | 10,40 |
| Data | Utilizzato per la data. Segue il formato W3C. | 07/05/2010 |
| Riferimento | Campo stringa contenente una chiave di un altro record (chiave esterna). | Contatta l&#39;azienda |
