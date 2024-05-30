---
title: "URL di base"
feature: REST API
description: "Descrive come utilizzare gli URL per Marketo."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


# URL di base

Il [Riferimento endpoint](endpoint-reference.md) La documentazione di ogni chiamata API mostra il metodo REST, il percorso, la risorsa e i parametri che devono essere aggiunti all’URL di base per formare una richiesta.

Di seguito è riportato un esempio di URL REST ben formato:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

composto dalle seguenti parti:

- URL di base: `https://284-RPR-133.mktorest.com/rest`
- Percorso: `/v1/lead/`
- Risorsa: `318582.json`
- Parametro query: `fields=email,firstName,lastName`

L’URL di base contiene l’ID account (alias ID Munchkin) ed è quindi univoco per ogni abbonamento a Marketo. Per trovare l’URL di base, accedi a Marketo e passa al file **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** menu. È etichettato come &quot;Endpoint:&quot; sotto la sezione &quot;REST API&quot; come mostrato nelle schermate seguenti.

![Endpoint URL di base servizi Web](assets/rest-api-base-url-web-services.png)

Una volta trovato l’URL di base, copialo e includilo negli URL utilizzati quando chiami una qualsiasi delle API REST.
