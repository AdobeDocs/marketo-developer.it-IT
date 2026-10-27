---
title: Operazioni MCP Marketo Engage
description: Scopri quali operazioni MCP di Marketo Engage sono disponibili per l’utilizzo con gli assistenti AI.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%
---

# [!DNL Marketo Engage] operazioni MCP

Il server MCP [!DNL Marketo Engage] supporta 182 operazioni per la gestione delle risorse, i dati dei lead e l&#39;automazione delle campagne.



## Operazioni supportate

Puoi scoprire le operazioni disponibili tramite l’assistente di intelligenza artificiale connesso.

Se la connessione espone gli strumenti di individuazione, utilizza i prompt seguenti:

* &quot;Utilizzare lo strumento Marketo MCP `list_tool_categories` per elencare le categorie di operazioni disponibili.&quot;
* &quot;Utilizzare lo strumento Marketo MCP `search_tools` per trovare le operazioni per la gestione delle campagne avanzate.&quot;

Sostituire l&#39;attività nel secondo prompt con l&#39;operazione necessaria. Se gli strumenti di individuazione non sono esposti, chiedere all&#39;assistente AI di elencare gli strumenti disponibili dal server MCP [!DNL Marketo].

I risultati riflettono le autorizzazioni della connessione e la disponibilità delle funzionalità. Descrivono gli strumenti MCP disponibili, non l&#39;API REST [!DNL Marketo] completa.

Per le connessioni che utilizzano le credenziali client [!DNL Marketo], le funzionalità seguenti non sono supportate:

* Strumenti SMS
* Strumenti di notifica push
* Messaggi in-app e strumenti del programma in-app
* Pianificazione delle campagne intelligenti con `schedule_smart_campaign`
* Strumenti di gestione utenti e di assistenza in blocco

L&#39;operazione `schedule_campaign` è supportata per le connessioni delle credenziali client.

Le operazioni disponibili dipendono dalle autorizzazioni utente API, dalle funzionalità attivate per l&#39;istanza [!DNL Marketo] e da eventuali restrizioni nel client MCP. Alcune operazioni che modificano o eliminano dati richiedono conferma e accesso aggiuntivo. Il supporto per una categoria non implica l’accesso a ogni endpoint REST.

Per istruzioni di connessione e prompt di esempio, vedere la pagina del server [MCP](mcp-server.md#available-operations). Per informazioni sulla gestione dei dati con l&#39;intelligenza artificiale [!DNL Marketo] e il server MCP [!DNL Marketo Engage], vedere la pagina [Informazioni sui dati](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/marketo-ai/data-information).