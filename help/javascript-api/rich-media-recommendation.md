---
title: Consigli per contenuti multimediali avanzati
description: Consigli per contenuti multimediali avanzati
feature: Javascript
exl-id: ee92e46d-e529-40a2-a0d0-ee233916f004
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 4%

---

# Consigli per contenuti multimediali avanzati

Nella pagina in cui desideri visualizzare il modello Consigli per contenuti multimediali avanzati devono essere impostati i seguenti tag e chiamate API.

1. Nell’intestazione della pagina
   1. Installazione del tag RTP
   1. Aggiungi la chiamata GET alla pagina per compilare i consigli
   1. Aggiungi la chiamata SET per configurare il modello
1. Nel corpo della pagina
   1. Posizionare il tag del modello (classe div) nella posizione in cui si desidera visualizzare il modello

Ulteriori informazioni sono disponibili [qui](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/predictive-content/enabling-predictive-content/enable-predictive-content-for-web-rich-media).

## Tag modello

| Attributo | Facoltativo/Obbligatorio | Descrizione |
|---|---|---|
| classe | Obbligatorio | Specifica che l&#39;elemento div HTML è un div di consigli RTP. |
| data-rtp-template-id | Obbligatorio | ID del modello. Questo determina l’allineamento del consiglio. Utilizzare &quot;template1&quot; per l&#39;allineamento orizzontale, &quot;template2&quot; per l&#39;allineamento verticale o &quot;template3&quot; per l&#39;allineamento verticale che include solo titolo e descrizione. Lo script inserisce il modello corrispondente in `div.Permissible` valori: template1, template2, template3. |

### Esempi

Per visualizzare i consigli in allineamento orizzontale, utilizza &quot;template1&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
```

Per visualizzare i consigli in allineamento verticale, utilizza &quot;template2&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template2"></div>
```

Per visualizzare i consigli in allineamento verticale solo con titolo e descrizione, utilizza &quot;modello3&quot;.

```html
<div class="RTP_RCMD2" data-rtp-template-id="template3"></div>
```

Vedi le schermate degli allineamenti dei modelli [qui](#example_of_rich_media_recommendation_template_1).

## Popolare consiglio

Questo metodo popola tutti i rich media `<divs>` sulla pagina con i consigli.

### Utilizzo

`rtp('get', 'rcmd', 'richmedia');`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| &#39;get&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;rcmd&#39; | Obbligatorio | Stringa | Nome del metodo. |
| &#39;richmedia&#39; | Obbligatorio | Stringa | Nome del metodo secondario. |


## Cambia configurazione modello

Questo metodo modifica la configurazione predefinita per il modello.

Nota: quando si utilizza questo metodo, è necessario chiamarlo prima di chiamare rtp(&#39;get&#39;,&#39;rcmd&#39;, &#39;richmedia&#39;);

### Utilizzo

`rtp('set', 'rcmd', 'richmedia', 'template_id', conf_obj);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| &#39;set&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;rcmd&#39; | Obbligatorio | Stringa | Nome del metodo. |
| &#39;richmedia&#39; | Obbligatorio | Stringa | Nome del metodo secondario. |
| template_id | Facoltativo | Stringa | ID modello per le modifiche alla configurazione. Consente di specificare la modifica delle impostazioni per un solo modello. |
| conf_obj | Obbligatorio | Oggetto | La nuova configurazione. L&#39;oggetto contiene tutte le configurazioni come coppia chiave/valore. |


### Esempi

Questo frammento di codice modifica il testo del titolo di un modello.

```javascript
rtp("set", "rcmd", "richmedia","template1",
    {
        "rcmd.title.text": "RECOMMENDED CONTENT"
    }
);
```

Questo frammento di codice mostra come impostare categorie con più configurazioni per un modello.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial",
            "category":
            [
                "webinar",
                "blog posts",
                "pricing_page_category",
                "product_a_category"
            ]
        }
    }
);
```

NOTA: utilizza &quot;categoria&quot; per filtrare il contenuto visualizzato nel risultato dei consigli sui contenuti predittivi. Per applicare il contenuto predittivo a tutte le parti di contenuto abilitate, lascia vuota la &quot;categoria&quot;. Se desideri consigliare solo contenuto specifico per l’output nel modello Rich Media, aggiungi una categoria per il contenuto nella pagina Imposta contenuto e associa tale categoria all’interno del codice del modello di consigli. Classificare i contenuti pertinenti in base alle sezioni del sito web (prodotti o soluzioni).

Questo frammento di codice mostra come impostare più configurazioni di modelli per un modello.

```javascript
rtp("set", "rcmd", "richmedia",
    {
        "template1":
        {
            "rcmd.title.text": "RECOMMENDED CONTENT",
            "rcmd.general.font.family": "arial"
        }
    }
);
```

#### Proprietà di configurazione

| Configurazione | Esempio | Descrizione |
|---|---|---|
| rcmd.general.font.family | &quot;rcmd.general.font.family&quot; : &quot;arial&quot; | Modifica la famiglia di caratteri per tutto il testo del modello. Questa proprietà supporta tutti i valori CSS per tipo di browser. È possibile utilizzare una famiglia di caratteri personalizzata se esiste nella pagina. |
| rcmd.content.background.color | &quot;rcmd.content.background.color&quot; : &quot;black&quot; | Modifica il colore di sfondo delle caselle interne del modello. Questa proprietà supporta tutti i valori CSS per tipo di browser. |
| rcmd.title.text | &quot;rcmd.title.text&quot; : &quot;CONTENUTO CONSIGLIATO&quot; | Modifica il titolo del modello. |
| rcmd.title.background.color | &quot;rcmd.title.background.color&quot; : &quot;blue&quot; | Modifica il colore di sfondo della casella del titolo. Questa proprietà supporta tutti i valori di colore css (nome colore, rgb, ...) |
| rcmd.title.font.size | &quot;rcmd.title.font.size&quot; : &quot;26px&quot; | Modifica la dimensione del carattere del titolo. La proprietà supporta tutte le possibili dimensioni di carattere del valore CSS (px, em, ...) |
| rcmd.title.font.color | &quot;rcmd.title.font.color&quot; : &quot;white&quot; | Cambia il colore del carattere del titolo. Questa proprietà supporta tutti i valori dei colori dei caratteri (rgb, hex, ...) |
| rcmd.description.font.color | &quot;rcmd.description.font.color&quot; : &quot;white&quot; | Cambia il colore del carattere della descrizione. Questa proprietà supporta tutti i valori dei colori dei caratteri (rgb, hex, ...) |
| rcmd.cta.background.color | &quot;rcmd.cta.background.color&quot; : &quot;green&quot; | Cambia il colore di sfondo del pulsante. Questa proprietà supporta tutto il valore del colore CSS (nome colore, rgb, ...) |
| rcmd.cta.font.color | &quot;rcmd.cta.font.color&quot; : &quot;rgb(90, 84, 164)&quot; | Cambia il colore del carattere del pulsante. Questa proprietà supporta tutti i valori dei colori dei caratteri (rgb, hex, ...) |
| rcmd.cta.text | &quot;rcmd.cta.text&quot;: &quot;Push&quot; | Modifica il testo del pulsante. Il testo è lo stesso per tutti i pulsanti. |
| categoria | &quot;categoria&quot;: [&quot;una categoria&quot;] | Modifica la categoria di consigli supportata dal modello. Il modello visualizza solo i consigli con una delle categorie impostate da questa configurazione. |


Nota: il supporto della configurazione può cambiare in base al modello.

#### Esempio di base

Questo esempio ha un modello con tre consigli. Copiare questo esempio in una pagina HTML e quindi sostituire il tag RTP con il tag.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Esempio avanzato

Questo esempio ha un modello con tre consigli. Il titolo del modello è &quot;CONTENUTO CONSIGLIATO&quot; e il testo del pulsante sarà &quot;Ulteriori informazioni&quot;. Copiare questo esempio in una pagina HTML e quindi sostituire il tag RTP con il tag.

```html
<!DOCTYPE>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>RTP recommendation</title>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i,e){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;c[a].e=e;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//example.rtp.com/rtp-api/v1/rtp.js","account_id");

// Send page view (required by  the recommendation)
rtp('send','view');
// Populate the recommendation zone
rtp('get', 'campaign',true);
// Change template configuration
rtp('set', 'rcmd', 'richmedia',
    {
        template1 :
        {
            "rcmd.title.text" : "RECOMMENDED CONTENT",
            "rcmd.cta.text" : "Read More"
        }
    }
);
// Populate recommendation
rtp('get','rcmd', 'richmedia');
</script>
<!-- End of RTP tag -->
</head>
<body>
<div class="RTP_RCMD2" data-rtp-template-id="template1"></div>
</body>
</html>
```

#### Esempio di #1 del modello di consigli per contenuti multimediali avanzati

**Nome**: modello1 **Descrizione**: contenuto orizzontale con immagine, titolo e descrizione e pulsante call to action.

![Modello Rich Media](assets/rich-media-template1.png)

#### Esempio di #2 del modello di consigli per contenuti multimediali avanzati

**Nome**: modello2 **Descrizione**: contenuto verticale con immagine, titolo e descrizione e pulsante call to action.

![Modello Rich Media](assets/rich-media-template2.png)

#### Esempio di #3 del modello di consigli per contenuti multimediali avanzati

**Nome**: modello3 **Descrizione**: contenuto verticale contenente solo titolo e descrizione. Al passaggio del mouse, l’intestazione cambia colore e viene collegata all’URL del contenuto. La descrizione contiene anche collegamenti al contenuto senza cambiamento di colore. ![Modello Rich Media](assets/rich-media-template3.png)
