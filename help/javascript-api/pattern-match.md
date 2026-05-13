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
source-git-commit: 00118a89f25a23b931fac671130932bb0e0e4e4e
workflow-type: tm+mt
source-wordcount: 171
ht-degree: 5%

---

# Corrispondenza pattern

RTP espone una funzione di utilità per verificare se il modello corrisponde a una determinata stringa. L&#39;utilità non può essere utilizzata in modalità asincrona perché restituisce un&#39;indicazione in caso di corrispondenza o meno.

Prima di utilizzare l&#39;API Contesto utente, è necessario diventare un cliente di Web Personalization e disporre del tag [RTP distribuito](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) sul sito.

## Utilizzo

> rtp.checkPattern(check_against, pattern);

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
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
