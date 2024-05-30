---
title: "Forms"
feature: REST API, Forms
description: "Crea e gestisci i moduli tramite l’API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 0%

---


# Forms

[Riferimento endpoint Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms)

[Riferimento endpoint campi modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields)

I moduli Marketo dispongono di un set complesso di endpoint che consentono il controllo completo della gestione dei moduli dai sistemi remoti. La struttura delle maschere può essere complessa, in quanto esistono molti tipi diversi di oggetti che devono essere gestiti come parte di un modulo: Forms, Fields, Fieldset, Regole di visibilità e Regole di pagina di follow-up.

## Query

Forms supporta i metodi standard per il recupero delle risorse, [per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET), [per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET), e [mediante navigazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET). Ogni risposta del modulo contiene tutte le proprietà ad eccezione dell’elenco dei campi.

### Per ID

[Ottieni modulo per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByIdUsingGET) prende una forma `id` come parametro di percorso e restituisce un record di modulo.

```
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

[Ottieni modulo per nome](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) prende una forma `name` come parametro di percorso e restituisce un record di modulo.

```
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

### Sfogliare

[Ottieni Forms](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/browseForms2UsingGET) Forms funziona come altri endpoint di navigazione dell’API Asset e consente di filtrare facoltativamente su `status`, `maxReturn`, e `offset`. Lo stato può essere: approvato, approvato con bozza o bozza.

```
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

Il recupero dell’elenco dei campi per un modulo viene eseguito in base al modulo.

```
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

Quando si modificano i campi o il loro comportamento all’interno di un modulo, l’elenco dei campi deve sempre essere recuperato prima di tentare di apportare modifiche. In questo modo puoi assegnare l’ID campo corretto quando esegui l’aggiornamento o l’eliminazione.

### Tipi di campi

| Tipo di interfaccia utente | Nome API |
|--------------|-----------------|
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

Il [Ottieni modulo utilizzato da](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getFormUsedByUsingGET) l’endpoint assume una forma `id` come parametro percorso e restituisce l’elenco delle risorse che dipendono dal modulo. Forms può essere utilizzato dai seguenti tipi di risorse: pagine di destinazione, elenchi avanzati, campagne avanzate, rapporti, programmi e-mail.

```
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

Quando [creazione di un modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/createLpFormsUsingPOST) sono disponibili solo due campi obbligatori: la cartella padre del modulo e il nome del modulo. Tutti gli altri parametri sono facoltativi con il valore predefinito. Al momento della creazione, il modulo viene fornito con tre campi predefiniti: Nome, Cognome, E-mail.

```
POST /rest/asset/v1/forms.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Forms sono [aggiornato](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormsUsingPOST) con una chiamata simile tramite il loro id. Durante la creazione o l&#39;aggiornamento, qualsiasi parametro di stile di base è accessibile e modificabile, consentendo di modificare la modalità di visualizzazione del modulo per l&#39;utente finale.

```
POST /rest/asset/v1/form/736.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

I comportamenti di pagina visitatore noto e ringraziamento non possono essere modificati tramite le chiamate per creare o aggiornare i moduli e devono essere accessibili tramite i rispettivi endpoint.

## Metadati campo

Per aggiungere o modificare correttamente i campi appartenenti a un modulo, è necessario recuperare l’elenco dei campi validi per l’istanza di destinazione. Le interazioni con i campi vengono sempre eseguite in base alla proprietà id del campo, che viene visualizzata per ogni elemento del risultato.

Per i campi Lead, viene utilizzato il [Ottieni campi modulo disponibili](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllFieldsUsingGET) e include il tipo di dati e i metadati predefiniti per il campo quando viene aggiunto a un modulo.

```
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

Per i campi personalizzati dei membri del programma, invoca [Recupera campi membri del programma modulo disponibili](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getAllProgramMemberFieldsUsingGET)  endpoint per recuperare i tipi di dati dei campi personalizzati del membro del programma e i metadati predefiniti. Per utilizzare questi campi in un modulo, il modulo deve trovarsi sotto un programma (non in Design Studio). Le pagine di destinazione contenenti moduli che utilizzano questi campi devono trovarsi anche sotto un programma (non possono risiedere in Design Studio o essere clonate in Design Studio).

```
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

Ogni modulo contiene un elenco modificabile di campi che verrà visualizzato all’utente finale al momento del caricamento. Ogni campo viene aggiunto, aggiornato o eliminato dall’elenco dei campi uno alla volta tramite i rispettivi endpoint.

[Aggiunta di un campo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldToAFormUsingPOST) richiede solo l’id del modulo principale e il fieldId del campo. Tutti gli altri campi saranno vuoti o avranno valori predefiniti basati sul loro tipo di dati e metadati di campo. I dati vengono passati come POST x-www-form-urlencoded, non come JSON.

```
POST /rest/asset/v1/form/{id}/fields.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Gli aggiornamenti possono modificare tutti gli stessi campi come l’aggiunta di un campo e allo stesso modo richiedere l’ID del modulo e il fieldId, tranne per il fatto che fieldId è un parametro di percorso e non un parametro di query durante l’esecuzione degli aggiornamenti.

```
POST /rest/asset/v1/form/{id}/field/LastName.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Nell’esempio precedente stiamo aggiornando il campo Cognome che è una stringa semplice. Alcuni campi modulo sono più complessi. Ad esempio, il campo Formula di apertura è un tipo di campo &quot;seleziona&quot; che contiene un elenco di elementi e un valore predefinito. Se si aggiunge o si aggiorna un campo di tipo selezionato, a meno che non si imposti una delle opzioni per avere un `isDefault` se il valore è true, la prima scelta non ha alcun valore ed è etichettata come &quot;Select...&quot;

![Saluto](assets/form-field-salutation.png)

Per aggiornare le voci di elenco, il formato del parametro &quot;values&quot; è il seguente:

```
POST /rest/asset/v1/form/{id}/field/Salutation.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

 

Per determinare come formattare un campo modulo complesso, esaminare la risposta di Aggiungi campo a modulo.

### Ridisposizione del campo

I campi di un modulo devono essere ridisposti come un&#39;unica unità tramite [Cambia posizioni campo modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST) endpoint. L’endpoint richiede un parametro denominato `positions`, che è un array JSON di oggetti con tre membri:

- columnNumber
- rowNumber
- fieldName (si riferisce all’ID del campo)

I campi di un modulo sono disposti in un&#39;interfaccia di tipo tabella, con un massimo di tre colonne e dieci righe. Sia la riga che la colonna sono indicizzate da 0, pertanto sia la prima riga che la prima colonna sono indicate passando uno 0. Tutti i campi devono occupare una posizione univoca

Se il campo di destinazione è anche un set di campi, il relativo record all&#39;interno della matrice di posizioni deve contenere anche un parametro denominato fieldList, una matrice di oggetti contenente gli stessi membri columnNumber, rowNumber e fieldName. Il set di campi stesso viene considerato come un singolo campo per la sua posizione nell&#39;elenco padre, mentre i relativi sottocampi vengono posizionati in base alle posizioni specificate nel parametro fieldList.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

I campi in formato Rich Text vengono aggiunti tramite una [endpoint separato](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addRichTextFieldUsingPOST) dai campi lead. Il contenuto del campo viene passato come dati multipart/form. Deve essere strutturato come contenuto HTML che non contiene script, metatag o tag di collegamento.

```
POST /rest/asset/v1/form/{id}/richText.json
```

```
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

In Marketo forms è disponibile un componente facoltativo denominato set di campi. I set di campi sono gruppi di campi che vengono trattati come un singolo campo all’interno dell’elenco dei campi di livello superiore a fini di spostamento e trattamento da parte delle regole di visibilità. Ad esempio, se è presente un campo per Requisiti di conformità e un client seleziona sì, potrebbe rivelare un set di campi contenente i campi per i requisiti di conformità HIPAA e PCI.

I campi all&#39;interno dei set di campi sono univoci per il modulo nel suo insieme, pertanto è possibile che i campi duplicati non siano presenti sia nell&#39;elenco dei campi padre del modulo che in un set di campi figlio. I set di campi vengono aggiunti tramite [Aggiungi set di campi al modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFieldSetUsingPOST) e verrà quindi visualizzato nel risultato di [Recupera campi per modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/getFormFieldByFormVidUsingGET). I campi vengono aggiunti a un set di campi spostandoli nel fieldList del set di campi tramite [Aggiorna posizioni campo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/updateFieldPositionsUsingPOST). Per questi endpoint, i dati vengono passati come POST x-www-form-urlencoded, non come JSON.

## Regola di visibilità

Ogni campo può avere un set di regole di visibilità che determinano se un visitatore può visualizzare il campo, a seconda dei valori immessi nel modulo. Le regole consentono di confrontare il valore di un oggettoField presente nel modulo con un elenco di valori specificati nella regola. Ogni campo può avere un tipo di regola di visibilità, mostra, nascondi o sempreMostra, quindi un elenco di regole da valutare. Le regole vengono valutate dall&#39;alto verso il basso e la prima regola che restituisce true è quella che verrà applicata.

La modifica delle regole di visibilità è un aggiornamento distruttivo.

```
POST /rest/asset/v1/form/{id}/field/Email/visibility.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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

Per l’elenco completo degli operatori disponibili, consulta la pagina di riferimento dell’endpoint per [Aggiungi regole di visibilità campo modulo](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/operation/addFormFieldVisibilityRuleUsingPOST).

## Follow-up

I Marketo form possono avere un comportamento dinamico di pagina di follow-up in cui le regole per il reindirizzamento a una determinata pagina o la permanenza nella pagina corrente possono essere applicate in base al contenuto dei campi designati al momento dell’invio. Le regole possono essere denominate regole di pagina di ringraziamento o regole di pagina di follow-up in modo intercambiabile. Queste regole sono rappresentate come array JSON con i membri `followupType`, `followupValue`, `operator`, `subjectField`, `values`, e `default`. `default` è un valore booleano per il quale può essere vero un solo record nell’array. Quando un visitatore non è idoneo per altre regole, viene utilizzata la regola designata come predefinita. `followupType` può essere lp o url, dove lp indica un ID pagina di destinazione Marketo per `followupValue`, e url indicheranno un URL per un’altra pagina. L’operatore viene utilizzato per confrontare il valore del campo soggetto con l’elenco di valori fornito.

## Pulsante Invia

Lo stile del pulsante Invia del modulo viene gestito con [Pulsante Aggiorna invio](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/updateFormSubmitButtonUsingPOST) endpoint. È possibile modificare buttonPosition, buttonStyle, label e waitLabel (l&#39;etichetta visualizzata quando l&#39;invio è in sospeso).

Questo è un aggiornamento distruttivo.

## Approvazione

Come la maggior parte delle altre risorse, i moduli seguono un modello approvato come bozza, in cui può essere presente una versione bozza e/o una versione approvata. Ogni volta che si applicano aggiornamenti a un modulo, questi vengono sempre applicati prima alla versione bozza e vengono visualizzati in diretta solo dopo l’approvazione del modulo. L’approvazione di un modulo richiede la versione bozza corrente e la sostituzione dell’eventuale versione approvata con la bozza. Se il modulo deve essere rimosso dalla versione live, deve prima essere non approvato, eliminando tutte le bozze correnti e abbassando di livello la versione approvata a uno stato di sola bozza. Prima di tentare l’eliminazione, Forms deve sempre essere non approvato.

## Profilatura progressiva

Quando per un modulo è abilitata la profilatura progressiva, nell&#39;elenco dei campi viene incluso un set di campi denominato &quot;Profilatura&quot;. Per aggiungere o rimuovere campi dall&#39;elenco di profilatura progressiva, è necessario utilizzare l&#39;endpoint Aggiorna posizioni campo. Questo endpoint effettua aggiornamenti distruttivi, pertanto tutti i campi nel modulo devono essere inclusi in ogni richiesta. L&#39;esempio seguente aggiunge il campo &quot;Phone&quot; all&#39;elenco di profilatura progressiva.

```
POST /rest/asset/v1/form/{id}/reArrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
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
