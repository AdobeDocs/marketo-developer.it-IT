---
title: Codici errore
feature: SOAP
description: Guida di riferimento ai codici di errore API di Marketo SOAP con messaggi e note, che descrivono gli errori di autenticazione, i limiti di tasso e concorrenza e i problemi di richiesta.
exl-id: 71796520-7bd6-4a37-94e7-b073d17df06f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 9%

---

# Codici errore

Durante lo sviluppo per Marketo, è molto importante che le richieste e le risposte vengano registrate quando si verifica un’eccezione imprevista.  Mentre alcuni tipi di eccezioni, ad esempio l’autenticazione scaduta, possono essere gestiti in modo sicuro tramite la riautenticazione, altri possono richiedere interazioni di supporto e in questo caso le richieste e le risposte verranno sempre richieste.

Di seguito è riportato un elenco di codici di errore API di SOAP.

| Codice | Messaggio | Note |
|--- |--- |--- |
| 10001 | Errore interno | Grave guasto del sistema |
| 20011 | Errore interno | Errore del servizio API |
| 20012 | Richiesta non compresa | Messaggio SOAP imprevisto |
| 20013 | Accesso negato | Il client è bloccato dall’accesso API |
| 20014 | Autenticazione non riuscita | Il client non ha fornito credenziali valide |
| 20015 | Limite di richieste superato | Il numero di chiamate di oggi ha superato la quota della sottoscrizione. La quota di abbonamento predefinita è 10.000/die. |
| 20016 | Richiesta scaduta | Firma della richiesta troppo vecchia. Il timestamp e la firma della richiesta specificati sono passati e non sono più validi. La richiesta può essere ritentata con una marca temporale e una firma appena generate. |
| 20017 | Richiesta non valida | Nella richiesta manca un parametro previsto |
| 20019 | Operazione non supportata | L&#39;operazione richiamata non è definita nel file WSDL dell&#39;API Marketo |
| 20022 | L&#39;intervallo di tempo specificato nel filtro query ha superato il limite | Il numero di giorni trascorsi tra i campi &quot;olderUpdatedAt&quot; e &quot;latestUpdatedAt&quot; è superiore a 30 |
| 20023 | Limite di tasso superato | Il numero di chiamate negli ultimi 20 secondi è stato superiore a 100 |
| 20024 | Limite di concorrenza superato | Il numero di chiamate simultanee era superiore a 10 |
| 20101 | Chiave lead richiesta | LeadKey è obbligatorio ma non è stato fornito |
| 20102 | Chiave lead non valida | LeadKeyType non valido |
| 20103 | Lead non trovato | Il valore LeadKey non corrisponde ad alcun lead |
| 20104 | Dettagli lead richiesti | LeadRecord richiesto ma non fornito |
| 20105 | Attributo lead non valido | LeadRecord contiene un attributo con un nome non valido |
| 20106 | Sincronizzazione lead non riuscita | Impossibile aggiornare o creare il leadRecord |
| 20107 | Chiave attività non valida | LeadActivityFilter contiene un tipo di attività non valido |
| 20108 | Proprietario lead non trovato | LeadKey specifica un proprietario di lead inesistente |
| 20109 | Parametro obbligatorio | Il valore del parametro è nullo o mancante |
| 20110 | Parametro non valido | Un valore di parametro non è valido |
| 20111 | Elenco non trovato | ListKey specifica un elenco inesistente |
| 20113 | Campagna non trovata | La campagna non esiste |
| 20114 | Parametro non valido | Il valore del parametro non è valido |
| 20122 | Posizione flusso non valida | Posizione del flusso non valida |
| 20123 | Streaming alla fine | La posizione del flusso indica che non sono disponibili altri record |
