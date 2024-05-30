---
title: "Corrispondenza pattern"
description: "Corrispondenza pattern"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 5%

---


# Corrispondenza pattern

RTP espone una funzione di utilità per verificare se il modello corrisponde a una determinata stringa. L&#39;utilità non può essere utilizzata in modalità asincrona perché restituisce un&#39;indicazione in caso di corrispondenza o meno.

Devi diventare un cliente di personalizzazione web e disporre del [Tag RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito prima di utilizzare l’API Contesto utente.

## Utilizzo

> rtp.checkPattern(check_against, pattern);

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| check_against | Obbligatorio | Stringa | Stringa su cui confrontare il modello. Ad esempio: URL della pagina corrente, nome del prodotto. |
| pattern | Obbligatorio | Stringa | Aggiungi % per il carattere jolly. Il modello può essere:start with end withcontainsfull match |


## Esempi

Imposta la variabile personalizzata nell’indice 1 se l’URL della pagina corrente termina con &quot;productA&quot;.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Il percorso URL corrente è &quot;/products/productB&quot;. Questo esempio controlla se il percorso contiene &quot;products&quot; e imposta la variabile personalizzata.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
