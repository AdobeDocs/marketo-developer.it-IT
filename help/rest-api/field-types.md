---
title: Tipi di campi
feature: REST API
description: Elenco completo dei tipi di campo Marketo con definizioni, esempi e formati, inclusi datetime ISO 8601, limiti dell'area di testo, valuta e booleano.
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
TQID: https://experienceleague.adobe.com/Q-L1NCCS1caYip-niSrBAkp6k37ErzmsLCFvn7fRJW0
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: c5f60233-d5ea-4453-a799-0ad258b4d399id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2: id: ad89fb33-8541-4339-afe7-bb13d1633714
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 373
ht-degree: 8%

---

# Tipi di campi

Segue una descrizione dei tipi di campo in Marketo. Ulteriori informazioni sui tipi di campo sono disponibili [qui](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary). Ulteriori informazioni sui limiti dei tipi di campo sono disponibili [qui](https://nation.marketo.com/t5/knowledgebase/marketo-field-limits-by-field-type/ta-p/251613).

| Tipo di campo | Descrizione | Esempio |
| --- | --- | --- |
| Data e ora | Utilizzato per immettere una data e un’ora. Segue il formato [W3C](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). Si consiglia di includere la compensazione del fuso orario. Data di completamento più ore e minuti: YYYY-MM-DDThh:mm:ssTZD dove TZD è &quot;+hh:mm&quot; o &quot;-hh:mm&quot; Nota: alcune API Asset restituiscono &quot;Z+0000&quot; come TZD per `updatedAt` e `createdAt`. | 05/05/2010 T15:41:32-05:00 |
| E-mail | Campo stringa che accetta indirizzi e-mail | <example@example.com> |
| Mobile | Campo numerico che contiene numeri reali e può utilizzare una cifra decimale. | 10,4 |
| Intero | Numero intero | 10 |
| Formula | Campi i i cui valori vengono generati manipolando i dati di altri campi presenti in un record lead. Non vengono esportati e non possono essere utilizzati in una campagna avanzata. | Vedi questo [articolo](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Percentuale | Una percentuale espressa come numero intero | 30 |
| URL | Campo di testo che limita l’input agli URL, incluso il protocollo dell’URL. | <http://example.com/> |
| Telefono | Numero di telefono | 111-111-1111 |
| Area di testo | Testo più lungo. | Supporta fino a 30.000 byte. I caratteri ASCII standard utilizzano 1 byte per carattere (fino a 30.000 caratteri). I caratteri Unicode possono utilizzare fino a 4 byte per carattere (riducendo il numero di caratteri consentito a meno di 30.000 caratteri). |
| Stringa | Testo più breve | Testo fino a 255 caratteri |
| Punteggio | Un campo intero che può essere manipolato con il passaggio di flusso Change Score | 10 |
| Booleano (precedentemente casella di controllo) | Consente agli utenti di selezionare un valore True (selezionato) o False (non selezionato). | True |
| Valuta | Campo float che rappresenta il tipo di valuta predefinito selezionato per l’abbonamento Marketo | 10,40 |
| Data | Utilizzato per la data. Segue il formato W3C. | 2010-05-07 |
| Riferimenti | Campo stringa contenente una chiave di un altro record (chiave esterna). | Contatta l&#39;azienda |
