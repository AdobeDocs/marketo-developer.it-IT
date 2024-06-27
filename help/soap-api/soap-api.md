---
title: API SOAP
feature: SOAP
description: Panoramica di Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# API SOAP

L’API SOAP non è più in fase di sviluppo attivo. Le chiamate funzionano ancora, ma il nostro sviluppo si concentra su [REST](https://developer.adobe.com/marketo-apis/) avanti.

L’API SOAP di Marketo consente di creare, recuperare e rimuovere entità e dati memorizzati in Marketo. È possibile trovare [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) su GitHub. Sono inoltre disponibili [librerie client](https://github.com/Marketo/Community-Supported-Client-Libraries) per farti risparmiare un po&#39; di tempo.

Ultima versione API: 3_1

## WSDL SOAP

Per recuperare il documento WSDL dell’SOAP, ottieni il tuo endpoint API SOAP dal tuo **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** menu.

![Endpoint SOAP](assets/endpoint-soap.png)

L&#39;URL WSDL è:

`<SOAP API Endpoint> + ?WSDL`

Non utilizzare il punto finale definito nel file WSDL. Ogni istanza di Marketo ha un endpoint univoco in cui effettuare chiamate a.

## Limiti

- **Quota giornaliera:** Alla maggior parte degli abbonamenti vengono assegnate 10.000 chiamate API al giorno (che vengono ripristinate ogni giorno alle 00.00 CST). Puoi aumentare la tua quota giornaliera tramite il tuo account manager.
- **Limite tariffa:** L’accesso API per istanza è limitato a 100 chiamate per 20 secondi.
- **Limite concorrenza:**  Massimo dieci chiamate API simultanee.

Si consiglia di non superare le 300 dimensioni del batch. Dimensioni maggiori non sono supportate e possono causare timeout e, in casi estremi, essere limitati.

## Impostazioni API SOAP in Marketo

1. Vai a **[!UICONTROL Admin]** e fai clic su **[!UICONTROL Web Services]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Impostare un [!UICONTROL Encryption Key], fai clic su **[!UICONTROL Save Changes]** e utilizzare l’API SOAP [!UICONTROL Endpoint], [!UICONTROL User ID], e [!UICONTROL Encryption Key] valori per generare il corretto [firma di autenticazione](authentication-signature.md) per ogni chiamata API SOAP.

![admin-web-services3](assets/admin-web-services3.png)
