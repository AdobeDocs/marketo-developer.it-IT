---
title: Oggetti Marketo
feature: Email Programs
description: Guida all’utilizzo di Marketo Velocity con lead, opportunità e oggetti personalizzati, al caricamento di campi, ai primi 10 accessi all’elenco, alle relazioni con SFDC e a $TriggerObject.
exl-id: 88c63d72-7aa5-4550-9e1a-887a479872e1
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Oggetti Marketo

L’implementazione Velocity di Marketo può funzionare su dati provenienti da diverse origini all’interno di Marketo: lead, opportunità, oggetti personalizzati, app mobile, installazione di app mobili.

## Caricamento campi

Per caricare un campo da utilizzare in uno script, tale campo deve essere selezionato nell’elenco corrispondente nell’editor dei token di script.

Se non carichi un campo a cui viene fatto riferimento all’interno dello script, l’esecuzione dello script non riesce in fase di esecuzione. Puoi trascinare i campi dal menu dei campi nello script. In questo modo è possibile caricarli e aggiungere un riferimento al campo in corrispondenza del cursore.

## Elenchi di oggetti personalizzati e di opportunità

Durante il recupero da Opportunità o Oggetti personalizzati, vengono caricati solo i 10 oggetti aggiornati più di recente di un tipo. Il numero di oggetti personalizzati disponibili può essere aumentato seguendo i passaggi qui descritti. Questi vengono forniti come elenco, con il nome di `<objectName>List` e sono ordinati dal record aggiornato più di recente a quello più recente. Pertanto, per accedere al campo Importo dall’opportunità aggiornata più di recente, utilizza quanto segue:

`${OpportunityList.get(0).Amount}`

In questo esempio si fa riferimento all&#39;oggetto OpportunityList, si utilizza il metodo get per accedere al record indicizzato in corrispondenza di 0 e quindi si recupera la proprietà Amount dall&#39;oggetto restituito. Se trascini un campo da un’opportunità o da un oggetto personalizzato nell’editor, questo recupererà automaticamente il campo dal record indicizzato su 0.

## Relazioni oggetto personalizzato SFDC

Per poter essere utilizzato, un oggetto personalizzato di SFDC deve avere una sola relazione con il lead di Marketo. Gli oggetti sono spesso collegati sia tramite il contatto che tramite l’account, pertanto è importante sincronizzare solo gli oggetti con Marketo con la relazione lead/contatto abilitata.

## Attiva oggetti

Quando una campagna viene attivata tramite l&#39;opzione Aggiunta all&#39;opportunità, l&#39;opportunità viene aggiornata o aggiunta a `<Custom Object Name>` trigger, diventa disponibile una variabile speciale nei token di script eseguiti nel contesto della campagna del trigger: `$TriggerObject ` (non supportato per `<Custom Object Name>` è il trigger aggiornato).  Se un token che utilizza un riferimento `$TriggerObject` viene utilizzato in una campagna batch, l&#39;invio dell&#39;e-mail non riuscirà, poiché questo oggetto non è disponibile nelle campagne batch di alcun tipo.  Questo è un riferimento all&#39;oggetto che ha attivato la campagna. L’oggetto contiene tutti i dati del record a cui si accede tramite un nome di variabile diverso.

Ad esempio, se una campagna è stata attivata tramite un oggetto personalizzato per un ordine di prodotto, l&#39;ordine a cui è stato aggiunto il lead viene esposto nella variabile `$TriggerObject`.

Ecco uno script di esempio per un’e-mail di follow-up dell’ordine:

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

Il vantaggio dell&#39;utilizzo della variabile `$TriggerObject` è che non è necessario dedicare codice per determinare da quali oggetti disponibili si desidera estrarre i dati locali.  L’oggetto è determinato dall’azione di attivazione. Questo è il modo più esplicito per scegliere un oggetto a cui fare riferimento e deve essere utilizzato quando disponibile e appropriato.

Nota: quando si utilizza `$TriggerObject`, i campi devono essere controllati nel riquadro di modifica affinché l&#39;oggetto sia disponibile per lo script.

Nota 2: `$TriggerObject` funziona solo per i trigger &quot;aggiunti&quot; e non per quelli &quot;aggiornati&quot;.
