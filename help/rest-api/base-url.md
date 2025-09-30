---
title: URL di base
feature: REST API
description: Scopri come generare richieste API REST di Marketo, comprendere la risorsa e i parametri del percorso dell’URL di base e trovare l’URL di base univoco.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---

# URL di base

La documentazione [Riferimento endpoint](endpoint-reference.md) per ogni chiamata API mostra il metodo REST, il percorso, la risorsa e i parametri che devono essere aggiunti all&#39;URL di base per formare una richiesta.

Di seguito è riportato un esempio di URL REST ben formato:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

composto dalle seguenti parti:

- URL di base: `https://284-RPR-133.mktorest.com/rest`
- Percorso: `/v1/lead/`
- Risorsa: `318582.json`
- Parametro query: `fields=email,firstName,lastName`

L’URL di base contiene l’ID account (alias Munchkin ID) ed è quindi univoco per ogni abbonamento a Marketo. L&#39;URL di base viene trovato accedendo a Marketo e passando al menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]**. È etichettato come &quot;Endpoint:&quot; sotto la sezione &quot;REST API&quot; come mostrato nelle schermate seguenti.

![Endpoint URL di base servizi Web](assets/rest-api-base-url-web-services.png)

Una volta trovato l’URL di base, copialo e includilo negli URL utilizzati quando chiami una qualsiasi delle API REST.
