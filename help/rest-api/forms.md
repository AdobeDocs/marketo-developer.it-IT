---
title: Moduli
feature: REST API, Forms
description: Guida REST API di Marketo Forms per creare e gestire moduli, recuperarli per ID o nome, sfogliarli con filtri di stato e gestire campi, set di campi e regole.
exl-id: 2e5dfa70-3163-4ab4-b269-3112417714c3
TQID: https://experienceleague.adobe.com/56tc1a14d8okxweS7TK7SzfGB8G03WAI2KBlFKQbSdM
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: a7170d27-32ab-462b-a333-269abc654483id: b0bb9048-d951-48d8-8232-45cf248a7e27id: d65b4a73-87a3-4d56-b638-74e74d9939ceid: e64968b2-4ee5-47f9-8cae-0588f184b9eb
subfeature_v2: id: d0251300-e25f-466f-9856-7e11ce8fa7aa
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 1494
ht-degree: 2%

---

# Moduli

[Riferimento endpoint Forms](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms)

[Riferimento endpoint campi modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields)

Utilizza gli endpoint Forms per gestire i moduli da sistemi remoti. Un modulo può includere diversi tipi di oggetto:

- Moduli
- Campi
- Set di campi
- Regole di visibilità
- Regole pagina di follow-up

## Query

Forms supporta i metodi di recupero risorse standard: [per id](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET) e per [navigazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/browseForms2UsingGET). Una risposta di un modulo contiene tutte le proprietà del modulo ad eccezione dell’elenco dei campi.

### Per ID

Passa un modulo `id` come parametro di percorso a [Ottieni modulo per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByIdUsingGET). L’endpoint restituisce il record modulo corrispondente.

```http
GET /rest/asset/v1/form/{id}.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Per nome

Passa un modulo `name` a [Ottieni modulo per nome](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getLpFormByNameUsingGET). L’endpoint restituisce il record modulo corrispondente.

```http
GET /rest/asset/v1/form/byName.json?name=newForm
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

### Sfoglia

[Ottieni Forms](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/browseForms2UsingGET) segue il pattern di navigazione standard di Asset API. Supporta i seguenti filtri opzionali:

- `status`: Filtri per `approved`, `approved with draft` o `draft`.
- `maxReturn`: limita il numero di record restituiti.
- `offset`: Pagine nel set di risultati.

```http
GET /rest/asset/v1/forms.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "645d#154e3d499ac",
    "result": [
        {
            "id": 227,
            "name": "aKAUVDfbsX",
            "description": "",
            "createdAt": "2016-05-18T20:36:20Z+0000",
            "updatedAt": "2016-05-18T20:36:20Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO227B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        },
        {
            "id": 695,
            "name": "AoMXgfFbma",
            "description": "",
            "createdAt": "2016-05-19T18:50:40Z+0000",
            "updatedAt": "2016-05-19T18:50:40Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO695B2",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 565,
                "folderName": "WfUvYmlcyT"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

### Elenco campi

Recupera l’elenco dei campi separatamente per ciascun modulo trasmettendo l’ID del modulo.

```http
GET /rest/asset/v1/form/{id}/fields.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2165#154eee00d01",
    "result": [
        {
            "id": "FirstName",
            "label": "First Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "LastName",
            "label": "Last Name:",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 1,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Email",
            "label": "Email Address:",
            "dataType": "email",
            "validationMessage": "Must be valid email. <span class='mktoErrorDetail'>example@yourdomain.com</span>",
            "rowNumber": 2,
            "columnNumber": 0,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        },
        {
            "id": "Profiling",
            "dataType": "profiling",
            "rowNumber": 3,
            "columnNumber": 0
        }
    ]
}
```

Prima di aggiornare o eliminare i campi o di modificarne il comportamento, recuperare l&#39;elenco dei campi del modulo. Utilizza l’ID campo restituito nelle richieste successive.

### Tipi di campi

| Tipo di interfaccia utente | Nome API |
| --- | --- |
| Caselle di controllo | casella di controllo |
| Pulsante di opzione | radio |
| Area di testo | textarea |
| Elenco a discesa | elenco a discesa |
| Stringa | stringa |
| E-mail | e-mail |
| Data | data |
| Numero | numero |
| Doppio | doppio |
| Telefono | telefono |
| URL | url |
| Valuta | valuta |
| Casella di controllo | casella di controllo_singola |
| Cursore | intervallo |

### Dipendenze

Passa un modulo `id` come parametro di percorso a [Ottieni modulo utilizzato da](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/getFormUsedByUsingGET). L’endpoint restituisce risorse che dipendono dal modulo.

I seguenti tipi di risorse possono utilizzare i moduli:

- Pagine di destinazione
- Elenchi avanzati
- Campagne avanzate
- Rapporti
- Programmi e-mail

```http
GET /rest/asset/v1/form/{id}/usedBy.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "fdf4#17285b25038",
    "warnings": [],
    "result": [
        {
            "id": 1038,
            "name": "LP Redirect Rules Program.LP Test 01",
            "type": "Landing Page",
            "status": "approved",
            "updatedAt": "2020-02-23T01:31:21Z+0000"
        }
    ]
}
```

## Crea e aggiorna

Per [creare un modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/createLpFormsUsingPOST), fornire due campi obbligatori:

- Cartella padre del modulo.
- Nome del modulo.

Tutti gli altri parametri sono facoltativi e hanno valori predefiniti. Un nuovo modulo include tre campi predefiniti: Nome, Cognome e E-mail.

```http
POST /rest/asset/v1/forms.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=newForm&description=test&folder={"type": "Folder","id": 293}&language=French
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "948f#154e3bad8e3",
    "result": [
        {
            "id": 736,
            "name": "newForm",
            "description": "test",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:05:54Z+0000",
            "url": "https://app-devlocal1.marketo.com/#FO736B2",
            "status": "draft",
            "theme": "simple",
            "language": "French",
            "locale": "fr_FR",
            "progressiveProfiling": false,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Envoyer",
            "waitingLabel": "Veuillez patienter"
        }
    ]
}
```

Per [aggiornare un modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/updateFormsUsingPOST), passarne l&#39;ID. Durante la creazione o l&#39;aggiornamento, è possibile impostare i parametri di stile di base che controllano la modalità di visualizzazione del modulo.

```http
POST /rest/asset/v1/form/736.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
name=updated name&description=This is a test for updateapi&language=English&progressiveProfiling=true&locale=en_US
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154e3cf6efe",
    "result": [
        {
            "id": 736,
            "name": "updated name",
            "description": "This is a test for update api",
            "createdAt": "2016-05-24T17:05:54Z+0000",
            "updatedAt": "2016-05-24T17:28:23Z+0000",
            "status": "draft",
            "theme": "simple",
            "language": "English",
            "locale": "en_US",
            "progressiveProfiling": true,
            "labelPosition": "left",
            "fontFamily": "Helvetica",
            "fontSize": "13px",
            "folder": {
                "type": "Folder",
                "value": 293,
                "folderName": "yyLNLHzgOM"
            },
            "knownVisitor": {
                "type": "form",
                "template": null
            },
            "thankYouList": [
                {
                    "followupType": "none",
                    "followupValue": null,
                    "default": true
                }
            ],
            "buttonLocation": 120,
            "buttonLabel": "Submit",
            "waitingLabel": "Please Wait"
        }
    ]
}
```

Gli endpoint del modulo di creazione e aggiornamento non modificano il comportamento della pagina di ringraziamento o del visitatore noto. Utilizza gli endpoint corrispondenti per gestire tali comportamenti.

## Metadati campo

Prima di aggiungere o modificare i campi modulo, recupera i campi validi per l’istanza di destinazione. Le operazioni sui campi utilizzano la proprietà `id` restituita per ogni campo.

Per i campi lead, utilizza l&#39;endpoint [Ottieni campi modulo disponibili](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/getAllFieldsUsingGET). La risposta include il tipo di dati di ogni campo e i metadati predefiniti applicati quando il campo viene aggiunto a un modulo.

```http
GET /rest/asset/v1/form/fields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "176ca#167a9808f4c",
    "warnings": [],
    "result": [
        {
            "id": "AnnualRevenue",
            "isRequired": false,
            "dataType": "currency"
        },
        {
            "id": "City",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Company",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Country",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Description",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 32000,
            "visibleRows": 2
        },
        {
            "id": "Email",
            "isRequired": false,
            "dataType": "email"
        },
        {
            "id": "Fax",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "FirstName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Industry",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LastName",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "LeadSource",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "MobilePhone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "NumberOfEmployees",
            "isRequired": false,
            "dataType": "int"
        },
        {
            "id": "Phone",
            "isRequired": false,
            "dataType": "phone"
        },
        {
            "id": "PostalCode",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Rating",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "Salutation",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "Mr.,Ms.,Mrs.,Dr.,Prof."
        },
        {
            "id": "State",
            "isRequired": false,
            "dataType": "picklist",
            "picklistValues": "AK::AK,AL::AL,AR::AR,AZ::AZ,CA::CA,CO::CO,CT::CT,DE::DE,FL::FL,GA::GA,HI::HI,IA::IA,ID::ID,IL::IL,IN::IN,KS::KS,KY::KY,LA::LA,MA::MA,MD::MD,ME::ME,MI::MI,MN::MN,MO::MO,MS::MS,MT::MT,NC::NC,ND::ND,NE::NE,NH::NH,NJ::NJ,NM::NM,NV::NV,NY::NY,OH::OH,OK::OK,OR::OR,PA::PA,RI::RI,SC::SC,SD::SD,TN::TN,TX::TX,UT::UT,VA::VA,VT::VT,WA::WA,WI::WI,WV::WV,WY::WY"
        },
        {
            "id": "Street",
            "isRequired": false,
            "dataType": "textarea",
            "maxLength": 2000,
            "visibleRows": 2
        },
        {
            "id": "Title",
            "isRequired": false,
            "dataType": "picklist"
        }
    ]
}
```

Per i campi personalizzati dei membri del programma, chiamare l&#39;endpoint [Ottieni campi membri modulo disponibili](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET). La risposta include i tipi di dati del campo personalizzato Membro del programma e i metadati predefiniti.

Per utilizzare questi campi, il modulo deve trovarsi in un programma e non in Design Studio. Anche una pagina di destinazione che contiene un modulo con questi campi deve trovarsi all’interno di un programma. Non può essere in Design Studio né clonato in esso.

```http
GET /rest/asset/v1/form/programMemberFields.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "109c6#16fa0b9c51a",
    "warnings": [],
    "result": [
        {
            "id": "pMCFCustomField01",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "pMCFCustomField02",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        },
        {
            "id": "myPMCF",
            "isRequired": false,
            "dataType": "string",
            "maxLength": 255
        }
    ]
}
```

### Modifica campo

Ogni modulo dispone di un elenco modificabile di campi visualizzati all’utente al momento del caricamento. Utilizza l’endpoint corrispondente per aggiungere, aggiornare o eliminare un campo alla volta.

Per [aggiungere un campo](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/addFieldToAFormUsingPOST), fornire l&#39;ID del modulo padre e il campo `fieldId`. Tutte le altre proprietà sono vuote o utilizzano valori predefiniti basati sul tipo di dati e sui metadati del campo.

Invia i dati come POST con `application/x-www-form-urlencoded`, non come JSON.

```http
POST /rest/asset/v1/form/{id}/fields.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
fieldId=NumberOfEmployees&maxLength=125&defaultValue=this is default&required=true&fieldWidth=100&validationMessage=hey, you there?&label=employee count&hintText=Hint me&minValue=10
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1826e#154f41b214c",
    "result": [
        {
            "id": "NumberOfEmployees",
            "label": "employee count",
            "fieldWidth": 100,
            "dataType": "number",
            "defaultValue": "this is default",
            "validationMessage": "hey, you there?",
            "rowNumber": 5,
            "columnNumber": 0,
            "required": true,
            "formPrefill": true,
            "fieldMetaData": {
                "minValue": 10,
                "maxValue": null
            },
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "hintText": "Hint me"
        }
    ]
}
```

Un aggiornamento può modificare le stesse proprietà utilizzate quando si aggiunge un campo. Richiede anche l&#39;ID modulo e `fieldId`, ma l&#39;endpoint di aggiornamento passa `fieldId` come parametro di percorso anziché come parametro di query.

```http
POST /rest/asset/v1/form/{id}/field/LastName.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
label=enter the last name here
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5634#15508303abb",
    "result": [
        {
            "id": "LastName",
            "label": "enter the last name here",
            "dataType": "text",
            "validationMessage": "This field is required.",
            "rowNumber": 0,
            "columnNumber": 0,
            "maxLength": 255,
            "required": false,
            "formPrefill": true,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            }
        }
    ]
}
```

L&#39;esempio precedente aggiorna `LastName`, che è un semplice campo stringa. Altri campi modulo contengono metadati più complessi. `Salutation` è ad esempio un campo `select` con un elenco di elementi e un valore predefinito.

Quando si aggiunge o si aggiorna un campo selezionato, impostare il valore `isDefault` di una scelta su `true`. In caso contrario, la prima scelta non ha alcun valore ed è etichettata `Select...`.

![Saluto](assets/form-field-salutation.png)

Per aggiornare gli elementi dell&#39;elenco, formattare il parametro `values` come illustrato nell&#39;esempio seguente:

```http
POST /rest/asset/v1/form/{id}/field/Salutation.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```sql
values=[{"label":"Select...","value":"","isDefault":true,"selected":true}, {"label":"MR","value":"MR"}, {"label":"MS","value":"MS"}, {"label":"MRS","value":"MRS"}, {"label":"DR","value":"DR"}, {"label":"PROF","value":"PROF"}]
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "71fd#1588d9d1b0c",
  "result": [
    {
      "id": "Salutation",
      "label": "Salutation:",
      "dataType": "select",
      "validationMessage": "This field is required.",
      "rowNumber": 3,
      "columnNumber": 0,
      "required": false,
      "formPrefill": true,
      "fieldMetaData": {
        "multiSelect": false,
        "values": [
          {
            "label": "Select...",
            "value": "",
            "isDefault": true,
            "selected": true
          },
          {
            "label": "MR",
            "value": "MR"
          },
          {
            "label": "MS",
            "value": "MS"
          },
          {
            "label": "MRS",
            "value": "MRS"
          },
          {
            "label": "DR",
            "value": "DR"
          },
          {
            "label": "PROF",
            "value": "PROF"
          }
        ],
        "visibleLines": 1
      },
      "visibilityRules": {
        "ruleType": "alwaysShow"
      }
    }
  ]
}
```

Utilizza la risposta Aggiungi campo a modulo per determinare come formattare un campo modulo complesso.

### Ridisposizione del campo

Utilizza l&#39;endpoint [Change Form Field Positions](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/updateFieldPositionsUsingPOST) per ridisporre tutti i campi modulo come un&#39;unica unità. L&#39;endpoint richiede `positions`, un array JSON di oggetti con tre membri:

- `columnNumber`
- `rowNumber`
- `fieldName`, che fa riferimento all&#39;ID campo

I campi modulo utilizzano una disposizione simile a una tabella con un massimo di tre colonne e 10 righe. Gli indici di riga e colonna iniziano da 0, pertanto sia la prima riga che la prima colonna utilizzano 0. Ogni campo deve occupare una posizione univoca.

Se il campo di destinazione è un set di campi, anche il record in `positions` deve contenere `fieldList`. Questo parametro è un array di oggetti con gli stessi membri `columnNumber`, `rowNumber` e `fieldName`.

L’elenco principale tratta il set di campi come un unico campo. Le posizioni in `fieldList` determinano la disposizione dei campi figlio.

```http
POST /rest/asset/v1/form/{id}/reArrange.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"FirstName"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"}, {"columnNumber":0,"rowNumber":2, "fieldName":"Email"}]
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bb18#15508ef9c04",
    "result": [
        {
            "id": 764
        }
    ]
}
```

### Rich Text

Utilizza un [endpoint separato](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/addRichTextFieldUsingPOST) per aggiungere campi Rich Text. Trasmettere il contenuto come HTML in una richiesta `multipart/form-data`. Il HTML non deve contenere script, metatag o tag di collegamento.

```http
POST /rest/asset/v1/form/{id}/richText.json
```

```html
Content-Type: multipart/form-data; boundary=---------------------------9051914041544843365972754266
-----------------------------9051914041544843365972754266
Content-Disposition: form-data; name="text"
Content-Type: text/html
<div>Fancy Rich Text Component</div>
-----------------------------9051914041544843365972754266--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "82c8#154f423bf5c",
    "result": [
        {
            "id": "SHRtbFRleHRfMjAxNi0wNS0yN1QxNDozNDoyNC4xMTVa",
            "labelWidth": 260,
            "dataType": "htmltext",
            "rowNumber": 8,
            "columnNumber": 0,
            "visibilityRules": {
                "ruleType": "alwaysShow"
            },
            "text": "<div>Fancy Rich Text Component</div>"
        }
    ]
}
```

### Set di campi

Un set di campi è un gruppo facoltativo di campi. L’elenco dei campi di livello superiore tratta un set di campi come un unico campo per le regole di posizionamento e visibilità. Ad esempio, selezionando sì per un campo Requisiti di conformità è possibile visualizzare un set di campi contenente campi di conformità HIPAA e PCI.

Un campo deve essere univoco all’interno del modulo. Lo stesso campo non può essere visualizzato sia nell&#39;elenco dei campi padre del modulo che in un set di campi figlio.

Aggiungi un set di campi con [Aggiungi set di campi all&#39;endpoint Form](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/addFieldSetUsingPOST). Il set di campi viene quindi visualizzato nella risposta [Ottieni campi per modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). Per aggiungere campi al set di campi, utilizzare [Aggiorna posizioni campo](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/updateFieldPositionsUsingPOST) per spostarli nel relativo `fieldList`.

Per questi endpoint, invia i dati come POST con `application/x-www-form-urlencoded`, non come JSON.

## Regola di visibilità

Le regole di visibilità determinano se un visitatore può visualizzare un campo in base ai valori immessi nel modulo. Ogni regola confronta il valore di un `subjectField` nel modulo con un elenco di valori nella regola.

Un campo può avere un tipo di regola di visibilità: `show`, `hide` o `alwaysShow`. L’API valuta le regole del campo dall’alto verso il basso e applica la prima regola che restituisce true.

La modifica delle regole di visibilità è un aggiornamento distruttivo.

```http
POST /rest/asset/v1/form/{id}/field/Email/visibility.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
visibilityRule={"ruleType":"show", "rules":[{"subjectField": "LastName", "operator": "isNotEmpty", "values": [], "altLabel": "Email:"}]}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "ab4a#15509030601",
    "result": [
        {
            "formFieldId": "Email",
            "ruleType": "show",
            "rules": [
                {
                    "subjectField": "LastName",
                    "operator": "isNotEmpty",
                    "values": [],
                    "altLabel": "Email:"
                }
            ]
        }
    ]
}
```

Per l&#39;elenco completo degli operatori, vedere [Aggiungi regole di visibilità campi modulo](https://developer.adobe.com/marketo-apis/api/asset#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Follow-up

Le regole di follow-up dinamiche possono reindirizzare i visitatori a una pagina o mantenerli nella pagina corrente in base ai valori dei campi designati al momento dell’invio. Le regole della pagina di ringraziamento e della pagina di follow-up fanno riferimento allo stesso comportamento.

Rappresenta le regole come array JSON i cui record contengono `followupType`, `followupValue`, `operator`, `subjectField`, `values` e `default`. Solo un record nell&#39;array può avere `default` booleano impostato su `true`. Il modulo utilizza tale record quando un visitatore non è idoneo per un’altra regola.

Il valore `followupType` può essere `lp` o `url`. Il valore `lp` indica che `followupValue` è un ID pagina di destinazione di Marketo. Il valore `url` indica che `followupValue` è l&#39;URL di un&#39;altra pagina. L’operatore confronta il valore del campo oggetto con i valori forniti.

## Pulsante Invia

Utilizza l&#39;endpoint [Aggiorna pulsante di invio](https://developer.adobe.com/marketo-apis/api/asset#tag/Forms/operation/updateFormSubmitButtonUsingPOST) per modificare lo stile del pulsante di invio. È possibile aggiornare `buttonPosition`, `buttonStyle`, `label` e `waitingLabel`. `waitingLabel` viene visualizzato mentre l&#39;invio è in sospeso.

Questo è un aggiornamento distruttivo.

## Approvazione

Forms segue un ciclo di vita approvato come bozza. Un modulo può avere una versione bozza, una versione approvata o entrambe. Gli aggiornamenti vengono sempre applicati alla bozza e diventano attivi solo dopo l’approvazione.

L’approvazione di un modulo sostituisce l’eventuale versione approvata esistente con la bozza corrente. Se si annulla l’approvazione di un modulo live, vengono eliminate tutte le bozze correnti e la versione approvata viene ridotta a uno stato di sola bozza. Annullare sempre l&#39;approvazione di un modulo prima di tentarne l&#39;eliminazione.

## Profilatura progressiva

Quando è abilitata la profilatura progressiva, l&#39;elenco dei campi modulo include un set di campi denominato `Profiling`. Utilizzare l&#39;endpoint Aggiorna posizioni campo per aggiungere o rimuovere campi dall&#39;elenco di profilatura progressiva.

Questo endpoint esegue aggiornamenti distruttivi, pertanto ogni richiesta deve includere tutti i campi del modulo. Nell&#39;esempio seguente `Phone` viene aggiunto all&#39;elenco di profilatura progressiva.

```http
POST /rest/asset/v1/form/{id}/reArrange.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
positions=[{"columnNumber":0,"rowNumber":0,"fieldName":"Email"},{"columnNumber":0,"rowNumber":1,"fieldName":"LastName"},{"columnNumber":0,"rowNumber":2,"fieldName":"Company"},{"columnNumber":0,"rowNumber":3,"fieldName":"Website"},{"columnNumber":0,"rowNumber":4,"fieldName":"Profiling","fieldList":[{"columnNumber":0,"rowNumber":0,"fieldName":"Phone"}]}]
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d6a#164190dbdf2",
    "result": [
        {
            "id": 1031
        }
    ]
}
```
