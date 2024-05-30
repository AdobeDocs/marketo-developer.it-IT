---
title: "Regole di reindirizzamento pagina di destinazione"
feature: REST API, Landing Pages
description: "Configurare le regole di reindirizzamento delle pagine di destinazione tramite l’API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 2%

---


# Regole di reindirizzamento pagina di destinazione

[Riferimento endpoint per regole di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo offre un set di API REST per l’esecuzione di operazioni CRUD sugli URL di reindirizzamento della pagina di destinazione. Queste API seguono il pattern di interfaccia standard per le API delle risorse, fornendo le opzioni Query, Create, Update e Delete.

Le regole di reindirizzamento per pagine di destinazione consentono di reindirizzare l’URL di una pagina di destinazione a un altro URL. Puoi reindirizzare pagine di destinazione di Marketo, pagine di destinazione non Marketo o relative combinazioni. Ulteriori informazioni sulle regole della pagina di destinazione di reindirizzamento sono disponibili [qui](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=it).

## Query

La query delle regole di reindirizzamento della pagina di destinazione segue i tipi di query standard per le risorse di [per ID](#by_id), e [navigazione](#browse).

### Per ID

Il [Ottieni regole di reindirizzamento pagina di destinazione per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) l’endpoint richiede un singolo reindirizzamento della regola della pagina di destinazione `id` parametro path e restituisce un singolo record della regola di reindirizzamento della pagina di destinazione.

```
GET /rest/asset/v1/redirectRule/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d0#1707b2521e4",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

### Sfogliare

Il [Ottieni regole di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) l’endpoint restituisce un elenco di record della regola di reindirizzamento pagina di destinazione.

Esistono diversi parametri di query facoltativi che possono essere trasmessi ai risultati del filtro.

Il `offset` Il parametro è un numero intero che specifica il numero massimo di voci da restituire (il valore predefinito è 20). Il massimo è 200. Il `maxReturn` Il parametro è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato insieme all&#39;offset (il valore di default è 0).

Il `hostname` Il parametro può essere utilizzato per filtrare in base al nome host delle pagine di destinazione.

Il `redirectToLandingPageId` è un numero intero che può essere utilizzato per filtrare in base all’ID della pagina di destinazione a cui stai reindirizzando. Il `redirectToPath` può essere utilizzato per filtrare in base al percorso delle pagine di destinazione a cui stai reindirizzando.

Il `earliestUpdatedAt` e `latestUpdatedAt` I parametri ti consentono di impostare filigrane di data e ora basse e alte per restituire le regole di reindirizzamento della pagina di destinazione aggiornate o create inizialmente all’interno dell’intervallo specificato.

```
GET /rest/asset/v1/redirectRules.json&maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "12213#1707b27efb5",
    "warnings": [],
    "result": [
        {
            "id": 5,
            "redirectFromUrl": "https://www.kirtideep.contact/LandingPage2.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5406
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:26:29Z+0000",
            "updatedAt": "2019-11-14T06:26:29Z+0000"
        },
        {
            "id": 6,
            "redirectFromUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectTo": {
                "type": "url",
                "value": "www.contactLogs.com"
            },
            "redirectToUrl": "www.contactLogs.com",
            "createdAt": "2019-11-14T06:27:10Z+0000",
            "updatedAt": "2019-11-14T06:27:10Z+0000"
        },
        {
            "id": 7,
            "redirectFromUrl": "https://www.kirtideep.contact/contact/log/check",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "path",
                "value": "/contact/log/check"
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:27:49Z+0000",
            "updatedAt": "2019-11-14T06:27:49Z+0000"
        }
    ]
}
```

## Crea

Il [Crea regola di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) l’endpoint viene eseguito con application/x-www-form-urlencoded POST con i tre parametri richiesti seguenti.

Il `hostname` Il parametro specifica il nome host per la pagina di destinazione. Deve appartenere a un dominio o alias di branding. La lunghezza massima è di 255 caratteri.

Il `redirectFrom` Il parametro specifica la pagina di destinazione di origine. Si tratta di un oggetto JSON che contiene una coppia tipo/valore che determina se l’origine è una pagina di destinazione di Marketo o non di Marketo. Il `type` può essere &quot;landingPageId&quot; o &quot;path&quot;.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| &#39;get&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;visitatore&#39; | Obbligatorio | Stringa | Nome del metodo. |
| callback | Obbligatorio | Funzione | Funzione di callback da attivare per ogni campagna restituita. |


Il `redirectTo` Il parametro specifica la pagina di destinazione di destinazione di target. Si tratta di un oggetto JSON che contiene una coppia tipo/valore che determina se l’origine è una pagina di destinazione di Marketo o non di Marketo. Il `type` può essere &quot;landingPageId&quot; o &quot;url&quot;.

| Tipo di pagina di destinazione | tipo redirectTo | Esempio |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Non Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Puoi trovare ulteriori informazioni sulla creazione di regole di reindirizzamento per le pagine di destinazione [qui](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

```
POST /rest/asset/v1/redirectRules.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
hostname=calqeauto.com&redirectFrom={"type":"landingPageId", "value":"5483"}&redirectTo={"type":"landingPageId", "value":"5559"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d7c6#1707b223522",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

## Aggiornamento

Il [Aggiorna regole di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) l’endpoint accetta una singola regola di reindirizzamento della pagina di destinazione `id` parametro di percorso. Questo endpoint viene eseguito con un POST application/x-www-form-urlencoded.

Come per la chiamata di creazione descritta in precedenza, vengono passati uno o più dei seguenti parametri di query per specificare l’attributo della regola da aggiornare: `hostname`, `redirectFrom`, `redirectTo`.

Nella risposta viene restituito il record aggiornato della regola di reindirizzamento pagina di destinazione.

```
POST /rest/asset/v1/redirectRule/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
redirectTo={"type":"landingPageId", "value":"5561"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "57b2#1707b3852d7",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5561
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage3.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T07:20:53Z+0000"
        }
    ]
}
```

## Elimina

Il [Elimina regola di reindirizzamento pagina di destinazione per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) l’endpoint richiede un singolo reindirizzamento della regola della pagina di destinazione `id` parametro di percorso.

```
POST /rest/asset/v1/redirectRule/{id}/delete.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "d505#154d01c8364",
  "result": [
    {
      "id": 2
    }
  ]
}
```

## Sfoglia i domini della pagina di destinazione

Il [Ottieni domini pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) l’endpoint restituisce un elenco di record di dominio della pagina di destinazione.

Esistono due parametri di query facoltativi che possono essere trasmessi ai risultati del filtro.

Il `offset` Il parametro è un numero intero che specifica il numero massimo di voci da restituire (il valore predefinito è 20, il valore massimo è 200).

Il `maxReturn` Il parametro è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato insieme a `offset` Il valore predefinito è 0.

```
POST /rest/asset/v1/landingPageDomains.json?maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6eb8#1707b43d3cb",
    "warnings": [],
    "result": [
        {
            "hostname": "calqeauto.com",
            "type": "domain"
        },
        {
            "hostname": "www.google.com",
            "type": "domain-alias"
        },
        {
            "hostname": "www.kirti.com",
            "type": "domain-alias"
        }
    ]
}
```
