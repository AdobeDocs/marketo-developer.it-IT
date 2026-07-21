---
title: Configurazione
description: Configura Marketo Munchkin con l’API di JavaScript. Scopri le impostazioni di Munchkin.init come altIds, anonymizeIP, asyncOnly, la durata del cookie, domainLevel, Beacon API.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
TQID: https://experienceleague.adobe.com/ip2cCGgoa83v8m9GYLYXe132veYxS1C6UWX1iLB6X5Q
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 541
ht-degree: 5%

---

# Configurazione

Munchkin accetta le impostazioni di configurazione che ne personalizzano il comportamento. Passa le impostazioni come proprietà di un oggetto JavaScript nel secondo parametro di [Munchkin.init()](api-reference.md#munchkin_init).

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
| --- | --- | --- |
| altIds | Array | Accetta un array di stringhe Munchkin ID. Se questa opzione è abilitata, duplica tutte le attività web agli abbonamenti identificati dai relativi ID Munchkin. |
| anonymizeIP | Booleano | Anonimizza l’indirizzo IP registrato in Marketo per i nuovi visitatori. |
| apiOnly | Booleano | Se impostato su true, la funzione `Munchkin.Init()` non chiamerà `visitsWebPage`. Questa funzione è utile per le applicazioni Web a pagina singola che richiedono il controllo completo su ogni evento `visitsWebPage`. |
| asyncOnly | Booleano | Se impostato su true, invia XMLHttpRequests in modo asincrono. Il valore predefinito è false. |
| clickTime | Intero | Imposta il tempo, in millisecondi, per il blocco dopo un clic in modo che la richiesta di tracciamento dei clic possa essere completata. La riduzione di questo valore riduce la precisione del tracciamento dei clic. Il valore predefinito è 350 ms. |
| cookieAnon | Booleano | Se impostato su false, impedisce il tracciamento e la creazione di cookie per i nuovi lead anonimi. I lead ricevono i cookie e vengono tracciati dopo l’invio di un modulo Marketo o il click-through da un’e-mail Marketo. Il valore predefinito è true. |
| cookieLifeDays | Intero | Imposta la data di scadenza di qualsiasi cookie di tracciamento di Munchkin appena creato su un numero così elevato di giorni in futuro. Il valore predefinito è 730 giorni (2 anni). |
| customName | Stringa | Nome pagina personalizzato. Solo per uso di sistema. |
| <a name="domainlevel"></a>livelloDominio | Intero | Imposta il numero di parti del dominio della pagina da utilizzare per l’attributo del dominio del cookie.<br><br>Per &quot;www.example.com&quot;, `domainLevel: 2` imposta il dominio del cookie su &quot;.example.com&quot; e `domainLevel: 3` lo imposta su &quot;.www.example.com&quot;.<br><br>Per impostazione predefinita, Munchkin utilizza due parti quando il dominio di primo livello ha tre lettere. Ad esempio, &quot;www.example.com&quot; utilizza &quot;.example.com&quot;.<br><br>Per i codici paese di due lettere come &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot; e &quot;.uk&quot;, Munchkin utilizza tre parti. Ad esempio, &quot;www.example.co.jp&quot; utilizza &quot;.example.co.jp&quot;.<br><br>Utilizza il parametro `domainLevel` quando il modello di dominio richiede un comportamento diverso. |
| domainSelectorV2 | Booleano | Se impostato su true, utilizza un metodo migliorato per determinare come impostare l’attributo di dominio del cookie. |
| httpsOnly | Booleano | Impostazione predefinita: false. Se è impostato su true, imposta il cookie in modo che utilizzi l&#39;impostazione Secure quando la pagina tracciata è stata trasmessa tramite https. |
| useBeaconAPI | Booleano | Impostazione predefinita: false. Se impostato su true, utilizza l&#39;API [Beacon](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) per inviare richieste non bloccanti anziché [XMLHttpRequest](https://developer.mozilla.org/it-IT/docs/Web/API/XMLHttpRequest). Se il browser non supporta l’API Beacon, Munchkin utilizza XMLHttpRequest. |
| wsInfo | Stringa | Esegue il targeting di un’area di lavoro. Ottieni l’ID dell’area di lavoro selezionando l’area di lavoro nel menu Admin > Integration > Munchkin (Amministrazione > Integrazione >).<br><br>Questa impostazione si applica solo quando viene creato un record cliente potenziale anonimo. Una volta stabilito il valore del cookie Munchkin per il record del lead, il parametro wsInfo non può modificare la partizione.<br><br>Poiché questa impostazione riguarda solo i lead anonimi, è rilevante solo per i [visitatori anonimi specifici della partizione nei report Web](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports). |

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

Questo esempio forza l&#39;invio asincrono di tutte le richieste XMLHttp dal thread principale.

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
