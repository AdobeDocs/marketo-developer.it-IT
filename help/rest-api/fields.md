---
title: "Campi"
feature: REST API, Field Management
description: '"Elenco di nomi di campo supportati".'
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 2%

---


# Campi

L’API REST e l’API SOAP utilizzano diverse convenzioni di denominazione per i campi lead.

## Recuperare l&#39;elenco dei nomi dei campi

Recupera l’elenco di tutti i nomi di campo supportati disponibili nei record dei lead utilizzando l’endpoint REST &quot;Descrivi lead&quot;.

## Dove utilizzare quale tipo di nome campo?

A volte è difficile sapere quale tipo di nome di campo utilizzare quando si utilizza una particolare funzione correlata all’integrazione. Di seguito è riportato un riferimento rapido per cui le funzionalità utilizzano i tipi di nome di campo REST o SOAP.

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

Il campo `sfdcId` è un campo formula che è stato incluso erroneamente nella mappa dei campi originale per l’API REST. I record recuperati tramite l’API REST non calcolano il valore dei campi formula, pertanto il valore sarà sempre nullo. Per acquisire l’ID SFDC effettivo, utilizza i campi denominati `sfdcLeadId` e `sfdcContactId`.
