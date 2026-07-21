---
title: Aziende
feature: REST API
description: Utilizza l’API REST per Marketo Companies per descrivere, eseguire query e sincronizzare i record aziendali, gestire i campi e la deduplicazione tramite externalCompanyId e notare la sincronizzazione CRM in sola lettura.
exl-id: 80e514a2-1c86-46a7-82bc-e4db702189b0
TQID: https://experienceleague.adobe.com/LdJYN4lx9JfcE-02zTz8ktfYXm4EdPtxMYOx9gGR0sg
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2:
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 582
ht-degree: 1%

---

# Aziende

[Riferimento endpoint società](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies)

Le aziende rappresentano le organizzazioni a cui appartengono i record dei lead. Per aggiungere un lead a una società, compila il relativo campo `externalCompanyId` utilizzando gli endpoint [Lead di sincronizzazione](https://developer.adobe.com/marketo-apis/api/mapi#tag/Leads/operation/syncLeadUsingPOST) o [Importazione lead in blocco](bulk-lead-import.md).

Non è possibile rimuovere un lead da una società a meno che non lo si aggiunga a un&#39;altra società. I lead collegati a un record società ereditano i valori da tale record come se i valori fossero presenti nel record lead.

Le API aziendali forniscono accesso in sola lettura per le sottoscrizioni che hanno [SFDC Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync.html?lang=en) o [Microsoft Dynamics Sync](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync.html?lang=en) abilitato.

## Descrivere

Descrivere l&#39;oggetto aziendale per recuperare le informazioni necessarie all&#39;interazione con i record aziendali.

```http
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

Il modello per [query sulle aziende](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompaniesUsingGET) segue da vicino l&#39;API Lead. Tuttavia, il parametro `filterType` accetta solo i campi elencati nella matrice searchableFields della risposta Descrivi società o dedupeFields.

I parametri di query sono:

- `filterType` e `filterValues`: parametri richiesti.
- `fields`, `nextPageToken` e `batchSize`: parametri opzionali che funzionano come i parametri corrispondenti nelle API Lead e Opportunità.

Quando si richiede un elenco di `fields`, un campo richiesto non restituito ha un valore implicito nullo.

Se ometti il parametro fields, la risposta restituisce questi campi per impostazione predefinita:

- id
- dedupeFields
- updatedAt
- createdAt

```http
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

L&#39;endpoint [Sync Companies](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/syncCompaniesUsingPOST) accetta un parametro `input` obbligatorio contenente un array di oggetti aziendali.

Come per le opportunità, l&#39;endpoint supporta tre modalità di creazione e aggiornamento: createOnly, updateOnly e createOrUpdate. Specifica la modalità nel parametro `action` della richiesta.

I parametri `dedupeBy` e `action` sono facoltativi. I valori predefiniti sono dedupeFields e createOrUpdate, rispettivamente.

```http
POST /rest/v1/companies.json
```

```text
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

L’oggetto company contiene campi definiti da attributi quali nome visualizzato, nome API e dataType. Insieme, questi attributi sono denominati metadati.

I seguenti campi di query degli endpoint sull’oggetto aziendale. L&#39;utente API deve disporre di un ruolo con l&#39;autorizzazione `Read-Write Schema Standard Field`, l&#39;autorizzazione `Read-Write Schema Custom Field` o entrambe.

### Campi query

Esegui la query di un campo società per nome API o recupera tutti i campi società.

#### Per nome

L&#39;endpoint [Ottieni campo società per nome](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldByNameUsingGET) recupera i metadati per un campo nell&#39;oggetto società. Il parametro di percorso `fieldApiName` richiesto specifica il nome API del campo.

La risposta è simile alla risposta Descrivi azienda, ma include metadati aggiuntivi. Ad esempio, l&#39;attributo `isCustom` indica se il campo è personalizzato.

```http
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

#### Sfoglia

L&#39;endpoint [Recupera campi società](https://developer.adobe.com/marketo-apis/api/mapi#tag/Companies/operation/getCompanyFieldsUsingGET) recupera i metadati per tutti i campi dell&#39;oggetto società. Per impostazione predefinita, restituisce un massimo di 300 record. Utilizzare il parametro di query `batchSize` per ridurre questo numero.

Se l&#39;attributo `moreResult` è true, sono disponibili altri risultati. Continuare a chiamare l&#39;endpoint con `nextPageToken` restituito fino a quando `moreResult` non è false.

```http
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

Specificare i criteri di eliminazione come elenco di valori di ricerca nell&#39;array `input`. Specificare il metodo di eliminazione nel parametro `deleteBy`.

I valori consentiti sono dedupeFields e idField. Il valore predefinito è dedupeFields.

```text
Content-Type: application/json
```

```http
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

- Se non diversamente specificato, gli endpoint aziendali hanno un timeout di 30 secondi.
- Per le società di sincronizzazione è previsto un timeout di 60 secondi.
- Il timeout di Delete Companies è di 60 secondi.
