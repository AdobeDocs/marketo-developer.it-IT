---
title: API JAVASCRIPT
description: Scopri come utilizzare l’API JavaScript di Marketo con il codice da incorporare per il tracciamento dei lead di Munchkin, Forms 2.0, Web Personalization e Predictive Content.
feature: Munchkin Tracking Code, Forms, Web Personalization, Predictive Content, Social, Javascript
exl-id: 6129a467-be44-44bd-9e02-62009070c318
TQID: 'https://experienceleague.adobe.com/R9kIFBiH6jc64ay85QkumV7jCsFnj9J0t5G4IJKEsJM'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
    internal-label: Personalization
  - id: 1c1a93b2-f024-5627-9905-6faf6fcc22be
    internal-label: Munchkin Tracking Code
  - id: 664d862c-1673-5ed4-a3d6-386ac83225e4
    internal-label: Web Personalization
  - id: 52412b34-abb2-53fa-9fea-8547c07823df
    internal-label: Predictive Content
  - id: dda1332a-c3e0-583f-9d9b-15f1934e0ad3
    internal-label: Javascript
  - id: c2dbad80-0f5c-4d96-a798-2a65f93b8721
    internal-label: Assets
subfeature_v2:
  - id: e9b7b90f-6f8a-4637-a2ca-00239808918c
    internal-label: Social
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
    internal-label: Machine learning
source-git-commit: 5620f050ba834be3f6648650b5cc7d781ea394bf
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 1%
---
# API JavaScript

Le integrazioni JavaScript lato client di Marketo forniscono funzionalità di tracciamento dei lead, moduli, personalizzazione web e contenuti predittivi. Per utilizzare queste funzionalità è necessario disporre di un account Marketo.

L’implementazione in genere comporta l’aggiunta di codice di incorporamento alla proprietà web. Per aggiungere funzionalità, puoi anche chiamare le funzioni JavaScript esposte dal codice di incorporamento.

Il codice di incorporamento è univoco per l’istanza di Marketo perché contiene un identificatore di account. Nell’interfaccia utente di Marketo, vai al pannello appropriato, copia il codice da incorporare negli Appunti e incollalo nella pagina web.

## Tracciamento lead (Munchkin)

Il [codice di tracciamento di Munchkin JavaScript](lead-tracking.md) di Marketo genera lead dalle visite al sito Web. Tiene inoltre traccia dei visitatori che non hanno fornito informazioni personali e crea lead anonimi che includono l’indirizzo IP dell’utente e altre informazioni.

Configura Munchkin sulla pagina Munchkin nell’area Amministratore di Marketo.

## Forms 2.0

[Forms 2.0](forms-api-reference.md) consente agli addetti al marketing di creare moduli web senza conoscenze sulla programmazione. Forms può trovarsi nelle pagine di destinazione di Marketo o essere incorporato in qualsiasi pagina del sito web.

Utilizza l’API JavaScript di Forms 2.0 per estendere le funzionalità di base di un modulo web Marketo.

## Personalizzazione web

[Marketo Web Personalization](web-personalization.md) ti consente di coinvolgere i potenziali clienti sul tuo sito Web in tempo reale in base a chi sono e a cosa fanno.

## Contenuto predittivo

[Marketo Predictive Content](predictive-content.md) utilizza machine learning e analisi predittive per presentare contenuti rilevanti ai visitatori Web. Aggiungi descrizioni di testo e immagini al contenuto e incorpora più consigli sui contenuti nel sito web.
