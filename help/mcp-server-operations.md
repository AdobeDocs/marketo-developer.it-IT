---
title: Operazioni MCP Marketo Engage
description: Scopri quali operazioni MCP di Marketo Engage sono disponibili per l’utilizzo con gli assistenti AI.
autotag-review: '2026-06-02T13:31:42.084Z'
TQID: 'https://experienceleague.adobe.com/qvrWbHOCsCCHctduNDxMhkE8JAKxZk8FCYfKvzxfcYA'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
  - id: dca84292-69e9-4116-a575-667d31fa060d
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: c631b7c3d571f29083673f9b97d22230d109abfc
workflow-type: tm+mt
source-wordcount: 1228
ht-degree: 25%

---


# [!DNL Marketo Engage] operazioni MCP

Le operazioni seguenti sono disponibili tramite il server MCP [!DNL Marketo Engage]. Il server fornisce endpoint di sola lettura o non distruttivi. Il sistema di IA non può utilizzare `Delete` o altre operazioni distruttive.

>[!NOTE]
>
>Gli strumenti Smart List e Smart Campaign `create` e `update` sono destinati alla versione di settembre 2026.

Per informazioni sulla gestione dei dati con Marketo AI e il server Marketo Engage MCP, visita la pagina [Informazioni sui dati](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/marketo-ai/data-information).

## Esportazione in blocco

[Riferimento API per esportazione in blocco](https://developer.adobe.com/marketo-apis/api/mapi){target="_blank"}

- `bulk_export_create`
- `bulk_export_enqueue`
- `bulk_export_file`
- `bulk_export_status`
- `get_import_status`

## Canali e tag

[Riferimento API canali](https://developer.adobe.com/marketo-apis/api/asset#tag/Channels){target="_blank"} | [Riferimento API tag](https://developer.adobe.com/marketo-apis/api/asset#tag/Tags){target="_blank"}

- `browse_channels`
- `browse_tag_types`
- `get_channel_by_name`
- `get_tag_type_by_name`

## E-mail

[Riferimento API per le e-mail](https://developer.adobe.com/marketo-apis/api/asset#tag/Emails){target="_blank"}

- `approve_email`
- `browse_emails`
- `create_email`
- `get_email_by_id`
- `get_email_by_name`
- `get_email_content`
- `update_email_content`

## Cartelle

[Riferimento API per le cartelle](https://developer.adobe.com/marketo-apis/api/asset#tag/Folders){target="_blank"}

- `browse_folders`
- `create_folder`
- `delete_folder`
- `get_folder_by_id`
- `get_folder_by_name`
- `get_folder_content`
- `update_folder`

## Moduli

[Riferimento API di Forms](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms){target="_blank"}

- `add_field_set`
- `add_field_to_form`
- `add_field_visibility_rule`
- `add_rich_text_field`
- `approve_form`
- `browse_forms`
- `clone_form`
- `create_form`
- `delete_field_from_fieldset`
- `delete_form`
- `delete_form_field`
- `discard_form_draft`
- `get_form_by_id`
- `get_form_by_name`
- `get_form_field_metadata`
- `get_form_fields`
- `get_forms_used_by`
- `get_program_member_fields`
- `get_thank_you_page`
- `set_field_autofill`
- `update_field_positions`
- `update_form`
- `update_form_field`

## Lead

[Riferimento API lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads){target="_blank"}

- `add_leads_to_list`
- `describe_lead`
- `get_activity_types`
- `get_lead_activities`
- `get_leads_by_filter`
- `get_leads_by_smart_list`
- `get_paging_token`

## Programmi

[Riferimento API per i programmi](https://developer.adobe.com/marketo-apis/api/asset#tag/Programs){target="_blank"}

- `approve_program`
- `browse_email_batch_programs`
- `browse_nurture_programs`
- `browse_program_details`
- `browse_program_events`
- `browse_programs`
- `browse_scheduled_programs`
- `clone_program`
- `create_program`
- `delete_program_tag`
- `get_program_by_id`
- `get_program_by_name`
- `get_program_creation_options`
- `get_program_smart_list`
- `get_programs_by_tag`
- `unapprove_program`
- `update_program`
- `update_program_tag`

## Campagne avanzate

[Riferimento API per campagne intelligenti](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Campaigns){target="_blank"}

- `activate_smart_campaign`
- `add_flow_step`
- `browse_smart_campaigns`
- `create_smart_campaign`
- `facet_smart_campaigns`
- `get_smart_campaign_auto_suggest`
- `get_smart_campaign_by_id`
- `get_smart_campaign_by_name`
- `get_smart_campaign_flow_step_by_name`
- `get_smart_campaign_flow_step_type_by_name`
- `get_smart_campaign_flow_step_types`
- `get_smart_campaign_flow_steps`
- `get_smart_campaign_rule_by_name`
- `get_smart_campaign_rules`
- `get_smart_campaign_scheduled_runs`
- `get_smart_campaign_used_by`
- `get_smart_list_by_campaign_id`
- `schedule_campaign`
- `trigger_campaign`
- `update_flow_step_choice`
- `update_smart_campaign`

## Elenchi avanzati

[Riferimento API per elenchi avanzati](https://developer.adobe.com/marketo-apis/api/asset#tag/Smart-Lists){target="_blank"}

- `add_smart_list_rule`
- `browse_smart_lists`
- `clone_smart_list`
- `create_smart_list`
- `delete_all_smart_list_rules`
- `get_smart_list_auto_suggest`
- `get_smart_list_by_id`
- `get_smart_list_by_name`
- `get_smart_list_rule_by_name`
- `get_smart_list_rules`
- `get_smart_list_used_by`
- `remove_smart_list_rule_constraint`
- `reorder_smart_list_rules`
- `update_smart_list_filter_logic`
- `update_smart_list_rule`

## Snippet

[Riferimento API per snippet](https://developer.adobe.com/marketo-apis/api/asset#tag/Snippets){target="_blank"}

- `approve_snippet`
- `browse_snippets`
- `clone_snippet`
- `create_snippet`
- `delete_snippet`
- `discard_snippet_draft`
- `facet_snippets`
- `get_snippet_by_id`
- `get_snippet_content`
- `get_snippet_dynamic_content`
- `unapprove_snippet`
- `update_snippet`
- `update_snippet_content`
- `update_snippet_dynamic_content`

## Elenchi statici

[Riferimento API per elenchi statici](https://developer.adobe.com/marketo-apis/api/mapi#tag/Static-Lists){target="_blank"}

- `browse_lists`
- `create_list`
- `get_list_by_id`
- `get_list_by_name`
- `get_list_members`
- `remove_from_list`
- `update_list`

## Token

[Riferimento API per token](https://developer.adobe.com/marketo-apis/api/asset#tag/Tokens){target="_blank"}

- `create_calendar_token`
- `create_token`
- `delete_token`
- `get_calendar_tokens`
- `get_tokens_by_folder`

## Strumenti per passaggi del flusso MCP abilitati

<table style="table-layout:auto">
<tr>
<th>Passaggi del flusso</th>
<th>Trigger</th>
<th>Filtri (attività)</th>
<th>Filtri (attributo)</th>
</tr>
<tr>
<td valign="top"><ul><li>Aggiungi a set di campi</li><li>Aggiungi all’elenco</li><li>Aggiungi a Microsoft Campaign</li><li>Aggiungi allo sviluppo</li><li>Aggiungere a campagna SFDC</li><li>Webhook di chiamata</li><li>Modifica valore dati</li><li>Modifica partizione lead</li><li>Cambia cadenza di sviluppo</li><li>Cambia traccia sviluppo</li><li>Modificare proprietario</li><li>Modificare il proprietario in Microsoft</li><li>Cambia dati programma</li><li>Modificare i dati membro del programma</li><li>Modificare fase ricavo</li><li>Modificare punteggio</li><li>Cambia segmento</li><li>Modifica stato in progressione</li><li>Modificare lo stato nella campagna SFDC</li><li>Converti lead</li><li>Creare attività</li><li>Creare attività in Microsoft</li><li>Elimina lead</li><li>Elimina lead da Microsoft</li><li>Elimina lead da SFDC</li><li>Eseguire campagna</li><li>Momento interessante</li><li>Rimuovi da set di campi</li><li>Rimuovere dal flusso</li><li>Rimuovi dall’elenco</li><li>Rimuovi da Microsoft Campaign</li><li>Rimuovere dalla campagna SFDC</li><li>Richiedere campagna</li><li>Invia avviso</li><li>Invia e-mail</li><li>Sincronizza lead con Microsoft</li><li>Sincronizza lead con SFDC</li><li>Attendere</li></ul></td>
<td valign="top"><ul><li>Attività registrata</li><li>L'attività è aggiornata</li><li>Aggiunto all'elenco</li><li>Aggiunto a Microsoft Campaign</li><li>Aggiunto allo sviluppo</li><li>Aggiunto all’opportunità</li><li>Aggiunto all’opportunità (account)</li><li>Aggiunto all’opportunità (contatto)</li><li>Aggiunto a SFDC Campaign</li><li>Pone domande durante l’evento</li><li>Partecipa all'evento</li><li>Campagna richiesta</li><li>Fa clic sul collegamento</li><li>Clic sul collegamento nell’e-mail</li><li>Clic sul collegamento nell’e-mail di vendita</li><li>Clic sul collegamento nel messaggio SMS</li><li>Clic su un collegamento</li><li>Modifiche al valore dei dati</li><li>Scarica una risorsa</li><li>Mancati recapiti e-mail</li><li>Mancato recapito e-mail non permanenti</li><li>E-mail consegnata</li><li>Coinvolge con un flusso conversazionale</li><li>Interagisce con un dialogo</li><li>Interagisce con un agente nel flusso conversazionale</li><li>Interagisce con un agente nella finestra di dialogo</li><li>Compila il modulo</li><li>Ha un momento interessante</li><li>Interagisce con il documento nel flusso conversazionale</li><li>Interagisce con il documento nella finestra di dialogo</li><li>E-Mail Di Vendita Inviata</li><li>Lead convertito</li><li>Lead creato</li><li>Lead eliminato da Microsoft</li><li>Lead eliminato da SFDC</li><li>Il lead viene inviato a Marketo</li><li>Lead sincronizzato con Microsoft</li><li>Lead sincronizzato con SFDC</li><li>Modifiche alla partizione lead</li><li>Modifica manuale dello staging</li><li>Cambiamenti di cadenza dello sviluppo</li><li>Modifiche alla traccia di sviluppo</li><li>Apre l’e-mail</li><li>Apre l'e-mail di vendita</li><li>Opportunità (account) aggiornata</li><li>Opportunità (contatto) aggiornata</li><li>L’opportunità è aggiornata</li><li>Modifiche proprietario</li><li>Modifiche al proprietario in Microsoft</li><li>I dati dei membri del programma vengono modificati</li><li>Lo stato della progressione è cambiato</li><li>Raggiunge l’obiettivo della finestra di dialogo</li><li>Raggiunge l’obiettivo nel flusso conversazionale</li><li>Ricevuto Inoltra a e-mail amico</li><li>Rimosso dall’elenco</li><li>Rimosso da Microsoft Campaign</li><li>Rimosso dall’opportunità</li><li>Rimosso dall’opportunità (account)</li><li>Rimosso dall’opportunità (contatto)</li><li>Rimosso da SFDC Campaign</li><li>Risposte all'e-mail di vendita</li><li>Risponde a un sondaggio</li><li>Risponde a un sondaggio</li><li>Fase ricavi modificata</li><li>E-mail di vendita non recapitate</li><li>E-mail di vendita ricevuta</li><li>Pianifica riunione in flusso conversazionale</li><li>Pianifica riunione nella finestra di dialogo</li><li>Punteggio modificato</li><li>Modifiche al segmento</li><li>Avviso inviato</li><li>Inoltro a e-mail amico</li><li>Mancato recapito messaggi SMS</li><li>Messaggio SMS recapitato</li><li>Lo stato viene modificato in SFDC Campaign</li><li>Annulla iscrizione all’e-mail</li><li>Visita la pagina web</li><li>Chiamata del webhook</li></ul></td>
<td valign="top"><ul><li>Attività registrata</li><li>L'attività è stata aggiornata</li><li>Avviso inviato</li><li>La campagna è stata eseguita</li><li>La campagna è stata richiesta</li><li>Fai clic sul collegamento</li><li>Collegamento selezionato nell’e-mail</li><li>Collegamento selezionato nell’e-mail di vendita</li><li>Collegamento su cui è stato fatto clic nel messaggio SMS</li><li>Clic su un collegamento</li><li>Valore dati modificato</li><li>Una risorsa è stata scaricata</li><li>E-mail non recapitata</li><li>E-mail non recapitata temporaneamente</li><li>Coinvolto in un flusso conversazionale</li><li>Impegnato con una finestra di dialogo</li><li>Coinvolto con un agente nel flusso conversazionale</li><li>Impegnato con un agente nella finestra di dialogo</li><li>Modulo compilato</li><li>Ha avuto un momento interessante</li><li>Ha posto domande durante l’evento</li><li>Ha partecipato all’evento</li><li>Interazione con il documento nel flusso conversazionale</li><li>Interagito con il documento nella finestra di dialogo</li><li>Partizione lead modificata</li><li>Lead convertito</li><li>Il lead è stato creato</li><li>Lead eliminato da Microsoft</li><li>Lead eliminato da SFDC</li><li>Il lead è stato inviato a Marketo</li><li>Il lead è stato sincronizzato con Microsoft</li><li>Il lead è stato sincronizzato con SFDC</li><li>Cadenza dello sviluppo modificata</li><li>Traccia di sviluppo modificata</li><li>E-mail aperta</li><li>E-mail vendita aperta</li><li>Opportunità (account) aggiornata</li><li>Opportunità (contatto) aggiornata</li><li>L’opportunità è stata aggiornata</li><li>Il proprietario è stato cambiato</li><li>Il proprietario è stato cambiato in Microsoft</li><li>I dati dei membri del programma sono stati modificati</li><li>Lo stato della progressione è stato modificato</li><li>Obiettivo finestra di dialogo raggiunto</li><li>Obiettivo raggiunto nel flusso conversazionale</li><li>Ricevuto Inoltra a e-mail amico</li><li>Risposta all'e-mail di vendita</li><li>Ha risposto a un sondaggio</li><li>Risposta a un sondaggio</li><li>La fase dei ricavi è stata modificata</li><li>E-mail di vendita non recapitata</li><li>E-mail di vendita ricevuta</li><li>Riunione programmata in flusso conversazionale</li><li>Riunione pianificata nella finestra di dialogo</li><li>Punteggio modificato</li><li>Segmento modificato</li><li>Inoltro a e-mail amico</li><li>Messaggio SMS non inviato</li><li>Annullamento iscrizione all’e-mail</li><li>Pagina Web visitata</li><li>È stato aggiunto all'elenco</li><li>È stato aggiunto allo sviluppo</li><li>È stato aggiunto all’opportunità</li><li>È stato aggiunto all’opportunità (account)</li><li>È stato aggiunto all’opportunità (contatto)</li><li>E-mail consegnata</li><li>Messaggio SMS recapitato</li><li>È stato rimosso dall'elenco</li><li>È stato rimosso dall’opportunità</li><li>È stato rimosso dall’opportunità (account)</li><li>È stato rimosso dall’opportunità (contatto)</li><li>E-mail inviata</li><li>E-Mail Di Vendita Inviata</li><li>Chiamata del webhook</li></ul></td>
<td valign="top"><ul><li>Indirizzo e-mail del proprietario dell’account</li><li>Nome proprietario account</li><li>Cognome del proprietario dell’account</li><li>Data di acquisizione</li><li>Programma di acquisizione</li><li>Nome del programma di acquisizione</li><li>Indirizzo</li><li>Entrata annuale</li><li>IP anonimo</li><li>Indirizzo di fatturazione</li><li>Città di fatturazione</li><li>Paese di fatturazione</li><li>Codice postale di fatturazione</li><li>Stato di fatturazione</li><li>Inserito in lista bloccati</li><li>Città</li><li>Tipo di azienda Microsoft</li><li>Nome della società</li><li>Paese</li><li>Data creazione</li><li>Data di nascita</li><li>Dipartimento</li><li>Non effettuare la chiamata</li><li>Motivo per cui non effettuare la chiamata</li><li>Campi duplicati</li><li>Indirizzo e-mail</li><li>E-mail non valida</li><li>E-mail causa non valida</li><li>E-mail sospesa</li><li>E-mail sospesa alle</li><li>Causa e-mail sospesa</li><li>Numero di fax</li><li>Nome</li><li>Nome completo</li><li>Ha un’opportunità</li><li>Settore</li><li>Città dedotta</li><li>Azienda in oggetto</li><li>Paese in oggetto</li><li>Area metropolitana dedotta</li><li>Prefisso telefonico dedotto</li><li>Codice postale dedotto</li><li>Area geografica dello stato dedotta</li><li>È cliente</li><li>È partner</li><li>Professione</li><li>Cognome</li><li>Indirizzo e-mail proprietario lead</li><li>Nome proprietario lead</li><li>Qualifica proprietario lead</li><li>Cognome proprietario lead</li><li>Numero di telefono proprietario lead</li><li>Nome partizione lead</li><li>Classificazione Lead</li><li>Punteggio Lead</li><li>Fonte Lead</li><li>Stato Lead</li><li>Numero di telefono</li><li>Marketing sospeso</li><li>Membro del set di campi</li><li>Membro dell’elenco</li><li>Membro di Nurture</li><li>Membro del programma</li><li>Membro del modello di ricavo</li><li>Fase membro ricavi</li><li>Membro della campagna SFDC</li><li>Membro della campagna intelligente</li><li>Membro dell’Elenco intelligente</li><li>Numero account Microsoft</li><li>Data di creazione Microsoft</li><li>Microsoft è stato eliminato</li><li>Tipo Microsoft</li><li>Secondo nome</li><li>Numero di cellulare</li><li>Note</li><li>Numero dipendenti</li><li>Numero di opportunità</li><li>Destinatario che inoltra originale</li><li>Motore di ricerca originale</li><li>Frase di ricerca originale</li><li>Informazioni sorgente originali</li><li>Tipo di sorgente originale</li><li>Nome azienda madre</li><li>Fuso orario della persona</li><li>Numero di telefono</li><li>Codice di avviamento postale</li><li>Campione casuale</li><li>Registrazione informazioni Source</li><li>Tipo Source registrazione</li><li>Ruolo</li><li>Formula di saluto</li><li>Numero account SFDC</li><li>Data di creazione SFDC</li><li>SFDC è stato eliminato</li><li>Tipo SFDC</li><li>Codice SIC (Standard Industrial Classification)</li><li>Sito</li><li>Stato</li><li>Importo totale dell’opportunità</li><li>Ricavi previsti opportunità totali</li><li>Annulla l'iscrizione</li><li>Motivo di annullamento dell'iscrizione</li><li>Data di aggiornamento</li><li>Sito Web</li></ul></td>
</tr>
</table>
