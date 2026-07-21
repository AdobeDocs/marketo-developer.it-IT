---
title: Regole di reindirizzamento pagina di destinazione
feature: REST API, Landing Pages
description: Utilizza le API REST di Marketo Asset per creare, eseguire query, aggiornare ed eliminare le regole di reindirizzamento delle pagine di destinazione con filtri, impaginazione, opzioni del nome host e destinazioni non Marketo.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
TQID: https://experienceleague.adobe.com/2gePbKA3xeoRdnL8mNnObN-GPTX00Ii4-zcM0lBjs-o
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615
feature_v2: id: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: 3e6d310c5aec1a3435424fb122b71d825db5af0e
workflow-type: tm+mt
source-wordcount: 626
ht-degree: 3%

---

# Regole di reindirizzamento pagina di destinazione

[Riferimento endpoint per regole di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules)

Utilizza le API REST delle regole di reindirizzamento della pagina di destinazione per eseguire query, creare, aggiornare ed eliminare gli URL di reindirizzamento della pagina di destinazione.

Le regole di reindirizzamento inviano l’URL di una pagina di destinazione a un altro URL. L’origine e la destinazione possono essere pagine Marketo o non Marketo. Per la documentazione sul prodotto correlato, consulta [Documentazione di Marketo Engage](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=it).

## Query

Eseguire una query sulle regole di reindirizzamento della pagina di destinazione [per ID](#by_id) o per [navigazione](#browse).

### Per ID

L&#39;endpoint [Get Landing Page Redirect Rules by ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) accetta un parametro di percorso `id` della regola di reindirizzamento e restituisce il record corrispondente.

```http
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

### Sfoglia

L&#39;endpoint [Get Landing Page Redirect Rules](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) restituisce record di regole di reindirizzamento della pagina di destinazione.

Utilizza parametri di query facoltativi per filtrare i risultati.

Il parametro `offset` è un numero intero che specifica il numero massimo di voci da restituire (il valore predefinito è 20). Il massimo è 200. Il parametro `maxReturn` è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato insieme all&#39;offset (il valore di default è 0).

Il parametro `hostname` filtra per nome host della pagina di destinazione.

Il numero intero `redirectToLandingPageId` è filtrato in base all&#39;ID della pagina di destinazione di destinazione. Il parametro `redirectToPath` filtra in base al percorso della pagina di destinazione di destinazione.

I parametri `earliestUpdatedAt` e `latestUpdatedAt` impostano i limiti di data/ora minimo e massimo. L’endpoint restituisce regole create o aggiornate all’interno dell’intervallo.

```http
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

Chiama l&#39;endpoint [Crea regola di reindirizzamento pagina di destinazione](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) con una richiesta POST `application/x-www-form-urlencoded`. La richiesta dispone di tre parametri obbligatori.

Il parametro `hostname` specifica il nome host della pagina di destinazione. Deve appartenere a un dominio o alias di branding e non può superare i 255 caratteri.

Il parametro `redirectFrom` specifica la pagina di destinazione di origine come oggetto JSON con una coppia tipo/valore. L&#39;attributo `type` può essere `landingPageId` per una pagina di destinazione Marketo o `path` per una pagina non Marketo.

| Parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| &#39;get&#39; | Obbligatorio | Stringa | Azione del metodo. |
| &#39;visitatore&#39; | Obbligatorio | Stringa | Nome del metodo. |
| callback | Obbligatorio | Funzione | Funzione di callback da attivare per ogni campagna restituita. |

Il parametro `redirectTo` specifica la destinazione come oggetto JSON con una coppia tipo/valore. L&#39;attributo `type` può essere `landingPageId` per una pagina di destinazione Marketo o `url` per una pagina non Marketo.

| Tipo di pagina di destinazione | tipo redirectTo | Esempio |
| --- | --- | --- |
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Non Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Per ulteriori informazioni, vedere [Reindirizzare una pagina di destinazione di Marketo a un&#39;altra pagina](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html).

```http
POST /rest/asset/v1/redirectRules.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

L&#39;endpoint [Update Landing Page Redirect Rules](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) accetta un parametro di percorso `id` della regola di reindirizzamento. Invia l&#39;aggiornamento come richiesta POST `application/x-www-form-urlencoded`.

Passa uno o più di questi parametri per selezionare gli attributi da aggiornare: `hostname`, `redirectFrom` o `redirectTo`.

La risposta restituisce il record regola di reindirizzamento aggiornato.

```http
POST /rest/asset/v1/redirectRule/{id}.json
```

```text
Content-Type: application/x-www-form-urlencoded
```

```text
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

L&#39;endpoint [Elimina regola di reindirizzamento pagina di destinazione per ID](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) accetta un parametro di percorso `id` della regola di reindirizzamento.

```http
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

L&#39;endpoint [Get Landing Page Domains](https://developer.adobe.com/marketo-apis/api/asset#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) restituisce i record del dominio della pagina di destinazione.

Utilizza due parametri di query facoltativi per filtrare i risultati.

Il parametro `offset` è un numero intero che specifica il numero massimo di voci da restituire (il valore predefinito è 20, il valore massimo è 200).

Il parametro `maxReturn` è un numero intero che specifica dove iniziare a recuperare le voci. Può essere utilizzato in combinazione con `offset` (il valore predefinito è 0).

```http
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
