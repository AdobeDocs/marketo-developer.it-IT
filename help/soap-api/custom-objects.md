---
title: "Oggetti personalizzati"
feature: SOAP
description: "Creazione di oggetti personalizzati."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

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

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
