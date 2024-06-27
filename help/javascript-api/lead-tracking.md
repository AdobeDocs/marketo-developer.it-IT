---
title: Tracciamento lead
description: API di tracciamento dei lead
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 0%

---

# API di tracciamento dei lead

Il JavaScript Munchkin di Marketo consente il tracciamento dei clic e delle visite degli utenti finali sulle pagine di destinazione e sulle pagine web esterne Marketo. Questi vengono registrati in Marketo come attività &quot;Visita pagina web&quot; e &quot;Collegamento cliccato su pagina web&quot;, che possono quindi essere utilizzate nei trigger e nei filtri per le campagne intelligenti e gli elenchi avanzati.

## Incorporazione del codice

L’istanza di Marketo fornisce automaticamente snippet di codice di tracciamento preconfigurati per incorporare nelle pagine esterne codice che tiene traccia dell’attività nell’istanza di Marketo. L&#39;utilizzo del codice di incorporamento è disciplinato da quanto segue [contratto di licenza](../munchkin-license.pdf).

Sono disponibili tre tipi di codici di tracciamento:

1. Semplice: viene caricato in modo sincrono
1. Asincrono - Caricamenti asincroni
1. jQuery asincrona: viene caricato in modo asincrono e richiede il caricamento anticipato di jQuery.

Si consiglia vivamente di utilizzare il codice di tracciamento asincrono per incorporare Munchkin in pagine esterne. Per garantire il più alto tasso di successo possibile per l’esecuzione, incorpora il codice di tracciamento asincrono in `<head>` di ogni pagina.

Alcuni sistemi di gestione dei contenuti possono disporre di metodi o restrizioni specifici quando incorporano script arbitrari.

Per riferimento, la pagina finale deve includere un codice simile a questo in `<head>` del documento HTML:

```html
<head>
    <script type="text/javascript">
    (function() {
        var didInit = false;
        function initMunchkin() {
            if(didInit === false) {
                didInit = true;
                Munchkin.init('CHANGE-ME');
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
    ...
</head>
```

## Comportamento Munchkin

Per impostazione predefinita, Marketo Munchkin esegue le seguenti operazioni al caricamento della pagina:

1. Verifica se il browser corrente dispone di un cookie Munchkin e, in caso contrario, creane uno.
1. Invia un evento &quot;Visita pagina web&quot; all’istanza di Marketo designata utilizzando le informazioni della pagina corrente e del browser. Registra un’attività nel record corrispondente in Marketo.
1. Invia l&#39;evento &quot;Collegamento selezionato su pagina Web&quot; per tutti i clic dell&#39;utente che si verificano sui collegamenti.

Il comportamento di Munchkin può essere modificato tramite l&#39;utilizzo di Munchkin [Impostazioni di configurazione](lead-tracking.md#lead-tracking-api), ad esempio se un cookie viene creato per tutti i lead quando si visita la pagina con `cookieAnon` impostazione o modifica del ritardo di clic con `clickTime` impostazione. L’invio dell’attività Visita può essere disabilitato impostando apiOnly su true. A partire dalla versione 162 (agosto 2022), fai clic su `tel` e `mailto` oltre a, vengono tracciati i collegamenti `http/s` collegamenti.

## Lead noti e anonimi

Durante la prima visita di un lead a una pagina del tuo dominio, viene creato un nuovo record lead anonimo in Marketo. La chiave primaria di questo record è il cookie Munchkin (`_mkto_trk`) creato nel browser dell&#39;utente. Tutte le successive attività web su quel browser vengono registrate su questo record anonimo. Per essere associato a un record noto in Marketo, è necessario che si verifichi una delle seguenti situazioni:

- Il lead deve visitare una pagina tracciata da Munchkin con `mkt_tok` nella stringa query da un collegamento e-mail Marketo tracciato.
- Il lead deve compilare un Marketo Form.
- A SOAP [syncLead](../soap-api/leads.md) o REST [Associa lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) è necessario inviare una chiamata.

Una volta soddisfatta una di queste condizioni, il cookie e tutte le attività web associate vengono associate al lead noto.

Viene creato un nuovo record anonimo di attività Web per ogni singolo browser, quindi se un lead visita il dominio per la prima volta utilizzando un nuovo computer e/o browser, l’associazione deve essere ripetuta.

## Domini

Munchkin crea e tiene traccia di singoli cookie per ogni dominio. In questo modo, affinché il tracciamento dei lead noti venga eseguito in più domini, è necessario che si verifichi un evento di associazione dei lead per ogni dominio. Ad esempio, se controllo due domini, `marketo.com`, e `example.com`e un lead compila un modulo in `marketo.com`, quindi passa a `example.com` più tardi, quindi la loro attività il `marketo.com` viene tracciato su un record lead noto, ma la loro attività su `example.com` è anonimo. Tuttavia, i lead noti persistono tra i sottodomini, pertanto un lead noto su `www.example.com` è anche un lead noto su `info.example.com`.

Se il dominio di primo livello è composto da due parti, ad esempio `.co.uk`, quindi aggiungi un parametro domainLevel al frammento Munchkin affinché il codice possa essere tracciato correttamente. Consulta [qui](lead-tracking.md#domains) per ulteriori dettagli.

## Cookie

Il cookie Munchkin utilizza la chiave `_mkto_trk`, e ha un valore che segue questo pattern:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

I cookie Munchkin sono specifici di ciascun dominio di secondo livello, ovvero `example.com`. La durata predefinita del cookie è di 2 anni (730 giorni).

## Beta

Per accedere al canale beta Munchkin per le pagine di destinazione, vai al tuo [Amministratore -> Torace tesoro](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) e abilitare l’impostazione &quot;Munchkin Beta sulle pagine di destinazione&quot;. Questo fornisce nuovi snippet di codice nel **[!UICONTROL Admin]** ->  **[!UICONTROL Munchkin]** per utilizzare la versione beta su siti esterni.

## Rinuncia

I visitatori possono rinunciare completamente al tracciamento Munchkin aggiungendo il `querystring` parametro &quot;marketo_opt_out=true&quot; per l’URL nel browser. Quando Munchkin JavaScript rileva questa impostazione, tenta di impostare un nuovo cookie &quot;mkto_opt_out&quot; con il valore `true`. Tutti gli altri cookie di tracciamento di Marketo vengono eliminati, non vengono impostati nuovi cookie e non vengono effettuate richieste HTTP da Munchkin quando viene rilevata questa impostazione.
