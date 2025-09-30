---
title: Reindirizza
description: Implementa l’API di reindirizzamento RTP per inviare i visitatori segmentati a URL mirati utilizzando campi come ABM, organizzazione, posizione e segmenti, con esempi e suggerimenti.
feature: Javascript
exl-id: bbf91245-42e5-47ae-a561-e522cc65ff49
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 7%

---

# Reindirizza

L’API di reindirizzamento RTP consente di reindirizzare i tipi di pubblico segmentati a un URL di destinazione.

- Prima di utilizzare l&#39;API Contesto utente, è necessario diventare un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.
- RTP non supporta gli elenchi di account denominati Account Based Marketing (Marketing basato su account). Gli elenchi e il codice ABM si riferiscono solo agli elenchi di account caricati (file CSV) gestiti all’interno di RTP.

## Utilizzo

`rtp('send' , 'redirect' , 'field_name' , [ 'values_array' , '...' , '...' ] , 'www.redirect_url.com' , true/false )`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---------------------------|-------------------|---------|-----------------------------|
| &#39;invia&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;reindirizza&#39; | Obbligatorio | Stringa | Nome del metodo. |
| nome_campo | Obbligatorio | Stringa | Nome del campo da confrontare. Esempio: &quot;abm.name&quot; (vedi sotto). |
| matrice_valori | Obbligatorio | Array | Elenco di valori con cui confrontare il campo (senza distinzione maiuscole/minuscole). |
| redirect_url | Obbligatorio | Stringa | URL di destinazione per reindirizzare i visitatori che corrispondono alla condizione. |
| redirect_matched_visitors | Facoltativo | Booleano | Se true, i visitatori con la condizione corrispondente verranno reindirizzati. Se false, i visitatori senza corrispondenza vengono reindirizzati. Impostazione predefinita: true. |

Organizzazione, settore, elenchi ABM, ubicazione, ISP, segmenti corrispondenti

| Condizione | Gerarchia dei dati | Esempio |
|-------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------|
| Segmenti corrispondenti (funziona solo dopo il primo clic) | matchedSegments.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchedSegments.name&#39; , [&#39;Fortune 1,000&#39; , &#39;Enterprise&#39;] , &#39;<http://www.marketo.com>&#39;); |
| Segmenti corrispondenti (funziona solo dopo il primo clic) | matchedSegments.id | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;matchedSegments.id&#39; , [106 , 107 , 190] , &#39;<http://www.marketo.com>&#39;); |
| Elenchi ABM | abm.name | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.name&#39; , [&#39;top_key_accounts&#39;, &#39;active_customers&#39;] , &#39;<http://www.marketo.com>&#39;); |
| Elenchi ABM | abm.code | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;abm.code&#39; , [13 , 15] , &#39;<http://www.marketo.com>&#39;); |
| Organizzazioni | org | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;org&#39;, [&#39;ebay&#39;], &#39;<http://www.marketo.com>&#39;); |
| Posizione | location.country | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.country&#39; , [&#39;Stati Uniti&#39;], &#39;<http://www.marketo.com>&#39;); |
| Posizione | location.state | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.state&#39;, [&#39;ca&#39;], &#39;<http://www.marketo.com>&#39;); |
| Posizione | location.city | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;location.city&#39;, [&#39;San Mateo&#39;], &#39;<http://www.marketo.com>&#39;); |
| Settori | settori | rtp( &#39;send&#39;, &#39;redirect&#39; , &#39;industrie&#39; , [&#39;Istruzione&#39;], &#39;<http://www.marketo.com>&#39;); |
| ISP | isp | rtp( &#39;send&#39;, &#39;redirect&#39; , isp , [&#39;False&#39;], &#39;<http://www.marketo.com>&#39;); |

## Note

- Se la regola/condizione di reindirizzamento è basata su Firmographics (company, industry, location), è possibile inserire il codice di reindirizzamento prima di rtp(&#39;send&#39;, &#39;view&#39;) e rtp(&#39;get&#39;,&#39;campaign&#39;) per ridurre la latenza.
- Il reindirizzamento tramite JavaScript è un reindirizzamento lato browser e dipende dal caricamento e dall’ottimizzazione del sito web per raggiungere la velocità massima.
- La best practice prevede di impostare il codice di reindirizzamento subito dopo il tag rtp e inserirlo nell’intestazione.
- Assicurati di non eseguire un reindirizzamento automatico (in rtp è presente una rete di sicurezza per bloccare le chiamate di reindirizzamento ciclico).

```html
<!DOCTYPE html>
<html lang="en-US">
<head>
<!-- RTP tag -->
<script type='text/javascript'>

// This tag needs to be replaced with your account tag
(function(c,h,a,f,i){c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
c[a].a=i;var g=h.createElement("script");g.async=true;g.type="text/javascript";
g.src=f+'?rh='+c.location.hostname+'&aid='+i;var b=h.getElementsByTagName("script")[0];b.parentNode.insertBefore(g,b);
})(window,document,"rtp","//xyz.marketo.com/rtp-api/v1/rtp.js","xyz");

// START REDIRECT EXAMPLE
//   - Using a helper redirect function
//   - Redirect based on named account
rtp('send','redirect','org', ['microsoft'],'http://www.marketo.com');

// Redirect based on named account list (ABM)
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
// END REDIRECT EXAMPLE
rtp('send','view');
rtp('get','campaign');
</script>
<!-- End of RTP tag -->
```

## Reindirizzare i visitatori tracciati

1. Aggiungi un parametro alla fine dell’URL di destinazione: ad esempio &lt;www.marketo.com?rtp=redirect>
1. Creare un segmento denominato - &quot;Reindirizzato da RTP&quot;
1. Utilizza il parametro &quot;Pagine specifiche&quot; per indirizzare i visitatori che visualizzano qualsiasi pagina con il parametro mostrato di seguito.

![visitatori reindirizzati al tracciamento](assets/tracking-redirected-vistors.png)

## Come definire più di una condizione con URL di destinazione diversi

La chiamata di reindirizzamento supporta più chiamate. Questo consente di reindirizzare con più campi e creare condizioni complesse con URL e valori diversi.

### Utilizzo

`rtp('send', 'redirect', field_name, url_values_map);`

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| &#39;invia&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;reindirizza&#39; | Obbligatorio | Stringa | Nome del metodo. |
| nome_campo | Obbligatorio | Stringa | Nome del campo da confrontare. Esempio: &quot;abm.name&quot; (vedi sopra). |
| url_values_map | Obbligatorio | Oggetto | Mappa tra l’URL di reindirizzamento e l’elenco di valori. Esempio:{&#39;<http://marketo.com>&#39; : [&#39;first_abm&#39;, &#39;second_abm&#39;]} |

#### Esempio

```javascript
rtp('send','redirect','abm.name', {
    // Redirect visitors that match 'first_abm' list to www.marketo.com
    'http://www.marketo.com' : ['first_abm'],
    // Redirect visitors that match 'second_abm' list to blog.marketo.com
    'http://blog.marketo.com' : ['second_abm']
});
rtp('send','redirect','org', {
    // Redirect visitors from 'Microsoft' to www.marketo.com/enterprise
    'http://www.marketo.com/enterprise' : ['microsoft']
});
```
