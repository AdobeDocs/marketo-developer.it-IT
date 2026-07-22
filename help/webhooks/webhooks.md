---
title: Webhook
feature: Webhooks
description: Scopri come configurare i webhook di Marketo per chiamare servizi di terze parti, impostare modelli di payload, codifica, mappature di risposta, token, intestazioni personalizzate e suggerimenti.
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
TQID: https://experienceleague.adobe.com/r-GpAqhYPKvlDtMw5l23jeJWzlSqycP65eYJPA3m9EM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
subfeature_v2:
  - id: ad89fb33-8541-4339-afe7-bb13d1633714
  - id: fc9b09fe-b844-4544-887b-e420c3b82065
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 590
ht-degree: 3%

---

# Webhook

I webhook di Marketo comunicano con i servizi web di terze parti. Un webhook utilizza il verbo HTTP GET o POST per inviare o recuperare dati da un URL specifico.

Per istruzioni sulla creazione di un webhook e sulla sua aggiunta a una campagna avanzata, consulta:

- [Creare un Webhook](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Webhook di chiamata](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Utilizzare un webhook in una campagna avanzata](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Configura ogni webhook con le seguenti proprietà:

- **[!UICONTROL URL]** - URL a cui inviare la richiesta del servizio Web.
- **[!UICONTROL Request Type]** - Metodo HTTP.
- **[!UICONTROL Payload Template]** - Modello per le informazioni inviate nel corpo del POST. Utilizza qualsiasi formato di dati che supporta HTTP POST, inclusi XML, JSON o SOAP. Il formato di serializzazione deve consentire virgolette doppie attorno alle stringhe. Per inserire un token, selezionare **[!UICONTROL Insert Token]**. Marketo racchiude automaticamente i token di tipo stringa tra virgolette doppie.
- **[!UICONTROL Request Token Encoding]** - Formato della richiesta, JSON o Form/Url, utilizzato per codificare i valori dei token che includono caratteri speciali come la e commerciale &quot;&amp;&quot;. Seleziona la codifica del corpo corretta in modo che il webhook comunichi correttamente con il servizio web.
- **[!UICONTROL Response Type]** - Formato della risposta, JSON o XML. Seleziona il tipo corretto per mappare le proprietà di risposta ai campi lead in Marketo.
- **[!UICONTROL Custom Headers]** - Coppie chiave-valore aggiunte come intestazioni HTTP tramite **[!UICONTROL Webhooks Actions]** > **[!UICONTROL Set Custom Header]**. Puoi aggiungere un numero qualsiasi di intestazioni personalizzate.

Utilizza [Mapping risposte](response-mappings.md) per scrivere i dati dalle risposte del servizio Web ai lead.

## Token

Tutti i campi del webhook in uscita, inclusi URL, modello e intestazioni personalizzate, popolano il contenuto del token nello stesso contesto del passaggio di flusso.

I token di lead e di sistema sono sempre disponibili. I token di attivazione, campagna e programma sono disponibili nei rispettivi ambiti. Per ulteriori informazioni, consulta:

- [Panoramica sui token](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossario dei token di sistema](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Token per i momenti di interesse](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Ad esempio, quando un programma o una campagna è mappato su una risorsa di terze parti, imposta un ID a livello di programma come `My Token`. Quindi passa l’ID nella richiesta webhook come token.

## Intestazioni personalizzate

I webhook possono inviare un numero qualsiasi di campi di intestazione personalizzati con una richiesta in uscita. Aggiungere intestazioni tramite **[!UICONTROL Webhooks Actions]** > **[!UICONTROL Set Custom Header]**.

Ogni intestazione è una coppia chiave-valore e può contenere token.

![Intestazioni personalizzate](assets/custom-headers.png)

## Suggerimenti

- Utilizza il passaggio Flusso del webhook di chiamata solo nelle campagne Trigger.
- I mapping di risposta aggiornano un record solo quando il servizio Web restituisce un codice di risposta HTTP 2xx.
- È possibile utilizzare i servizi Web per eseguire l&#39;arricchimento, la convalida o la normalizzazione dei dati personalizzati da servizi interni o esterni.
- Il tempo di esecuzione del webhook dipende dal tempo di risposta del servizio e può causare lunghi ritardi nell’esecuzione della campagna. Anche se l’esecuzione di un servizio richiede solo 50 ms, l’esecuzione di 100.000 esecuzioni richiede 1,5 ore.
- Marketo attende fino a 30 secondi per una determinata chiamata del servizio prima di terminare la chiamata (nota anche come timeout).
- Marketo trasmette i caratteri nel campo URL come scritti. Ad esempio, &#39;&amp;&#39; viene inviato come &#39;&amp;&#39; e &#39;%26&#39; viene inviato come &#39;%26&#39;.
  - Per inviare un carattere con codifica percentuale al server destinatario, passa esplicitamente la stringa che rappresenta tale carattere.
