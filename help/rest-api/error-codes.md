---
title: Codici errore
feature: REST API
description: Descrizioni dei codici di errore di Marketo.
exl-id: a923c4d6-2bbc-4cb7-be87-452f39b464b6
source-git-commit: 71e685efb25acffb5c1000293daaea4e936fc4f5
workflow-type: tm+mt
source-wordcount: '2320'
ht-degree: 3%

---

# Codici errore

Di seguito sono riportati gli elenchi dei codici di errore REST API e una spiegazione di come gli errori vengono restituiti alle applicazioni.

## Gestione e registrazione delle eccezioni

Durante lo sviluppo per Marketo, è importante che le richieste e le risposte vengano registrate quando si verifica un’eccezione imprevista. Mentre alcuni tipi di eccezioni, ad esempio l’autenticazione scaduta, possono essere gestiti in modo sicuro tramite la riautenticazione, altri possono richiedere interazioni di supporto e in questo caso le richieste e le risposte verranno sempre richieste.

## Tipi di errore

L’API REST di Marketo può restituire tre diversi tipi di errori in condizioni operative normali:

* Livello HTTP: questi errori sono indicati da un codice `4xx`.
* Response-Level: questi errori sono inclusi nell’array &quot;errors&quot; della risposta JSON.
* Record-Level: questi errori sono inclusi nell’array &quot;result&quot; della risposta JSON e sono indicati su base record individuale con il campo &quot;status&quot; e l’array &quot;reason&quot;.

Per i tipi di errore a livello di risposta e di record, viene restituito il codice di stato HTTP 200. Per tutti i tipi di errore, la frase relativa al motivo HTTP non deve essere valutata in quanto è facoltativa e soggetta a modifiche.

### Errori a livello HTTP

In circostanze operative normali, Marketo dovrebbe restituire solo due errori del codice di stato HTTP, `413 Request Entity Too Large` e `414 Request URI Too Long`. Entrambi possono essere recuperati recuperando l’errore, modificando la richiesta e riprovando, ma con le pratiche di codifica intelligente non dovresti mai incontrarli in modo selvaggio.

Marketo restituirà 413 se il payload della richiesta supera 1 MB, o 10 MB in caso di lead di importazione. Nella maggior parte degli scenari è improbabile che questi limiti vengano raggiunti, ma l’aggiunta di un controllo alle dimensioni della richiesta e lo spostamento di eventuali record, che causano il superamento del limite a una nuova richiesta, dovrebbe evitare qualsiasi circostanza che porti alla restituzione di questo errore da parte di qualsiasi endpoint.

Verrà restituito 414 quando l’URI di una richiesta GET supera gli 8 KB. Per evitarlo, confrontalo con la lunghezza della stringa di query per vedere se supera questo limite. Se la richiesta viene modificata in un metodo POST, immettere la stringa di query come corpo della richiesta con il parametro aggiuntivo `_method=GET`. In questo modo viene superata la limitazione sugli URI. È raro che questo limite venga raggiunto nella maggior parte dei casi, ma è piuttosto comune quando si recuperano grandi batch di record con valori di filtro singoli lunghi, ad esempio un GUID.
L&#39;endpoint [Identity](https://developer.adobe.com/marketo-apis/api/identity/) può restituire un errore 401 Unauthorized. Ciò è in genere dovuto a un ID client non valido o a un segreto client non valido. Codici di errore a livello HTTP

<table>
  <thead>
    <tr>
      <th> Codice di risposta </th>
      <th> Descrizione </th>
      <th colspan="1"> Commento </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="413"></a>413</td>
      <td>Entità richiesta troppo grande</td>
      <td>Il payload ha superato il limite di 1 MB.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>URI richiesta troppo lungo</td>
      <td>L’URI della richiesta ha superato gli 8k. La richiesta deve essere ritentata come POST con il parametro `_method=GET` nell’URL e il resto della stringa di query nel corpo della richiesta.</td>
    </tr>
  </tbody>
</table>

#### Errori a livello di risposta

Gli errori a livello di risposta sono presenti quando il parametro `success` della risposta è impostato su false e sono strutturati nel modo seguente:

```json
{
    "requestId": "e42b#14272d07d78",
    "success": false,
    "errors": [
        {
            "code": "601",
            "message": "Unauthorized"
        }
    ]
}
```

Ogni oggetto nell&#39;array &quot;errors&quot; ha due membri, `code`, che è un numero intero tra 601 e 799 e un `message` che fornisce il motivo plaintext dell&#39;errore. I codici 6xx indicano sempre che una richiesta non è riuscita completamente e non è stata eseguita. Un esempio è un 601, &quot;Token di accesso non valido&quot;, che è recuperabile autenticando nuovamente e passando il nuovo token di accesso con la richiesta. Gli errori 7xx indicano che la richiesta non è riuscita perché non sono stati restituiti dati o perché la richiesta non era parametrizzata correttamente, ad esempio includendo una data non valida o mancando un parametro obbligatorio.

#### Codici di errore a livello di risposta

Una chiamata API che restituisce questo codice di risposta non viene conteggiata rispetto alla quota giornaliera o al limite di tariffa.

<table>
  <thead>
    <tr>
      <th> Codice di risposta </th>
      <th> Descrizione </th>
      <th> Commento </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="502"></a>502</td>
      <td>Gateway non valido</td>
      <td>Errore restituito dal server remoto. Probabile timeout. La richiesta deve essere ritentata con un backoff esponenziale.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Token di accesso non valido</td>
      <td>Nella richiesta è stato incluso un parametro Token di accesso, ma il valore non era un token di accesso valido.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Token di accesso scaduto</td>
      <td>Il token di accesso incluso nella chiamata non è più valido a causa della scadenza.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Accesso negato</td>
      <td>L’autenticazione è riuscita, ma l’utente non dispone di autorizzazioni sufficienti per chiamare questa API. [Autorizzazioni aggiuntive](custom-services.md) potrebbero dover essere assegnate al ruolo utente, oppure <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access">il Inserisco nell'elenco Consentiti per l'accesso API basato su IP</a> potrebbe essere abilitato.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Timeout della richiesta</td>
      <td>La richiesta era in esecuzione da troppo tempo (ad esempio, si sono verificati conflitti nel database) o superava il periodo di timeout specificato nell’intestazione della chiamata.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>Metodo HTTP non supportato</td>
      <td>GET non è supportato per l’endpoint di sincronizzazione dei lead. È necessario utilizzare POST.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Limite di velocità massimo "%s"; superato con in "%s" sec</td>
      <td>Il numero di chiamate negli ultimi 20 secondi è stato superiore a 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Quota giornaliera raggiunta</td>
      <td>Il numero di chiamate di oggi ha superato la quota dell’abbonamento (viene ripristinato ogni giorno alle 00:00 CST).&gt;La quota si trova nel menu Admin-&gt;Web Services. Puoi aumentare la tua quota tramite il tuo account manager.</td>
    </tr>
    <tr>
      <td><a name="608"></a>608*</td>
      <td>API temporaneamente non disponibile</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="609"></a>609</td>
      <td>JSON non valido</td>
      <td>Il corpo incluso nella richiesta non è un JSON valido.</td>
    </tr>
    <tr>
      <td><a name="610"></a>610</td>
      <td>Risorsa richiesta non trovata</td>
      <td>L’URI nella chiamata non corrisponde a un tipo di risorsa API REST. Ciò è spesso dovuto a un URI di richiesta scritto in modo errato o formattato in modo errato</td>
    </tr>
    <tr>
      <td><a name="611"></a>611*</td>
      <td>Errore di sistema</td>
      <td>Tutte le eccezioni non gestite</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Tipo di contenuto non valido</td>
      <td>Se visualizzi questo errore, aggiungi alla richiesta un’intestazione di tipo di contenuto che specifica il formato JSON. Ad esempio, prova a utilizzare "content type: application/json". <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type">Per ulteriori dettagli, vedere la domanda StackOverflow</a>.</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Richiesta multipart non valida</td>
      <td>Il contenuto multipart del POST non è stato formattato correttamente</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Sottoscrizione non valida</td>
      <td>Impossibile trovare la sottoscrizione di destinazione oppure la sottoscrizione non è raggiungibile. Questo in genere indica un’inaccessibilità temporanea.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Limite di accesso simultaneo raggiunto</td>
      <td>Al massimo, le richieste vengono elaborate da qualsiasi abbonamento 10 alla volta. Viene restituito se sono già presenti 10 richieste in corso.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Tipo di sottoscrizione non valido</td>
      <td>Per accedere all’API dei metadati dell’oggetto personalizzato è necessario il tipo di abbonamento Marketo appropriato. Per informazioni, consulta il tuo CSM.</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s non può essere vuoto</td>
      <td>Il campo segnalato nella richiesta non può essere vuoto</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>Nessun dato trovato per un dato scenario di ricerca</td>
      <td>Nessun record corrisponde ai parametri di ricerca specificati.
        Nota: molte operazioni di ricerca non riuscite restituiscono "success = true" e nessun errore e impostano una stringa informativa di avvisi.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>La funzione non è abilitata per la sottoscrizione</td>
      <td>Funzione beta non abilitata in nell’abbonamento di un utente</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Formato data non valido</td>
      <td><ul>
          <li>È stata specificata una data con formato non corretto</li>
          <li>È stato specificato un ID di contenuto dinamico non valido</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Violazione di una regola business</td>
      <td>La chiamata non può essere soddisfatta perché viola il requisito di creare o aggiornare una risorsa, ad esempio se si tenta di creare un messaggio e-mail senza un modello. È inoltre possibile ricevere questo errore quando si tenta di:
        <ul>
          <li>Recupera il contenuto per le pagine di destinazione che contengono contenuti social.</li>
          <li>Clona un programma che contiene alcuni tipi di risorse (vedi <a href="programs.md#clone">Clone programma</a> per ulteriori informazioni).</li>
          <li>Approva una risorsa senza bozza (cioè che è già stata approvata).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Cartella padre non trovata</td>
      <td>Impossibile trovare la cartella principale specificata</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Tipo di cartella non compatibile</td>
      <td>La cartella specificata non è del tipo corretto per soddisfare la richiesta</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>Operazione di unione a persona Account non valida</td>
      <td>Chiamata Unisci lead non riuscita a causa di un tentativo di unione di lead che sono account persona Salesforce.  Gli account persona Salesforce devono essere uniti in Salesforce.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>Errore transitorio</td>
      <td>Al momento della chiamata API una risorsa di sistema non era temporaneamente disponibile. Quando si verifica questo errore, si consiglia di attendere e riprovare la richiesta.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>Impossibile trovare il tipo di record predefinito</td>
      <td>Chiamata di unione lead non riuscita. Impossibile trovare un tipo di record predefinito.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>ExternalSalesPersonID non trovato</td>
      <td>È stata effettuata una chiamata Sync Opportunities con un valore "ExternalSalesPersonID" inesistente.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Eccezione di timeout di attesa del blocco</td>
      <td>È stata effettuata una chiamata al programma Clone ed è stato eseguito il timeout in attesa di un blocco.</td>
    </tr>
  </tbody>
</table>

### Record-Level {#record_level_errors}

Gli errori a livello di record indicano che non è stato possibile completare un&#39;operazione per un singolo record, ma la richiesta stessa era valida. Una risposta con errori a livello di record segue questo schema:

#### Risposta

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "status":"skipped",
         "reasons":[
            {
               "code":"1005",
               "message":"Lead already exists"
            }
         ]
      }
   ]
}
```

I record inclusi nell’array di risultati delle chiamate vengono ordinati nello stesso modo dell’array di input di una richiesta.
Ogni record in una richiesta corretta può avere esito positivo o negativo su base individuale, indicato dal campo di stato di ogni record incluso nella matrice dei risultati di una risposta. Il campo &quot;status&quot; di questi record verrà &quot;ignorato&quot; ed è presente un array &quot;reason&quot;. Ogni motivo contiene un membro &quot;code&quot; e un membro &quot;message&quot;. Il codice è sempre 1xxx e il messaggio indica il motivo per cui il record è stato ignorato. Un esempio potrebbe essere un caso in cui una richiesta di sincronizzazione dei lead ha &quot;action&quot; impostato su &quot;createOnly&quot;, ma esiste già un lead per una delle chiavi nei record inviati. Questo caso restituisce il codice 1005 e un messaggio di &quot;Lead esiste già&quot; come mostrato sopra.

#### Codici di errore a livello di record

>[!NOTE]
>
><table>
><tbody>
>    <tr>
>      <td>Codice di risposta</td>
>      <td>Descrizione</td>
>      <td>Commento</td>
>    </tr>
>    <tr>
>      <td><a name="1001"></a>1001</td>
>      <td>Valore '%s' non valido. Richiesto di tipo "%s"</td>
>      <td>Viene generato un errore ogni volta che un valore di parametro presenta un tipo non corrispondente. Ad esempio, il valore stringa specificato per un parametro intero.</td>
>    </tr>
>    <tr>
>      <td><a name="1002"></a>1002</td>
>      <td>Valore mancante per il parametro obbligatorio '%s'</td>
>      <td>Viene generato un errore quando manca un parametro obbligatorio nella richiesta</td>
>    </tr>
>    <tr>
>      <td><a name="1003"></a>1003</td>
>      <td>Dati non validi</td>
>      <td>Quando i dati inviati non sono un tipo valido per l’endpoint o la modalità specificati, ad esempio quando l’ID viene inviato per un lead con azione designata come createOnly o quando si utilizza Request Campaign su una campagna batch.</td>
>    </tr>
>    <tr>
>      <td><a name="1004"></a>1004</td>
>      <td>Lead non trovato</td>
>      <td>Per syncLead, quando l'azione è "updateOnly" e se il lead non viene trovato</td>
>    </tr>
>    <tr>
>      <td><a name="1005"></a>1005</td>
>      <td>Il lead esiste già</td>
>      <td>Per syncLead, quando action è "createOnly" e se un lead esiste già</td>
>    </tr>
>    <tr>
>      <td><a name="1006"></a>1006</td>
>      <td>Campo '%s' non trovato</td>
>      <td>Un campo incluso nella chiamata non è valido.</td>
>    </tr>
>    <tr>
>      <td><a name="1007"></a>1007</td>
>      <td>Più lead corrispondono ai criteri di ricerca</td>
>      <td>Più lead corrispondono ai criteri di ricerca. È possibile eseguire aggiornamenti solo quando la chiave corrisponde a un singolo record</td>
>    </tr>
>    <tr>
>      <td><a name="1008"></a>1008</td>
>      <td>Accesso negato alla partizione '%s'</td>
>      <td>L'utente del servizio personalizzato non ha accesso a un'area di lavoro con la partizione in cui esiste il record.</td>
>    </tr>
>    <tr>
>      <td><a name="1009"></a>1009</td>
>      <td>È necessario specificare il nome della partizione</td>
>      <td></td>
>    </tr>
>    <tr>
>      <td><a name="1010"></a>1010</td>
>      <td>Aggiornamento della partizione non consentito</td>
>      <td>Il record specificato esiste già in una partizione lead separata.</td>
>    </tr>
>    <tr>
>      <td><a name="1011"></a>1011</td>
>      <td>Campo '%s' non supportato</td>
>      <td>Quando il campo di ricerca o "filterType" è specificato con campi standard non supportati (ad esempio: firstName, lastName)</td>
>    </tr>
>    <tr>
>      <td><a name="1012"></a>1012</td>
>      <td>Valore cookie '%s' non valido</td>
>      <td>Può verificarsi quando si chiama <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST">Associa lead</a> con un valore non valido per il parametro 'cookie'.
>        Ciò si verifica anche quando si chiamano <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET">Get Leads by Filter Type</a> con "filterType=cookies" e un valore non valido per il parametro "filterValues".</td>
>    </tr>
>    <tr>
>      <td><a name="1013"></a>1013</td>
>      <td>Oggetto non trovato</td>
>      <td>Ottieni oggetto (elenco, campagna) per ID restituisce questo codice di errore</td>
>    </tr>
>    <tr>
>      <td><a name="1014"></a>1014</td>
>      <td>Impossibile creare l'oggetto</td>
>      <td>Impossibile creare l'oggetto (elenco)</td>
>    </tr>
>    <tr>
>      <td><a name="1015"></a>1015</td>
>      <td>Lead non nell’elenco</td>
>      <td>Il lead designato non è un membro dell'elenco di destinazione</td>
>    </tr>
>    <tr>
>      <td><a name="1016"></a>1016</td>
>      <td>Troppe importazioni</td>
>      <td>Troppe importazioni in coda. È consentito un massimo di 10</td>
>    </tr>
>    <tr>
>      <td><a name="1017"></a>1017</td>
>      <td>L’oggetto esiste già</td>
>      <td>Creazione non riuscita perché il record esiste già</td>
>    </tr>
>    <tr>
>      <td><a name="1018"></a>1018</td>
>      <td>CRM abilitato</td>
>      <td>Impossibile eseguire l'azione perché per l'istanza è abilitata un'integrazione CRM nativa.</td>
>    </tr>
>    <tr>
>      <td><a name="1019"></a>1019</td>
>      <td>Importazione in corso</td>
>      <td>L’elenco di destinazione è già in fase di importazione in</td>
>    </tr>
>    <tr>
>      <td><a name="1020"></a>1020</td>
>      <td>Troppi cloni da programmare</td>
>      <td>L’abbonamento ha raggiunto l’uso assegnato di "cloneToProgramName" nel programma di pianificazione della giornata</td>
>    </tr>
>    <tr>
>      <td><a name="1021"></a>1021</td>
>      <td>Aggiornamento della società non consentito</td>
>      <td>Aggiornamento società non consentito durante syncLead</td>
>    </tr>
>    <tr>
>      <td><a name="1022"></a>1022</td>
>      <td>Oggetto in uso</td>
>      <td>Eliminazione non consentita quando un oggetto è utilizzato da un altro oggetto</td>
>    </tr>
>    <tr>
>      <td><a name="1025"></a>1025</td>
>      <td>Stato del programma non trovato</td>
>      <td>È stato specificato uno stato per cambiare lo stato del programma lead che non corrisponde a uno stato disponibile per il canale del programma.</td>
>    </tr>
>    <tr>
>      <td><a name="1026"></a>1026</td>
>      <td>Oggetto personalizzato non abilitato</td>
>      <td>Impossibile eseguire l'azione perché per l'istanza non è abilitata l'integrazione di oggetti personalizzati.</td>
>    </tr>
>    <tr>
>      <td><a name="1027"></a>1027</td>
>      <td>Limite massimo tipi di attività raggiunto</td>
>      <td>L’abbonamento ha raggiunto il numero massimo di tipi di attività personalizzati disponibili.</td>
>    </tr>
>    <tr>
>      <td><a name="1028"></a>1028</td>
>      <td>È stato raggiunto il limite massimo di campi</td>
>      <td>Le attività personalizzate hanno un massimo di 20 attributi secondari.</td>
>    </tr>
>    <tr>
>      <td><a name="1029"></a>1029</td>
>      <td><ul>
>          <li>Troppi processi in coda</li>
>          <li>Esporta quota giornaliera superata</li>
>          <li>Processo già in coda</li>
>        </ul></td>
>      <td><ul>
>          <li>Le sottoscrizioni possono estrarre in blocco un massimo di 10 processi in coda in un determinato momento.</li>
>          <li>Per impostazione predefinita, i processi di estrazione sono limitati a 500 MB al giorno (ripristinati ogni giorno alle 00:00 CST).</li>
>          <li>L'ID di esportazione è già stato inserito nella coda.</li>
>        </ul></td>
>    </tr>
>    <tr>
>      <td><a name="1035"></a>1035</td>
>      <td>Tipo di filtro non supportato</td>
>      <td>In alcune sottoscrizioni, i seguenti tipi di filtro Estrazione lead bulk non sono supportati: updateAt, smartListId, smartListName.</td>
>    </tr>
>    <tr>
>      <td><a name="1036"></a>1036</td>
>      <td>Oggetto duplicato trovato nell'input</td>
>      <td>È stata effettuata una chiamata per aggiornare due o più record utilizzando la stessa chiave esterna. Ad esempio, una chiamata Sync Companies che utilizza lo stesso externalCompanyId per più società.</td>
>    </tr>
>    <tr>
>      <td><a name="1037"></a>1037</td>
>      <td>Il lead è stato saltato</td>
>      <td>Il lead è stato ignorato perché si trova già in o oltre questo stato.</td>
>    </tr>
>    <tr>
>      <td><a name="1042"></a>1042</td>
>      <td>Data runAt non valida</td>
>      <td>La data runAt specificata per Schedule Campaign era troppo lontana nel futuro (il massimo è 2 anni).</td>
>    </tr>
>    <tr>
>      <td><a name="1048"></a>1048</td>
>      <td>Eliminazione bozza oggetto personalizzato non riuscita</td>
>      <td>È stata effettuata una chiamata per scartare la versione bozza di un oggetto personalizzato.</td>
>    </tr>
>    <tr>
>      <td><a name="1049"></a>1049</td>
>      <td>Impossibile creare l'attività</td>
>      <td>Matrice di attributi troppo lunga.
>        La matrice di attributi passati al record supera la lunghezza massima di 65536 byte</td>
>    </tr>
>    <tr>
>      <td><a name="1076"></a>1076</td>
>      <td>La chiamata <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Unisci lead</a> con il flag mergeInCRM è 4.</td>
>      <td>Si sta creando un record duplicato. In alternativa, è consigliabile utilizzare un record esistente.
>        Questo è il messaggio di errore che Marketo riceve durante l’unione in Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1077"></a>1077</td>
>      <td>Chiamata <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Unisci lead</a> non riuscita a causa della lunghezza del campo "SFDC"</td>
>      <td>Una chiamata di Merge Leads con mergeInCRM impostato su true non è riuscita perché "SFDC Field" ha superato il limite di caratteri consentiti. Per correggerlo, riduci la lunghezza di "SFDC Field" o imposta mergeInCRM su false.</td>
>    </tr>
>    <tr>
>      <td><a name="1078"></a>1078</td>
>      <td>Chiamata <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Unisci lead</a> non riuscita a causa di un'entità eliminata, non un lead/contatto o perché i criteri di filtro del campo non corrispondono.</td>
>      <td>Errore di unione. Impossibile eseguire l'operazione di unione in CRM sincronizzato in modo nativo
>        Questo è il messaggio di errore che Marketo riceve durante l’unione in Salesforce.</td>
>    </tr>
>    <tr>
>      <td><a name="1079"></a>1079</td>
>      <td>Chiamata <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST">Unisci lead</a> non riuscita a causa di un conflitto di URL personalizzati in record duplicati</td>
>      <td>Una chiamata di unione di lead ha specificato molti lead con lo stesso URL personalizzato. Per risolvere, utilizzare l'interfaccia utente di Marketo Engage per unire questi record.</td>
>    </tr>
>  </tbody>
></table>
