---
title: API SOAP
feature: SOAP
description: L’API SOAP di Marketo è diventata obsoleta dopo il 31 ottobre 2025. Scopri come migrare a REST, recuperare il tuo WSDL, vedere quote, limiti di tariffa e configurazione di autenticazione.
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
TQID: https://experienceleague.adobe.com/Atnarr7XLzW3B2R5I8nLtayeE5kt3Bd4T46K6yIPc-8
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 253
ht-degree: 0%

---

# API SOAP

L’API SOAP diventerà obsoleta e non sarà più disponibile dopo il 31 luglio 2026. Tutti i nuovi sviluppi devono essere eseguiti con Marketo [REST API](../rest-api/rest-api.md) e i servizi esistenti devono essere migrati entro tale data per evitare interruzioni del servizio. Se si dispone di un servizio che utilizza l&#39;API SOAP, consultare la [Guida alla migrazione dell&#39;API SOAP](./migration.md) per informazioni su come eseguire la migrazione.

## WSDL SOAP

Per recuperare il documento SOAP WSDL, ottenere l&#39;endpoint API SOAP dal menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]**.

![Endpoint SOAP](assets/endpoint-soap.png)

L&#39;URL WSDL è:

`<SOAP API Endpoint> + ?WSDL`

Non utilizzare il punto finale definito nel file WSDL. Ogni istanza di Marketo ha un endpoint univoco in cui effettuare chiamate a.

## Limiti

- **Quota giornaliera:** alla maggior parte degli abbonamenti sono assegnate 10.000 chiamate API al giorno (che vengono ripristinate ogni giorno a 12:00AM CST). Puoi aumentare la tua quota giornaliera tramite il tuo account manager.
- **Limite di frequenza:** accesso API per istanza limitato a 100 chiamate per 20 secondi.
- **Limite concorrenza:** massimo di dieci chiamate API simultanee.

Si consiglia di non superare le 300 dimensioni del batch. Dimensioni maggiori non sono supportate e possono causare timeout e, in casi estremi, essere limitati.

## Impostazioni API di SOAP in Marketo

1. Andare alla sezione **[!UICONTROL Admin]** e selezionare **[!UICONTROL Web Services]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Imposta un [!UICONTROL Encryption Key] appropriato, seleziona **[!UICONTROL Save Changes]** e utilizza i valori API SOAP [!UICONTROL Endpoint], [!UICONTROL User ID] e [!UICONTROL Encryption Key] per generare la [firma di autenticazione](authentication-signature.md) corretta per ogni chiamata API SOAP.

![admin-web-services3](assets/admin-web-services3.png)
