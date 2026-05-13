---
title: API JAVASCRIPT
description: Scopri come utilizzare l’API JavaScript di Marketo con il codice da incorporare per il tracciamento dei lead di Munchkin, Forms 2.0, Web Personalization e Predictive Content.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 1%

---

# API JavaScript

Di seguito è riportata una panoramica delle funzionalità di integrazione JavaScript lato client di Marketo. Devi disporre di un account Marketo per utilizzare queste funzionalità. In genere, l’implementazione comporta semplicemente l’aggiunta di un &quot;codice di incorporamento&quot; alla proprietà web. Facoltativamente, puoi utilizzare funzionalità aggiuntive richiamando le funzioni JavaScript esposte dal codice di incorporamento. Queste funzioni sono completamente documentate qui.

Il codice di incorporamento è univoco per l’istanza di Marketo perché contiene un identificatore di account. Ottieni il codice da incorporare accedendo al pannello appropriato all’interno dell’interfaccia utente di Marketo, copialo negli Appunti e incollalo nella pagina web.

## Tracciamento lead (Munchkin)

Il codice di tracciamento di Marketo [Munchkin JavaScript](lead-tracking.md) è fondamentale per le funzionalità di Marketo. Consente di generare lead dalle visite al sito web. Tiene traccia anche dei visitatori che non ti hanno ancora fornito i loro dati personali, creando lead anonimi che includono l’indirizzo IP dell’utente e altre informazioni. Puoi impostare Munchkin nella pagina Munchkin dell’area Amministratore di Marketo.

## Forms 2.0

[Forms 2.0](forms-api-reference.md) consente agli addetti al marketing di creare moduli web belli, stabili e flessibili senza alcuna conoscenza in materia di programmazione. Forms può trovarsi nelle pagine di destinazione di Marketo ed essere incorporato in qualsiasi pagina del sito web. Le funzionalità di base di un modulo web Marketo possono essere estese utilizzando l’API JavaScript di Forms 2.0.

## Personalizzazione web

[Marketo Web Personalization](web-personalization.md) è una piattaforma di targeting e Personalization che consente di coinvolgere migliaia di potenziali clienti sul sito Web in tempo reale in base a chi sono e a cosa fanno.

## Contenuto predittivo

[Marketo Predictive Content](predictive-content.md) ti consente di coinvolgere i visitatori del Web con i contenuti più rilevanti, grazie all&#39;apprendimento automatico e all&#39;analisi predittiva. Migliora il contenuto con descrizioni testuali e immagini e incorpora più consigli sui contenuti nel sito web.

