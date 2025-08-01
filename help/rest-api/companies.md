---
title: Aziende
feature: REST API
description: Configurare i dati aziendali con le API di Marketo.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
source-git-commit: 981ed9b254f277d647a844803d05a1a2549cbaed
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 1%

---

# Aziende

[Riferimento endpoint società](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Le società rappresentano l&#39;organizzazione a cui appartengono i record dei lead. I lead vengono aggiunti a un&#39;azienda compilando il campo `externalCompanyId` corrispondente utilizzando [Lead sincronizzati](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) o [Endpoint importazione lead in blocco](bulk-lead-import.md). Una volta aggiunto un lead a un&#39;azienda, non è possibile eliminarlo da tale azienda (a meno che non si aggiunga il lead a un&#39;altra azienda). I lead collegati a un record società ereditano direttamente i valori da un record società come se i valori fossero presenti nel record del lead.

Le API della società sono di sola lettura per le sottoscrizioni che hanno [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) abilitato.

## Descrivere

La descrizione dell’oggetto aziendale fornisce tutte le informazioni necessarie per interagire con esso.

```
GET /rest/v1/companies/describe.json
```

```json
{
   "success":true,
   "requestId":"5847#14d44113ad7",
   "result":[
      {
         "name":"Company",
         "description":"Company object",
         "createdAt":"2015-05-11T17:11:32Z",
         "updatedAt":"2015-05-11T17:11:32Z",
         "idField":"id",
         "dedupeFields":[
            "externalCompanyId"
         ],
         "searchableFields":[
            [
               "externalCompanyId"
            ],
            [
               "id"
            ],
            [
               "company"
            ]
         ],
         "fields":[
            {
               "name":"createdAt",
               "displayName":"Created At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"externalCompanyId",
               "displayName":"External Company Id",
               "dataType":"string",
               "length":100,
               "updateable":false
            },
            {
               "name":"id",
               "displayName":"Id",
               "dataType":"integer",
               "updateable":false
            },
            {
               "name":"updatedAt",
               "displayName":"Updated At",
               "dataType":"datetime",
               "updateable":false
            },
            {
               "name":"annualRevenue",
               "displayName":"Annual Revenue",
               "dataType":"currency",
               "updateable":true
            }
            {
               "name":"company",
               "displayName":"Company Name",
               "dataType":"string",
               "length":255,
               "updateable":true
            }
         ]
      }
   ]
}
```

## Query

Il modello per [query sulle società](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) segue da vicino quello dell&#39;API lead con la restrizione aggiunta che il parametro `filterType` accetta i campi elencati nella matrice searchableFields della chiamata Describe Companies o dedupeFields.

`filterType` e `filterValues` sono parametri di query obbligatori.  `fields`, `nextPageToken` e `batchSize` sono parametri facoltativi.  I parametri funzionano come i parametri corrispondenti nelle API Lead e Opportunità. Quando si richiede un elenco di `fields`, se un particolare campo è richiesto ma non restituito, il valore è implicitamente nullo.

Se il parametro fields viene omesso, il set di campi predefinito restituito è:

- id
- dedupeFields
- updatedAt
- createdAt

```
GET /rest/v1/companies.json?filterType=id&filterValues=3433,5345
```

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "seq":0,
         "id":3433,
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "seq":1,
         "id":5345,
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
      }
   ]
}
```

## Crea e aggiorna

L&#39;endpoint [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) accetta il parametro `input` richiesto che contiene un array di oggetti aziendali. Proprio come le opportunità, esistono tre modalità per creare e aggiornare le aziende: createOnly, updateOnly e createOrUpdate.  Le modalità sono specificate nel parametro `action` della richiesta. Entrambi i parametri `dedupeBy` e `action` sono facoltativi e per impostazione predefinita sono rispettivamente le modalità dedupeFields e createOrUpdate.

```
POST /rest/v1/companies.json
```

```
Content-Type: application/json
```

```json
{
   "action":"createOrUpdate",
   "dedupeBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000",
         "company":"Google"
      },
      {
         "externalCompanyId":"29UYA31581L000000",
         "company":"Yahoo"
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
         "id":1232
      },
      {
         "seq":1,
         "status":"created",
         "id":1323
      }
   ]
}
```

### Campi

L’oggetto company contiene un set di campi. Ogni definizione di campo è composta da un insieme di attributi che descrivono il campo. Esempi di attributi sono nome visualizzato, nome API e dataType. Questi attributi sono noti collettivamente come metadati.

I seguenti endpoint consentono di eseguire query sui campi dell’oggetto aziendale. Queste API richiedono che l&#39;utente API proprietario abbia un ruolo con una o entrambe le autorizzazioni `Read-Write Schema Standard Field` o `Read-Write Schema Custom Field`.

### Campi query

La query dei campi dell’azienda è semplice. Puoi eseguire query in un singolo campo aziendale per nome API o nel set di tutti i campi aziendali.

#### Per nome

L&#39;endpoint [Ottieni campo società per nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) recupera i metadati per un singolo campo nell&#39;oggetto società. Il parametro di percorso `fieldApiName` richiesto specifica il nome API del campo. La risposta è simile all&#39;endpoint Describe Company ma contiene metadati aggiuntivi, ad esempio l&#39;attributo `isCustom`, che indica se il campo è un campo personalizzato.

```
GET /rest/v1/companies/schema/fields/industry.json
```

```json
{
    "requestId": "88f6#17e976d6ab4",
    "result": [
        {
            "displayName": "Industry",
            "name": "industry",
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
    "success": true
}
```

#### Sfogliare

L&#39;endpoint [Recupera campi società](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) recupera i metadati per tutti i campi dell&#39;oggetto società. Per impostazione predefinita, vengono restituiti al massimo 300 record. È possibile utilizzare il parametro di query `batchSize` per ridurre questo numero. Se l&#39;attributo `moreResult` è true, significa che sono disponibili altri risultati. Continua a chiamare questo endpoint fino a quando l’attributo moreResult restituisce false, il che significa che non sono disponibili risultati. I `nextPageToken` restituiti da questa API devono essere sempre riutilizzati per la successiva iterazione di questa chiamata.

```
GET /rest/v1/companies/schema/fields.json?batchSize=5
```

```json
{
    "requestId": "b50e#17e995c2d35",
    "result": [
        {
            "displayName": "Company Name",
            "name": "company",
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
            "displayName": "Site",
            "name": "site",
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
            "displayName": "Website",
            "name": "website",
            "description": null,
            "dataType": "url",
            "length": 255,
            "isHidden": false,
            "isHtmlEncodingInEmail": true,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        },
        {
            "displayName": "Main Phone",
            "name": "mainPhone",
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
            "displayName": "Annual Revenue",
            "name": "annualRevenue",
            "description": null,
            "dataType": "currency",
            "isHidden": false,
            "isHtmlEncodingInEmail": false,
            "isSensitive": false,
            "isCustom": false,
            "isApiCreated": false
        }
    ],
    "success": true,
    "nextPageToken": "L7XD3EFJ3OLFZKXKJBWYULOTRA======",
    "moreResult": true
}
```

### Elimina

I criteri di eliminazione sono specificati nell&#39;array `input`, che contiene un elenco di valori di ricerca.  Metodo di eliminazione specificato nel parametro `deleteBy`.  I valori consentiti sono: dedupeFields, idField.  Il valore predefinito è dedupeFields.

```
Content-Type: application/json
```

```
POST /rest/v1/companies/delete.json
```

```json
{
   "deleteBy":"dedupeFields",
   "input":[
      {
         "externalCompanyId":"19UYA31581L000000"
      },
      {
         "externalCompanyId":"29UYA31581L000000"
      },
      {
         "externalCompanyId":"39UYA31581L000000"
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
         "id":1234,
         "status":"deleted"
      },
      {
         "seq":1,
         "id":56456,
         "status":"deleted"
      },
      {
         "seq":2,
         "status":"skipped",
         "reasons":[
            {
               "code":"1013",
               "message":"Record not found"
            }
         ]
      }
   ]
}
```

## Timeout

- Gli endpoint aziendali hanno un timeout di 30 secondi, a meno che non sia indicato di seguito
   - Aziende di sincronizzazione: anni 60 
   - Cancella Aziende: 60s
