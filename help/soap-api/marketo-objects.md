---
title: Oggetti Marketo
feature: SOAP
description: Panoramica degli oggetti MO di Marketo, inclusi tipi, attributi, comportamento chiave esterna e API SOAP supportate per opportunità, programma e record correlati.
exl-id: 99b9aed4-94e8-46e8-84d9-2cc5215b0c13
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '265'
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

Le chiavi esterne sono campi personalizzati definiti negli oggetti Marketo, ad esempio Lead o Opportunità. Il nome è il nome del campo e il valore è il valore del campo, generato in un sistema esterno. **Marketo non applica un vincolo univoco a questi valori.** È responsabilità dell&#39;utente API verificare che i valori siano univoci. Se si verifica un duplicato, Marketo utilizza l’oggetto aggiunto più di recente. È simile al comportamento del campo standard Indirizzo e-mail.

### API disponibili

| API | Può funzionare il |
|---|---|
| descriptionMObject | ActivityRecord, LeadRecord, Opportunità, OpportunityPersonRole |
| getMObjects | Opportunità, OpportunityPersonRole, programma |
| syncMObjects | Opportunità, OpportunityPersonRole, programma |
| deleteMObjects | Opportunità, OpportunityPersonRole |
| listMObjects | ActivityRecord, LeadRecord, Opportunità, OpportunityPersonRole |
