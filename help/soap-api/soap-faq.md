---
title: Domande frequenti su SOAP
feature: SOAP
description: Scopri come elencare programmi con getMObjects, ottimizzare getMultipleLeads, creare opportunità e inviare o pianificare e-mail personalizzate tramite l’API SOAP di Marketo.
exl-id: a2d8f144-cd5f-41bc-8231-29c42af935b8
TQID: https://experienceleague.adobe.com/AWgJgPdDXmapXqvG-J93utvXGV8-zLnKO-DvWFzOYoI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 360
ht-degree: 0%

---

# Domande frequenti su SOAP

**Q:** Come posso ottenere un elenco di tutti i programmi all&#39;interno di Marketo insieme ai loro metadati?

**A:** Per recuperare un elenco di tutti i programmi, è possibile utilizzare [getMObjects](./getmobjects.md) passando il tipo uguale a &quot;Program&quot; e impostando includeDetails su true.

**Q:** Esiste un modo per velocizzare le prestazioni di getMultipleLeads?

**A:** Sono disponibili alcune opzioni per velocizzare le prestazioni della chiamata getMultipleLeads. Il primo è quello di ridurre il batchSize che stai richiedendo per ogni chiamata. 200 è la dimensione batch consigliata. La seconda opzione consente di specificare i campi a cui si è interessati utilizzando il filtro includeAttributes. In questo modo la query è più veloce e il payload della risposta ricevuta viene ridotto. L’approccio finale consiste nell’utilizzare LastUpdateAtSelector e specificare olderUpdatedAt e latestUpdatedAt. Puoi specificare intervalli di date diversi e quindi thread più richieste simultaneamente. Se utilizzi l&#39;approccio thread, assicurati che il client SOAP/WSDL supporti [connessioni persistenti](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**Q:** Come posso creare opportunità tramite l&#39;API SOAP quando non sono integrate con un sistema di gestione delle relazioni con i clienti come Salesforce o Microsoft Dynamics?

**A:** È possibile creare opportunità utilizzando l&#39;API di SOAP utilizzando la chiamata [syncMObjects](syncmobjects.md) per scrivere sui tipi OpportunityPersonRole e Opportunity [MObject](marketo-objects.md).

**Q:** Posso inviare un&#39;e-mail da Marketo a livello di programmazione? In caso affermativo, come posso inviare contenuto personalizzato per ogni destinatario e-mail?

**A:** Assolutamente. Puoi richiedere l&#39;invio di un&#39;e-mail da Marketo utilizzando [requestCampaign](requestcampaign.md) o la combinazione di [importToList](importtolist.md) e [scheduleCampaign](schedulecampaign.md) API SOAP. Per inviare immediatamente un&#39;e-mail a una o più persone, puoi utilizzare [requestCampaign](requestcampaign.md). Se desideri pianificare l&#39;invio di un&#39;e-mail in una data e un&#39;ora specificate, utilizza [importToList](importtolist.md) per specificare i destinatari dell&#39;e-mail e [scheduleCampaign](schedulecampaign.md) per specificare quando tali destinatari riceveranno l&#39;e-mail.

Se si desidera personalizzare il contenuto di ogni destinatario e-mail, è possibile eseguire l&#39;override dei valori di [token di programma](../rest-api/tokens.md) impostati nel modello e-mail.
