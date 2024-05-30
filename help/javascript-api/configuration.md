---
title: "Configurazione"
description: "API di configurazione"
feature: Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 3%

---


# Configurazione

Munchkin può accettare varie impostazioni di configurazione per personalizzare il comportamento. Le impostazioni di configurazione sono proprietà di un oggetto JavaScript passato come secondo parametro durante la chiamata [Munchkin.init()](lead-tracking.md#munchkin-behavior)

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
| altIds | Array | Accetta un array di stringhe ID Munchkin. Se questa opzione è abilitata, tutte le attività web vengono duplicate nelle sottoscrizioni di destinazione, in base al relativo ID Munchkin. |
| anonymizeIP | Booleano | Anonimizza l&#39;indirizzo IP registrato in Marketo per i nuovi visitatori.È possibile determinare se l&#39;abbonamento è stato fornito con Munchkin V2 verificando se `{Munchkin-Id}.mktoresp.com` il dominio ha uno dei seguenti indirizzi: `192.28.144.124` `134.213.193.62` `192.28.147.68` `103.237.104.82`. In alternativa, puoi eseguire lo script seguente da una shell unix: nslookup {munchkin-id}.mktoresp.com | grep -E -c -e &quot;(192.28.144.124,134.213.193.62,192.28.147.68,103.237.104.82)&quot; Se il comando restituisce &#39;0&#39;, il provisioning dell&#39;abbonamento non viene eseguito con Munchkin V2. Se restituisce 1 o più, viene eseguito il provisioning. |
| apiOnly | Booleano | Se impostato su true, `Munchkin.Init()` la funzione non chiamerà `visitsWebPage`. Questa funzione è utile per le applicazioni web a pagina singola che richiedono il controllo completo su ogni `visitsWebPage` evento. |
| asyncOnly | Booleano | Se è impostato su true, invia il file XMLHttpRequest in modo asincrono. Il valore predefinito è false. |
| clickTime | Intero | Imposta il tempo di blocco dopo un clic per consentire la richiesta di tracciamento dei clic (in millisecondi). La riduzione di questo valore riduce la precisione del tracciamento dei clic. Il valore predefinito è 350 ms. |
| cookieAnon | Booleano | Se impostato su false, impedisce il tracciamento e la creazione di cookie di nuovi lead anonimi. I lead dispongono di cookie e vengono tracciati dopo la compilazione di un modulo di Marketo o facendo clic su un messaggio e-mail di Marketo. Il valore predefinito è true. |
| cookieLifeDays | Intero | Imposta la data di scadenza di tutti i cookie di tracciamento Munchkin appena creati sul numero di giorni futuri. Il valore predefinito è 730 giorni (2 anni). |
| customName | Stringa | Nome pagina personalizzato. Solo per uso di sistema. |
| domainLevel | Intero | Imposta il numero di parti dal dominio della pagina da utilizzare quando si imposta l&#39;attributo di dominio del cookie.Ad esempio, supponiamo che il dominio della pagina corrente sia &quot;www.example.com&quot;.domainLevel: 2 imposterà l&#39;attributo del dominio del cookie su &quot;.example.com&quot;domainLevel: 3 imposterà l&#39;attributo del dominio del cookie su &quot;.www.example.com&quot;Background:Munchkin gestirà automaticamente alcuni domini di primo livello a due lettere. Il valore predefinito è due parti nei casi normali in cui il dominio di primo livello è composto da tre lettere. Ad esempio, &quot;www.example.com&quot;, le due parti più a destra vengono utilizzate per impostare il cookie, &quot;.example.com&quot;.Per i codici dei paesi a due lettere come &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; e &quot;.uk&quot;, il codice è composto di default da tre parti. Ad esempio, &quot;www.example.co.jp&quot; utilizzerà tre parti di dominio più a destra, &quot;.example.co.jp&quot;.Se il modello di dominio richiede un comportamento diverso, questo deve essere specificato utilizzando `domainLevel` parametro. |
| domainSelectorV2 | Booleano | Se impostato su true, utilizza un metodo migliorato per determinare come impostare l’attributo di dominio del cookie. |
| httpsOnly | Booleano | Impostazione predefinita: false. Se è impostato su true, imposta il cookie in modo che utilizzi l&#39;impostazione Secure quando la pagina tracciata è stata trasmessa tramite https. |
| useBeaconAPI | Booleano | Impostazione predefinita: false. Se impostato su true, utilizza l’API Beacon per inviare richieste non di blocco anziché XMLHttpRequest. Se il browser non supporta questa API, Munchkin utilizza come fallback XMLHttpRequest. |
| wsInfo | Stringa | Richiede una stringa per eseguire il targeting di un&#39;area di lavoro. Questo ID area di lavoro si ottiene selezionando l’area di lavoro nel menu Amministrazione > Integrazione > Munchkin. Questa impostazione si applica solo alla creazione iniziale di un record lead anonimo. Una volta stabilito il valore del cookie Munchkin per il record del lead, il parametro wsInfo non può essere utilizzato per modificare la partizione. Poiché questa impostazione influisce solo sui lead anonimi, è pertinente solo per i visitatori anonimi specifici della partizione nei report web. |

## Esempi

### Invia attività a più abbonamenti

Questo esempio invia tutte le attività web alle istanze con ID Munchkin &quot;AAA-BBB-CCC&quot; e &quot;XXX-YYY-ZZZ&quot;.

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
