---
title: API SOAP
feature: SOAP
description: L’API SOAP di Marketo è diventata obsoleta dopo il 31 ottobre 2025. Scopri come migrare a REST, recuperare il tuo WSDL, vedere quote, limiti di tariffa e configurazione di autenticazione.
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# API SOAP

L’API SOAP diventerà obsoleta e non sarà più disponibile dopo il 31 ottobre 2025. Tutti i nuovi sviluppi devono essere eseguiti con Marketo [REST API](../rest-api/rest-api.md) e i servizi esistenti devono essere migrati entro tale data per evitare interruzioni del servizio. Se si dispone di un servizio che utilizza l&#39;API SOAP, consultare la [Guida alla migrazione dell&#39;API SOAP](./migration.md) per informazioni su come eseguire la migrazione.

## WSDL SOAP

Per recuperare il documento SOAP WSDL, ottenere l&#39;endpoint API SOAP dal menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]**.

![Endpoint SOAP](assets/endpoint-soap.png)

L&#39;URL WSDL è:

`<SOAP API Endpoint> + ?WSDL`

Non utilizzare il punto finale definito nel file WSDL. Ogni istanza di Marketo ha un endpoint univoco in cui effettuare chiamate a.

## Limiti

- **Quota giornaliera:** alla maggior parte degli abbonamenti sono assegnate 10.000 chiamate API al giorno (che vengono ripristinate ogni giorno a 12:00AM CST). Puoi aumentare la tua quota giornaliera tramite il tuo account manager.
- **Limite di frequenza:** accesso API per istanza limitato a 100 chiamate per 20 secondi.
- **Limite concorrenza:**  Massimo dieci chiamate API simultanee.

Si consiglia di non superare le 300 dimensioni del batch. Dimensioni maggiori non sono supportate e possono causare timeout e, in casi estremi, essere limitati.

## Impostazioni API di SOAP in Marketo

1. Andare alla sezione **[!UICONTROL Admin]** e fare clic su **[!UICONTROL Web Services]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Imposta un [!UICONTROL Encryption Key] appropriato, fai clic su **[!UICONTROL Save Changes]** e utilizza i valori API di SOAP [!UICONTROL Endpoint], [!UICONTROL User ID] e [!UICONTROL Encryption Key] per generare la [firma di autenticazione](authentication-signature.md) corretta per ogni chiamata API di SOAP.

![admin-web-services3](assets/admin-web-services3.png)
