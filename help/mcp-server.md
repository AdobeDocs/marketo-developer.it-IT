---
title: Server MCP
description: Scopri come collegare un assistente AI a Marketo utilizzando il server MCP. Configura Claude Desktop, Cursore, Claude Code o VS Code con le tue credenziali Marketo.
hidefromtoc: true
badgeBeta: label="Beta" type="informative" tooltip="Questa funzione è attualmente in una versione beta anticipata"
exl-id: ab446e56-6250-4af5-b03e-162991d09a5c
source-git-commit: b21ecb7a4dd2730807b0ad601b95fe387498f8f0
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 1%

---

# Server MCP [!DNL Marketo]

Il protocollo MCP (Model Context Protocol) è uno standard aperto che consente agli strumenti di intelligenza artificiale di comunicare con i servizi esterni. Il server MCP [!DNL Marketo] funge da ponte tra l&#39;assistente AI e [!DNL Marketo]. Espone più di 100 operazioni tra moduli, programmi, campagne intelligenti, lead, e-mail, snippet, elenchi e cartelle.

Quando lo strumento di intelligenza artificiale chiama il server MCP, il server esegue la chiamata API REST corrispondente per tuo conto, utilizzando le credenziali fornite in ogni richiesta. Non è necessario installare, distribuire o eseguire software lato server.

>[!IMPORTANT]
>
>Il Model Context Protocol (MCP) è uno standard open source emergente e può presentare rischi per la sicurezza o l&#39;affidabilità. Le integrazioni server MCP di Adobe e la relativa documentazione vengono fornite &quot;così come sono&quot;, senza garanzie di alcun tipo.
>La connessione di client o server MCP ai prodotti Adobe è una configurazione scelta dal cliente e i clienti sono responsabili della valutazione della sicurezza e dell’idoneità di qualsiasi integrazione MCP. Adobe non è responsabile dei problemi derivanti da configurazione errata, utilizzo errato di MCP, vulnerabilità in implementazioni di terze parti o azioni non intenzionali eseguite tramite flussi di lavoro abilitati per MCP.
>Per ridurre i rischi, Adobe incoraggia a testare le integrazioni in un ambiente sandbox prima di utilizzarle in modo produttivo e a rivedere e convalidare attentamente tutte le azioni e le risposte avviate da MCP prima di confermarle o di fare affidamento su di esse.

## Prerequisiti

- Un&#39;istanza [!DNL Marketo] con accesso REST API abilitato
- Accesso amministratore per creare credenziali API in [!DNL Marketo] LaunchPoint
- Uno dei seguenti strumenti di intelligenza artificiale: Claude Desktop, Cursore, Claude Code (CLI) o VS Code con GitHub Copilot
- Accesso di rete all&#39;URL del server MCP: `https://marketo-mcp.adobe.io/mcp`

## Ottieni credenziali Marketo

Sono necessari i seguenti valori dall&#39;istanza [!DNL Marketo]:

- **ID client**
- **Segreto client**
- **ID account Munchkin**

Se ne hai già uno, passa a [Configura il tuo strumento di intelligenza artificiale](#configure-your-ai-tool).

### ID client e segreto client

1. Vai a **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**.
1. Fai clic sul servizio API. Se non ne hai uno, seleziona **[!UICONTROL New]** > **[!UICONTROL New Service]**, scegli **[!UICONTROL Custom]** come tipo di servizio e assegna un utente API dedicato.
1. Fare clic su **[!UICONTROL View Details]** e copiare i valori **[!UICONTROL Client ID]** e **[!UICONTROL Client Secret]**.

### ID account Munchkin

1. Vai a **[!UICONTROL Admin]** > **[!UICONTROL Munchkin]**.
1. Copia **[!UICONTROL Munchkin Account ID]**. Il formato è `XXX-XXX-XXX` e corrisponde al prefisso dell&#39;URL istanza.

## Configurare lo strumento AI

Ogni strumento di intelligenza artificiale legge la configurazione del server MCP da una posizione diversa. Individuare lo strumento e seguire la procedura per aggiungere il server MCP [!DNL Marketo].

### Claude Desktop

Il file di configurazione è `claude_desktop_config.json`. Apri da una delle seguenti posizioni:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

Se il file contiene già altri server MCP, aggiungere la voce `marketo` in `mcpServers`. L&#39;esempio seguente mostra il blocco `mcpServers` completo:

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

Salvare il file, uscire da Claude Desktop e riaprirlo.

### Cursore

Se la configurazione MCP cursore contiene già altri server, aggiungere la voce `marketo` in `mcpServers`. L&#39;esempio seguente mostra il blocco `mcpServers` completo in **[!UICONTROL Settings]** > **[!UICONTROL MCP]** o `.cursor/mcp.json` nella directory del progetto:

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

Riavvia cursore.

### Codice Claude (CLI)

Esegui il comando seguente nel terminale, sostituendo le credenziali:

```bash
claude mcp add --transport http marketo \
  https://marketo-mcp.adobe.io/mcp \
  --header "X-Marketo-Client-Id: YOUR-CLIENT-ID" \
  --header "X-Marketo-Client-Secret: YOUR-CLIENT-SECRET" \
  --header "X-Marketo-Munchkin-Id: YOUR-MUNCHKIN-ID"
```

### Codice VS con GitHub Copilot

Aprire il codice VS `settings.json` premendo **[!UICONTROL Ctrl+Shift+P]** o **[!UICONTROL Cmd+Shift+P]** su macOS, quindi selezionando **[!UICONTROL Preferences: Open User Settings (JSON)]**. Aggiungi il seguente esempio:

```json
{
  "mcp": {
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
}
```

Premere **[!UICONTROL Ctrl+Shift+P]** (o **[!UICONTROL Cmd+Shift+P]** su macOS), digitare **[!UICONTROL Reload Window]** e premere Invio.

>[!NOTE]
>
>Per motivi di sicurezza, utilizza l’interpolazione delle variabili di ambiente nei file di configurazione anziché incollare direttamente le credenziali. È possibile fare riferimento a variabili utilizzando la sintassi `${MARKETO_CLIENT_SECRET}` e impostarle nell&#39;ambiente. Questo impedisce che le credenziali vengano archiviate in testo normale in file che possono essere salvati nel controllo delle versioni.

## Operazioni disponibili

Una volta connesso, puoi chiedere all’assistente AI di eseguire operazioni nelle seguenti categorie.

### Moduli

Sfogliare, creare, clonare e approvare i moduli. Aggiungi o rimuovi campi, configura regole di visibilità dei campi e identifica dove sono incorporati i moduli.

Esempio di prompt:

- &quot;Mostra tutti i moduli approvati&quot;
- &quot;Clonare il modulo Contattaci nella cartella Q2 Campaign&quot;
- &quot;Aggiungere un campo Società al modulo di richiesta demo&quot;

### Campagne avanzate

Crea campagne intelligenti, configura filtri di elenchi avanzati, aggiungi passaggi di flusso e attiva o disattiva campagne.

Esempio di prompt:

- &quot;Quali campagne intelligenti sono attive in questo momento?&quot;
- &quot;Crea una nuova campagna intelligente denominata Aggiornamento punteggio lead nella cartella Operazioni&quot;
- &quot;Mostrami i passaggi del flusso nella campagna e-mail di benvenuto&quot;

### Lead ed elenchi

Trova i lead per indirizzo e-mail, crea o aggiorna i record dei lead e gestisci l’iscrizione all’elenco statico.

Esempio di prompt:

- &quot;Trova il lead tramite e-mail jane@example.com&quot;
- &quot;Aggiungere 12345 ID lead all&#39;elenco MQL Q2&quot;
- &quot;Crea un nuovo elenco statico denominato Partecipanti ad eventi estivi&quot;

### Programmi

Creare, clonare e assegnare tag ai programmi. Sfoglia i programmi per tipo, canale o intervallo di date.

Esempio di prompt:

- &quot;Clonare il programma del webinar Q4 nella cartella Eventi 2026&quot;
- &quot;Crea un nuovo programma e-mail denominato Vendita estiva nella cartella Campagne&quot;
- &quot;Mostra tutti i programmi contrassegnati come webinar&quot;

### E-mail e snippet

Sfoglia le e-mail, crea e-mail dai modelli, aggiorna le sezioni di contenuto e gestisci snippet riutilizzabili.

Esempio di prompt:

- &quot;Mostra tutte le bozze di e-mail&quot;
- &quot;Aggiornare la sezione dell’intestazione dell’e-mail di benvenuto&quot;
- &quot;Quali risorse utilizzano lo snippet di promozioni per le feste?&quot;

### Struttura dell’istanza

Sfogliare cartelle, canali, tipi di tag e tipi di attività per comprendere la configurazione di [!DNL Marketo].

Esempio di prompt:

- &quot;Elenca tutte le cartelle in Marketo&quot;
- &quot;Mostra tutti i canali disponibili&quot;
- &quot;Quali tipi di tag sono configurati?&quot;

### Operazioni in blocco

Esporta i dati del lead in blocco e controlla lo stato del processo di importazione o esportazione.

Esempio di prompt:

- &quot;Crea un&#39;esportazione in blocco di lead creati negli ultimi 30 giorni&quot;
- &quot;Controllare lo stato del processo di esportazione xx&quot;

## Risoluzione dei problemi

| Errore | Causa | Correggi |
| ------- | ------- | ----- |
| &quot;Endpoint Marketo non fornito&quot; | Intestazione `X-Marketo-Endpoint` mancante nella configurazione. | Controlla nuovamente la configurazione MCP e conferma che tutte e quattro le intestazioni siano presenti. |
| &quot;Credenziali Marketo non fornite&quot; | Uno o più di `X-Marketo-Client-Id`, `X-Marketo-Client-Secret` o `X-Marketo-Munchkin-Id` sono mancanti. | Verifica che tutte e quattro le intestazioni siano presenti nella configurazione. |
| &quot;Errore di autenticazione&quot; | Credenziali non valide o scadute. | Ricontrolla l&#39;ID client e il segreto client in **[!UICONTROL Admin]** > **[!UICONTROL LaunchPoint]**. |
| &quot;403 Forbidden&quot; | Il tuo Munchkin ID non si trova nel server di cui è stato eseguito il inserisco nell&#39;elenco Consentiti di. | Contatta l&#39;amministratore MCP di [!DNL Marketo] per aggiungere il tuo Munchkin ID. |
| Timeout o rifiuto della connessione | Il server MCP non è raggiungibile dalla rete. | Conferma di poter raggiungere l’URL del server dal tuo ambiente. Se applicabile, controlla i requisiti VPN. |
| Le chiamate allo strumento restituiscono risultati vuoti | L’utente API non dispone delle autorizzazioni necessarie per il tipo di risorsa richiesto. | Chiedi all&#39;amministratore di [!DNL Marketo] di rivedere il ruolo utente API e le autorizzazioni. |

## Domande frequenti

+++I miei dati sono sicuri?

Le credenziali vengono trasmesse in intestazioni HTTP con ogni singola richiesta. Il server non memorizza o memorizza nella cache le credenziali tra le sessioni e ogni richiesta è completamente isolata.

+++

+++Più persone possono utilizzarlo contemporaneamente?

Sì. Il server è multi-tenant. Ogni utente si connette con le proprie credenziali e le richieste sono isolate l’una dall’altra.

+++

+++Cosa succede se il token di accesso scade?

Quando esegui l’autenticazione utilizzando ID client e Segreto client, il server gestisce automaticamente l’aggiornamento del token. Non è necessario eseguire alcuna azione.

+++

+++Devo installare o eseguire qualcosa?

No. Il server MCP è ospitato da Adobe. Devi solo configurare lo strumento di intelligenza artificiale per connetterti a esso.

+++

+++Di quali autorizzazioni [!DNL Marketo] ha bisogno l&#39;utente API?

L’utente API deve poter accedere ai tipi di risorse che intendi gestire. Assegna almeno un ruolo di sola lettura per le operazioni di navigazione e un ruolo di lettura/scrittura per la creazione o la modifica delle risorse. Rivolgiti al tuo amministratore [!DNL Marketo] per assegnare le autorizzazioni appropriate.

+++

+++Quali sono i limiti delle tariffe?

Il server MCP eredita i limiti di velocità API dell&#39;istanza [!DNL Marketo]. Utilizza un utente API dedicato per monitorare e gestire il consumo di quote.

+++

+++Quali strumenti di intelligenza artificiale sono supportati?

Claude Desktop, Cursore, Claude Code (CLI) e VS Code con GitHub Copilot. Qualsiasi strumento di intelligenza artificiale che supporta Model Context Protocol su HTTP dovrebbe funzionare.

+++

+++È possibile connettersi a più istanze di [!DNL Marketo]?

Sì. Aggiungi più voci nella configurazione MCP dello strumento di intelligenza artificiale, ciascuna con un nome univoco e le credenziali per l’istanza corrispondente. Ad esempio, è possibile configurare `marketo-prod` e `marketo-staging` come server separati.

+++

## Considerazioni sulla sicurezza

>[!IMPORTANT]
>
>Utilizza un utente API dedicato in [!DNL Marketo] con solo le autorizzazioni necessarie per il tuo lavoro. Non riutilizzare le credenziali di amministratore per l’accesso API.

- **Credenziali per richiesta.** L’ID client, il segreto client, l’ID Munchkin e l’endpoint REST API vengono trasmessi nelle intestazioni HTTP a ogni richiesta. Il server non li memorizza né li memorizza in cache.
- **Isolamento multi-tenant.** Ogni richiesta utilizza il proprio set di credenziali. I dati non si intersecano con la sessione di un altro utente.
- **Munchkin ID inserire nell&#39;elenco Consentiti.** Il server accetta solo richieste per [!DNL Marketo] istanze approvate. Le richieste che utilizzano un Munchkin ID non autorizzato vengono rifiutate con un errore 403.
- **Mantieni le credenziali al di fuori del controllo della versione.** Utilizzare l&#39;interpolazione delle variabili di ambiente (`${MARKETO_CLIENT_SECRET}`) se lo strumento di intelligenza artificiale lo supporta, in modo che le credenziali non vengano memorizzate in testo normale nei file di cui è stato eseguito il commit in un repository.
