---
title: "DOMANDE FREQUENTI SU SOAP"
feature: SOAP
description: "DOMANDE FREQUENTI SU SOAP"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---


# DOMANDE FREQUENTI SU SOAP

**D:** Come posso ottenere un elenco di tutti i programmi all’interno di Marketo insieme ai loro metadati?

**R:** Per recuperare un elenco di tutti i programmi, è possibile utilizzare [getMObjects](./getmobjects.md) passando il tipo uguale a &quot;Program&quot; e impostando includeDetails su true.

**D:** Esiste un modo per velocizzare le prestazioni di getMultipleLeads?

**R:** Sono disponibili alcune opzioni per velocizzare le prestazioni della chiamata getMultipleLeads. Il primo è quello di ridurre il batchSize che stai richiedendo per ogni chiamata. 200 è la dimensione batch consigliata. La seconda opzione consente di specificare i campi a cui si è interessati utilizzando il filtro includeAttributes. In questo modo la query è più veloce e il payload della risposta ricevuta viene ridotto. L’approccio finale consiste nell’utilizzare LastUpdateAtSelector e specificare olderUpdatedAt e latestUpdatedAt. Puoi specificare intervalli di date diversi e quindi thread più richieste simultaneamente. Se utilizzi l’approccio thread, assicurati che il client SOAP/WSDL supporti [connessioni permanenti](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html).

**D:** Come posso creare opportunità tramite l’API SOAP quando non sono integrate con un sistema di gestione delle relazioni con i clienti come SalesForce o Microsoft Dynamics?

**R:** Puoi creare opportunità utilizzando l’API SOAP utilizzando [syncMObjects](syncmobjects.md) chiamata di scrittura a OpportunityPersonRole e Opportunity [Oggetto MO](marketo-objects.md) tipi.

**D:** Posso inviare un’e-mail a livello di programmazione da Marketo? In caso affermativo, come posso inviare contenuto personalizzato per ogni destinatario e-mail?

**R:** Assolutamente. Puoi richiedere l’invio di un’e-mail da Marketo utilizzando [requestCampaign](requestcampaign.md) o combinazione di [importToList](importtolist.md) e [scheduleCampaign](schedulecampaign.md) API SOAP. Per inviare immediatamente un’e-mail a una o più persone, utilizza [requestCampaign](requestcampaign.md). Se desideri pianificare l’invio di un’e-mail in una data e un’ora specificate, utilizza [importToList](importtolist.md) per specificare i destinatari dell’e-mail, e [scheduleCampaign](schedulecampaign.md) per specificare quando verrà inviata l’e-mail a tali destinatari.

Se desideri personalizzare il contenuto di ogni destinatario e-mail, puoi farlo ignorando i valori di [token di programma](../rest-api/tokens.md) che sono impostate all’interno del modello e-mail.
