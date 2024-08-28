---
title: Riferimento API di Munchkin
description: Utilizza l’API JavaScript Munchkin per personalizzare i dati Munchkin.
feature: Javascript
source-git-commit: c6c0a492ede415471e10efb6213eb3f590e63ebe
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 7%

---


# Riferimento API di Munchkin

Munchkin fornisce diverse funzioni che possono essere richiamate manualmente tramite JavaScript. Questi possono consentire il tracciamento personalizzato degli eventi del browser, come la riproduzione di video, o i clic su elementi non collegati.

## Funzioni

L&#39;API Munchkin include le funzioni seguenti: `init`, `createTrackingCookie`, `munchkinFunction`.

### Munchkin.init()

`Munchkin.init()` deve essere chiamato prima di qualsiasi altra funzione. Munchkin viene configurato sulla pagina corrente per inviare attività a un&#39;istanza specifica e genera un&#39;attività &quot;Pagina Web visite&quot; per la pagina corrente.

| Nome parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| ID Munchkin | Obbligatorio | Stringa | ID account Munchkin trovato nel menu Amministrazione > Integrazione > Munchkin. Imposta l&#39;istanza di destinazione a cui inviare le attività. |
| [Impostazioni di configurazione](configuration.md) | Facoltativo | Oggetto | Abilita impostazioni di comportamento alternative per Munchkin. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

Quando viene chiamato, questo controlla che nel browser esista un cookie `_mkto_trk` e, in caso contrario, ne crea uno. Questa opzione è utile per tenere traccia degli utenti durante azioni specifiche, ad esempio la registrazione o il download di una risorsa, se `cookieAnon` è impostato su false.

| Nome parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| forceCreate | Obbligatorio | Booleano | Crea cookie anche se `cookieAnon` è impostato su false. |


```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Utilizzato per generare comportamenti di tracciamento personalizzati, come riproduzioni e pause del lettore video, o visite alle pagine per navigazione non standard, come i codici hash.

| Nome parametro | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| Tipo di funzione | Obbligatorio | Stringa | Determina l’attività da registrare. Valori consentiti: `visitWebPage`, `clickLink`, `associateLead` |
| Dati | Obbligatorio | Oggetto | Contiene i dati per l’attività da registrare. |

#### visitWebPage

La chiamata a `munchkinFunction()` con `visitWebPage` invia un&#39;attività &#39;visita&#39; per l&#39;utente corrente a Marketo. È possibile personalizzare l&#39;URL e `querystring` inviati con l&#39;oggetto dati nel secondo argomento.

| Nome proprietà dati | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| url | Obbligatorio | Stringa | Percorso del file URL utilizzato per registrare una visita di pagina.  Questo valore viene aggiunto al nome di dominio corrente per creare il nome di pagina completo. Ad esempio, se l&#39;URL è `/index.html` e il nome di dominio è `www.example.com`, la pagina visitata viene registrata come `www.example.com/index.html`. |
| parametri | Facoltativo | Stringa | Stringa di query dei parametri desiderati da registrare. |

Ad esempio, `foo=bar&biz=baz`.

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

La chiamata a `munchkinFunction()` con `clickLink` invia un&#39;attività di clic per l&#39;utente corrente a Marketo. È possibile personalizzare l&#39;URL di clic con la proprietà `href` nell&#39;oggetto dati.

| Nome proprietà dati | Facoltativo/Obbligatorio | Tipo | Descrizione |
| --- | --- | --- | --- |
| href | Obbligatorio | Stringa | Percorso del file URL utilizzato per registrare il clic su un collegamento. Questo valore viene aggiunto al nome di dominio corrente per creare un collegamento completo. |

Ad esempio, se href è `/index.html` e il nome di dominio è `www.example.com`, il clic sul collegamento verrà registrato come `www.example.com/index.html`.

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### associateLead

Questo metodo è obsoleto e non è più disponibile.
