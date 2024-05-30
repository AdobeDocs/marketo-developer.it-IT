---
title: "Aziende"
feature: REST API
description: "Configurare i dati aziendali con le API di Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---


# Aziende

[Riferimento endpoint società](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies)

Le società rappresentano l&#39;organizzazione a cui appartengono i record dei lead. I lead vengono aggiunti a un’azienda compilando i corrispondenti `externalCompanyId` campo utilizzando [Sincronizza lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/syncLeadUsingPOST) o [Importazione lead in blocco](bulk-lead-import.md) endpoint. Una volta aggiunto un lead a un&#39;azienda, non è possibile eliminarlo da tale azienda (a meno che non si aggiunga il lead a un&#39;altra azienda). I lead collegati a un record società ereditano direttamente i valori da un record società come se i valori fossero presenti nel record del lead.

Le API aziendali sono accesso in sola lettura per gli abbonamenti che hanno [Sincronizzazione SFDC](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) o [Sincronizzazione Microsoft Dynamics](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) sono attivati.

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

Il pattern per [query sulle aziende](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompaniesUsingGET) segue da vicino quella dell’API lead con la restrizione aggiunta che il `filterType` Il parametro accetta i campi elencati nella matrice searchableFields della chiamata Describe Companies o dedupeFields.

`filterType` e `filterValues` sono parametri di query obbligatori.  `fields`, `nextPageToken`, e `batchSize` sono parametri facoltativi.  I parametri funzionano come i parametri corrispondenti nelle API Lead e Opportunità. Quando si richiede un elenco di `fields`, se un particolare campo viene richiesto ma non restituito, il valore deve essere nullo.

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

Il [Sincronizza società](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/syncCompaniesUsingPOST) l&#39;endpoint accetta il valore `input` parametro che contiene un array di oggetti aziendali. Proprio come le opportunità, esistono tre modalità per creare e aggiornare le aziende: createOnly, updateOnly e createOrUpdate.  Le modalità sono specificate in `action` parametro della richiesta. Entrambe `dedupeBy` e `action` I parametri sono facoltativi e predefiniti rispettivamente per le modalità dedupeFields e createOrUpdate.

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

I seguenti endpoint consentono di eseguire query sui campi dell’oggetto aziendale. Queste API richiedono che l’utente API proprietario abbia un ruolo con una o entrambe le `Read-Write Schema Standard Field` o `Read-Write Schema Custom Field` autorizzazioni.

### Campi query

La query dei campi dell’azienda è semplice. Puoi eseguire query in un singolo campo aziendale per nome API o nel set di tutti i campi aziendali.

#### Per nome

Il [Ottieni campo società per nome](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldByNameUsingGET) l’endpoint recupera i metadati per un singolo campo sull’oggetto aziendale. Il valore richiesto `fieldApiName` Il parametro path specifica il nome API del campo. La risposta è simile all’endpoint &quot;Descrivi azienda&quot;, ma contiene metadati aggiuntivi, ad esempio `isCustom` attributo che indica se il campo è un campo personalizzato.

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

Il [Recupera campi società](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies/operation/getCompanyFieldsUsingGET) l’endpoint recupera i metadati per tutti i campi sull’oggetto aziendale. Per impostazione predefinita, vengono restituiti al massimo 300 record. È possibile utilizzare `batchSize` parametro di query per ridurre questo numero. Se il `moreResult` è true, il che significa che sono disponibili più risultati. Continua a chiamare questo endpoint fino a quando l’attributo moreResult restituisce false, il che significa che non sono disponibili risultati. Il `nextPageToken` restituiti da questa API devono sempre essere riutilizzati per la successiva iterazione di questa chiamata.

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

I criteri di eliminazione sono specificati nella `input` , che contiene un elenco di valori di ricerca.  Il metodo di eliminazione è specificato nel `deleteBy` parametro.  I valori consentiti sono: dedupeFields, idField.  Il valore predefinito è dedupeFields.

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
