---
title: "Mappature risposte"
feature: Webhooks
description: "Mappature di risposta per Marketo"
source-git-commit: bcc0c0c8e8209cf9fb962a85c8e7da354d95a8fe
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---


# Mappature risposte

Marketo può tradurre i dati ricevuti da un webhook da due tipi di contenuto e restituire questi valori a un campo lead: JSON e XML. Il parametro Campo Marketo utilizzerà sempre il [Nome API SOAP](../rest-api/fields.md) del campo. Ogni webhook può disporre di un numero illimitato di mappature di risposta, che vengono aggiunte e modificate facendo clic sul pulsante [!UICONTROL Edit] nel riquadro Mapping risposte del webhook:

![Response-Mapping](assets/response-mapping.png)

Le mappature di risposta vengono create tramite un accoppiamento di un &quot;Attributo di risposta&quot;, il percorso della proprietà desiderata nel documento XML o JSON e il &quot;Campo Marketo&quot;, che specifica il campo Lead contenente il valore scritto dall&#39;Attributo di risposta.

Le chiavi per le proprietà devono essere costituite da caratteri alfanumerici, trattino (-), carattere di sottolineatura (_), due punti (:) e spazi vuoti a cui accedere tramite le mappature di risposta di Marketo.

## Mappature JSON

Le proprietà JSON sono accessibili con la notazione del punto e la notazione dell’array. La notazione della matrice in Marketo non accetta stringhe come input e accetta solo numeri interi. Per recuperare dati da un documento JSON, il tipo di risposta deve essere impostato su JSON:

```json
{ "foo":"bar"}
```

Per accedere al `foo` in una mappatura di risposta, utilizza `name` della proprietà poiché si trova nel primo livello dell’oggetto JSON, `foo`. Ecco come si presenta in Marketo:

![Mappatura risposta](assets/json-resp.png)

Di seguito un esempio più complicato con un array:

```json
{
    "profileId" : 1234,
    "firstName" : "Jane",
    "lastName" : "Doe",
    "orders" : [
        {
            "orderId" : 5678,
            "orderDate" : "2015-01-01",
            "orderProductId" : "4982"
        },
        {
            "orderId" : 5678,
            "orderDate" : "2014-05-07",
            "orderProductId" : "4982"
        }
    ]
}
```

Desideriamo accedere a orderDate dal primo elemento della matrice ordini. Per accedere a questa proprietà, utilizzare quanto segue: `orders[0].orderDate`

## Mappature XML

È possibile accedere ai valori dai singoli elementi nei documenti XML. Questo utilizza una notazione del punto simile alle mappature JSON. Diamo un’occhiata a questo semplice esempio:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Per accedere alla proprietà foo in questo punto, utilizzare quanto segue: `example.foo`

È necessario fare riferimento all&#39;elemento di esempio prima di accedere a `foo`. Per accedere a una proprietà, nel mapping deve essere fatto riferimento a tutti gli elementi della gerarchia. I documenti XML con array sono un po&#39; più complicati. Utilizza l’esempio seguente:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<elementList>
    <element>
        <foo>baz</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
</elementList>
```

Il documento è costituito dall&#39;array principale `elementList`, con elementi figlio, elemento che contiene una proprietà: `foo`. Ai fini delle mappature di risposta di Marketo, l’array è indicato come `elementList.element`, in modo che gli elementi secondari di elementList siano accessibili tramite `elementList.element[i]`. Per ottenere il valore di foo dal primo elemento figlio di elementList, utilizziamo questo attributo di risposta: `elementList.element[0].foo` Questo restituisce il valore &quot;baz&quot; al campo designato. Se si tenta di accedere alle proprietà all’interno di elementi che contengono nomi di elementi sia univoci che non univoci, si verifica un comportamento non definito. Ogni elemento deve essere una singola proprietà o un array, i tipi non possono essere combinati.

## Tipi

Quando mappi gli attributi ai campi, devi assicurarti che il tipo nella risposta del webhook sia compatibile con il campo di destinazione. Ad esempio, se il valore nella risposta è una stringa e il campo selezionato è di tipo intero, il valore non viene scritto. Ulteriori informazioni [Tipi di campi](../rest-api/field-types.md).
