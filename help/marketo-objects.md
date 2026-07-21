---
title: Oggetti Marketo
feature: Email Programs
description: Guida all’utilizzo di Marketo Velocity con lead, opportunità e oggetti personalizzati, al caricamento di campi, ai primi 10 accessi all’elenco, alle relazioni con SFDC e a $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
TQID: https://experienceleague.adobe.com/PvLJb-AOk6DKaNINycpzk5ojZiL8UNcanRg3vXmsGCI
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 452
ht-degree: 0%

---

# Oggetti Marketo

L’implementazione Velocity di Marketo può utilizzare i dati provenienti dalle seguenti origini Marketo:

- Lead
- Opportunità
- Oggetti personalizzati
- App mobile
- Installazione app mobile

## Caricamento campi

Per utilizzare un campo in uno script, seleziona il campo sotto l’elenco corrispondente nell’editor dei token di script.

Se uno script fa riferimento a un campo non caricato, lo script non riesce in fase di esecuzione. Trascinare un campo dal menu dei campi nello script per caricarlo e aggiungere un riferimento in corrispondenza del cursore.

## Elenchi di oggetti personalizzati e di opportunità

Per Opportunità e Oggetti personalizzati, Marketo carica solo gli ultimi 10 oggetti aggiornati di ciascun tipo. È possibile aumentare il numero di oggetti personalizzati disponibili seguendo i passaggi descritti in questa sezione.

Marketo fornisce gli oggetti in un elenco denominato `<objectName>List`, ordinati dal record aggiornato più di recente al record aggiornato meno di recente. Per accedere al campo Importo dell’opportunità aggiornata più di recente, utilizza:

`${OpportunityList.get(0).Amount}`

Questo esempio fa riferimento all&#39;oggetto OpportunityList, utilizza il metodo get per accedere al record in corrispondenza dell&#39;indice 0 e recupera la proprietà Amount da tale record.

Quando si trascina un campo Opportunità o Oggetto personalizzato nell&#39;editor, Marketo recupera automaticamente il campo dal record all&#39;indice 0.

## Relazioni oggetto personalizzato SFDC

Per utilizzare un oggetto personalizzato di SFDC, l&#39;oggetto deve avere una sola relazione con il lead di Marketo. Gli oggetti vengono spesso collegati tramite il contatto e l’account. Sincronizza solo gli oggetti per i quali è abilitata la relazione lead/contatto.

## Attiva oggetti

Quando una campagna utilizza il trigger Aggiunto all&#39;opportunità, L&#39;opportunità viene aggiornata o Aggiunta al trigger `<Custom Object Name>`, la variabile `$TriggerObject` è disponibile per i token di script eseguiti nella campagna del trigger. Questa variabile non è supportata per il trigger `<Custom Object Name>` è aggiornato.

Questa variabile fa riferimento all’oggetto che ha attivato la campagna. Contiene gli stessi dati del record disponibili quando si accede all&#39;oggetto tramite un altro nome di variabile.

Non utilizzare un token che fa riferimento a `$TriggerObject` in una campagna batch. L’oggetto non è disponibile nelle campagne batch e l’invio dell’e-mail non riesce.

Ad esempio, se un oggetto personalizzato per un ordine di prodotti attiva una campagna, la variabile `$TriggerObject` espone l&#39;ordine a cui è stato aggiunto il lead.

L’esempio seguente mostra uno script per un’e-mail di follow-up dell’ordine:

```html
<div>
<strong>Your order information:</strong>
##pull information from the Triggering Order and format it in a list
<ul>
<li>Product Ordered: $!{TriggerObject.ProductName}</li>
<li>Product Quantity: $!{TriggerObject.Quanitity}</li>
<li>Shipping Address: $!{TriggerObject.ShippingAddress}</li>
<li>Billing Address: $!{TriggerObject.BillingAddress}</li>
<li>Order Total: $!{TriggerObject.Amount}</li>
</ul>
<p><a href="$!{TriggerObject.OrderURL}">View Your Order Online</a></p>
</div>
```

L&#39;azione di attivazione determina l&#39;oggetto. Non è necessario codice aggiuntivo per determinare quale oggetto disponibile contiene i dati locali. Utilizzare `$TriggerObject` quando è disponibile e appropriato perché identifica esplicitamente l&#39;oggetto a cui fare riferimento.

Nota: quando si utilizza `$TriggerObject`, selezionare i campi dell&#39;oggetto nel riquadro di modifica per renderli disponibili per lo script.

Nota 2: `$TriggerObject` funziona solo per i trigger &quot;aggiunti&quot; e non per quelli &quot;aggiornati&quot;.
