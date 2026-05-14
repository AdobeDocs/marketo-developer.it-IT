---
title: Attività
feature: SOAP
description: Scopri come interagire con le attività utilizzando SOAP, recuperare le attività lead e tenere traccia delle modifiche dei lead con getLeadActivities e getLeadChanges
exl-id: fd695ab6-e7be-4ced-89c9-c4cd2d4c2ab0
TQID: https://experienceleague.adobe.com/6zUkvoDCqlRmblFDPWzLjdwITsyWxcXrJBKbLux76WI
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: e71bcf289229867bc969345d79c8f014761aaaf9
workflow-type: tm+mt
source-wordcount: 79
ht-degree: 2%

---

# Attività

Le seguenti chiamate SOAP possono essere utilizzate per interagire con le attività.

- [getLeadActivities](getleadactivity.md)
- [getLeadChanges](getleadchanges.md)

>[!CAUTION]
>
>A partire dal 2026-12-30, le chiamate agli endpoint `Get Lead Activities` e `Get Lead Changes` che includono il parametro `listId` non riusciranno (codice di errore 1003) se gli elenchi di destinazione contengono 10.000 o più lead. Per evitare interruzioni del servizio, assicurati che le chiamate abbiano un ambito corretto per evitare questo limite. Consulta la [Guida alla migrazione](migration.md).
