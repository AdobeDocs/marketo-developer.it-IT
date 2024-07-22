---
title: Campi
feature: REST API, Field Management
description: Elenco di nomi di campi supportati.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 2%

---

# Campi

L’API REST e l’API SOAP utilizzano diverse convenzioni di denominazione per i campi lead.

## Recuperare l&#39;elenco dei nomi dei campi

Recupera l’elenco di tutti i nomi di campo supportati disponibili nei record dei lead utilizzando l’endpoint REST &quot;Descrivi lead&quot;.

## Dove utilizzare quale tipo di nome campo?

A volte è difficile sapere quale tipo di nome di campo utilizzare quando si utilizza una particolare funzione correlata all’integrazione. Di seguito è riportato un riferimento rapido per le funzioni che utilizzano i tipi di nome di campo REST o SOAP.

| Funzione | Tipo di nome campo da utilizzare |
|--- |--- |
| API di tracciamento dei lead (Munchkin) | SOAP |
| API Forms 2.0 | SOAP |
| Importazione elenco (interfaccia utente) | SOAP |
| Importazione elenco (REST API) | REST |
| Mappature risposta webhook | SOAP |
| Script e-mail (Velocity) | SOAP |
| API SOAP | SOAP |
| REST API | REST |

### Perché il campo API REST sfdcId restituisce sempre un valore null?

Il campo `sfdcId` è un campo formula che è stato erroneamente incluso nella mappa del campo originale per l&#39;API REST. I record recuperati tramite l’API REST non calcolano il valore dei campi formula, pertanto il valore sarà sempre nullo. Per acquisire l&#39;ID SFDC effettivo, è necessario utilizzare i campi denominati `sfdcLeadId` e `sfdcContactId`.
