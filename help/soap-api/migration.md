---
title: Migrazione all’API REST
feature: SOAP
description: Guida dettagliata alla migrazione di Marketo Engage da SOAP a REST entro il 31 gennaio 2026, con mappature degli endpoint, OAuth, metodi di sincronizzazione dei lead e architetture di riferimento.
exl-id: c2956db3-defe-4163-99f3-58654ce8ee2b
source-git-commit: 5881ab969eca3a37d19f56b6570e42828994eff3
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 1%

---

# Migrazione all’API REST

L’API SOAP di Marketo Engage verrà ritirata dopo il 31 marzo 2026. Tutte le integrazioni esistenti che utilizzano l&#39;API SOAP devono essere ritirate o migrate all&#39;[API REST Marketo Engage](https://experienceleague.adobe.com/it/docs/marketo-developer/marketo/rest/rest-api) entro questa data per evitare interruzioni del servizio.

## Migrazione

L&#39;API SOAP supporta un intervallo limitato di casi d&#39;uso rispetto all&#39;[API REST](https://experienceleague.adobe.com/it/docs/marketo-developer/marketo/rest/rest-api)I. Per determinare quali endpoint mappare i casi d&#39;uso, segui [Best practice di integrazione di Marketo](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/marketo-integration-best-practices)

[Architetture di riferimento](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/reference-architectures) disponibili per [Sincronizzazione CRM](https://experienceleague.adobe.com/docs/marketo-developer/assets/sync-architecture-whitepaper.pdf?lang=en) e [Esportazione Data Warehouse](https://experienceleague.adobe.com/docs/marketo-developer/assets/reference_architecture.pdf?lang=en) casi d&#39;uso.

## Autenticazione

[Documentazione sull&#39;autenticazione](https://experienceleague.adobe.com/it/docs/marketo-developer/marketo/rest/authentication)

L’API REST di Marketo utilizza l’autenticazione basata su OAuth 2.0 con il tipo di concessione Credenziali client. I token di accesso sono validi per un&#39;ora dopo la creazione.

## Lead

[Documentazione API lead](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads)

L&#39;API SOAP supporta la sincronizzazione dei dati dei lead, [l&#39;associazione dei cookie Munchkin](https://experienceleague.adobe.com/it/docs/marketo-developer/marketo/javascriptapi/leadtracking/lead-tracking) e l&#39;unione dei lead. Se l&#39;applicazione chiama il metodo syncLead di SOAP e imposta il parametro `marketoCookie`, è possibile eseguire la migrazione in uno dei modi seguenti:

1. Utilizzo del metodo REST [Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST), seguito da [Lead associato](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)
2. È possibile chiamare [Invia modulo](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads), anche se questo richiede la configurazione di alcune Assets di marketing e l&#39;interazione con l&#39;[API Forms](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/forms)

Le applicazioni che utilizzano il tipo di chiave `foreignSysPersonId` devono migrare a utilizzando un campo lead personalizzato per rappresentare questo identificatore esterno e utilizzare i metodi REST [Lead di sincronizzazione](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads#create-and-update) o [Importazione lead in blocco](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import).

| Metodo SOAP | Metodo/i REST |
| --- | --- |
| [getLead](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/getlead) | [Ottieni lead per ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Ottieni lead per tipo filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) |
| [getMultipleLeads](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/getmultipleleads) | [Ottieni lead per ID](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [Ottieni lead per tipo filtro](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), [Ottieni lead per ID programma](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByProgramIdUsingGET), [Ottieni lead per ID elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByListIdUsingGET), [Esportazione lead in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads) |
| [mergeLeads](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/mergeleads) | [Unisci lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) |
| [syncLead](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/synclead) | [Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Invia modulo](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) [Associa lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST) |
| [syncMultipleLeads](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/syncmultipleleads) | [Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [Importazione in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |

## Oggetti M

M Objects era un concetto onnicomprensivo per supportare l&#39;esportazione dei dati di Attribution dell&#39;opportunità per l&#39;analisi esterna e utilizzava tre tipi di oggetto: Opportunità, Ruoli opportunità e Programmi.

Documentazione REST:

- [Opportunità](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/opportunities)
- [Ruoli](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/opportunity-roles)
- [Programmi](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/programs)

| Metodo SOAP | Metodo/i REST |
| --- | --- |
| [deleteMObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/deletemobjects) | [Elimina opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST), [Elimina ruoli opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunityRolesUsingPOST) |
| [descriptionMObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/describemobject) | [Descrivi opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_4), [Descrivi ruolo opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [getMObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/getmobjects) | [Ottieni opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getOpportunitiesUsingGET), [Ottieni ruoli opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [listMObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/listmobjects) | N/D |
| [syncMObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/syncmobjects) | [Opportunità di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunitiesUsingPOST), [Ruoli opportunità di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunityRolesUsingPOST) |
| [getChannels](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/programs/getchannels) | [Ottieni canali](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllChannelsUsingGET) |
| [getTags](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/programs/gettags) | [Ottieni tipi di tag](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagTypesUsingGET), [Ottieni tag per nome](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagByNameUsingGET) |

## Elenchi statici

I casi di utilizzo per elenchi statici nell&#39;API di SOAP sono limitati all&#39;acquisizione dei dati di appartenenza e lead e alla rimozione dell&#39;appartenenza che può essere eseguita con i metodi REST [Aggiungi all&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST), [Importa in blocco lead](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) o [Rimuovi dall&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE).

| Metodo SOAP | Metodo/i REST |
| --- | --- |
| [getImportToListStatus](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/getimporttoliststatus) | [Lead importazione in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [importToList](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/importtolist) | [Aggiungi All&#39;Elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) [Lead Di Importazione In Blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [listOperation](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/listoperation) | [Rimuovi dall&#39;elenco](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) |

## Attività

L’API SOAP supporta solo il recupero delle attività.

Documentazione REST:

- [Attività sincrone](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/activities)
- [Estrazione attività in blocco](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-activity-extract)

| Metodo SOAP | Metodo/i REST |
| --- | --- |
| [getLeadActivity](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/activities/getleadactivity) | [Attività di esportazione in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Attività lead ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) |
| [getLeadChanges](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/activities/getleadchanges) | [Attività di esportazione in blocco](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [Modifiche lead](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadChangesUsingGET) |

## Campagne

Documentazione REST:

- [Campagne avanzate](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns)

L&#39;API SOAP supporta solo tre casi d&#39;uso per le campagne intelligenti: [Attivazione dei lead per qualificarsi per una campagna avanzata richiedente](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns#trigger), recupero delle campagne richieste e [Pianificazione dell&#39;esecuzione futura di una campagna avanzata](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns#schedule).

| Metodo SOAP | Metodo/i REST |
| --- | --- |
| [getCampaignsForSource](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/getcampaignsforsource) | [Ottieni campagne intelligenti](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllSmartCampaignsGET) |
| [requestCampaign](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/requestcampaign) | [Richiedi campagna](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) |
| [pianificazioneCampaign](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/schedulecampaign) | [Pianifica campagna](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) |

## Oggetti personalizzati

Documentazione REST:

- [Oggetti personalizzati](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/custom-objects)

L’API SOAP supportava solo operazioni CRUD per gli oggetti personalizzati.

| Metodo SOAP | Metodo/i REST |
| --- | --- |
| [deleteCustomObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/deletecustomobjects) | [Elimina oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteCustomObjectsUsingPOST) |
| [getCustomObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/getcustomobjects) | [Ottieni oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCustomObjectsUsingGET) |
| [syncCustomObjects](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/synccustomobjects) | [Sincronizza oggetti personalizzati](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncCustomObjectsUsingPOST) [Importa oggetti personalizzati in blocco](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import) |
