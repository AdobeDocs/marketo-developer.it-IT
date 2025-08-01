---
title: Opportunità
feature: REST API
description: ' Configura le opportunità con l’API di Marketo.'
exl-id: 46451285-4125-4857-890a-575069a68288
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 0%

---

# Opportunità

[Riferimento endpoint opportunità](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities)

Marketo espone le API per la lettura, la scrittura, la creazione e l’aggiornamento dei record di opportunità. In Marketo, i record opportunità sono collegati ai record lead e contatto tramite l’oggetto Ruolo opportunità intermedio, pertanto un’opportunità può essere collegata a molti lead singoli.  Entrambi questi tipi di oggetto sono esposti tramite l’API e, come la maggior parte dei tipi di oggetto del database lead, entrambi dispongono di una chiamata Describe corrispondente, che restituisce metadati sui tipi di oggetto.

Le API dell&#39;opportunità sono di sola lettura per le sottoscrizioni che hanno [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=it) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=it) abilitate.

## Descrivere

La descrizione dei record Opportunità segue il modello standard per gli oggetti di database lead.

```
GET /rest/v1/opportunities/describe.json
```

```json
{
   "requestId":"185d6#14b51985ff0",
   "success":true,
   "result":[
      {
         "name":"opportunity",
         "displayName":"Opportunity",
         "createdAt":"2015-02-03T22:36:23Z",
         "updatedAt":"2015-02-03T22:36:24Z",
         "idField":"marketoGUID",
         "dedupeFields":[
            "externalOpportunityId"
         ],
         "searchableFields":[
            [
               "externalOpportunityId"
            ],
            [
               "marketoGUID"
            ]
         ],
         "fields":[
            {
               "name":"marketoGUID",
               "displayName":"Marketo GUID",
               "dataType":"string",
               "length":36,
               "updateable":false
            },
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"externalOpportunityId",
               "displayName":"External Opportunity Id",
               "dataType":"string",
               "length":50,
               "updateable":false
            }
         ]
      }
   ]
}
```

I campi più importanti per questo tipo di risposta sono `idField`, `dedupeFields` e `searchableFields`.  idField indica la chiave primaria per le opportunità, marketoGUID.  Si tratta di una chiave univoca generata dal sistema, che può essere utilizzata per le operazioni di lettura e aggiornamento, ma non per gli inserimenti, poiché è gestita dal sistema.  La matrice dedupeFields indica quali campi sono chiavi valide per le operazioni di inserimento; nel caso delle opportunità, questo è solo externalOpportunityId.  La matrice searchableFields fornisce il set di campi validi per l&#39;esecuzione di query, externalOpportunityId e marketoGUID.

## Query

Il modello per le [opportunità di query](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunitiesUsingGET) segue da vicino quello dell&#39;API lead con la restrizione aggiunta che il parametro `filterType` accetta i campi elencati nell&#39;array `searchableFields` o della chiamata di descrizione corrispondente, o dedupeFields.  Se si utilizzano campi opportunità personalizzati, nella matrice searchableFields verranno elencati solo i campi opportunità personalizzati di tipo String o Integer.

```
GET /rest/v1/opportunities.json?filterType=marketoGUID&filterValues=dff23271-f996-47d7-984f-f2676861b5fa&dff23271-f996-47d7-984f-f2676861b5fc,dff23271-f996-47d7-984f-f2676861b5fb
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fa ",
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "seq":1,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fc ",
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

È inoltre possibile includere i parametri di query facoltativi `fields`, per la restituzione di campi opportunità aggiuntivi, `nextPageToken`, per il paging attraverso set di dimensioni maggiori del batch, `batchSize`, che per impostazione predefinita ha un massimo di 300.  Quando si richiede un elenco di `fields`, se un particolare campo è richiesto ma non restituito, il valore è implicitamente nullo.

## Crea e aggiorna

Le opportunità seguono da vicino il pattern API dei lead, con alcune limitazioni.  I valori disponibili per `action` sono: createOnly, createOrUpdate e updateOnly.  Quando si utilizza la modalità createOnly o createOrUpdate, il campo externalOpportunityId deve essere incluso in ogni record.  Per la modalità updateOnly è possibile utilizzare marketoGUID o externalOpportunityId.  La modalità predefinita è createOrUpdate se non specificata.

Il parametro `lookupField` dell&#39;API lead non è disponibile e viene sostituito dal parametro dedupeBy, valido solo se action è updateOnly.  I valori disponibili per dedupeBy sono &quot;dedupeFields&quot; o &quot;idField&quot; specificati dalla chiamata di descrizione rispettivamente come externalOpportunityId e marketoGUID.  Se dedupeBy non è specificato, per impostazione predefinita viene utilizzata la modalità dedupeFields.  Il campo &quot;name&quot; non può essere nullo.

È possibile inviare fino a 300 record alla volta.

```
POST /rest/v1/opportunities.json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000",
         "name":"Chairs",
         "description":"Chairs",
         "amount":"1604.47",
         "source":"Inbound Sales Call/Email"
      },
      {
         "externalOpportunityId":"29UYA31581L000000",
         "name":"Big Dog Day Care-Phase12",
         "description":"Big Dog Day Care-Phase12",
         "amount":"1604.47",
         "source":"Email"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "status":"updated",
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq":1,
         "status":"created",
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb"
      }
   ]
}
```

L&#39;API risponderà con `marketoGUID` per ogni record, nonché con un campo `status`, che indica l&#39;esito positivo o negativo di ogni record e un campo `seq` utilizzato per correlare i record inviati, in base all&#39;ordine della risposta.  Il numero nel campo è l’indice del record inviato nella richiesta.

### Campi

L’oggetto company contiene un set di campi.  Ogni definizione di campo è composta da un insieme di attributi che descrivono il campo.  Esempi di attributi sono nome visualizzato, nome API e dataType.  Questi attributi sono noti collettivamente come metadati.

I seguenti endpoint consentono di eseguire query sui campi dell’oggetto aziendale. Queste API richiedono che l&#39;utente API proprietario abbia un ruolo con una o entrambe le autorizzazioni `Read-Write Schema Standard Field` o `Read-Write Schema Custom Field`.

### Campi query

La query dei campi dell’opportunità è semplice.  Puoi eseguire query in un singolo campo aziendale per nome API o nel set di tutti i campi aziendali.

#### Per nome

L&#39;endpoint [Ottieni campo opportunità per nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldByNameUsingGET) recupera i metadati per un singolo campo nell&#39;oggetto aziendale.  Il parametro di percorso `fieldApiName` richiesto specifica il nome API del campo.  La risposta è simile all&#39;endpoint Describe Opportunity ma contiene metadati aggiuntivi, ad esempio l&#39;attributo `isCustom`, che indica se il campo è un campo personalizzato.

```
GET /rest/v1/opportunities/schema/fields/externalOpportunityId.json
```

```json
{
    "requestId": "12331#17e9779cb4b",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true
}
```

#### Sfogliare

L&#39;endpoint [Get Opportunity Fields](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities/operation/getOpportunityFieldsUsingGET) recupera i metadati per tutti i campi nell&#39;oggetto società.  Per impostazione predefinita, vengono restituiti al massimo 300 record.  È possibile utilizzare il parametro di query `batchSize` per ridurre questo numero.  Se l&#39;attributo `moreResult` è true, significa che sono disponibili altri risultati.  Continua a chiamare questo endpoint fino a quando l’attributo moreResult restituisce false, il che significa che non sono disponibili risultati.  I `nextPageToken` restituiti da questa API devono essere sempre riutilizzati per la successiva iterazione di questa chiamata.

```
GET /rest/v1/opportunities/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b4a#17e995b31da",
    "result": [
        {
            "displayName": "SFDC Oppty Id",
            "name": "externalOpportunityId",
            "description": null,
            "dataType": "string",
            "length": 50,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Name",
            "name": "name",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Description",
            "name": "description",
            "description": null,
            "dataType": "string",
            "length": 2000,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Type",
            "name": "type",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Stage",
            "name": "stage",
            "description": null,
            "dataType": "string",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "E5ZONGE4SAHALYYW6FS25KB5BM======",
    "moreResult": true
}
```

#### Elimina

Puoi eliminare le opportunità tramite campi di deduplicazione o campo ID. Specificare utilizzando il parametro `deleteBy` con un valore dedupeFields o idField. Se non viene specificato, il valore predefinito è dedupeFields. Il corpo della richiesta contiene un array `input` di opportunità da eliminare. È consentito un massimo di 300 opportunità per chiamata.

```
POST /rest/v1/opportunities/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalOpportunityId":"19UYA31581L000000"
      },
      {
         "externalOpportunityId":"29UYA31581L000000"
      }
   ]
}
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "marketoGUID":"dff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      },
      {
         "seq":1,
         "marketoGUID":"cff23271-f996-47d7-984f-f2676861b5fb",
         "status":"deleted"
      }
   ]
}
```

## Timeout

- Gli endpoint di opportunità hanno un timeout di 30 secondi, a meno che non sia indicato di seguito
   - Opportunità di sincronizzazione: anni 60 
   - Elimina opportunità: anni 60
