---
title: Posizione flusso
feature: SOAP
description: Spiega la posizione del flusso per la impaginazione dei dati in sequenza temporale in SOAP, in formati semplici e complessi e l’utilizzo in getLeadChanges, getLeadActivity e altro ancora
exl-id: c3a3fc1e-086b-4822-b2c7-2a7959db557c
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Posizione flusso

Gli elementi di posizione del flusso contengono un riferimento di posizione per uno o più flussi logici di dati in sequenza temporale. Il riferimento di posizione può essere una specifica esterna approssimativa come una marca temporale o una specifica interna opaca di posizione restituita da una chiamata API precedente. Le posizioni di flusso possono essere definite come un tipo complesso a più elementi o come una stringa.

La posizione del flusso viene utilizzata per recuperare i dati in batch e consente al chiamante di impaginare il risultato. La posizione del flusso passata all’interno di una richiesta API è il valore della posizione del flusso restituita nella risposta precedente. La modifica della posizione del flusso restituita dalla chiamata API precedente non è consigliata e potrebbe causare un comportamento API imprevisto.

## API che supportano la posizione del flusso

- [getCustomObjects](getcustomobjects.md)
- [getLeadChanges](getleadchanges.md)
- [getLeadActivity](getleadactivity.md)
- [getMObjects](getmobjects.md)
- [getMultipleLeads](getmultipleleads.md)

## Posizione semplice flusso

```
<streamPosition>8UJZetaMb1V6uUZl+L7DcPP2jG+PMmtpF</streamPosition>
```

## Posizione flusso complesso

```xml
<startPosition>
  <latestCreatedAt  />
  <oldestCreatedAt>2013-08-01T00:13:13+00:00</oldestCreatedAt>
  <activityCreatedAt  />
  <offset>ID:1086173</offset>
</startPosition>
```
