---
title: Corrispondenza pattern
description: Utilizzare l'utilità RTP rtp.checkPattern per verificare i pattern di stringhe con caratteri jolly percentuali. Vedere limitazioni di sincronizzazione, esempi di utilizzo e URL e la configurazione dei tag RTP richiesta.
feature: Javascript
exl-id: 4ebd13e3-375b-449b-850f-3b18f570ca75
TQID: https://experienceleague.adobe.com/-HopUg6-2EchL9kJrPDbz62mRlrqYaXYdufILjkvP1Y
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: ed6be6bb-75bb-4ea9-9a42-3bcaa65e1bcc
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 188
ht-degree: 4%

---

# Corrispondenza pattern

RTP fornisce una funzione di utilità che controlla se un modello corrisponde a una stringa. L&#39;utility restituisce un risultato di corrispondenza in modo sincrono e non può essere utilizzata in modo asincrono.

Prima di utilizzare l&#39;API Contesto utente, è necessario essere un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/it/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

## Utilizzo

> rtp.checkPattern(check_against, pattern);

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| check_against | Obbligatorio | Stringa | Stringa in base alla quale confrontare il pattern, ad esempio l’URL della pagina corrente o un nome di prodotto. |
| pattern | Obbligatorio | Stringa | Pattern da associare. Aggiungere `%` come carattere jolly per far corrispondere l&#39;inizio, la fine o il contenuto di una stringa. Ometti `%` per una corrispondenza completa. |

## Esempi

Questo esempio imposta una variabile personalizzata in corrispondenza dell’indice 1 quando l’URL della pagina corrente termina con &quot;productA&quot;.

```javascript
if (rtp.checkPattern(window.location.href, '%productA')) {
    rtp('set', 'custom1', 'productA');
}
```

Nell’esempio seguente, il percorso URL corrente è &quot;/products/productB&quot;. L’esempio controlla se il percorso contiene &quot;prodotti&quot; e quindi imposta una variabile personalizzata.

```javascript
var currentURLPath = '/products/productB';
if (rtp.checkPattern(currentURLPath, '%products%')) {
    rtp('set', 'custom1', 'products');
}
```
