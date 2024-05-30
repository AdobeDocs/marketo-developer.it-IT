---
title: "API SOAP"
feature: SOAP
description: "Panoramica di Marketo SOAP"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# API SOAP

L’API SOAP non è più in fase di sviluppo attivo. Le chiamate funzionano ancora, ma il nostro sviluppo si concentra su [REST](https://developer.adobe.com/marketo-apis/) avanti.

L’API SOAP di Marketo consente di creare, recuperare e rimuovere entità e dati memorizzati in Marketo. È possibile trovare [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) su GitHub. Sono inoltre disponibili [librerie client](https://github.com/Marketo/Community-Supported-Client-Libraries) per farti risparmiare un po&#39; di tempo.

Ultima versione API: 3_1

## WSDL SOAP

Per recuperare il documento WSDL SOAP, ottenere l&#39;endpoint API SOAP dal file **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** menu.

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

1. Andare alla sezione Amministratore e fare clic su Servizi Web.

![admin-web-services2](assets/admin-web-services2.png)

1. Impostare una chiave di crittografia appropriata, fare clic su &quot;Salva modifiche&quot; e utilizzare i valori dell&#39;endpoint API SOAP, dell&#39;ID utente e della chiave di crittografia per generare la chiave corretta [firma di autenticazione](authentication-signature.md) per ogni chiamata API SOAP.

![admin-web-services3](assets/admin-web-services3.png)
