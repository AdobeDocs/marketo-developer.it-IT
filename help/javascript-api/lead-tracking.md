---
title: Tracciamento lead
description: Scopri come incorporare Marketo Munchkin JavaScript, tenere traccia di visite e clic, gestire lead noti e anonimi, cookie tra domini diversi e rinunciare a campagne intelligenti.
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
TQID: https://experienceleague.adobe.com/nGUcLLgL9X7PBKf2E5IzppDj8e-SyEtxmkQaESd90mE
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: a7170d27-32ab-462b-a333-269abc654483
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
subfeature_v2:
  - id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 718
ht-degree: 0%

---

# API di tracciamento dei lead

Munchkin JavaScript di Marketo tiene traccia delle visite alle pagine e dei clic sui collegamenti sulle pagine di destinazione e sulle pagine web esterne di Marketo. Marketo registra queste interazioni come attività &quot;Visita pagina web&quot; e &quot;Collegamento cliccato su pagina web&quot;.

Utilizza le attività in trigger e filtri per Campagne avanzate ed Elenchi smart.

## Incorporazione del codice

L’istanza Marketo fornisce snippet di codice preconfigurato per il tracciamento dell’attività da pagine esterne. L&#39;utilizzo del codice di incorporamento è disciplinato dal [contratto di licenza](../munchkin-license.pdf).

Sono disponibili tre tipi di codici di tracciamento:

1. Semplice (Simple) - Carica in modo sincrono.
1. Asincrono: viene caricato in modo asincrono.
1. Asynchronous jQuery - Carica in modo asincrono e richiede che jQuery venga caricato per primo.

Utilizza il codice di tracciamento asincrono per incorporare Munchkin in pagine esterne. Per ottenere il tasso di successo di esecuzione più alto possibile, inserire il codice nell&#39;elemento `<head>` di ogni pagina.

Alcuni sistemi di gestione dei contenuti possono disporre di metodi o restrizioni specifici quando incorporano script arbitrari.

Nell&#39;elemento `<head>` del documento HTML la pagina finale deve includere un codice simile al seguente esempio:

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

## Comportamento di Munchkin

Per impostazione predefinita, Marketo Munchkin esegue le azioni seguenti al caricamento di una pagina:

1. Verifica se il browser corrente dispone di un cookie Munchkin e, se necessario, ne crea uno.
1. Invia un evento &quot;Visita pagina Web&quot; all&#39;istanza di Marketo designata utilizzando le informazioni della pagina corrente e del browser. Questo evento registra un’attività nel record Marketo corrispondente.
1. Invia un evento &quot;Collegamento selezionato su pagina Web&quot; quando l&#39;utente seleziona un collegamento.

Utilizza le [impostazioni di configurazione](configuration.md) di Munchkin per modificare questo comportamento. Ad esempio, utilizza `cookieAnon` per controllare se Munchkin crea un cookie per tutti i lead che visitano la pagina o utilizza `clickTime` per modificare il ritardo di clic.

Per disabilitare l&#39;attività Visita, impostare `apiOnly` su true. A partire dalla versione 162 (agosto 2022), Munchkin tiene traccia dei clic su `tel` e `mailto` collegamenti oltre a `http/s` collegamenti.

## Lead noti e anonimi

Quando un lead visita per la prima volta una pagina del dominio, Marketo crea un record lead anonimo. La chiave primaria per questo record è il cookie di Munchkin (`_mkto_trk`) creato nel browser dell&#39;utente.

Marketo registra la successiva attività web da quel browser sul record anonimo. Per associare l’attività a un record Marketo noto, deve verificarsi uno dei seguenti eventi:

- Il lead deve visitare una pagina tracciata da Munchkin con un parametro `mkt_tok` nella stringa query da un collegamento e-mail di Marketo tracciato.
- Il lead deve compilare un Marketo Form.
- È necessario inviare una chiamata REST [Associa lead](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/associateLeadUsingPOST).

Quando si verifica uno di questi eventi, Marketo associa il cookie e tutte le attività web correlate al lead noto.

Marketo crea un record di attività web anonimo per ogni browser. Se un lead visita il dominio da un nuovo computer o browser, l’associazione deve ripetersi.

## Domini

Munchkin crea e tiene traccia dei cookie in base al singolo dominio. Per tenere traccia di un lead noto tra più domini, è necessario che su ciascun dominio si verifichi un evento di associazione lead.

Si supponga ad esempio di controllare `marketo.com` e `example.com`. Un lead invia un modulo il `marketo.com` e successivamente passa a `example.com`. L&#39;attività su `marketo.com` è associata al lead noto, ma l&#39;attività su `example.com` è anonima.

I lead noti persistono tra i sottodomini. Un lead noto su `www.example.com` è anche un lead noto su `info.example.com`.

Se il dominio di primo livello include due parti, ad esempio `.co.uk`, aggiungere un parametro `domainLevel` al frammento di codice Munchkin. Per ulteriori informazioni, vedere [Configurazione](configuration.md#domainlevel).

## Cookie

Il cookie Munchkin utilizza la chiave `_mkto_trk` e un valore che segue uno dei seguenti pattern:

`id:561-HYG-937&token:_mch-marketo.com-1374552656411-90718`

Oppure

`id:561-HYG-937&token:_mch-marketo.com-97bf4361ef4433921a6da262e8df45a`

I cookie di Munchkin sono specifici di ogni dominio di secondo livello, ad esempio `example.com`. La durata predefinita del cookie è di 2 anni (730 giorni).

## Beta

Per accedere al canale beta Munchkin per le pagine di destinazione, passa a [Amministratore -> Treasure Chest](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) e abilita l&#39;impostazione &quot;Munchkin Beta sulle pagine di destinazione&quot;.

Questa impostazione aggiunge snippet di codice al menu **[!UICONTROL Admin]** -> **[!UICONTROL Munchkin]**. Utilizza questi snippet per eseguire la versione beta su siti esterni.

## Rinuncia

I visitatori possono rinunciare al tracciamento di Munchkin aggiungendo il parametro `querystring` &quot;marketo_opt_out=true&quot; all&#39;URL nel browser. Quando Munchkin JavaScript rileva questa impostazione, tenta di impostare un nuovo cookie &quot;mkto_opt_out&quot; con un valore di `true`.

Munchkin elimina quindi tutti gli altri cookie di tracciamento di Marketo, non imposta nuovi cookie e non effettua richieste HTTP.
