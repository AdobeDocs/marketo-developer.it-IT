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
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 194
ht-degree: 2%

---

# Campi

L’API REST e l’API SOAP utilizzano diverse convenzioni di denominazione per i campi lead. Utilizza la convenzione nome campo richiesta da ogni funzione di integrazione.

## Recuperare l&#39;elenco dei nomi dei campi

Utilizza l’endpoint REST &quot;Descrivi lead&quot; per recuperare tutti i nomi di campo supportati per i record dei lead.

## Dove utilizzare quale tipo di nome campo?

Il tipo di nome campo richiesto dipende dalla funzione di integrazione. Nella tabella seguente viene indicato se ciascuna funzionalità utilizza i nomi dei campi REST o SOAP.

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

Il campo `sfdcId` è un campo formula incluso nella mappa del campo originale per l&#39;API REST. I record recuperati tramite l&#39;API REST non calcolano i valori dei campi formula, pertanto `sfdcId` restituisce sempre null.

Per recuperare l&#39;ID SFDC, utilizzare i campi `sfdcLeadId` e `sfdcContactId`.
