---
title: Campi
feature: REST API, Field Management
description: Scopri la denominazione dei campi lead REST e SOAP, i campi elenco tramite REST Descrivi il lead, la mappatura delle funzioni, il motivo per cui sfdcId è nullo e utilizza sfdcLeadId o sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
TQID: https://experienceleague.adobe.com/H2Bvhy-67U8JJ1V3JwYJ0O0vj4i11fwUCyYQtjxm8u0
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 213
ht-degree: 2%

---

# Campi

L’API REST e l’API SOAP utilizzano diverse convenzioni di denominazione per i campi lead.

## Recuperare l&#39;elenco dei nomi dei campi

Recupera l’elenco di tutti i nomi di campo supportati disponibili nei record dei lead utilizzando l’endpoint REST &quot;Descrivi lead&quot;.

## Dove utilizzare quale tipo di nome campo?

A volte è difficile sapere quale tipo di nome di campo utilizzare quando si utilizza una particolare funzione correlata all’integrazione. Di seguito è riportato un riferimento rapido per le funzioni che utilizzano i tipi di nome di campo REST o SOAP.

| Funzione | Tipo di nome campo da utilizzare |
| --- | --- |
| API di tracciamento dei lead (Munchkin) | SOAP |
| API Forms 2.0 | SOAP |
| Importazione elenco (interfaccia utente) | SOAP |
| Importazione elenco (REST API) | REST |
| Mappature risposta webhook | SOAP |
| Script e-mail (Velocity) | SOAP |
| API SOAP | SOAP |
| REST API | REST |

### Perché il campo API REST sfdcId restituisce sempre un valore null?

Il campo `sfdcId` è un campo formula che è stato erroneamente incluso nella mappa del campo originale per l&#39;API REST. I record recuperati tramite l’API REST non calcolano il valore dei campi formula, pertanto il valore sarà sempre nullo. Per acquisire il SFDC ID effettivo, utilizzare i campi denominati `sfdcLeadId` e `sfdcContactId`.
