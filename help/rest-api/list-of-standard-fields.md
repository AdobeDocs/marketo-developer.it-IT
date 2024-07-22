---
title: Campi standard
feature: REST API, Field Management
description: Tabella di campi standard di Marketo.
exl-id: 147dbdff-4bc9-4ab3-8918-c4de3e1aa97a
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 11%

---

# Campi standard

Di seguito è riportato un elenco dei campi standard disponibili in Marketo che sono accessibili tramite l’API.

Puoi recuperare l&#39;elenco di tutti i nomi di campo supportati disponibili nei record dei lead utilizzando l&#39;endpoint REST [Descrivi lead](https://developer.adobe.com/marketo-apis/api/mapi/).

| Nome API REST | Nome API SOAP | Etichetta intuitiva | Descrizione |
| --- | --- | --- | --- |
| indirizzo | Indirizzo | Indirizzo | Indirizzo del lead |
| AnnualRevenue | Reddito Annuo | Entrata annuale | Ricavi annuali della società del lead |
| anonymousIP | AnonimoIP | IP anonimo | Indirizzo IP della prima visita web registrata del lead |
| billingCity | BillingCity | Città di fatturazione | Città dell&#39;indirizzo di fatturazione del lead |
| billingCountry | BillingCountry | Paese di fatturazione | Paese dell’indirizzo di fatturazione del lead |
| billingPostalCode | BillingPostalCode | Codice postale di fatturazione | Codice postale dell&#39;indirizzo di fatturazione del lead |
| billingState | BillingState | Stato di fatturazione | Stato o provincia dell&#39;indirizzo di fatturazione del lead |
| billingStreet | BillingStreet | Indirizzo di fatturazione | Indirizzo di fatturazione della società del lead |
| città | Città | Città | Città del lead |
| azienda | Azienda | Nome della società | Nome società del lead |
| paese | Paese | Paese | Paese del lead |
| dateOfBirth | Data di nascita | Data di nascita | Data di nascita del lead |
| reparto | Reparto | Reparto | Reparto del lead nella loro azienda |
| doNotCall | DoNotCall | Non effettuare la chiamata | Preferenza di non chiamata del lead |
| doNotCallReason | DoNotCallReason | Motivo per cui non effettuare la chiamata | Spiegazione della preferenza di non chiamata del lead |
| e-mail | E-mail | Indirizzo e-mail | Indirizzo e-mail del lead. Campo chiave Marketo standard per i record dei lead |
| fax | Fax | Numero di fax | Numero di fax del lead |
| firstName | FirstName | Nome | Nome del lead |
| settore | Settore | Settore | Settore del lead |
| inferredCompany | InferredCompany | Azienda dedotta | Nome dell’azienda dedotto dalla ricerca inversa dell’IP della prima visita web registrata del lead |
| inferredCountry | InferredCountry | Paese dedotto | Paese dedotto dalla ricerca inversa dell’IP della prima visita web registrata del lead |
| lastName | Cognome | Cognome | Cognome del lead |
| leadRole | LeadRole | Ruolo | Ruolo del lead nella loro azienda |
| leadScore | PunteggioLead | Punteggio Lead | Punteggio intero assegnato al lead tramite campagne e programmi di punteggio |
| leadSource | LeadSource | Fonte Lead | Registrazione sul campo della fonte da cui ha avuto origine il lead |
| leadStatus | StatoLead | Stato Lead | Campo che registra lo stato attuale di marketing/vendite del lead |
| mainPhone | MainPhone | Numero di telefono | Numero di telefono principale della società del lead |
| jigsawContactId | ID contatto Jigsaw Marketo | ID MARKETO Data.com | ID Data.com del lead, se disponibile |
| jigsawContactStatus | Stato contatto Jigsaw Marketo | Stato Marketo Data.com | Stato Data.com del lead, se disponibile |
| facebookDisplayName | MarketoSocialFacebookDisplayName | Nome visualizzato Marketo Social Facebook | Nome visualizzato Facebook del lead. Sistema popolato durante l&#39;accesso tramite social network |
| facebookId | MarketoSocialFacebookId | ID Marketo Social Facebook | ID Facebook del lead. Sistema popolato durante l&#39;accesso tramite social network |
| facebookPhotoURL | MarketoSocialFacebookPhotoURL | URL foto Marketo Social Facebook | URL della foto del profilo Facebook del lead. Sistema popolato durante l&#39;accesso tramite social network |
| facebookProfileURL | MarketoSocialFacebookProfileURL | URL profilo Marketo Social Facebook | URL del profilo Facebook del lead. Sistema popolato durante l&#39;accesso tramite social network |
| facebookReach | MarketoSocialFacebookReach | Marketo Social Facebook Reach | La portata Facebook del lead. Sistema popolato durante l&#39;accesso tramite social network |
| facebookReferredEnrollments | MarketoSocialFacebookReferredEnrollments | Iscrizioni con riferimenti a Marketo Social Facebook | Numero di iscrizioni a cui viene fatto riferimento attribuite al lead tramite Facebook. Gestito dal sistema |
| facebookReferredVisits | MarketoSocialFacebookReferredVisits | Visite di riferimento di Marketo Social Facebook | Numero di visite con riferimento attribuite al lead tramite Facebook. Gestito dal sistema |
| genere | MarketoSocialGender | Genere social Marketo | Genere del lead. Sistema popolato durante l&#39;accesso tramite social network |
| lastReferredEnrollment | MarketoSocialLastReferredEnrollment | Ultima iscrizione riferimento a Marketo Social | Data dell’ultimo riferimento completato. Gestito dal sistema |
| lastReferredVisit | MarketoSocialLastReferredVisit | Ultima visita di riferimento di Marketo Social | Data dell’ultima visita di riferimento. Gestito dal sistema |
| linkedInDisplayName | MarketoSocialLinkedInDisplayName | Nome visualizzato Marketo Social LinkedIn | Nome visualizzato LinkedIn del lead. Sistema popolato durante l&#39;accesso tramite social network |
| linkedInId | MarketoSocialLinkedInId | ID Marketo Social LinkedIn | ID LinkedIn del lead. Sistema popolato durante l&#39;accesso tramite social network |
| linkedInPhotoURL | MarketoSocialLinkedInPhotoURL | URL foto Marketo Social LinkedIn | URL della foto LinkedIn del lead. Sistema popolato durante l&#39;accesso tramite social network |
| linkedInProfileURL | MarketoSocialLinkedInProfileURL | URL profilo Marketo Social LinkedIn | Profilo LinkedIn del lead. Sistema popolato durante l&#39;accesso tramite social network |
| linkedInReach | MarketoSocialLinkedInReach | Marketo Social LinkedIn Reach | Portata LinkedIn del lead. Sistema popolato durante l&#39;accesso tramite social network |
| linkedInReferredEnrollments | MarketoSocialLinkedInReferredEnrollments | Iscrizioni con riferimenti a Marketo Social LinkedIn | Numero di iscrizioni a cui viene fatto riferimento attribuite al lead tramite LinkedIn. Gestito dal sistema |
| linkedInReferredVisits | MarketoSocialLinkedInReferredVisits | Visite di riferimento di Marketo Social LinkedIn | Numero di visite con riferimento attribuite al lead tramite LinkedIn. Gestito dal sistema |
| syndicationId |  - | ID Marketo Social Syndication | ID social Marketo interno del lead. Gestito dal sistema |
| totalReferredEnrollments | MarketoSocialTotalReferredEnrollments | Totale iscrizioni inoltrate Social Marketo | Numero totale di iscrizioni di riferimento completate attribuite al lead |
| totalReferredVisits | MarketoSocialTotalReferredVisits | Totale visite inoltrate Social Marketo | Numero totale di visite riferite attribuite al lead |
| twitterDisplayName | MarketoSocialTwitterDisplayName | Nome visualizzato Twitter social network Marketo | Nome visualizzato del Twitter del lead. Sistema popolato durante l&#39;accesso tramite social network |
| twitterId | MarketoSocialTwitterId | ID Twitter social network Marketo | ID Twitter del lead. Sistema popolato durante l&#39;accesso tramite social network |
| twitterPhotoURL | MarketoSocialTwitterPhotoURL | URL foto Twitter Marketo Social | URL della foto del Twitter del lead. Sistema popolato durante l&#39;accesso tramite social network |
| twitterProfileURL | MarketoSocialTwitterProfileURL | URL profilo Twitter social network Marketo | URL del profilo di Twitter del lead. Sistema popolato durante l&#39;accesso tramite social network |
| twitterReach | MarketoSocialTwitterReach | Marketo - Raggiungimento Twitter social network | La portata del Twitter del piombo. Sistema popolato durante l&#39;accesso tramite social network |
| twitterReferredEnrollments | MarketoSocialTwitterReferredEnrollments | Iscrizioni di riferimento a Marketo Social Twitter | Numero di iscrizioni a cui viene fatto riferimento attribuite al Twitter lead-through. Gestito dal sistema |
| twitterReferredVisits | MarketoSocialTwitterReferredVisits | Visite di riferimento per Twitter social network Marketo | Numero di visite con riferimento attribuite al Twitter lead-through. Gestito dal sistema |
| middleName | Secondo nome | Secondo nome | Secondo nome del lead |
| mobilePhone | Cellulare | Numero di telefono | Numero di telefono cellulare del lead |
| numberOfEmployees | NumberOfEmployees | Numero dipendenti | Numero di dipendenti della società del lead |
| telefono | Telefono | Numero di telefono | Numero di telefono del lead |
| postalCode | CAP | Codice postale | Codice postale del lead |
| valutazione | Valutazione | Classificazione Lead | Valutazione marketing/vendite del lead |
| saluto | Formula di saluto | Formula di saluto | Formula di apertura preferita del lead, ad esempio Mister, Misses, ecc. |
| sicCode | Codice SICC | Codice SIC (Standard Industrial Classification) | Codice di classificazione industriale standard della società del lead |
| sito | Sito | Sito |  |
| stato | Stato | Stato | Stato del lead |
| titolo | Titolo | Qualifica | Qualifica del lead |
| annullato abbonamento | Annulla l&#39;iscrizione | Annulla l&#39;iscrizione | Stato del lead per cui è stato annullato l’abbonamento all’e-mail. Gestito parzialmente dal sistema. Impedisce la ricezione di e-mail non operative se impostato su true. |
| unsubscscribeReason | Motivo annullamento abbonamento | Motivo di annullamento dell&#39;iscrizione | Motivo dello stato di annullamento dell’iscrizione del lead. Gestito parzialmente dal sistema. Viene compilata con informazioni e-mail se il lead ha annullato l’abbonamento direttamente da un’e-mail di Marketo. |
| sito web | Sito web | Sito web | URL del sito web della società del lead |
| createdAt |  - | Data creazione | Ora di creazione iniziale del record del lead. Gestito dal sistema |
| updatedAt |  - | Data di aggiornamento | L’ultima volta che il record del lead è stato aggiornato. Gestito dal sistema |
| emailInvalid |  - | E-mail non valida | Stato e-mail non valido. Tutte le e-mail inviate all’indirizzo saranno bloccate se impostato su true. I messaggi non recapitati che indicano che l’e-mail non è valida imposteranno automaticamente questo campo su true. |
| emailInvalidCause |  - | E-mail causa non valida | Causa dello stato non valido dell’e-mail. Il messaggio di mancato recapito di istanza verrà registrato in questo campo quando e-mail non valida è impostata su true. |
| inferredCity |  - | Città dedotta | Città del lead dedotta dalla ricerca IP inversa della prima visita web registrata del lead. |
| inferredMetropolitanArea |  - | Area metropolitana dedotta | L’area metropolitana del lead è dedotta dalla ricerca IP inversa della prima visita web registrata del lead. |
| inferredPhoneAreaCode |  - | Prefisso telefonico dedotto | Prefisso telefonico del lead dedotto dalla ricerca IP inversa della prima visita web registrata del lead. |
| inferredPostalCode |  - | Codice postale dedotto | Codice postale del lead dedotto dalla ricerca IP inversa della prima visita web registrata del lead. |
| inferredStateRegion |  - | Area geografica dello stato dedotta | L’area dello stato del lead è dedotta dalla ricerca IP inversa della prima visita web registrata del lead. |
| isAnonymous |  - | È anonimo | Stato anonimo del record del lead. Gestito dal sistema. |
| priorità |  - | Priorità | Priorità informazioni vendite del lead. Gestito dal sistema. |
| relativeScore |  - | Punteggio relativo | Punteggio relativo di Sales Insight del lead. Gestito dal sistema. |
| urgenza |  - | Urgenza | Urgenza di approfondimento vendite del lead. Gestito dal sistema. |
