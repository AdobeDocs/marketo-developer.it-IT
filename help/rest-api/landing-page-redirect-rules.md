---
title: Regole di reindirizzamento pagina di destinazione
feature: REST API, Landing Pages
description: Configura le regole di reindirizzamento delle pagine di destinazione tramite l’API.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 2%

---

# Regole di reindirizzamento pagina di destinazione

[Riferimento endpoint regole di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo offre un set di API REST per l’esecuzione di operazioni CRUD sugli URL di reindirizzamento della pagina di destinazione. Queste API seguono il pattern di interfaccia standard per le API delle risorse, fornendo le opzioni Query, Create, Update e Delete.

Le regole di reindirizzamento per pagine di destinazione consentono di reindirizzare l’URL di una pagina di destinazione a un altro URL. Puoi reindirizzare pagine di destinazione di Marketo, pagine di destinazione non Marketo o relative combinazioni. Ulteriori informazioni sulle regole della pagina di destinazione di reindirizzamento sono disponibili [qui](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=it).

## Query

La query sulle regole di reindirizzamento della pagina di destinazione segue i tipi di query standard per le risorse di [per ID](#by_id) e [esplorazione](#browse).

### Per ID

L&#39;endpoint [Get Landing Page Redirect Rules by Id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) accetta un singolo parametro di percorso di reindirizzamento della regola della pagina di destinazione `id` e restituisce un singolo record della regola di reindirizzamento della pagina di destinazione.

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

L&#39;endpoint [Get Landing Page Redirect Rules](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) restituisce un elenco di record della regola di reindirizzamento della pagina di destinazione.

Esistono diversi parametri di query facoltativi che possono essere trasmessi ai risultati del filtro.

Il parametro `offset` è un numero intero che specifica il numero massimo di voci da restituire (il valore predefinito è 20). Il massimo è 200. Il parametro `maxReturn` è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato insieme all&#39;offset (il valore di default è 0).

Il parametro `hostname` può essere utilizzato per filtrare in base al nome host delle pagine di destinazione.

`redirectToLandingPageId` è un numero intero che può essere utilizzato per filtrare in base all&#39;ID della pagina di destinazione a cui si sta reindirizzando. `redirectToPath` può essere utilizzato per filtrare in base al percorso delle pagine di destinazione a cui si sta reindirizzando.

I parametri `earliestUpdatedAt` e `latestUpdatedAt` consentono di impostare filigrane di data/ora basse e alte per restituire le regole di reindirizzamento della pagina di destinazione aggiornate o create inizialmente all&#39;interno dell&#39;intervallo specificato.

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

## Creare

L&#39;endpoint [Crea regola di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) viene eseguito con un POST urlencoded application/x-www-form-urlencoded con i seguenti tre parametri obbligatori.

Il parametro `hostname` specifica il nome host per la pagina di destinazione. Deve appartenere a un dominio o alias di branding. La lunghezza massima è di 255 caratteri.

Il parametro `redirectFrom` specifica la pagina di destinazione di origine. Si tratta di un oggetto JSON che contiene una coppia tipo/valore che determina se l’origine è una pagina di destinazione di Marketo o non di Marketo. L&#39;attributo `type` può essere &quot;landingPageId&quot; o &quot;path&quot;.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
|---|---|---|---|
| &#39;get&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;visitatore&#39; | Obbligatorio | Stringa | Nome del metodo. |
| callback | Obbligatorio | Funzione | Funzione di callback da attivare per ogni campagna restituita. |

Il parametro `redirectTo` specifica la pagina di destinazione di destinazione di destinazione. Si tratta di un oggetto JSON che contiene una coppia tipo/valore che determina se l’origine è una pagina di destinazione di Marketo o non di Marketo. L&#39;attributo `type` può essere &quot;landingPageId&quot; o &quot;url&quot;.

| Tipo di pagina di destinazione | tipo redirectTo | Esempio |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Non Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Ulteriori informazioni sulla creazione di regole di reindirizzamento per le pagine di destinazione sono disponibili [qui](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html?lang=it).

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

L&#39;endpoint [Aggiorna regole di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) accetta un parametro di percorso `id` per la regola di reindirizzamento di una singola pagina di destinazione. Questo endpoint viene eseguito con un POST codificato in formato application/x-www-form-urlencoded.

Come per la chiamata di creazione descritta in precedenza, uno o più dei seguenti parametri di query vengono passati per specificare quale attributo della regola aggiornare: `hostname`, `redirectFrom`, `redirectTo`.

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

L&#39;endpoint [Elimina regola di reindirizzamento pagina di destinazione per ID](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) accetta un singolo parametro di percorso di reindirizzamento della regola della pagina di destinazione `id`.

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

L&#39;endpoint [Get Landing Page Domains](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) restituisce un elenco di record di dominio della pagina di destinazione.

Esistono due parametri di query facoltativi che possono essere trasmessi ai risultati del filtro.

Il parametro `offset` è un numero intero che specifica il numero massimo di voci da restituire (il valore predefinito è 20, il valore massimo è 200).

Il parametro `maxReturn` è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato in combinazione con `offset` (il valore predefinito è 0).

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
