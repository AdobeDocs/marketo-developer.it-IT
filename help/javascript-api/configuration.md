---
title: Configurazione
description: Configura Marketo Munchkin con l’API di JavaScript. Scopri le impostazioni di Munchkin.init come altIds, anonymizeIP, asyncOnly, la durata del cookie, domainLevel, Beacon API.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 3%

---

# Configurazione

Munchkin può accettare varie impostazioni di configurazione per personalizzare il comportamento. Le impostazioni di configurazione sono proprietà di un oggetto JavaScript passato come secondo parametro durante la chiamata a [Munchkin.init()](api-reference.md#munchkin_init)

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

L&#39;oggetto impostazioni di configurazione può contenere un numero qualsiasi di proprietà della tabella seguente.

## Proprietà

| Nome | Tipo di dati | Descrizione |
|---|---|---|
| altIds | Array | Accetta un array di stringhe Munchkin ID. Se questa opzione è abilitata, duplica tutte le attività web agli abbonamenti target, in base al loro Munchkin ID. |
| anonymizeIP | Booleano | Anonimizza l’indirizzo IP registrato in Marketo per i nuovi visitatori. |
| apiOnly | Booleano | Se impostato su true, la funzione `Munchkin.Init()` non chiamerà `visitsWebPage`. Questa funzione è utile per le applicazioni Web a pagina singola che richiedono il controllo completo su ogni evento `visitsWebPage`. |
| asyncOnly | Booleano | Se è impostato su true, invia il file XMLHttpRequest in modo asincrono. Il valore predefinito è false. |
| clickTime | Intero | Imposta il tempo di blocco dopo un clic per consentire la richiesta di tracciamento dei clic (in millisecondi). La riduzione di questo valore riduce la precisione del tracciamento dei clic. Il valore predefinito è 350 ms. |
| cookieAnon | Booleano | Se impostato su false, impedisce il tracciamento e la creazione di cookie di nuovi lead anonimi. I lead dispongono di cookie e vengono tracciati dopo la compilazione di un modulo di Marketo o facendo clic su un messaggio e-mail di Marketo. Il valore predefinito è true. |
| cookieLifeDays | Intero | Imposta la data di scadenza di qualsiasi cookie di tracciamento di Munchkin appena creato su un numero così elevato di giorni in futuro. Il valore predefinito è 730 giorni (2 anni). |
| customName | Stringa | Nome pagina personalizzato. Solo per uso di sistema. |
| <a name="domainlevel"></a>livelloDominio | Intero | Imposta il numero di parti dal dominio della pagina da utilizzare quando si imposta l&#39;attributo di dominio del cookie.Ad esempio, supponiamo che il dominio della pagina corrente sia &quot;www.example.com&quot;.domainLevel: 2 imposterà l&#39;attributo del dominio del cookie su &quot;.example.com&quot;domainLevel: 3 imposterà l&#39;attributo del dominio del cookie su &quot;.www.example.com&quot;Background:Munchkin gestirà automaticamente alcuni domini di primo livello a due lettere. Il valore predefinito è due parti nei casi normali in cui il dominio di primo livello è composto da tre lettere. Ad esempio, &quot;www.example.com&quot;, le due parti più a destra vengono utilizzate per impostare il cookie, &quot;.example.com&quot;.Per i codici dei paesi a due lettere come &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; e &quot;.uk&quot;, il codice è composto di default da tre parti. Ad esempio, &quot;www.example.co.jp&quot; utilizzerà tre parti di dominio più a destra, &quot;.example.co.jp&quot;.Se il modello di dominio richiede un comportamento diverso, è necessario specificarlo utilizzando il parametro `domainLevel`. |
| domainSelectorV2 | Booleano | Se impostato su true, utilizza un metodo migliorato per determinare come impostare l’attributo di dominio del cookie. |
| httpsOnly | Booleano | Impostazione predefinita: false. Se è impostato su true, imposta il cookie in modo che utilizzi l&#39;impostazione Secure quando la pagina tracciata è stata trasmessa tramite https. |
| useBeaconAPI | Booleano | Impostazione predefinita: false. Se impostato su true, utilizza l&#39;API [Beacon](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) per inviare richieste non bloccanti anziché [XMLHttpRequest](https://developer.mozilla.org/it-IT/docs/Web/API/XMLHttpRequest). Se il browser non supporta questa API, Munchkin utilizza come fallback XMLHttpRequest. |
| wsInfo | Stringa | Richiede una stringa per eseguire il targeting di un&#39;area di lavoro. Questo ID area di lavoro si ottiene selezionando Workspace nel menu Admin > Integration > Munchkin (Amministrazione > Integrazione >). Questa impostazione si applica solo alla creazione iniziale di un record lead anonimo. Una volta stabilito il valore del cookie Munchkin per il record del lead, il parametro wsInfo non può essere utilizzato per modificare la partizione. Poiché questa impostazione influisce solo sui lead anonimi, è pertinente solo per [Visitatori anonimi specifici della partizione nei report Web](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports). |

## Esempi

### Invia attività a più abbonamenti

Questo esempio invia tutte le attività web alle istanze con Munchkin ID &quot;AAA-BBB-CCC&quot; e &quot;XXX-YYY-ZZZ&quot;.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCCC', { 'altIds': ['XXX-YYY-ZZZ'] });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

### Imposta tracciamento su asincrono

Questo esempio forza l&#39;invio asincrono di tutte le proprietà XMLHttpRequest dal thread principale.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCC', { 'asyncOnly': true });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin-beta.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```
