---
title: Server Marketo Engage MCP
description: Scopri come collegare un assistente AI a Marketo utilizzando il server MCP di Marketo Engage. Configura Claude Desktop, Cursore, Claude Code o VS Code con le tue credenziali Marketo.
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
autotag-review: '2026-06-02T13:31:15.329Z'
TQID: 'https://experienceleague.adobe.com/PJJm7yv8HmbwMB2fsnfDCXs8zprDJK5Q5z2uiiCJRZI'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
    internal-label: Smart Campaigns
  - id: b0bb9048-d951-48d8-8232-45cf248a7e27
    internal-label: Forms
  - id: b13bd2ad-8e65-49e5-9691-2a0d31067b35
    internal-label: Integrations
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
    internal-label: Configuration
  - id: c2dbad80-0f5c-4d96-a798-2a65f93b8721
    internal-label: Assets
  - id: dca84292-69e9-4116-a575-667d31fa060d
    internal-label: APIs
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
    internal-label: Implementation
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
    internal-label: Programs
  - id: f82558ea-6af5-44eb-a424-5b3389abb0a3
    internal-label: Templates
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
topic_v2:
  - id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
    internal-label: Artificial intelligence
source-git-commit: 1cdb28422994b772a0fa430e213facaf26817952
workflow-type: tm+mt
source-wordcount: '2556'
ht-degree: 0%
---

# Server MCP [!DNL Marketo Engage]

Model Context Protocol (MCP) è uno standard aperto che collega gli strumenti di intelligenza artificiale a servizi esterni. Il server MCP [!DNL Marketo] collega l&#39;assistente AI a [!DNL Marketo]. Supporta le operazioni per la gestione delle risorse, i dati dei lead e l’automazione delle campagne. Consulta [Operazioni MCP supportate](mcp-server-operations.md) per eccezioni e requisiti di disponibilità.

Quando lo strumento di intelligenza artificiale chiama il server MCP, il server utilizza le credenziali in tale richiesta per eseguire la chiamata API REST corrispondente. Non è necessario installare, distribuire o eseguire software lato server.

Per ulteriori informazioni sulla gestione dei dati con Marketo AI e il server Marketo Engage MCP, vedere la pagina [Informazioni sui dati](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/marketo-ai/data-information).

>[!IMPORTANT]
>
>Il Model Context Protocol (MCP) è uno standard open source emergente e può presentare rischi per la sicurezza o l&#39;affidabilità. Le integrazioni server MCP di Adobe e la relativa documentazione vengono fornite &quot;così come sono&quot;, senza garanzie di alcun tipo.
>La connessione di client o server MCP ai prodotti Adobe è una configurazione scelta dal cliente e i clienti sono responsabili della valutazione della sicurezza e dell’idoneità di qualsiasi integrazione MCP. Adobe non è responsabile dei problemi derivanti da configurazione errata, utilizzo errato di MCP, vulnerabilità in implementazioni di terze parti o azioni non intenzionali eseguite tramite flussi di lavoro abilitati per MCP.
>Per ridurre i rischi, Adobe incoraggia a testare le integrazioni in un ambiente sandbox prima di utilizzarle in modo produttivo e a rivedere e convalidare attentamente tutte le azioni e le risposte avviate da MCP prima di confermarle o di fare affidamento su di esse.

## Nozioni di base su MCP

>Pensa a MCP come a una porta USB-C per applicazioni AI. USB-C offre un modo standardizzato per collegare i dispositivi a varie periferiche e accessori e MCP offre un modo standardizzato per collegare i modelli AI a origini dati e strumenti. — [Protocollo contesto modello](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

MCP consente a uno strumento di intelligenza artificiale di connettersi simultaneamente a più servizi esterni. Ad esempio, un assistente AI può:

* Connettersi a un elaboratore di testi per la generazione di documenti basati sull’intelligenza artificiale
* Connettersi a strumenti di animazione, come Blender, per creare visualizzazioni
* Connessione a Adobe After Effects per l&#39;editing video

Qualsiasi applicazione può implementare MCP per esporre dati e azioni a strumenti di intelligenza artificiale.

## Funzionamento di [!DNL Marketo Engage] MCP

Comprendere l’ambito di MCP consente di impostare le aspettative prima di collegare lo strumento AI.

**Operazione MCP:**

* Fornisce l&#39;accesso ai dati e alle funzionalità [!DNL Marketo] tramite API REST standard
* Eseguire chiamate API per conto dell’utente utilizzando le credenziali fornite con ogni richiesta
* Supporto di più utenti simultanei, ciascuno connesso con le proprie credenziali
* Gestisci aggiornamento token OAuth automaticamente. Non è necessario gestire la scadenza del token
* Operare in ambienti isolati dal tenant in modo che i dati non intersechino mai con la sessione di un altro utente

**MCP non:**

* Utilizza, ospita o esegui qualsiasi modello di intelligenza artificiale o apprendimento automatico. Tutta l’elaborazione di IA avviene nello strumento di IA, non in MCP
* Formazione o apprendimento da qualsiasi dato, inclusi i dati dei clienti
* Genera previsioni, consigli o decisioni. Il processo decisionale è responsabilità dello strumento di IA a valle o dell&#39;utente
* Memorizza o mantieni credenziali, dati di richiesta o stato di sessione tra le richieste
* Richiede l&#39;installazione, la distribuzione o la gestione di qualsiasi software lato server

MCP può trasmettere dati, inclusi campi potenzialmente sensibili, a seconda dell’utilizzo dell’API, ma i dati B2B coinvolgono i dati aziendali del cliente e non i dati PII.

## Prerequisiti

* Un&#39;istanza [!DNL Marketo] con accesso REST API abilitato
* Accesso amministratore per creare credenziali API in [!DNL Marketo] LaunchPoint
* Accesso di rete all&#39;URL del server MCP: `https://marketo-mcp.adobe.io/mcp`

## Ottieni credenziali Marketo

Sono necessari i seguenti valori dall&#39;istanza [!DNL Marketo]:

* **ID client**
* **Segreto client**
* **ID account Munchkin**

Se ne hai già uno, passa a [Configura il tuo strumento di intelligenza artificiale](#configure-your-ai-tool).

### ID client e segreto client

1. Vai a **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**.
1. Seleziona il servizio API. Se non ne hai uno, seleziona **[!UICONTROL New]** > **[!UICONTROL New Service]**, scegli **[!UICONTROL Custom]** come tipo di servizio e assegna un utente API dedicato.
1. Selezionare **[!UICONTROL View Details]** e copiare i valori **[!UICONTROL Client ID]** e **[!UICONTROL Client Secret]**.

### ID account Munchkin

1. Vai a **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]**.
1. Copia **[!UICONTROL Munchkin Account ID]**. Il formato è `XXX-XXX-XXX` e corrisponde al prefisso dell&#39;URL istanza.

## Configurare lo strumento AI

Le configurazioni variano leggermente a seconda dello strumento. In questa sezione vengono forniti esempi di connessione per gli strumenti comuni.

>[!TIP]
>
>Per connettersi a più istanze di [!DNL Marketo], aggiungere voci separate nella configurazione MCP con nomi univoci: `marketo-prod` e `marketo-staging`, ciascuna con le credenziali corrispondenti.

>[!BEGINTABS]

>[!TAB Claude Desktop]

### Claude Desktop {#claude-desktop}

Per connettersi a Claude Desktop, scarica [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) e decomprimi. Posiziona `marketo-mcp-bridge.mjs` in una posizione nota e utilizza il relativo percorso assoluto nella configurazione seguente.

È inoltre necessaria una versione LTS supportata di Node.js installata nel computer. Il bridge non richiede pacchetti npm aggiuntivi.

1. Apri Claude Desktop.
1. Apri `claude_desktop_config.json`. In macOS, il file si trova in `~/Library/Application Support/Claude/`. In Windows è in `%APPDATA%\Claude\`. Nelle versioni con il menu delle impostazioni per gli sviluppatori, seleziona **[!UICONTROL Settings]** > **[!UICONTROL Developer]** > **[!UICONTROL Edit Config]** per aprirlo.
1. Aggiungere la voce seguente in `mcpServers`, mantenendo le impostazioni esistenti. Sostituisci il percorso e i segnaposto delle credenziali con i valori:

```json
{
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/marketo-mcp-bridge.mjs"],
      "env": {
        "MARKETO_MCP_PROD_CLIENT_ID": "<your-client-id>",
        "MARKETO_MCP_PROD_CLIENT_SECRET": "<your-client-secret>",
        "MARKETO_MCP_PROD_MUNCHKIN_ID": "<your-munchkin-id>"
      }
    }
  }
}
```

1. Riavviare Claude Desktop.

Se Claude Desktop non riesce a trovare Node.js, sostituisci `node` in `command` con il percorso assoluto del tuo eseguibile Node.js. Per ulteriori informazioni, vedere [Connessione ai server MCP locali](https://modelcontextprotocol.io/docs/develop/connect-local-servers){target="_blank"}.

>[!TAB Cursore]

### Cursore {#cursor}

Aggiungi la voce `marketo` in `mcpServers` in `~/.cursor/mcp.json` per il tuo account utente o `.cursor/mcp.json` nella directory del progetto. Mantenere i server esistenti e sostituire i segnaposto delle credenziali con i valori:

```json
{
  "mcpServers": {
    "marketo": {
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

Aprire **[!UICONTROL Customize]** in Cursore e verificare che il server sia acceso. Per ulteriori informazioni, vedere [Configurazione MCP cursore](https://cursor.com/docs/context/mcp){target="_blank"}.

>[!TAB CLI codice Claude]

### Codice Claude (CLI) {#claude-code}

Esegui il comando seguente nel terminale, sostituendo le credenziali:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

Il comando aggiunge il server per il progetto corrente e l&#39;account utente. Per renderla disponibile in più progetti, aggiungere `--scope user`. In Claude Code, eseguire `/mcp` per verificare la connessione. Per ulteriori informazioni, vedere [Configurazione MCP codice Claude](https://code.claude.com/docs/en/mcp){target="_blank"}.

>[!TAB Codice OpenAI]

### Codice OpenAI {#codex}

Aggiungi quanto segue a `~/.codex/config.toml`, mantenendo eventuali configurazioni esistenti:

```toml
[mcp_servers.marketo]
url = "https://marketo-mcp.adobe.io/mcp"

[mcp_servers.marketo.env_http_headers]
X-Marketo-Client-Id = "MARKETO_CLIENT_ID"
X-Marketo-Client-Secret = "MARKETO_CLIENT_SECRET"
X-Marketo-Munchkin-Id = "MARKETO_MUNCHKIN_ID"
```

Impostare `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` e `MARKETO_MUNCHKIN_ID` nell&#39;ambiente del processo che esegue Codex. I valori in `env_http_headers` sono nomi di variabili di ambiente, non credenziali.

Avviare una nuova sessione Codex ed eseguire `/mcp` per verificare la connessione. Per ulteriori informazioni, vedere [Configurazione MCP Codex](https://developers.openai.com/codex/mcp){target="_blank"}.

>[!TAB CLI Gemini]

### CLI Gemini

Per aggiungere il server MCP [!DNL Marketo Engage] a Gemini CLI, aggiungere la seguente voce `mcpServers` a `.gemini/settings.json` nella directory del progetto o `~/.gemini/settings.json` per l&#39;account utente. Mantenere le impostazioni esistenti e sostituire i segnaposto delle credenziali con i valori.

```json
{
  "mcpServers": {
    "marketo": {
      "httpUrl": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "YOUR-CLIENT-ID",
        "X-Marketo-Client-Secret": "YOUR-CLIENT-SECRET",
        "X-Marketo-Munchkin-Id": "YOUR-MUNCHKIN-ID"
      }
    }
  }
}
```

In alternativa, impostare `MARKETO_CLIENT_ID`, `MARKETO_CLIENT_SECRET` e `MARKETO_MUNCHKIN_ID` nell&#39;ambiente della shell ed eseguire il comando seguente. La shell espande le variabili prima che CLI memorizzi i valori di intestazione nella configurazione utente:

```bash
gemini mcp add --transport http -s user marketo https://marketo-mcp.adobe.io/mcp \
  -H "X-Marketo-Client-Id: $MARKETO_CLIENT_ID" \
  -H "X-Marketo-Client-Secret: $MARKETO_CLIENT_SECRET" \
  -H "X-Marketo-Munchkin-Id: $MARKETO_MUNCHKIN_ID"
```

Riavviare la sessione ed eseguire `/mcp` per verificare la connessione. Per ulteriori informazioni, vedere [Configurazione MCP CLI Gemini](https://geminicli.com/docs/tools/mcp-server/){target="_blank"}.

>[!TAB Codice VS con Copilot GitHub]

### Codice VS con GitHub Copilot {#vscode}

Utilizza la configurazione del Copilot portatile per le versioni più recenti di Codice VS. Aggiungere la seguente voce in `mcpServers` in `~/.copilot/mcp-config.json` o `$COPILOT_HOME/mcp-config.json` se si imposta `COPILOT_HOME`. Mantieni i server esistenti:

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "X-Marketo-Client-Id": "${MARKETO_CLIENT_ID}",
        "X-Marketo-Client-Secret": "${MARKETO_CLIENT_SECRET}",
        "X-Marketo-Munchkin-Id": "${MARKETO_MUNCHKIN_ID}"
      }
    }
  }
}
```

Impostare le tre variabili di ambiente nell&#39;ambiente che esegue Codice VS o nell&#39;host remoto che si connette al server. Avviare una nuova sessione Copilot e rivedere eventuali richieste di attendibilità del server.

Per utilizzare la configurazione guidata, eseguire **[!UICONTROL MCP: Add Server]**, selezionare **[!UICONTROL HTTP (HTTP or Server-Sent Events)]**, immettere l&#39;URL del server e scegliere **[!UICONTROL Global]** > **[!UICONTROL Copilot Global]**. Quindi aggiungi le intestazioni alla configurazione salvata.

Le versioni precedenti supportano anche **[!UICONTROL MCP: Open User Configuration]** o `.vscode/mcp.json`. Questi file utilizzano `servers` invece della sintassi `mcpServers` e `${env:MARKETO_CLIENT_ID}` per le variabili di ambiente. Non mescolare i due formati. Per ulteriori informazioni, vedere [Configurazione MCP codice VS](https://code.visualstudio.com/docs/agents/reference/mcp-configuration){target="_blank"}.

>[!TAB Copilot Studio]

### Copilot Studio

Attiva l&#39;orchestrazione generativa per l&#39;agente prima di aggiungere strumenti MCP. Utilizzare un connettore personalizzato per definire tutte e tre le intestazioni [!DNL Marketo] richieste.

Nella pagina **[!UICONTROL Tools]** del tuo agente, seleziona **[!UICONTROL Add a tool]** > **[!UICONTROL New tool]** > **[!UICONTROL Custom connector]**. In Power Apps, creare un connettore personalizzato e utilizzare **[!UICONTROL Swagger editor]** per immettere la seguente definizione:

```yaml
swagger: '2.0'
info:
  title: Marketo MCP Server
  description: Connect to the Marketo MCP server.
  version: 1.0.0
host: marketo-mcp.adobe.io
basePath: /
schemes:
  - https
paths:
  /mcp:
    post:
      summary: Marketo MCP Server
      description: Invoke the Marketo MCP server.
      operationId: InvokeServer
      x-ms-agentic-protocol: mcp-streamable-1.0
      parameters:
        - name: X-Marketo-Client-Id
          in: header
          description: Client ID.
          type: string
          required: true
        - name: X-Marketo-Client-Secret
          in: header
          description: Client secret.
          type: string
          required: true
        - name: X-Marketo-Munchkin-Id
          in: header
          description: Munchkin ID.
          type: string
          required: true
      responses:
        '200':
          description: Immediate Response
securityDefinitions: {}
security: []
```

Salva o aggiorna il connettore, torna a Copilot Studio e aggiungilo al tuo agente. Crea una connessione e, quando richiesto, fornisci i tre valori di intestazione. Non inserire credenziali nella definizione OpenAPI e nelle istruzioni dell’agente. Per ulteriori informazioni, vedere [Connettere un agente a un server MCP esistente](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-add-existing-server-to-agent){target="_blank"}.

>[!TAB Glean]

### Glean {#glean}

Per connettere Glean al server MCP [!DNL Marketo Engage], seguire le istruzioni di installazione MCP remoto di [Glean](https://docs.glean.com/administration/tools/connect-remote-mcp-servers-to-glean){target="_blank"} per aggiungere `https://marketo-mcp.adobe.io/mcp`. Controllare prima il catalogo **[!UICONTROL Connectors]**. Per un server personalizzato o non elencato, utilizzare **[!UICONTROL Admin console]** > **[!UICONTROL Platform]** > **[!UICONTROL Tools]** > **[!UICONTROL Import tools from MCP server]**.

Il team del supporto tecnico [Glean deve configurare le intestazioni personalizzate](https://docs.glean.com/administration/tools/mcp-custom-headers){target="_blank"} di seguito. Includi il nome del server, l’URL e i nomi di intestazione richiesti nella richiesta di supporto e coordina la gestione sicura dei valori delle credenziali. Una connessione chiave API standard da sola non fornisce tutte e tre le intestazioni.

| Intestazione | Valore |
| ------ | ----- |
| `X-Marketo-Client-Id` | ID client |
| `X-Marketo-Client-Secret` | Segreto client |
| `X-Marketo-Munchkin-Id` | ID del tuo account Munchkin |

Dopo che Glean conferma che le intestazioni sono attive, verifica la connessione con una richiesta di sola lettura prima di rendere gli strumenti disponibili ad altri utenti.

>[!TAB Altri strumenti]

### Altri strumenti {#other-tools}

Adobe ospita il server MCP [!DNL Marketo] e lo espone a un URL pubblico. Qualsiasi client MCP che supporti server remoti tramite trasporto HTTP semplificabile può connettersi ad esso.
I client che supportano connessioni HTTP remote dirette non richiedono un bridge o un software server installato localmente. Se lo strumento non è elencato sopra, utilizza i dettagli di connessione riportati di seguito per configurarlo manualmente.

**Dettagli connessione:**

| Impostazione | Valore |
| ------- | ----- |
| Trasporto | HTTP (HTTP semplificabile) |
| URL server | `https://marketo-mcp.adobe.io/mcp` |

**Intestazioni di autenticazione:**

Invia tutte e tre le intestazioni con ogni richiesta. La posizione in cui inserisci l’URL del server e le intestazioni dipende dallo strumento, quindi consulta la relativa documentazione MCP.

| Intestazione | Valore |
| ------ | ----- |
| `X-Marketo-Client-Id` | ID client |
| `X-Marketo-Client-Secret` | Segreto client |
| `X-Marketo-Munchkin-Id` | ID del tuo account Munchkin |

Se lo strumento accetta una configurazione JSON, inizia con gli esempi di [Cursore](#cursor) o [Codice VS](#vscode) e regola le chiavi e la sintassi della variabile di ambiente in modo che corrispondano allo schema dello strumento.

>[!ENDTABS]

Dopo la connessione, chiedere all&#39;assistente AI di utilizzare il server MCP [!DNL Marketo] per elencare i canali disponibili senza modificare i dati. Ispeziona la chiamata dello strumento e la relativa risposta per verificare che la richiesta utilizzi il server previsto e restituisca i dati senza un errore di connessione o autenticazione.

## Operazioni disponibili

Una volta connesso, puoi chiedere all’assistente AI di eseguire operazioni nelle seguenti categorie. Per eccezioni e requisiti di disponibilità, vedere [Operazioni MCP supportate](mcp-server-operations.md).

### Moduli

Sfogliare, creare, clonare e approvare i moduli. Aggiungi o rimuovi campi, configura regole di visibilità dei campi e identifica dove sono incorporati i moduli.

Esempio di prompt:

* &quot;Mostra tutti i moduli approvati&quot;
* &quot;Clonare il modulo Contattaci nella cartella Q2 Campaign&quot;
* &quot;Aggiungere un campo Società al modulo di richiesta demo&quot;

### Campagne avanzate

Crea campagne intelligenti, configura filtri di elenchi avanzati, aggiungi passaggi di flusso e attiva o disattiva campagne.

Esempio di prompt:

* &quot;Quali campagne intelligenti sono attive in questo momento?&quot;
* &quot;Crea una nuova campagna intelligente denominata Aggiornamento punteggio lead nella cartella Operazioni&quot;
* &quot;Mostrami i passaggi del flusso nella campagna e-mail di benvenuto&quot;

### Lead ed elenchi

Trova i lead per indirizzo e-mail, crea record di lead e gestisci l’iscrizione a un elenco statico.

Esempio di prompt:

* &quot;Trova il lead tramite e-mail jane@example.com&quot;
* &quot;Aggiungere 12345 ID lead all&#39;elenco MQL Q2&quot;
* &quot;Crea un nuovo elenco statico denominato Partecipanti ad eventi estivi&quot;

### Programmi

Creare, clonare e assegnare tag ai programmi. Sfoglia i programmi per tipo, canale o intervallo di date.

Esempio di prompt:

* &quot;Clonare il programma del webinar Q4 nella cartella Eventi 2026&quot;
* &quot;Crea un nuovo programma e-mail denominato Vendita estiva nella cartella Campagne&quot;
* &quot;Mostra tutti i programmi contrassegnati come webinar&quot;

### E-mail e snippet

Sfoglia le e-mail, crea e-mail dai modelli, aggiorna le sezioni di contenuto e gestisci snippet riutilizzabili.

Esempio di prompt:

* &quot;Mostra tutte le bozze di e-mail&quot;
* &quot;Aggiornare la sezione dell’intestazione dell’e-mail di benvenuto&quot;
* &quot;Quali risorse utilizzano lo snippet di promozioni per le feste?&quot;

### Struttura dell’istanza

Per comprendere la configurazione di [!DNL Marketo], sfoglia cartelle, canali, tipi di tag e tipi di attività.

Esempio di prompt:

* &quot;Elenca tutte le cartelle in Marketo&quot;
* &quot;Mostra tutti i canali disponibili&quot;
* &quot;Quali tipi di tag sono configurati?&quot;

### Operazioni in blocco

Esporta i dati del lead in blocco e controlla lo stato del processo di importazione o esportazione.

Esempio di prompt:

* &quot;Crea un&#39;esportazione in blocco di lead creati negli ultimi 30 giorni&quot;
* &quot;Controllare lo stato del processo di esportazione xx&quot;

## Risoluzione dei problemi

| Errore | Causa | Correggi |
| ------- | ------- | ----- |
| &quot;Credenziali Marketo non fornite&quot; | Uno o più di `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` o `X-Marketo-Munchkin-Id` sono mancanti. | Verifica che tutte le intestazioni delle credenziali del client Marketo siano presenti nella configurazione. |
| &quot;401 Non autorizzato&quot; | Credenziali mancanti, non valide o scadute. Con le credenziali client di Marketo, l’ID client o il segreto client non è corretto. Con un token IMS, il token non è valido o è scaduto. | Verifica le credenziali per il metodo di autenticazione. Per le credenziali client, ricontrolla **[!UICONTROL Client ID]** e **[!UICONTROL Client Secret]** in **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. Per un token IMS, genera un nuovo token e aggiorna l&#39;intestazione `Authorization`. |
| &quot;403 Forbidden&quot; | Le credenziali sono valide, ma l&#39;istanza [!DNL Marketo] non è abilitata per l&#39;accesso MCP. | Contatta l&#39;amministratore MCP di [!DNL Marketo] per abilitare l&#39;accesso MCP per il tuo ID account Munchkin. |
| &quot;Troppe richieste&quot; (limite di tariffa) | Hai inviato troppe richieste in un breve periodo o troppe richieste contemporaneamente e hai raggiunto i limiti API dell&#39;istanza [!DNL Marketo]. | Riduci la frequenza e il numero di richieste inviate contemporaneamente e attendi un breve periodo di tempo prima di riprovare. Utilizza un utente API dedicato per tenere traccia e gestire la tua quota. |
| Timeout o rifiuto della connessione | Il server MCP non è raggiungibile dalla rete. | Conferma di poter raggiungere l’URL del server dal tuo ambiente. Se applicabile, controlla i requisiti VPN. |
| Le chiamate allo strumento restituiscono risultati vuoti | L’utente API non dispone delle autorizzazioni necessarie per il tipo di risorsa richiesto. | Chiedi all&#39;amministratore di [!DNL Marketo] di rivedere il ruolo utente API e le autorizzazioni. |

## Considerazioni sulla sicurezza

>[!IMPORTANT]
>
>Utilizza un utente API dedicato in [!DNL Marketo] con solo le autorizzazioni necessarie per il tuo lavoro. Non riutilizzare le credenziali di amministratore per l’accesso API.

* **Credenziali per richiesta.** L’ID client, il segreto client, l’ID Munchkin e l’endpoint REST API vengono trasmessi nelle intestazioni HTTP a ogni richiesta. Il server non li memorizza né li memorizza in cache.
* **Isolamento multi-tenant.** Ogni richiesta utilizza il proprio set di credenziali. I dati non si intersecano con la sessione di un altro utente.
* **Munchkin DI ID inserire nell&#39;elenco Consentiti.** Il server accetta solo richieste per [!DNL Marketo] istanze approvate. Le richieste che utilizzano un Munchkin ID non autorizzato vengono rifiutate con un errore 403.
* **Limiti di velocità API.** Il server MCP eredita i limiti di velocità API dell&#39;istanza [!DNL Marketo]. Utilizza un utente API dedicato per monitorare e gestire il consumo di quote.
* **Mantieni le credenziali al di fuori del controllo della versione.** Utilizzare l&#39;interpolazione delle variabili di ambiente (`${MARKETO_CLIENT_SECRET}`) se lo strumento di intelligenza artificiale lo supporta, in modo che le credenziali non vengano memorizzate in testo normale nei file del repository.

## Governance e conservazione dei dati

### Gestione delle credenziali

* Le credenziali del cliente non vengono rese persistenti lato server e vengono fornite dal client per richiesta, il che consente di limitare l’esposizione delle credenziali all’interno del servizio.

### Modello di interazione API

* Utilizzo agente: gli agenti possono utilizzare il server MCP per richiamare le API Marketo supportate.
* Allineamento modello di autenticazione: il servizio utilizza lo stesso modello di autenticazione API esterna documentato per le API Marketo.

### Autenticazione e autorizzazione

* Privilegi minimi: le autorizzazioni effettive vengono ereditate dall&#39;utente Marketo solo API assegnato al servizio LaunchPoint del cliente, consentendo l&#39;amministrazione dei privilegi minimi nella configurazione Marketo del cliente.
* Nessuna persistenza dei token lato server: il servizio continua a evitare l’archiviazione lato server delle credenziali del cliente o dei token.  

### Registrazione e monitoraggio

* Registrazione di sicurezza: i registri JSON strutturati vengono instradati attraverso Fluent Bit a Splunk, con mascheramento dei dati sensibili e filtro aggiuntivo per supportare i requisiti di conformità.
* Supporto di audit: questi controlli supportano il monitoraggio continuo della disponibilità dei servizi, degli eventi relativi alla sicurezza e della qualità operativa.
* Nessuna archiviazione segreta lato server: le credenziali del cliente non vengono memorizzate dalla distribuzione MCP e devono essere fornite dai client per richiesta.
* Gestione dei token: i token di accesso hanno vita breve, le risposte dei token sono contrassegnate come non archiviate e i token sono accettati tramite meccanismi di autorizzazione standard anziché tramite la trasmissione di stringhe di query.
* Accesso operativo basato sul ruolo: l’accesso all’implementazione amministrativa è gestito tramite i ruoli dell’infrastruttura Adobe e i controlli basati sul gruppo, mentre le autorizzazioni del piano dati vengono ereditate dalla configurazione utente dell’API Marketo del cliente.
* Audit e osservabilità: la registrazione, il mascheramento, il monitoraggio e gli avvisi di sicurezza sono abilitati per supportare l’indagine, il tracciamento dello stato del servizio e la supervisione operativa.
