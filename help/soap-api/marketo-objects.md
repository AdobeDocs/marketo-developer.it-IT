---
title: "Oggetti Marketo"
feature: SOAP
description: "Panoramica sugli oggetti di Marketo"
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---


# Oggetti Marketo

Marketo utilizza oggetti Marketo (MObjects) per rappresentare varie classi come Program, Opportunity e OpportunityPersonRole.

## Struttura degli oggetti MO

Gli oggetti MO possono essere di tre tipi: Standard, Personalizzato o Virtuale. Gli oggetti MO standard e personalizzati rappresentano entità distinte, ad esempio Lead o Company, mentre gli oggetti virtuali, ad esempio LeadRecord, sono composti da campi di uno o più oggetti. Gli oggetti virtuali sono oggetti di convenienza utilizzati nell’API ma non esistono nell’applicazione Marketo.

Gli oggetti MO sono costituiti da:

- Un piccolo insieme di attributi fissi comuni a tutti gli oggetti MO
   - Tipo richiesto
   - ExternalKey opzionale
   - id di sola lettura, createdAt, updatedAt
- Elenco di uno o più attributi specifici dell&#39;oggetto, come coppie nome/valore, alcuni dei quali possono essere richiesti. Ad esempio, nome su Opportunità.
- Un elenco di riferimenti a oggetti associati, come object-name plus
   - MARKETO ID o
   - External-key come coppia attributo-nome/attributo-valore.

### Chiavi Esterne

Le chiavi esterne sono campi personalizzati definiti negli oggetti Marketo, ad esempio Lead o Opportunità. Il nome è il nome del campo e il valore è il valore del campo, generato in un sistema esterno. **Marketo non applica un vincolo di univocità a questi valori.** È responsabilità dell’utente API verificare che i valori siano univoci. Se si verifica un duplicato, Marketo utilizza l’oggetto aggiunto più di recente. È simile al comportamento del campo standard Indirizzo e-mail.

### API disponibili

| API | Può funzionare il |
|---|---|
| descriptionMObject | ActivityRecord, LeadRecord, Opportunità, OpportunityPersonRole |
| getMObjects | Opportunità, OpportunityPersonRole, programma |
| syncMObjects | Opportunità, OpportunityPersonRole, programma |
| deleteMObjects | Opportunità, OpportunityPersonRole |
| listMObjects | ActivityRecord, LeadRecord, Opportunità, OpportunityPersonRole |
