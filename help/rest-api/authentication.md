---
title: Autenticazione
feature: REST API
description: Autentica le API REST di Marketo con 2 gambe OAuth 2.0, crea e utilizza token di accesso, passa all’intestazione Autorizzazione, gestisci la scadenza, gestisci gli errori 601 e 602.
exl-id: f89a8389-b50c-4e86-a9e4-6f6acfa98e7e
TQID: https://experienceleague.adobe.com/cIeI0m61CyIWq4HEosZ-QAsxzZb0WcrQRpCud2qysfY
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 519
ht-degree: 0%

---

# Autenticazione

Le API REST di Marketo utilizzano OAuth 2.0 a 2 gambe per l’autenticazione. Un servizio personalizzato fornisce l’ID client e il segreto client utilizzati per ottenere un token di accesso.

Ogni servizio personalizzato appartiene a un utente solo API. I ruoli e le autorizzazioni dell’utente autorizzano il servizio a eseguire azioni specifiche. Un token di accesso appartiene a un singolo servizio personalizzato e la sua scadenza è indipendente dai token per altri servizi personalizzati nell’istanza.

## Creazione di un token di accesso

Per trovare `Client ID` e `Client Secret`, passa a **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL LaunchPoint]**. Selezionare il servizio personalizzato, quindi selezionare **[!UICONTROL View Details]**.

![Ottieni dettagli servizio REST](assets/authentication-service-view-details.png)

![Credenziali Launchpoint](assets/admin-launchpoint-credentials.png)

Per trovare `Identity URL`, passa a **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]**. L’URL viene visualizzato nella sezione API REST.

Creare un token di accesso con una richiesta HTTP GET o POST:

```http
GET <Identity URL>/oauth/token?grant_type=client_credentials&client_id=<Client Id>&client_secret=<Client Secret>
```

Se la richiesta era valida, riceverai una risposta JSON simile alla seguente:

```json
{
    "access_token": "cdf01657-110d-4155-99a7-f986b2ff13a0:int",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "apis@acmeinc.com"
}
```

La risposta contiene i campi seguenti:

- `access_token`: token passato con le chiamate successive per l&#39;autenticazione con l&#39;istanza di destinazione.
- `token_type`: metodo di autenticazione OAuth.
- `expires_in`: durata rimanente del token corrente, in secondi. Un nuovo token di accesso ha una durata di 3.600 secondi o un’ora.
- `scope`: utente proprietario del servizio personalizzato utilizzato per l&#39;autenticazione.

## Utilizzo di un token di accesso

Ogni chiamata API REST deve includere un token di accesso in un’intestazione HTTP.

>[!IMPORTANT]
>
>Il supporto per l&#39;autenticazione tramite il parametro di query `access_token` verrà rimosso il 31 agosto 2026. Se il progetto utilizza un parametro di query per passare il token di accesso, è necessario aggiornarlo per utilizzare al più presto l&#39;[intestazione autorizzazione](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/authentication#using-an-access-token). Il nuovo sviluppo deve utilizzare esclusivamente l&#39;intestazione `Authorization`.

### Passaggio all’intestazione Autorizzazione

Per sostituire il parametro di query `access_token` con un&#39;intestazione Autorizzazione, aggiorna la modalità di invio del token da parte della richiesta.

Il seguente esempio di cURL invia il valore `access_token` come parametro di modulo con il flag `-F`:

```bash
curl ...  -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

L&#39;esempio seguente invia lo stesso valore nell&#39;intestazione HTTP `Authorization: Bearer` con il flag `-H`:

```bash
curl ... -H 'Authorization: Bearer <Access Token>' <REST API Endpoint Base URL>/bulk/v1/apiCall.json
```

## Suggerimenti e best practice

Memorizza il token di accesso e il periodo di scadenza della risposta di identità. La gestione della scadenza dei token consente di evitare errori di autenticazione imprevisti durante il normale funzionamento.

Prima di effettuare una chiamata REST, controlla la durata residua del token. Se il token è scaduto, rinnovarlo chiamando l&#39;endpoint [Identity](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET). Il rinnovo proattivo evita gli errori causati dai token scaduti e rende la latenza delle chiamate REST più prevedibile, importante per le applicazioni rivolte all’utente finale.

Gli errori di autenticazione restituiscono i seguenti codici:

- `602`: token di accesso scaduto.
- `601`: token di accesso non valido.

Se il client riceve uno di questi codici, rinnova il token chiamando l’endpoint Identity.

Se chiami l’endpoint Identity prima della scadenza del token, la risposta restituisce lo stesso token e la durata rimanente.

I token di accesso appartengono a servizi personalizzati, non agli utenti. Se le credenziali di due servizi diversi producono risposte di identità con ambito dello stesso utente, i token di accesso e i periodi di scadenza rimangono indipendenti.

Quando un’applicazione utilizza più set di credenziali, utilizza l’ID client come chiave per gestire ogni token in modo indipendente.
