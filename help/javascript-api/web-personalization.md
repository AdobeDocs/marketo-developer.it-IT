---
title: Web Personalization
description: Web Personalization
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 4%

---

# Web Personalization

L’API Web Personalization JavaScript estende la funzionalità di personalizzazione automatizzata della piattaforma. Consente il tracciamento degli eventi e la personalizzazione dinamica di una pagina web. Funzionalità aggiuntive: [Eventi dati personalizzati](custom-data-events.md), [Contenuto dinamico](web-personalization.md), [Ottieni dati visitatore](get-visitor-data.md), [Escludi tag per bot specifici](#exclude_tag_for_specific_bots).

- Prima di utilizzare l&#39;API Contesto utente, è necessario diventare un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.
- RTP non supporta gli elenchi di account denominati Account Based Marketing (Marketing basato su account). Gli elenchi e il codice ABM si riferiscono solo agli elenchi di account caricati (file CSV) gestiti all’interno di RTP.

## Impostazione tag

Il tag RTP deve essere inserito nell’intestazione della pagina personalizzata.

```javascript
<!-- RTP tag --> 
<script type='text/javascript'>
(function(c,h,a,f,e,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].p=e;c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b)})
(window,document,"rtp","[rtp-js-cdn-url]","[pod-url]","[accountId]");
</script>
<!-- End of RTP tag -->
```

## Configurazione account

Questo metodo viene chiamato automaticamente a livello di tag per impostare l’ID account rilevante. Puoi impostare l’ID account quando desideri suddividerlo tra domini diversi.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|--------------|-------------------|--------|--------------|
| &#39;setAccount&#39; | Obbligatorio | Stringa | Nome del metodo. |
| accountId | Obbligatorio | Stringa | ID account. |


```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funzioni di invio degli eventi

Questo metodo invia un evento di visualizzazione, utilizzato per il tracciamento delle pagine. Nell’esempio seguente, l’URL della pagina corrente viene tracciato come visualizzazione della pagina visitatore.

Trasmettendo il parametro opzionale &quot;page&quot; in questo metodo, la pagina corrente può essere sovrascritta.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|-----------|-------------------|--------|---------------------------------|
| &#39;invia&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;visualizza&#39; | Obbligatorio | Stringa | Nome del metodo. |
| pagina | Facoltativo | Stringa | Percorso relativo o URL della pagina intera. |


```javascript
// Example for Default Page
rtp('send', 'view');

// Example for Overriding Default Page
var page = 'my-page?param=1';
rtp('send', 'view', page);
```

## Escludi tag per bot specifici (agenti utente)

Per escludere specifici browser dall’invio di dati alla piattaforma Web Personalization (nel caso di bot identificati), aggiungi la seguente istruzione IF allo script tag.

Nell’esempio di codice seguente, &quot;Googlebot|msnbot&quot; viene utilizzato come esempio di bot da escludere dalle attività di Web Personalization.

```javascript
<!-- RTP tag --> 
<script type='text/javascript'>
if(navigator.userAgent.match(/.(Googlebot|msnbot)./gi) == null){
    (function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
    g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//[cdn-pod-X-url]/rtp-api/v1/rtp.js","[accountId]");

    rtp('send','view');
    rtp('get', 'campaign', true);
}
</script>
<!-- End of RTP tag -->
```

## Spiegazione delle chiamate JavaScript

Descrizione di JavaScript che viene aggiunta a un sito web quando si utilizzano Personalization web e Contenuto predittivo.

### JavaScript core/dipendenti

| Nome | Descrizione | Controllo |
|---------------------------|-------------|--------------------------------------------------------|
| rtp.js | - | Controllato da Marketo |
| jquery.min.js | v1.8.3 | Può essere disattivato contattando l’Assistenza clienti di Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Può essere disattivato contattando l’Assistenza clienti di Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Può essere disattivato contattando l’Assistenza clienti di Marketo |


*Utilizzato solo se manca la finestra di dialogo dell’interfaccia utente jQuery

### JavaScript on-demand

| Nome | Descrizione | Controllo |
|-------------------------|-----------------------------------------------------------------------|-----------------------|
| ga-integration-2.0.1.js | Utilizzato se l’integrazione Google Analytics/Facebook/SiteCatalyst è abilitata | Controllato da Marketo |
| insightera-bar-2.1.js | Utilizzato se la barra dei consigli di contenuti predittivi è abilitata | Controllato da Marketo |
| froogaloop2.min.js | Utilizzato se il tracciamento del contenuto è abilitato e il lettore Vimeo esiste nella pagina | - |
| iframe-api-v1.js | Utilizzato se il tracciamento dei contenuti è abilitato e se il lettore YouTube esiste nella pagina | - |
