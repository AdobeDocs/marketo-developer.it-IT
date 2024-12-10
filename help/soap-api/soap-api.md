---
title: API SOAP
feature: SOAP
description: Panoramica di Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7a3df193e47e7ee363c156bf24f0941879c6bd13
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# API SOAP

L’API dell’SOAP è diventata obsoleta e non sarà più disponibile dopo il 31 ottobre 2025.  Tutti i nuovi sviluppi devono essere eseguiti con l&#39;API [REST](https://developer.adobe.com/marketo-apis/) di Marketo e i servizi esistenti devono essere migrati entro tale data per evitare interruzioni del servizio.

## WSDL SOAP

Per recuperare il documento WSDL SOAP, ottenere l&#39;endpoint API SOAP dal menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]**.

![Endpoint SOAP](assets/endpoint-soap.png)

L&#39;URL WSDL è:

`<SOAP API Endpoint> + ?WSDL`

Non utilizzare il punto finale definito nel file WSDL. Ogni istanza di Marketo ha un endpoint univoco in cui effettuare chiamate a.

## Limiti

- **Quota giornaliera:** alla maggior parte degli abbonamenti sono assegnate 10.000 chiamate API al giorno (che vengono ripristinate ogni giorno alle 00.00 CST). Puoi aumentare la tua quota giornaliera tramite il tuo account manager.
- **Limite di frequenza:** accesso API per istanza limitato a 100 chiamate per 20 secondi.
- **Limite concorrenza:**  Massimo dieci chiamate API simultanee.

Si consiglia di non superare le 300 dimensioni del batch. Dimensioni maggiori non sono supportate e possono causare timeout e, in casi estremi, essere limitati.

## Impostazioni API SOAP in Marketo

1. Andare alla sezione **[!UICONTROL Admin]** e fare clic su **[!UICONTROL Web Services]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Imposta un [!UICONTROL Encryption Key] appropriato, fai clic su **[!UICONTROL Save Changes]** e utilizza i valori API SOAP [!UICONTROL Endpoint], [!UICONTROL User ID] e [!UICONTROL Encryption Key] per generare la [firma di autenticazione](authentication-signature.md) corretta per ogni chiamata API SOAP.

![admin-web-services3](assets/admin-web-services3.png)
