---
title: Webhook
feature: Webhooks
description: Panoramica dei webhook
exl-id: fd283c66-05a1-4aa4-8412-0d41b8d1e3c8
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 0%

---

# Webhook

Marketo consente l’utilizzo di webhook per comunicare con servizi web di terze parti. I webhook supportano l’utilizzo dei verbi HTTP GET o POST per inviare o recuperare dati da un URL specifico. Per istruzioni dettagliate sulla creazione di webhook all’interno dell’applicazione e su come aggiungerli alle campagne intelligenti, consulta i seguenti articoli:

- [Creare un webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook)
- [Chiama webhook](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/call-webhook)
- [Utilizzare un webhook in una campagna avanzata](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/use-a-webhook-in-a-smart-campaign)

Ogni singolo webhook ha le seguenti proprietà:

- [!UICONTROL URL] - Immettere l&#39;URL utilizzato per inviare la richiesta al servizio Web.
- [!UICONTROL Request Type] - Il metodo HTTP.
- [!UICONTROL Payload Template] - Se si desidera trasmettere le informazioni nel corpo del POST, inserire il modello. Utilizza qualsiasi formato di dati che supporta HTTP POST, inclusi XML, JSON o SOAP. Il formato di serializzazione deve consentire virgolette doppie attorno alle stringhe. Per inserire un token nel modello, fai clic su **[!UICONTROL Insert Token]**.  I token di tipo stringa vengono automaticamente racchiusi tra virgolette doppie.
- [!UICONTROL Request Token Encoding] - Se i valori del token includono caratteri speciali (ad esempio una e commerciale, &#39;&amp;&#39;), indica il formato della richiesta (JSON o Form/Url). È necessario selezionare la codifica corretta per il corpo per garantire che il webhook comunichi correttamente con il servizio Web.
- [!UICONTROL Response Type] : seleziona il formato della risposta ricevuta dal servizio (JSON o XML). È necessario selezionare il tipo di risposta corretto per mappare le proprietà della risposta ai campi lead in Marketo
- [!UICONTROL Custom Headers] - Accesso tramite [!UICONTROL Webhooks Actions] -> [!UICONTROL Set Custom Header], questo menu consente di aggiungere un numero qualsiasi di coppie chiave-valore personalizzate come intestazioni HTTP.

I dati possono essere scritti ai lead dalle risposte ai servizi web utilizzando [Mappature risposte](response-mappings.md)

## Token

Tutti i campi in uscita in un webhook (URL, modello e intestazioni personalizzate) popolano il contenuto dei token nello stesso contesto del passaggio di flusso. Ciò significa che i token di lead e di sistema sono sempre disponibili, mentre i token di trigger, Campaign e Program sono disponibili nei rispettivi ambiti. Consulta gli articoli relativi ai token:

- [Panoramica dei token](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/demand-generation/landing-pages/personalizing-landing-pages/tokens-overview)
- [Glossario dei token di sistema](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/using-tokens/system-tokens-glossary)
- [Token per i momenti di interesse](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-sales-insight/msi-for-salesforce/features/tabs-in-the-msi-panel/interesting-moments/trigger-tokens-for-interesting-moments)

Un caso comune si verifica quando un programma o una campagna sono mappati in modo esplicito a una risorsa di terze parti. Un ID può essere impostato a livello di programma come `My Token`, quindi passato nella richiesta Webhook come token.

## Intestazioni personalizzate

I webhook consentono l’utilizzo di un numero qualsiasi di campi di intestazione personalizzati da inviare insieme alla richiesta in uscita. Questi possono essere aggiunti tramite **[!UICONTROL Webhooks Actions]** > **[!UICONTROL Set Custom Header]**. Ogni intestazione viene registrata come una semplice coppia chiave-valore. In quest’area è possibile utilizzare i token.

![Intestazioni personalizzate](assets/custom-headers.png)

## Suggerimenti

- Il passaggio Flusso del webhook di chiamata è valido solo nelle campagne Trigger.
- Gli aggiornamenti tramite i mapping di risposta si verificano solo se il servizio web risponde con un codice di risposta HTTP 2xx. Altri tipi di codici non comporteranno aggiornamenti del record.
- È possibile utilizzare i servizi Web per eseguire l&#39;arricchimento, la convalida o la normalizzazione dei dati personalizzati da servizi interni o esterni.
- Il tempo di esecuzione del webhook è in balia del tempo di risposta del servizio utilizzato e può causare lunghi ritardi nell’esecuzione della campagna. Anche se l’esecuzione di un servizio richiede solo 50 ms, ovvero 1,5 ore se eseguito 100.000 volte.
- Marketo attende fino a 30 secondi per una determinata chiamata del servizio prima di terminare la chiamata (ovvero per timeout).
- I caratteri incorporati nel campo URL vengono passati come scritti. Ad esempio &#39;&amp;&#39; viene inviato come &#39;&amp;&#39;, &#39;%26&#39; viene inviato come &#39;%26&#39;
   - Se un carattere deve essere codificato in percentuale quando viene ricevuto dal server destinatario, deve essere passato esplicitamente come stringa che rappresenta tale carattere
