---
title: Personalizzazione web
description: Guida all’API Web Personalization JavaScript e al tag RTP, che descrive gli eventi di visualizzazione della pagina, la configurazione dell’account, le esclusioni di bot e gli script core e on-demand
feature: Web Personalization, Javascript
exl-id: b2c26b28-e9bf-4faf-8b6e-c102f41aeaa1
TQID: https://experienceleague.adobe.com/yplunKmgjOJ7gJTA2TDc9cfJXyXbrVWuM-NdVbDMN4A
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
subfeature_v2:
  - id: cdd4e0f6-e87e-453f-88ee-2ee54a7de272
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 435
ht-degree: 6%

---

# Personalizzazione web

L’API JavaScript di Web Personalization tiene traccia degli eventi e personalizza dinamicamente le pagine web. Estende le funzionalità di personalizzazione automatizzata della piattaforma.

Le funzionalità correlate includono [Eventi dati personalizzati](custom-data-events.md), [Contenuto dinamico](web-personalization.md), [Ottieni dati visitatore](get-visitor-data.md) e [Escludi tag per bot specifici](#exclude_tag_for_specific_bots).

- Prima di utilizzare l&#39;API Contesto utente, è necessario essere un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.
- RTP non supporta gli elenchi di account denominati Account Based Marketing (Marketing basato su account). Gli elenchi e il codice ABM si riferiscono solo agli elenchi di account caricati (file CSV) gestiti all’interno di RTP.

## Impostazione tag

Inserisci il tag RTP nell’intestazione di ogni pagina personalizzata.

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

Il tag chiama automaticamente questo metodo per impostare l’ID account pertinente. Imposta esplicitamente l’ID account quando desideri utilizzare account diversi per domini diversi.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| &#39;setAccount&#39; | Obbligatorio | Stringa | Nome del metodo. |
| accountId | Obbligatorio | Stringa | ID account. |

```javascript
var accountId = '561-HYG-937';
rtp('setAccount', accountId);
```

## Funzioni di invio degli eventi

Questo metodo invia un evento di visualizzazione per il tracciamento della pagina. La prima chiamata nell’esempio seguente tiene traccia dell’URL della pagina corrente come visualizzazione della pagina visitatore.

Passa il parametro opzionale &quot;page&quot; per ignorare la pagina corrente, come mostrato nella seconda chiamata.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
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

Per evitare che i bot identificati inviino dati alla piattaforma Web Personalization, aggiungi la seguente istruzione `if` allo script tag.

Questo esempio esclude gli agenti utente &quot;Googlebot|msnbot&quot; dalle attività di Web Personalization.

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

Le tabelle seguenti descrivono il JavaScript aggiunto a un sito Web che utilizza Web Personalization e Predictive Content.

### JavaScript core/dipendenti

| Nome | Descrizione | Controllo |
| --- | --- | --- |
| rtp.js | - | Controllato da Marketo |
| jquery.min.js | v1.8.3 | Può essere disattivato contattando l’Assistenza clienti di Marketo |
| jquery-custom-ui-min.js | v1.9.2 | Può essere disattivato contattando l’Assistenza clienti di Marketo |
| query-ui-1.8.17-dialog.js | v1.9.2* | Può essere disattivato contattando l’Assistenza clienti di Marketo |

*Utilizzato solo se manca la finestra di dialogo jQuery UI.

### JavaScript on-demand

| Nome | Descrizione | Controllo |
| --- | --- | --- |
| ga-integration-2.0.1.js | Utilizzato se l’integrazione Google Analytics/Facebook/SiteCatalyst è abilitata | Controllato da Marketo |
| insightera-bar-2.1.js | Utilizzato se la barra dei consigli di contenuti predittivi è abilitata | Controllato da Marketo |
| froogaloop2.min.js | Utilizzato se il tracciamento del contenuto è abilitato e il lettore Vimeo esiste nella pagina | - |
| iframe-api-v1.js | Utilizzato se il tracciamento dei contenuti è abilitato e se il lettore YouTube esiste nella pagina | - |
