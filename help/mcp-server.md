---
title: Server Marketo Engage MCP
description: Scopri come collegare un assistente AI a Marketo utilizzando il server MCP di Marketo Engage. Configura Claude Desktop, Cursore, Claude Code o VS Code con le tue credenziali Marketo.
badgeBeta: label="Disponibilità limitata" type="informative" tooltip="Questa funzione è attualmente in versione beta limitata"
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
autotag-review: '2026-06-02T13:31:15.329Z'
TQID: 'https://experienceleague.adobe.com/PJJm7yv8HmbwMB2fsnfDCXs8zprDJK5Q5z2uiiCJRZI'
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: b13bd2ad-8e65-49e5-9691-2a0d31067b35id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: c2dbad80-0f5c-4d96-a798-2a65f93b8721id: dca84292-69e9-4116-a575-667d31fa060did: e2290edd-b061-4880-9d79-dee306cf5aa9id: e64968b2-4ee5-47f9-8cae-0588f184b9ebid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: bbbea26f-9621-49eb-9ab8-e06fb3bbce8c
source-git-commit: 6ec3c35ba988834f3a66b45acaa42af7a92cce20
workflow-type: tm+mt
source-wordcount: 1732
ht-degree: 1%

---

# Server MCP [!DNL Marketo Engage]

>[!AVAILABILITY]
>
> Questa funzione è a disponibilità limitata. Per richiedere l&#39;accesso, compila [questo modulo](https://forms.cloud.microsoft/Pages/ResponsePage.aspx?id=Wht7-jR7h0OUrtLBeN7O4Y-uSf63sAxCmWyqMJg8eMFUMVZSVExSNDA3T0I4SEcwRDFSVTBGWU01Uy4u&origin=QRCode){target="_blank"}. Assicurati di avere il Munchkin ID della tua iscrizione pronto.

Model Context Protocol (MCP) è uno standard aperto che consente agli strumenti di intelligenza artificiale di comunicare con servizi esterni. Il server MCP [!DNL Marketo] funge da ponte tra l&#39;assistente AI e [!DNL Marketo]. Espone più di 100 operazioni tra moduli, programmi, campagne intelligenti, lead, e-mail, snippet, elenchi e cartelle.

Quando lo strumento di intelligenza artificiale chiama il server MCP, il server esegue la chiamata API REST corrispondente per tuo conto, utilizzando le credenziali fornite in ogni richiesta. Non è necessario installare, distribuire o eseguire software lato server.


>[!IMPORTANT]
>
>Il Model Context Protocol (MCP) è uno standard open source emergente e può presentare rischi per la sicurezza o l&#39;affidabilità. Le integrazioni server MCP di Adobe e la relativa documentazione vengono fornite &quot;così come sono&quot;, senza garanzie di alcun tipo.
>La connessione di client o server MCP ai prodotti Adobe è una configurazione selezionata dal cliente e i clienti sono responsabili della valutazione della sicurezza e dell&#39;idoneità di qualsiasi integrazione MCP. Adobe non è responsabile dei problemi derivanti da configurazione errata, utilizzo errato di MCP, vulnerabilità in implementazioni di terze parti o azioni non intenzionali eseguite tramite flussi di lavoro abilitati per MCP.
>Per ridurre i rischi, Adobe incoraggia a testare le integrazioni in un ambiente sandbox prima di utilizzarle in modo produttivo e a rivedere e convalidare attentamente tutte le azioni e le risposte avviate da MCP prima di confermarle o affidarsi a esse.

## Nozioni di base su MCP

>Pensa a MCP come a una porta USB-C per applicazioni AI. Proprio come USB-C offre un modo standardizzato per collegare i dispositivi a varie periferiche e accessori, MCP offre un modo standardizzato per collegare i modelli AI a sorgenti di dati e strumenti. — [Protocollo contesto modello](https://modelcontextprotocol.io/docs/getting-started/intro){target="_blank"}

MCP consente a uno strumento di intelligenza artificiale di connettersi a più servizi esterni contemporaneamente. Ad esempio, un assistente AI potrebbe:

* Connettersi a un elaboratore di testi per la generazione di documenti basati sull’intelligenza artificiale
* Connettersi a strumenti di animazione, come Blender, per creare visualizzazioni
* Connessione a Adobe After Effects per l&#39;editing video

MCP è un protocollo di comunicazione: uno standard aperto che qualsiasi applicazione può implementare per esporre i propri dati e azioni agli strumenti di intelligenza artificiale.

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
* Uno dei seguenti strumenti di intelligenza artificiale: Claude Desktop, Cursore, Codex, Claude Code (CLI) o VS Code con GitHub Copilot
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

Ogni strumento di intelligenza artificiale ha una configurazione leggermente diversa. Per gli strumenti comuni vengono forniti esempi di connessione.

* [Claude Desktop](#claude-desktop)
* [Cursore](#cursor)
* [CLI Claude Code](#claude-code)
* [Codice OpenAI](#codex)
* [VSCode con GitHub Copilot](#vscode)
* [Glean](#glean)
* [Altri strumenti](#other-tools)

>[!TIP]
>
>Per connettersi a più istanze di [!DNL Marketo], aggiungere voci separate nella configurazione MCP con nomi univoci: `marketo-prod` e `marketo-staging`, ciascuna con le credenziali corrispondenti.

### Claude Desktop {#claude-desktop}

Per connettersi a Claude Desktop, scarica [marketo-mcp-bridge.zip](assets/marketo-mcp-bridge.zip) e decomprimi. Posiziona `marketo-mcp-bridge.mjs` in una posizione nota in modo da poter fare riferimento al passaggio successivo.

Avrà inoltre bisogno di:

* Node.js v18+
* npm

1. Apri Claude Desktop
1. Vai a **Impostazioni > Sviluppatore > Modifica configurazione**
1. Aggiungi quanto segue a `claude_desktop_config.json`:

```json
{
  "preferences": {
    ...
  },
  "mcpServers": {
    "marketo-mcp": {
      "command": "node",
      "args": ["/path/to/marketo-bridge/bridge.mjs"],
      "env": {
        "MARKETO_MCP_PROD_CLIENT_ID": "<your-client-id>",
        "MARKETO_MCP_PROD_CLIENT_SECRET": "<your-client-secret>",
        "MARKETO_MCP_PROD_MUNCHKIN_ID": "<your-munchkin-id>"
      }
    }
  }
}
```

1. Riavvia Claude Desktop

### Cursore {#cursor}

Se la configurazione MCP cursore contiene già altri server, aggiungere la voce `marketo` in `mcpServers`.
L&#39;esempio seguente mostra il blocco `mcpServers` completo in **[!UICONTROL Settings]** > **[!UICONTROL MCP]** o `.cursor/mcp.json` nella directory del progetto:

>[!BEGINTABS]

>[!TAB Token IMS]

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "Authorization": "Bearer YOUR-IMS-TOKEN",
        "x-gw-ims-org-id": "YOUR-IMS-ORG-ID"
      }
    }
  }
}
```

>[!TAB Credenziali client Marketo]

```json
{
  "mcpServers": {
    "marketo": {
      "type": "http",
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

>[!ENDTABS]

Riavvia cursore.

### Codice Claude (CLI) {#claude-code}

Esegui il comando seguente nel terminale, sostituendo le credenziali:

>[!BEGINTABS]

>[!TAB Token IMS]

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "Authorization: Bearer YOUR-IMS-TOKEN" \
  --header "x-gw-ims-org-id: YOUR-IMS-ORG-ID"
```

>[!TAB Credenziali client Marketo]

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

>[!ENDTABS]

### Codice OpenAI {#codex}

1. Vai a Impostazioni > Server MCP > Aggiungi server
1. Aggiungi l&#39;URL del server: `https://marketo-mcp.adobe.io/mcp`
1. Aggiungi le intestazioni per il metodo di autenticazione:

>[!BEGINTABS]

>[!TAB Token IMS]

* Autorizzazione: &quot;Bearer YOUR-IMS-TOKEN&quot;
* x-gw-ims-org-id: &quot;YOUR-IMS-ORG-ID&quot;

>[!TAB Credenziali client Marketo]

* X-Marketo-Client-Id: &quot;YOUR-CLIENT-ID&quot;
* X-Marketo-Client-Secret: &quot;YOUR-CLIENT-SECRET&quot;
* X-Marketo-Munchkin-Id: &quot;YOUR-MUNCHKIN-ID&quot;

>[!ENDTABS]

1. Fai clic su Salva per completare il processo.


### Codice VS con GitHub Copilot {#vscode}

Premere **[!UICONTROL Ctrl+Shift+P]** (o **[!UICONTROL Cmd+Shift+P]** su macOS), digitare **[!UICONTROL MCP: Open User Configuration]** e premere Invio. Verrà aperto `mcp.json`. Aggiungi la voce `marketo` all&#39;interno dell&#39;oggetto `servers`:

>[!BEGINTABS]

>[!TAB Token IMS]

```json
{
  "servers": {
    "marketo": {
      "type": "http",
      "url": "https://marketo-mcp.adobe.io/mcp",
      "headers": {
        "Authorization": "Bearer YOUR-IMS-TOKEN",
        "x-gw-ims-org-id": "YOUR-IMS-ORG-ID"
      }
    }
  }
}
```

>[!TAB Credenziali client Marketo]

```json
{
  "servers": {
    "marketo": {
      "type": "http",
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

>[!ENDTABS]

>[!NOTE]
>
>Per motivi di sicurezza, utilizza l’interpolazione delle variabili di ambiente nei file di configurazione anziché incollare direttamente le credenziali. È possibile fare riferimento a variabili utilizzando la sintassi `${MARKETO_CLIENT_SECRET}` e impostarle nell&#39;ambiente. In questo modo le credenziali non vengono memorizzate come testo normale in file controllati dalla versione.

### Glean {#glean}

Per connettere Glean al server Marketo Engage MCP, il [team di supporto Glean](https://docs.glean.com/release-notes/releases/2026-04-22-april-release#admin-features) deve configurare le seguenti intestazioni personalizzate.

| Intestazione | Valore |
| ------ | ----- |
| `X-Marketo-Client-Id` | ID client |
| `X-Marketo-Client-Secret` | Segreto client |
| `X-Marketo-Munchkin-Id` | ID del tuo account Munchkin |

### Altri strumenti {#other-tools}

Il server MCP [!DNL Marketo] è ospitato da Adobe ed esposto a un URL pubblico. Qualsiasi client MCP che supporti server remoti tramite trasporto HTTP semplificabile può connettersi ad esso.
Non è necessario un bridge specifico per lo strumento o un software installato localmente. Se lo strumento non è elencato sopra, utilizza i dettagli di connessione riportati di seguito per configurarlo manualmente.

**Dettagli connessione:**

| Impostazione | Valore |
| ------- | ----- |
| Trasporto | HTTP (HTTP semplificabile) |
| URL server | `https://marketo-mcp.adobe.io/mcp` |

**Intestazioni di autenticazione:**

Invia le intestazioni per uno dei seguenti metodi di autenticazione a ogni richiesta. La posizione in cui inserisci l’URL del server e le intestazioni dipende dallo strumento, quindi consulta la relativa documentazione MCP.

>[!BEGINTABS]

>[!TAB Token IMS]

| Intestazione | Valore |
| ------ | ----- |
| `Authorization` | `Bearer YOUR-IMS-TOKEN` |
| `x-gw-ims-org-id` | ID organizzazione IMS |

>[!TAB Credenziali client Marketo]

| Intestazione | Valore |
| ------ | ----- |
| `X-Marketo-Client-Id` | ID client |
| `X-Marketo-Client-Secret` | Segreto client |
| `X-Marketo-Munchkin-Id` | ID del tuo account Munchkin |

>[!ENDTABS]

Se lo strumento accetta una configurazione JSON, inizia con gli esempi di [Cursore](#cursor) o [Codice VS](#vscode) e regola le chiavi (`mcpServers`, `servers`) in modo che corrispondano allo schema dello strumento.

## Operazioni disponibili

Una volta connesso, puoi chiedere all’assistente AI di eseguire operazioni nelle seguenti categorie. Per l&#39;elenco completo delle operazioni supportate con riferimenti API, vedere [Operazioni MCP supportate](mcp-server-operations.md).

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

Trova i lead per indirizzo e-mail, crea o aggiorna i record dei lead e gestisci l’iscrizione all’elenco statico.

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
