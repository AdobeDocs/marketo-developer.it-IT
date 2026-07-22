---
title: Mappature risposte
feature: Webhooks
description: Mappature di risposta dei webhook Marketo per JSON e XML, mappatura degli attributi ai campi lead con nomi API SOAP, notazione in punti e array e compatibilità dei tipi.
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
TQID: https://experienceleague.adobe.com/-OGDeKLPS1KmWGIKj6BGq5DGXoCSj5ip-dVr7-kKDro
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 373
ht-degree: 0%

---

# Mappature risposte

Marketo può tradurre i dati del webhook da JSON o XML e scrivere i valori nei campi lead. Il parametro Campo Marketo utilizza sempre il nome API [SOAP](../rest-api/fields.md) del campo.

Ogni webhook può avere un numero illimitato di mappature di risposta. Per aggiungere o modificare le mappature, selezionare [!UICONTROL Edit] nel riquadro Mapping risposte del webhook:

![Mappatura risposte](assets/response-mapping.png)

Una mappatura di risposta associa questi valori:

- &quot;Response Attribute&quot;: percorso della proprietà desiderata nel documento XML o JSON.
- &quot;Campo Marketo&quot;: campo lead in cui Marketo scrive il valore dell’attributo di risposta.

Per accedere a una proprietà tramite le mappature di risposta di Marketo, la relativa chiave deve contenere solo caratteri alfanumerici, trattino (-), carattere di sottolineatura (_), due punti (:) e spazi vuoti.

## Mappature JSON

Accedi alle proprietà JSON con notazione del punto e notazione dell’array. La notazione dell’array Marketo accetta solo numeri interi, non stringhe.

Per recuperare dati da un documento JSON, imposta il tipo di risposta su JSON:

```json
{ "foo":"bar"}
```

La proprietà `foo` si trova al primo livello dell&#39;oggetto JSON. Utilizza la proprietà `name`, `foo`, nel mapping di risposta:

![Mappatura risposta](assets/json-resp.png)

L’esempio seguente contiene un array:

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

Per accedere a orderDate dal primo elemento della matrice ordini, utilizzare `orders[0].orderDate`.

## Mappature XML

Accedi ai valori dai singoli elementi XML utilizzando la notazione del punto, in modo simile ai mapping JSON. Prendi in considerazione questo esempio:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Per accedere alla proprietà foo, utilizzare `example.foo`.

Fare riferimento all&#39;elemento di esempio prima di accedere a `foo`. Una mappatura deve fare riferimento a ogni elemento nella gerarchia delle proprietà.

Per un documento XML con un array, considerare questo esempio:

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

L&#39;array principale è `elementList`. Ogni elemento figlio contiene la proprietà `foo`. I mapping di risposta di Marketo fanno riferimento all&#39;array come `elementList.element` e accedono ai relativi elementi figlio tramite `elementList.element[i]`.

Per ottenere il valore di foo dal primo elemento figlio di elementList, utilizzare l&#39;attributo di risposta `elementList.element[0].foo`. Questa mappatura restituisce il valore &quot;baz&quot; al campo designato.

L&#39;accesso alle proprietà all&#39;interno di elementi che contengono sia nomi di elementi univoci che non univoci genera un comportamento non definito. Ogni elemento deve essere una singola proprietà o un array. Non mescolare i tipi.

## Tipi

Quando mappi gli attributi ai campi, assicurati che il tipo di risposta del webhook sia compatibile con il campo di destinazione. Ad esempio, Marketo non scrive un valore di risposta stringa in un campo di tipo integer. Per ulteriori informazioni, vedere [Tipi di campo](../rest-api/field-types.md).
