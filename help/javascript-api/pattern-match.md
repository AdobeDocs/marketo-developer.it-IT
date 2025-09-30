---
title: Corrispondenza pattern
description: Utilizzare l'utilità RTP rtp.checkPattern per verificare i pattern di stringhe con caratteri jolly percentuali. Vedere limitazioni di sincronizzazione, esempi di utilizzo e URL e la configurazione dei tag RTP richiesta.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 5%

---

# Corrispondenza pattern

RTP espone una funzione di utilità per verificare se il modello corrisponde a una determinata stringa. L&#39;utilità non può essere utilizzata in modalità asincrona perché restituisce un&#39;indicazione in caso di corrispondenza o meno.

Prima di utilizzare l&#39;API Contesto utente, è necessario diventare un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

## Utilizzo

> rtp.checkPattern(check_against, pattern);

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| check_against | Obbligatorio | Stringa | Stringa su cui confrontare il modello. Ad esempio: URL della pagina corrente, nome del prodotto. |
| pattern | Obbligatorio | Stringa | Aggiungi % per il carattere jolly. Il modello può essere :start con contenitore con corrispondenza completa |

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
