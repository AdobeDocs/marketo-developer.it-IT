---
title: Riferimento endpoint
feature: REST API
description: Elenco completo degli endpoint API REST di Marketo con metodi, URI e autorizzazioni richieste per attività, esportazione in blocco, identità, lead, risorse, utenti.
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '4464'
ht-degree: 7%

---

# Riferimento endpoint

Di seguito sono riportati i collegamenti ai riferimenti API REST di Marketo.

- [Risorsa](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identità](https://developer.adobe.com/marketo-apis/api/identity/)
- [Database lead](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Gestione utente](https://developer.adobe.com/marketo-apis/api/user/)

## Elenco endpoint {#endpoint_list}

Elenco completo degli endpoint REST API.

| Nome | Gruppo | Metodo | URI | Autorizzazione necessaria |
|---|---|---|---|---|
| Aggiungi attività personalizzate | Attività | POST | /rest/v1/activities/external.json | Attività di lettura/scrittura |
| Approva tipo di attività personalizzato | Attività | POST | /rest/v1/activities/external/type/{apiName}/approve.json | Metadati attività di lettura/scrittura |
| Creare attributi di tipo di attività personalizzati | Attività | POST | /rest/v1/activities/external/type/{apiName}/attributes/create.json | Metadati attività di lettura/scrittura |
| Creare tipi di attività personalizzati | Attività | POST | /rest/v1/activities/external/type.json | Metadati attività di lettura/scrittura |
| Elimina tipo di attività personalizzato | Attività | POST | /rest/v1/activities/external/type/{apiName}/delete.json | Metadati attività di lettura/scrittura |
| Elimina attributi tipo di attività personalizzato | Attività | POST | /rest/v1/activities/external/type/{apiName}/attributes/delete.json | Metadati attività di lettura/scrittura |
| Descrivi tipo di attività personalizzato | Attività | GET | /rest/v1/activities/external/type/{apiName}/describe.json | Metadati attività di sola lettura |
| Elimina bozza tipo di attività personalizzato | Attività | POST | /rest/v1/activities/external/type/{apiName}/discardDraft.json | Metadati attività di lettura/scrittura |
| Ottieni tipi di attività | Attività | GET | /rest/v1/activities/types.json | Attività di sola lettura |
| Ottieni tipi di attività personalizzati | Attività | GET | /rest/v1/activities/external/types.json | Metadati attività di sola lettura |
| Ottieni lead eliminati | Attività | GET | /rest/v1/activities/deletedleads.json | Attività di sola lettura |
| Ottieni attività lead | Attività | GET | /rest/v1/activities.json | Attività di sola lettura |
| Ottieni modifiche lead | Attività | GET | /rest/v1/activities/leadchanges.json | Attività di sola lettura |
| Ottieni token di paging | Attività | GET | /rest/v1/activities/pagingtoken.json | Attività di sola lettura |
| Aggiorna tipo di attività personalizzato | Attività | POST | /rest/v1/activities/external/type/{apiName}.json | Metadati attività di lettura/scrittura |
| Aggiorna attributi del tipo di attività personalizzato | Attività | POST | /rest/v1/activities/external/type/{apiName}/attributes/update.json | Metadati attività di lettura/scrittura |
| Identità | Autenticazione | GET o POST | /identity/oauth/token | Nessuna |
| Annulla processo attività esportazione | Attività esportazione in blocco | POST | /bulk/v1/activities/export/{exportid}/cancel.json | Attività di sola lettura |
| Crea processo attività di esportazione | Attività esportazione in blocco | POST | /bulk/v1/activities/export/create.json | Attività di sola lettura |
| Accoda processo attività di esportazione | Attività esportazione in blocco | POST | /bulk/v1/activities/export/{exportid}/enqueue.json | Attività di sola lettura |
| Ottieni file attività di esportazione | Attività esportazione in blocco | GET | /bulk/v1/activities/export/{exportid}/file.json | Attività di sola lettura |
| Ottieni stato processo attività di esportazione | Attività esportazione in blocco | GET | /bulk/v1/activities/export/{exportid}/status.json | Attività di sola lettura |
| Ottieni processi attività di esportazione | Attività esportazione in blocco | GET | /bulk/v1/activities/export.json | Attività di sola lettura |
| Annulla processo di esportazione oggetto personalizzato | Esportazione in blocco di oggetti personalizzati | POST | /bulk/v1/customobjects/export/{exportid}/cancel.json | Oggetto personalizzato di sola lettura |
| Crea processo di esportazione oggetto personalizzato | Esportazione in blocco di oggetti personalizzati | POST | /bulk/v1/customobjects/export/create.json | Oggetto personalizzato di sola lettura |
| Processo oggetto personalizzato esportazione accodamento | Esportazione in blocco di oggetti personalizzati | POST | /bulk/v1/customobjects/export/{exportid}/enqueue.json | Oggetto personalizzato di sola lettura |
| Ottieni file oggetto personalizzato esportazione | Esportazione in blocco di oggetti personalizzati | GET | /bulk/v1/customobjects/export/{exportid}/file.json | Oggetto personalizzato di sola lettura |
| Ottieni stato processo di esportazione oggetto personalizzato | Esportazione in blocco di oggetti personalizzati | GET | /bulk/v1/customobjects/export/{exportid}/status.json | Oggetto personalizzato di sola lettura |
| Ottieni processi oggetto personalizzati esportazione | Esportazione in blocco di oggetti personalizzati | GET | /bulk/v1/customobjects/export.json | Oggetto personalizzato di sola lettura |
| Annulla processo di esportazione lead | Lead esportazione in blocco | POST | /bulk/v1/lead/export/{exportid}/cancel.json | Lead di sola lettura |
| Crea processo lead esportazione | Lead esportazione in blocco | POST | /bulk/v1/leads/export/create.json | Lead di sola lettura |
| Accoda processo lead esportazione | Lead esportazione in blocco | POST | /bulk/v1/lead/export/{exportid}/enqueue.json | Lead di sola lettura |
| Ottieni file lead esportazione | Lead esportazione in blocco | GET | /bulk/v1/lead/export/{exportid}/file.json | Lead di sola lettura |
| Ottieni stato processo lead esportazione | Lead esportazione in blocco | GET | /bulk/v1/lead/export/{exportid}/status.json | Lead di sola lettura |
| Ottieni processi lead esportazione | Lead esportazione in blocco | GET | /bulk/v1/leads/export.json | Lead di sola lettura |
| Annulla processo membro programma di esportazione | Esportazione in blocco dei membri del programma | POST | /bulk/v1/program/members/export/{exportid}/cancel.json | Lead di sola lettura |
| Crea processo membro programma di esportazione | Esportazione in blocco dei membri del programma | POST | /bulk/v1/program/members/export/create.json | Lead di sola lettura |
| Accoda processo membro programma di esportazione | Esportazione in blocco dei membri del programma | POST | /bulk/v1/program/members/export/{exportid}/enqueue.json | Lead di sola lettura |
| Ottieni file membro del programma di esportazione | Esportazione in blocco dei membri del programma | GET | /bulk/v1/program/members/export/{exportid}/file.json | Lead di sola lettura |
| Ottieni stato processo membro programma di esportazione | Esportazione in blocco dei membri del programma | GET | /bulk/v1/program/members/export/{exportid}/status.json | Lead di sola lettura |
| Ottieni processi membri programma di esportazione | Esportazione in blocco dei membri del programma | GET | /bulk/v1/program/members/export.json | Lead di sola lettura |
| Errori di importazione oggetti personalizzati | Importazione in blocco di oggetti personalizzati | GET | /bulk/v1/customobjects/import/{id}/failures.json | Oggetto personalizzato di lettura/scrittura |
| Ottieni stato oggetto personalizzato importazione | Importazione in blocco di oggetti personalizzati | GET | /bulk/v1/customobjects/import/{id}/status.json | Oggetto personalizzato di lettura/scrittura |
| Avvisi di importazione oggetti personalizzati | Importazione in blocco di oggetti personalizzati | GET | /bulk/v1/customobjects/import/{id}/warnings.json | Oggetto personalizzato di lettura/scrittura |
| Importa oggetti personalizzati | Importazione in blocco di oggetti personalizzati | POST | /bulk/v1/customobjects/{apiName}/import.json | Oggetto personalizzato di lettura/scrittura |
| Recupera errori lead di importazione | Lead importazione in blocco | GET | /bulk/v1/lead/batch/{id}/failures.json | Lead di lettura/scrittura |
| Ottieni stato lead di importazione | Lead importazione in blocco | GET | /bulk/v1/lead/batch/{id}.json | Lead di lettura/scrittura |
| Ottieni avvisi lead di importazione | Lead importazione in blocco | GET | /bulk/v1/lead/batch/{id}/warnings.json | Lead di lettura/scrittura |
| Importa lead | Lead importazione in blocco | POST | /bulk/v1/leads.json | Lead di lettura/scrittura |
| Recupera errori membri del programma di importazione | Membri del programma Importazione in blocco | GET | /bulk/v1/program/members/import/{id}/failures.json | Lead di lettura/scrittura |
| Ottieni stato membro del programma di importazione | Membri del programma Importazione in blocco | GET | /bulk/v1/program/members/import/{id}/status.json | Lead di lettura/scrittura |
| Ottieni avvisi per i membri del programma di importazione | Membri del programma Importazione in blocco | GET | /bulk/v1/program/members/import/{id}/warnings.json | Lead di lettura/scrittura |
| Importa membri del programma | Membri del programma Importazione in blocco | POST | /bulk/v1/program/{programId}/members/import.json | Lead di lettura/scrittura |
| Ottieni campagna per ID | Campagne | GET | /rest/v1/campaigns/{id}.json | Campagne di sola lettura |
| Recupera campagne | Campagne | GET | /rest/v1/campaigns.json | Campagne di sola lettura |
| Richiedere campagna | Campagne | POST | /rest/v1/campaigns/{id}/trigger.json | Campagne di lettura/scrittura |
| Pianifica campagna | Campagne | POST | /rest/v1/campaigns/{id}/schedule.json | Campagne di lettura/scrittura |
| Ottieni canale per nome | Canali | GET | /rest/asset/v1/channel/byName.json | Risorsa di sola lettura |
| Ottieni canali | Canali | GET | /rest/asset/v1/channels.json | Risorsa di sola lettura |
| Elimina società | Aziende | POST | /rest/v1/companies/delete.json | Società di lettura/scrittura |
| Descrizione delle società | Aziende | GET | /rest/v1/companies/describe.json | Società di sola lettura |
| Ottieni Aziende | Aziende | GET | /rest/v1/companies.json | Società di sola lettura |
| Sincronizza società | Aziende | POST | /rest/v1/companies.json | Società di lettura/scrittura |
| Ottieni campo società per nome | Aziende | GET | /rest/v1/companies/schema/fields/{fieldApiName}.json | Campo personalizzato schema di lettura-scrittura |
| Recupera campi società | Aziende | GET | /rest/v1/companies/schema/fields.json | Campo personalizzato schema di lettura-scrittura |
| Aggiungi campi tipo di oggetto personalizzato | Oggetti personalizzati | POST | /rest/v1/customobjects/schema/{apiName}/addField.json | Tipo di oggetto personalizzato di lettura/scrittura |
| Approva tipo di oggetto personalizzato | Oggetti personalizzati | POST | /rest/v1/customobjects/schema/{apiName}/approve.json | Tipo di oggetto personalizzato di lettura/scrittura |
| Elimina oggetti personalizzati | Oggetti personalizzati | POST | /rest/v1/customobjects/{name}/delete.json | Oggetto personalizzato di lettura/scrittura |
| Elimina tipo di oggetto personalizzato | Oggetti personalizzati | POST | /rest/v1/customobjects/schema/{apiName}/delete.json | Tipo di oggetto personalizzato di lettura/scrittura |
| Elimina campi tipo di oggetto personalizzato | Oggetti personalizzati | POST | /rest/v1/customobjects/schema/{apiName}/deleteField.json | Tipo di oggetto personalizzato di lettura/scrittura |
| Descrivi oggetti personalizzati | Oggetti personalizzati | GET | /rest/v1/customobjects/{name}/describe.json | Oggetto personalizzato di sola lettura |
| Descrivi tipo di oggetto personalizzato | Oggetti personalizzati | GET | /rest/v1/customobjects/schema/{apiName}/describe.json | Tipo di oggetto personalizzato di sola lettura |
| Elimina bozza tipo di oggetto personalizzato | Oggetti personalizzati | POST | /rest/v1/customobjects/schema/{apiName}/discardDraft.json | Tipo di oggetto personalizzato di lettura/scrittura |
| Ottieni oggetti personalizzati | Oggetti personalizzati | GET | /rest/v1/customobjects/{name}.json | Oggetto personalizzato di sola lettura |
| Ottieni oggetti collegabili a oggetti personalizzati | Oggetti personalizzati | GET | /rest/v1/customobjects/schema/linkableObjects.json | Tipo di oggetto personalizzato di sola lettura |
| Ottieni Assets personalizzato dipendente da oggetti | Oggetti personalizzati | GET | /rest/v1/customobjects/schema/{apiName}/dependentAssets.json | Tipo di oggetto personalizzato di sola lettura |
| Ottieni tipi di dati per campi tipo di oggetto personalizzato | Oggetti personalizzati | GET | /rest/v1/customobjects/schema/fieldDataTypes.json | Tipo di oggetto personalizzato di sola lettura |
| Elenco di oggetti personalizzati | Oggetti personalizzati | GET | /rest/v1/customobjects.json | Oggetto personalizzato di sola lettura |
| Elenco dei tipi di oggetti personalizzati | Oggetti personalizzati | GET | /rest/v1/customobjects/schema.json | Tipo di oggetto personalizzato di sola lettura |
| Sincronizza oggetti personalizzati | Oggetti personalizzati | POST | /rest/v1/customobjects/{name}.json | Oggetto personalizzato di lettura/scrittura |
| Sincronizza tipo di oggetto personalizzato | Oggetti personalizzati | POST | /rest/v1/customobjects/schema.json | Tipo di oggetto personalizzato di lettura/scrittura |
| Aggiorna campo tipo di oggetto personalizzato | Oggetti personalizzati | POST | /rest/v1/customobjects/schema/{apiName}/updateField.json | Tipo di oggetto personalizzato di lettura/scrittura |
| Approva bozza modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplate/{id}/approveDraft.json | Risorsa di lettura/scrittura |
| Clona modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplate/{id}/clone.json | Risorsa di lettura/scrittura |
| Crea modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplates.json | Risorsa di lettura/scrittura |
| Elimina modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplate/{id}/delete.json | Risorsa di lettura/scrittura |
| Elimina bozza modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplate/{id}/discardDraft.json | Risorsa di lettura/scrittura |
| Ottieni modello e-mail per ID | Modelli e-mail | GET | /rest/asset/v1/emailTemplate/{id}.json | Risorsa di sola lettura |
| Ottieni modello e-mail per nome | Modelli e-mail | GET | /rest/asset/v1/emailTemplate/byName.json | Risorsa di sola lettura |
| Ottieni contenuto modello e-mail per ID | Modelli e-mail | GET | /rest/asset/v1/emailTemplate/{id}/content.json | Risorsa di sola lettura |
| Ottieni modello e-mail utilizzato da | Modelli e-mail | GET | /rest/asset/v1/emailTemplates/{id}/usedBy.json | Risorsa di sola lettura |
| Ottieni modelli e-mail | Modelli e-mail | GET | /rest/asset/v1/emailTemplates.json | Risorsa di sola lettura |
| Annulla approvazione bozza modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplate/{id}/unapprove.json | Risorsa di lettura/scrittura |
| Aggiorna contenuto modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplate/{id}/content.json | Risorsa di lettura/scrittura |
| Aggiorna metadati modello e-mail | Modelli e-mail | POST | /rest/asset/v1/emailTemplate/{id}.json | Risorsa di lettura/scrittura |
| Aggiungi modulo e-mail | E-mail | POST | /rest/asset/v1/email/{id}/content/{moduleId}/add.json | Risorsa di lettura/scrittura |
| Approva bozza e-mail | E-mail | POST | /rest/asset/v1/email/{id}/approveDraft.json | Risorsa di lettura/scrittura |
| Clona e-mail | E-mail | POST | /rest/asset/v1/email/{id}/clone.json | Risorsa di lettura/scrittura |
| Crea e-mail | E-mail | POST | /rest/asset/v1/emails.json | Risorsa di lettura/scrittura |
| Elimina e-mail | E-mail | POST | /rest/asset/v1/email/{id}/delete.json | Risorsa di lettura/scrittura |
| Elimina modulo | E-mail | POST | /rest/asset/v1/email/{id}/content/{moduleId}/delete.json | Risorsa di lettura/scrittura |
| Elimina bozza e-mail | E-mail | POST | /rest/asset/v1/email/{id}/discardDraft.json | Risorsa di lettura/scrittura |
| Duplica modulo e-mail | E-mail | POST | /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json | Risorsa di lettura/scrittura |
| Ottieni e-mail per ID | E-mail | GET | /rest/asset/v1/email/{id}.json | Risorsa di sola lettura |
| Ricevi e-mail per nome | E-mail | GET | /rest/asset/v1/email/byName.json | Risorsa di sola lettura |
| Ottieni contenuto e-mail | E-mail | GET | /rest/asset/v1/email/{id}/content.json | Risorsa di sola lettura |
| Ottieni contenuto dinamico e-mail | E-mail | GET | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Risorsa di sola lettura |
| Ottieni contenuto completo e-mail | E-mail | GET | /rest/asset/v1/email/{id}/fullContent.json | Risorsa di sola lettura |
| Ottieni variabili e-mail | E-mail | GET | /rest/asset/v1/email/{id}/variables.json | Risorsa di sola lettura |
| Ottieni campi CC e-mail | E-mail | GET | /rest/asset/v1/email/ccFields.json | Risorsa di sola lettura |
| Ricevi e-mail | E-mail | GET | /rest/asset/v1/emails.json | Risorsa di sola lettura |
| Ridisponi moduli e-mail | E-mail | POST | /rest/asset/v1/email/{id}/content/rearrange.json | Risorsa di lettura/scrittura |
| Rinomina modulo e-mail | E-mail | POST | /rest/asset/v1/email/{id}/content/{moduleId}/rename.json | Risorsa di lettura/scrittura |
| Invia e-mail di esempio | E-mail | POST | /rest/asset/v1/email/{id}/sendSample.json | Risorsa di lettura/scrittura |
| Annulla approvazione e-mail | E-mail | POST | /rest/asset/v1/email/{id}/unapprove.json | Risorsa di lettura/scrittura |
| Aggiorna contenuto e-mail | E-mail | POST | /rest/asset/v1/email/{id}/content.json | Risorsa di lettura/scrittura |
| Sezione Aggiorna contenuto e-mail | E-mail | POST | /rest/asset/v1/email/{id}/content/{htmlId}.json | Risorsa di lettura/scrittura |
| Sezione Aggiornamento contenuto dinamico e-mail | E-mail | POST | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId}.json | Risorsa di lettura/scrittura |
| Aggiorna contenuto completo e-mail | E-mail | POST | /rest/asset/v1/emails/{id}/fullContent.json | Risorsa di lettura/scrittura |
| Aggiorna metadati e-mail | E-mail | POST | /rest/asset/v1/email/{id}.json | Risorsa di lettura/scrittura |
| Aggiorna variabile e-mail | E-mail | POST | /rest/asset/v1/email/{id}/variable/{name}.json | Risorsa di lettura/scrittura |
| Crea file | File | POST | /rest/asset/v1/files.json | Risorsa di lettura/scrittura |
| Ottieni file per ID | File | GET | /rest/asset/v1/file/{id}.json | Risorsa di sola lettura |
| Ottieni file per nome | File | GET | /rest/asset/v1/file/byName.json | Risorsa di sola lettura |
| Ottieni file | File | GET | /rest/asset/v1/files.json | Risorsa di sola lettura |
| Aggiorna contenuto file | File | POST | /rest/asset/v1/file/{id}/content.json | Risorsa di lettura/scrittura |
| Crea cartella | Cartelle | POST | /rest/asset/v1/folders.json | Risorsa di lettura/scrittura |
| Elimina cartella | Cartelle | POST | /rest/asset/v1/folder/{id}/delete.json | Risorsa di lettura/scrittura |
| Ottieni cartella per ID | Cartelle | GET | /rest/asset/v1/folder/{id}.json | Risorsa di sola lettura |
| Ottieni cartella per nome | Cartelle | GET | /rest/asset/v1/folder/byName.json | Risorsa di sola lettura |
| Ottieni contenuto cartella | Cartelle | GET | /rest/asset/v1/folder/{id}/content.json | Risorsa di sola lettura |
| Ottieni cartelle | Cartelle | GET | /rest/asset/v1/folders.json | Risorsa di sola lettura |
| Aggiorna metadati cartella | Cartelle | POST | /rest/asset/v1/folder/{id}.json | Risorsa di lettura/scrittura |
| Aggiungi campo al modulo | Campi del modulo | POST | /rest/asset/v1/form/{id}/fields.json | Risorsa di lettura/scrittura |
| Aggiungi set di campi al modulo | Campi del modulo | POST | /rest/asset/v1/form/{id}/fieldSet.json | Risorsa di lettura/scrittura |
| Aggiungi regole di visibilità campo modulo | Campi del modulo | POST | /rest/asset/v1/form/{formId}/field/{fieldId}/visibility.json | Risorsa di lettura/scrittura |
| Aggiungi campo Rich Text | Campi del modulo | POST | /rest/asset/v1/form/{id}/richText.json | Risorsa di lettura/scrittura |
| Elimina campo da set di campi | Campi del modulo | POST | /rest/asset/v1/form/{id}/fieldSet/{fieldSetId}/field/{fieldId}/delete.json | Risorsa di lettura/scrittura |
| Elimina campo modulo | Campi del modulo | POST | /rest/asset/v1/form/{id}/field/{fieldId}/delete.json | Risorsa di lettura/scrittura |
| Ottieni campi modulo disponibili | Campi del modulo | GET | /rest/asset/v1/form/fields.json | Risorsa di sola lettura |
| Recupera campi membri del programma modulo disponibili | Campi del modulo | GET | /rest/asset/v1/form/programMemberFields.json | Risorsa di sola lettura |
| Recupera campi per modulo | Campi del modulo | GET | /rest/asset/v1/form/{id}/fields.json | Risorsa di sola lettura |
| Aggiorna posizioni campo | Campi del modulo | POST | /rest/asset/v1/form/{id}/reArrange.json | Risorsa di lettura/scrittura |
| Aggiorna campo modulo | Campi del modulo | POST | /rest/asset/v1/form/{id}/field/{fieldId}.json | Risorsa di lettura/scrittura |
| Approva bozza modulo | Forms | POST | /rest/asset/v1/form/{id}/approveDraft.json | Risorsa di lettura/scrittura |
| Clona modulo | Forms | POST | /rest/asset/v1/form/{id}/clone.json | Risorsa di lettura/scrittura |
| Crea modulo | Forms | POST | /rest/asset/v1/forms.json | Risorsa di lettura/scrittura |
| Ottieni modulo utilizzato da | Forms | GET | /rest/asset/v1/form/{id}/usedBy.json | Risorsa di lettura/scrittura |
| Elimina modulo | Forms | POST | /rest/asset/v1/form/{id}/delete.json | Risorsa di lettura/scrittura |
| Elimina bozza modulo | Forms | POST | /rest/asset/v1/form/{id}/discardDraft.json | Risorsa di lettura/scrittura |
| Ottieni modulo per ID | Forms | GET | /rest/asset/v1/form/{id}.json | Risorsa di sola lettura |
| Ottieni modulo per nome | Forms | GET | /rest/asset/v1/form/byName.json | Risorsa di sola lettura |
| Ottieni Forms | Forms | GET | /rest/asset/v1/forms.json | Risorsa di sola lettura |
| Ottieni pagina di ringraziamento per ID modulo | Forms | GET | /rest/asset/v1/form/{id}/thankYouPage.json | Risorsa di sola lettura |
| Aggiorna metadati modulo | Forms | POST | /rest/asset/v1/form/{id}.json | Risorsa di lettura/scrittura |
| Pulsante Aggiorna invio | Forms | POST | /rest/asset/v1/{id}/submitButton.json | Risorsa di lettura/scrittura |
| Aggiorna pagina di ringraziamento | Forms | POST | /rest/asset/v1/form/{id}/thankYouPage.json | Risorsa di lettura/scrittura |
| Aggiungi sezione contenuto pagina di destinazione | Contenuto della pagina di destinazione | POST | /rest/asset/v1/landingPage/{id}/content.json | Risorsa di lettura/scrittura |
| Elimina sezione contenuto pagina di destinazione | Contenuto della pagina di destinazione | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}/delete.json | Risorsa di lettura/scrittura |
| Ottieni contenuto pagina di destinazione | Contenuto della pagina di destinazione | GET | /rest/asset/v1/landingPage/{id}/content.json | Risorsa di sola lettura |
| Ottieni contenuto dinamico pagina di destinazione | Contenuto della pagina di destinazione | GET | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Risorsa di sola lettura |
| Sezione Aggiorna contenuto pagina di destinazione | Contenuto della pagina di destinazione | POST | /rest/asset/v1/landingPage/{id}/content/{contentId}.json | Risorsa di lettura/scrittura |
| Aggiorna sezione contenuti dinamici della pagina di destinazione | Contenuto della pagina di destinazione | POST | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json | Risorsa di lettura/scrittura |
| Approva bozza modello pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate/{id}/approveDraft.json | Risorsa di lettura/scrittura |
| Clona modello pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate/{id}/clone.json | Risorsa di lettura/scrittura |
| Crea modello per pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate.json | Risorsa di lettura/scrittura |
| Elimina modello pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate/{id}/delete.json | Risorsa di lettura/scrittura |
| Elimina bozza modello pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate/{id}/discardDraft.json | Risorsa di lettura/scrittura |
| Ottieni modello pagina di destinazione per ID | Modelli pagina di destinazione | GET | /rest/asset/v1/landingPageTemplate/{id}.json | Risorsa di sola lettura |
| Ottieni modello pagina di destinazione per nome | Modelli pagina di destinazione | GET | /rest/asset/v1/landingPageTemplates/byName.json | Risorsa di sola lettura |
| Ottieni contenuto modello pagina di destinazione | Modelli pagina di destinazione | GET | /rest/asset/v1/landingPageTemplate/{id}/content.json | Risorsa di sola lettura |
| Ottieni modelli pagina di destinazione | Modelli pagina di destinazione | GET | /rest/asset/v1/landingPageTemplates.json | Risorsa di sola lettura |
| Annulla approvazione modello pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate/{id}/unapprove.json | Risorsa di lettura/scrittura |
| Aggiorna contenuto modello pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate/{id}/content.json | Risorsa di lettura/scrittura |
| Aggiorna metadati modello pagina di destinazione | Modelli pagina di destinazione | POST | /rest/asset/v1/landingPageTemplate/{id}.json | Risorsa di lettura/scrittura |
| Approva bozza pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/landingPage/{id}/approveDraft.json | Risorsa di lettura/scrittura |
| Clona pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/landingPage/{id}/clone.json | Risorsa di lettura/scrittura |
| Creare pagine di destinazione | Pagine di destinazione | POST | /rest/asset/v1/landingPages.json | Risorsa di lettura/scrittura |
| Elimina pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/landingPage/{id}/delete.json | Risorsa di lettura/scrittura |
| Elimina bozza pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/landingPage/{id}/discardDraft.json | Risorsa di lettura/scrittura |
| Ottieni pagina di destinazione per ID | Pagine di destinazione | GET | /rest/asset/v1/landingPage/{id}.json | Risorsa di sola lettura |
| Ottieni pagina di destinazione per nome | Pagine di destinazione | GET | /rest/asset/v1/landingPage/byName.json | Risorsa di sola lettura |
| Ottieni variabili pagina di destinazione | Pagine di destinazione | GET | /rest/asset/v1/landingPage/{id}/variables.json | Risorsa di sola lettura |
| Ottieni pagine di destinazione | Pagine di destinazione | GET | /rest/asset/v1/landingPages.json | Risorsa di sola lettura |
| Anteprima pagina di destinazione | Pagine di destinazione | GET | /rest/asset/v1/landingPage/{id}/preview.json | Risorsa di sola lettura |
| Annulla approvazione pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/landingPage/{id}/unapprove.json | Risorsa di lettura/scrittura |
| Aggiorna metadati pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/{id}.json | Risorsa di lettura/scrittura |
| Aggiorna variabili della pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/landingPage/{id}/variable/{variableId}.json | Risorsa di lettura/scrittura |
| Creare regole di reindirizzamento per la pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/redirectRules.json | Regole di reindirizzamento di lettura-scrittura |
| Elimina regola di reindirizzamento pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/redirectRule/{id}/delete.json | Regole di reindirizzamento di lettura-scrittura |
| Ottieni regole di reindirizzamento pagina di destinazione | Pagine di destinazione | GET | /rest/asset/v1/redirectRules.json | Regole di reindirizzamento di sola lettura |
| Ottieni regola di reindirizzamento pagina di destinazione per ID | Pagine di destinazione | GET | /rest/asset/v1/redirectRule/{id}.json | Regole di reindirizzamento di sola lettura |
| Aggiorna regola di reindirizzamento pagina di destinazione | Pagine di destinazione | POST | /rest/asset/v1/redirectRule/{id}.json | Regole di reindirizzamento di lettura-scrittura |
| Ottieni domini pagina di destinazione | Pagine di destinazione | GET | /rest/asset/v1/landingPageDomains.json | Regole di reindirizzamento di sola lettura |
| Associa lead | Lead | POST | /rest/v1/lead/{id}/associate.json | Lead di lettura/scrittura |
| Modifica stato programma lead | Lead | POST | /rest/v1/lead/programmi/{programId}/status.json | Lead di lettura/scrittura |
| Elimina lead | Lead | POST | /rest/v1/leads.json | Lead di lettura/scrittura |
| Descrivi lead | Lead | GET | /rest/v1/leads/describe.json | Lead di sola lettura |
| Descrivi lead2 | Lead | GET | /rest/v1/leads/describe2.json | Lead di sola lettura |
| Descrivi membro programma | Lead | GET | /rest/v1/program/members/describe.json | Lead di sola lettura |
| Ottieni lead per ID | Lead | GET | /rest/v1/lead/{id}.json | Lead di sola lettura |
| Ottieni partizioni lead | Lead | GET | /rest/v1/leads/partitions.json | Lead di sola lettura |
| Ottieni lead per tipo di filtro | Lead | GET | /rest/v1/leads.json | Lead di sola lettura |
| Ottieni lead per ID programma | Lead | GET | /rest/v1/lead/programs/{programId}.json | Lead di sola lettura |
| Unisci lead | Lead | POST | /rest/v1/lead/{id}/merge.json | Lead di lettura/scrittura |
| Ottieni elenchi per ID lead | Lead | GET | /rest/v1/lead/{leadId}.json | Risorsa di sola lettura |
| Ottieni programmi per ID lead | Lead | GET | /rest/v1/lead/{leadId}programMembership.json | Risorsa di sola lettura |
| Ottieni campagne intelligenti per ID lead | Lead | GET | /rest/v1/lead/{leadId}/smartCampaignMembership.json | Risorsa di sola lettura |
| Invia lead a Marketo | Lead | POST | /rest/v1/leads/partitions.json | Lead di lettura/scrittura |
| Invia modulo | Lead | POST | /rest/v1/leads/submitForm.json | Lead di lettura/scrittura |
| Sincronizza lead | Lead | POST | /rest/v1/leads.json | Lead di lettura/scrittura |
| Aggiorna partizione lead | Lead | POST | /rest/v1/leads/partitions.json | Lead di lettura/scrittura |
| Ottieni campo lead per nome | Lead | GET | /rest/v1/lead/schema/fields/{fieldApiName}.json | Campo personalizzato schema di lettura-scrittura |
| Ottieni campi lead | Lead | GET | /rest/v1/leads/schema/fields.json | Campo personalizzato schema di lettura-scrittura |
| Crea campi lead | Lead | POST | /rest/v1/leads/schema/fields.json | Campo personalizzato schema di lettura-scrittura |
| Aggiorna campo lead | Lead | POST | /rest/v1/lead/schema/fields/{fieldApiName}.json | Campo personalizzato schema di lettura-scrittura |
| Aggiungi membri elenco account denominati | Elenchi di account denominati | POST | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Account denominato di lettura-scrittura |
| Elimina elenchi account denominati | Elenchi di account denominati | POST | /rest/v1/namedaccountlists/delete.json | Elenco account denominati di lettura-scrittura |
| Ottieni membri elenco account denominati | Elenchi di account denominati | GET | /rest/v1/namedaccountlist/{id}/namedaccounts.json | Account denominato di sola lettura |
| Ottieni elenchi account denominati | Elenchi di account denominati | GET | /rest/v1/namedaccountlists.json | Elenco di account denominati di sola lettura |
| Rimuovi membri elenco account denominati | Elenchi di account denominati | POST | /rest/v1/namedaccountlist/{id}/namedaccounts/remove.json | Account denominato di lettura-scrittura |
| Sincronizza elenchi account denominati | Elenchi di account denominati | POST | /rest/v1/namedaccountlists.json | Elenco account denominati di lettura-scrittura |
| Elimina account denominati | Account denominati | POST | /rest/v1/namedaccounts/delete.json | Account denominato di lettura-scrittura |
| Descrizione account denominati | Account denominati | GET | /rest/v1/namedaccounts/describe.json | Account denominato di sola lettura |
| Ottieni account denominati | Account denominati | GET | /rest/v1/namedaccounts.json | Account denominato di sola lettura |
| Sincronizza account denominati | Account denominati | POST | /rest/v1/namedaccounts.json | Account denominato di lettura-scrittura |
| Ottieni campo account denominato per nome | Account denominati | GET | /rest/v1/namedaccounts/schema/fields/{fieldApiName}.json | Campo personalizzato schema di lettura-scrittura |
| Recupera campi account denominati | Account denominati | GET | /rest/v1/namedaccounts/schema/fields.json | Campo personalizzato schema di lettura-scrittura |
| Elimina opportunità | Opportunità | POST | /rest/v1/opportunities/delete.json | Opportunità di lettura/scrittura |
| Elimina ruoli opportunità | Opportunità | POST | /rest/v1/opportunities/roles/delete.json | Opportunità di lettura/scrittura |
| Descrivi opportunità | Opportunità | GET | /rest/v1/opportunities/describe.json | Opportunità di sola lettura |
| Descrizione del ruolo opportunità | Opportunità | GET | /rest/v1/opportunities/roles/describe.json | Opportunità di sola lettura |
| Ottieni opportunità | Opportunità | GET | /rest/v1/opportunities.json | Opportunità di sola lettura |
| Ottieni ruoli opportunità | Opportunità | GET | /rest/v1/opportunities/roles.json | Opportunità di sola lettura |
| Opportunità di sincronizzazione | Opportunità | POST | /rest/v1/opportunities.json | Opportunità di lettura/scrittura |
| Sincronizza ruoli opportunità | Opportunità | POST | /rest/v1/opportunities/roles.json | Opportunità di lettura/scrittura |
| Ottieni campo opportunità per nome | Opportunità | GET | /rest/v1/offers/schema/fields/{fieldApiName}.json | Campo personalizzato schema di lettura-scrittura |
| Ottieni campi opportunità | Opportunità | GET | /rest/v1/opportunities/schema/fields.json | Campo personalizzato schema di lettura-scrittura |
| Elimina membri del programma | Membri del programma | POST | /rest/v1/programs/{programId}/members/delete.json | Lead di lettura/scrittura |
| Descrivi membro programma | Membri del programma | GET | /rest/v1/programs/members/describe.json | Lead di sola lettura |
| Ottieni membri del programma | Membri del programma | GET | /rest/v1/programs/{programId}/members.json | Lead di sola lettura |
| Sincronizza dati membro programma | Membri del programma | POST | /rest/v1/programs/{programId}/members.json | Lead di lettura/scrittura |
| Sincronizza stato membro programma | Membri del programma | POST | /rest/v1/programs/{programId}/members/status.json | Lead di lettura/scrittura |
| Ottieni campo membro programma per nome | Membri del programma | GET | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Campo personalizzato schema di lettura-scrittura |
| Recupera campi membri del programma | Membri del programma | GET | /rest/v1/programs/members/schema/fields.json | Campo personalizzato schema di lettura-scrittura |
| Crea campi membri del programma | Membri del programma | POST | /rest/v1/programs/members/schema/fields.json | Campo personalizzato schema di lettura-scrittura |
| Aggiorna campo membro del programma | Membri del programma | POST | /rest/v1/programs/members/schema/fields/{fieldApiName}.json | Campo personalizzato schema di lettura-scrittura |
| Approva programma | Programmi | POST | /rest/asset/v1/program/{id}/approve.json | Risorsa di lettura/scrittura |
| Clona programma | Programmi | POST | /rest/asset/v1/program/{id}/clone.json | Risorsa di lettura/scrittura |
| Creare i programmi | Programmi | POST | /rest/asset/v1/programs.json | Risorsa di lettura/scrittura |
| Elimina programma | Programmi | POST | /rest/asset/v1/program/{id}/delete.json | Risorsa di lettura/scrittura |
| Ottieni programma per ID | Programmi | GET | /rest/asset/v1/program/{id}.json | Risorsa di sola lettura |
| Ottieni programma per nome | Programmi | GET | /rest/asset/v1/program/byName.json | Risorsa di sola lettura |
| Ottieni programmi | Programmi | GET | /rest/asset/v1/programs.json | Risorsa di sola lettura |
| Ottieni programmi per tag | Programmi | GET | /rest/asset/v1/program/byTag.json | Risorsa di sola lettura |
| Ottieni elenco avanzato per ID programma | Programmi | GET | /rest/asset/v1/program/{id}/smartList.json | Risorsa di sola lettura |
| Annulla approvazione programma | Programmi | POST | /rest/asset/v1/program/{id}/unapprove.json | Risorsa di lettura/scrittura |
| Aggiorna metadati programma | Programmi | POST | /rest/asset/v1/program/{id}.json | Risorsa di lettura/scrittura |
| Aggiorna tag programma | Programmi | POST | /rest/asset/v1/program/{id}/tag/{tagType}.json | Risorsa di lettura/scrittura |
| Elimina tag programma | Programmi | POST | /rest/asset/v1/program/{id}/tag/{tagType}/delete.json | Risorsa di lettura/scrittura |
| Elimina SalesPersons | Venditori | POST | /rest/v1/salespersons/delete.json | Persona di vendita di lettura/scrittura |
| Descrivi Venditori | Venditori | GET | /rest/v1/salespersons/describe.json | Venditore di sola lettura |
| Ottieni addetti alle vendite | Venditori | GET | /rest/v1/salespersons.json | Venditore di sola lettura |
| Sincronizza SalesPersons | Venditori | POST | /rest/v1/salespersons.json | Persona di vendita di lettura/scrittura |
| Ottieni segmentazioni | Segmenti | GET | /rest/asset/v1/segmentation.json | Risorsa di sola lettura |
| Ottieni Segmenti Per Segmentazioni | Segmenti | GET | /rest/asset/v1/segmentation/{id}/segments.json | Risorsa di sola lettura |
| Attiva campagna avanzata | Campagne avanzate | POST | /rest/asset/v1/smartCampaign/{id}/activate.json | Attiva campagna |
| Clona campagna avanzata | Campagne avanzate | POST | /rest/asset/v1/smartCampaign/{id}/clone.json | Risorsa di lettura/scrittura |
| Creare una campagna avanzata | Campagne avanzate | POST | /rest/asset/v1/smartCampaigns.json | Risorsa di lettura/scrittura |
| Disattivare la campagna avanzata | Campagne avanzate | POST | /rest/asset/v1/smartCampaign/{id}/deactivate.json | Disattiva campagna |
| Elimina campagna avanzata | Campagne avanzate | POST | /rest/asset/v1/smartCampaign/{id}/delete.json | Risorsa di lettura/scrittura |
| Ottieni campagne intelligenti | Campagne avanzate | GET | /rest/asset/v1/smartCampaigns.json | Risorsa di sola lettura |
| Ottieni campagna avanzata per ID | Campagne avanzate | GET | /rest/asset/v1/smartCampaign/{id}.json | Risorsa di sola lettura |
| Ottieni campagna avanzata per nome | Campagne avanzate | GET | /rest/asset/v1/smartCampaign/byName.json | Risorsa di sola lettura |
| Ottieni elenco avanzato per ID campagna avanzato | Campagne avanzate | GET | /rest/asset/v1/smartCampaign/{id}/smartList.json | Risorsa di sola lettura |
| Aggiornare Smart Campaign | Campagne avanzate | POST | /rest/asset/v1/smartCampaign/{id}.json | Risorsa di lettura/scrittura |
| Clona elenco avanzato | Elenchi avanzati | POST | /rest/asset/v1/smartList/{id}/clone.json | Risorsa di lettura/scrittura |
| Elimina elenco avanzato | Elenchi avanzati | POST | /rest/asset/v1/smartList/{id}/delete.json | Risorsa di lettura/scrittura |
| Ottieni elenco avanzato per ID | Elenchi avanzati | GET | /rest/asset/v1/smartList/{id}.json | Risorsa di sola lettura |
| Ottieni elenco avanzato per nome | Elenchi avanzati | GET | /rest/asset/v1/smartList/byName.json | Risorsa di sola lettura |
| Ottieni elenchi avanzati | Elenchi avanzati | GET | /rest/asset/v1/smartLists.json | Risorsa di sola lettura |
| Approva bozza frammento | Snippet | POST | /rest/asset/v1/snippet/{id}/approveDraft.json | Risorsa di lettura/scrittura |
| Clona snippet | Snippet | POST | /rest/asset/v1/snippet/{id}/clone.json | Risorsa di lettura/scrittura |
| Crea snippet | Snippet | POST | /rest/asset/v1/snippets.json | Risorsa di lettura/scrittura |
| Elimina snippet | Snippet | POST | /rest/asset/v1/snippet/{id}/delete.json | Risorsa di lettura/scrittura |
| Elimina bozza frammento | Snippet | POST | /rest/asset/v1/snippet/{id}/discardDraft.json | Risorsa di lettura/scrittura |
| Ottieni contenuto dinamico | Snippet | GET | /rest/asset/v1/snippet/{id}/dynamicContent.json | Risorsa di sola lettura |
| Ottieni snippet per ID | Snippet | GET | /rest/asset/v1/snippet/{id}.json | Risorsa di sola lettura |
| Ottieni contenuto frammento | Snippet | GET | /rest/asset/v1/snippet/{id}/content.json | Risorsa di sola lettura |
| Recupera snippet | Snippet | GET | /rest/asset/v1/snippets.json | Risorsa di sola lettura |
| Annulla approvazione snippet | Snippet | POST | /rest/asset/v1/snippet/{id}/unapprove.json | Risorsa di lettura/scrittura |
| Aggiorna contenuto snippet | Snippet | POST | /rest/asset/v1/snippet/{id}/content.json | Risorsa di lettura/scrittura |
| Aggiorna contenuto dinamico frammento | Snippet | POST | /rest/asset/v1/snippet/{id}/dynamicContent/{segmentId}.json | Risorsa di lettura/scrittura |
| Aggiorna metadati frammento | Snippet | POST | /rest/asset/v1/snippet/{id}.json | Risorsa di lettura/scrittura |
| Aggiungi all’elenco | Elenchi statici | POST | /rest/v1/LISTS/{listId}/leads.json | Lead di lettura/scrittura |
| Crea elenco statico | Elenchi statici | POST | /asset/v1/staticLists.json | Risorsa di lettura/scrittura |
| Elimina elenco statico | Elenchi statici | POST | /asset/v1/staticList/{id}/delete.json | Risorsa di lettura/scrittura |
| Ottieni lead per ID elenco | Elenchi statici | GET | /rest/v1/LISTS/{listId}/leads.json | Lead di sola lettura |
| Ottieni elenco per ID | Elenchi statici | GET | /rest/v1/LISTS/{id}.json | Lead di sola lettura |
| Ottieni elenchi | Elenchi statici | GET | /rest/v1/lists.json | Lead di sola lettura |
| Ottieni elenco statico per ID | Elenchi statici | GET | /asset/v1/staticList/{id}.json | Risorsa di sola lettura |
| Ottieni elenco statico per nome | Elenchi statici | GET | /asset/v1/staticList/byName.json | Risorsa di sola lettura |
| Ottieni elenchi statici | Elenchi statici | GET | /asset/v1/staticLists.json | Risorsa di sola lettura |
| Membro dell&#39;elenco | Elenchi statici | GET | /rest/v1/LISTS/{listId}/leads/ismember.json | Lead di sola lettura |
| Rimuovi dall’elenco | Elenchi statici | DELETE | /rest/v1/LISTS/{listId}/leads.json | Lead di lettura/scrittura |
| Aggiorna metadati elenco statico | Elenchi statici | POST | /asset/v1/staticList/{id}.json | Risorsa di lettura/scrittura |
| Ottieni tag per nome | Tag | GET | /rest/asset/v1/tagType/byName.json | Risorsa di sola lettura |
| Ottieni tipi di tag | Tag | GET | /rest/asset/v1/tagTypes.json | Risorsa di sola lettura |
| Crea token | Token | POST | /rest/asset/v1/folder/{id}/tokens.json | Risorsa di lettura/scrittura |
| Elimina token per nome | Token | POST | /rest/asset/v1/folder/{id}/tokens/delete.json | Risorsa di lettura/scrittura |
| Ottieni token per ID cartella | Token | GET | /rest/asset/v1/folder/{id}/tokens.json | Risorsa di sola lettura |
| Aggiungi Ruoli | Gestione utente | POST | /userservice/management/v1/users/{userid}/roles/create.json | Accedere All’Api User Management |
| Elimina utente invitato | Gestione utente | POST | /userservice/management/v1/users/{userId}/invite/delete.json | Accedere All’Api User Management |
| Elimina Ruoli | Gestione utente | POST | /userservice/management/v1/users/{userid}/roles/delete.json | Accedere All’Api User Management |
| Elimina utente | Gestione utente | POST | /userservice/management/v1/users/{userId}/delete.json | Accedere All’Api User Management |
| Ottieni utente invitato per ID | Gestione utente | GET | /userservice/management/v1/users/{userid}/invite.json | Accedere All’Api User Management |
| Ottieni ruoli | Gestione utente | GET | /userservice/management/v1/users/roles.json | Accedere All’Api User Management |
| Ottieni ruoli e aree di lavoro per ID | Gestione utente | GET | /userservice/management/v1/users/{userid}/roles.json | Accedere All’Api User Management |
| Ottieni utenti | Gestione utente | GET | /userservice/management/v1/users/allusers.json | Accedere All’Api User Management |
| Ottieni utente per ID | Gestione utente | GET | /userservice/management/v1/users/{userid}/user.json | Accedere All’Api User Management |
| Ottieni aree di lavoro | Gestione utente | GET | /userservice/management/v1/users/workspaces.json | Accedere All’Api User Management |
| Invita utente | Gestione utente | POST | /userservice/management/v1/users/invite.json | Accedere All’Api User Management |
| Aggiorna attributi utente | Gestione utente | POST | /userservice/management/v1/users/{userId}/update.json | Accedere All’Api User Management |
