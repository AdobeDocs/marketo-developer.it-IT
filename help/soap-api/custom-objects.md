---
title: Oggetti personalizzati
feature: SOAP
description: Scopri come gli oggetti personalizzati di Marketo collegano un lead a molti record, con struttura, limiti e chiamate API di SOAP per ottenere, sincronizzare, eliminare più l’utilizzo di elenchi avanzati e e-mail.
exl-id: 29d65841-4b44-4d94-b14e-c583d433d015
TQID: https://experienceleague.adobe.com/Fy3h8qfKs8gizeakXkhoj5mJhib-QA2kYOu41YMIEZs
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 286
ht-degree: 1%

---

# Oggetti personalizzati

Un oggetto personalizzato di Marketo consente di creare una relazione uno a molti tra i lead di Marketo e i record oggetto personalizzati. Ad esempio, potrebbe essere utile tenere traccia di tutti i roadshow a cui partecipa un lead. Poiché i lead possono partecipare a diversi eventi itineranti (nell&#39;arco di più anni), gli oggetti personalizzati si prestano in modo particolare alla memorizzazione di queste informazioni.

Gli oggetti personalizzati sono supportati su tutte le edizioni di Marketo ad eccezione di Spark. Una volta eseguito correttamente il provisioning dell’oggetto personalizzato Marketo nell’account, puoi

- Creare/leggere/aggiornare/eliminare record nell’oggetto personalizzato tramite API SOAP di Marketo
- Utilizza il trigger elenco avanzato quando vengono aggiunti nuovi record all’oggetto personalizzato
- Utilizzare i dati oggetto personalizzati come filtro negli elenchi avanzati
- Utilizzare i dati oggetto personalizzati nelle e-mail utilizzando Marketo Email Scripting

## Struttura degli oggetti personalizzati

Gli oggetti personalizzati sono costituiti da:

- Un piccolo insieme di attributi fissi comuni a tutti gli oggetti personalizzati:
   - Nome oggetto (nome del tipo di oggetto)
   - Descrizione oggetto
   - Nome campo collegamento oggetto personalizzato a lead Marketo: campo sull&#39;oggetto lead (persona) a cui fa riferimento l&#39;oggetto personalizzato
   - Nome campo chiave oggetto: la chiave primaria utilizzata dall&#39;oggetto
- Uno o più campi specifici dell’oggetto (sono supportati al massimo 50 campi di questo tipo)

## Operazioni per oggetti personalizzati

Le seguenti chiamate possono essere utilizzate per interagire con i CO.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
