---
title: Riferimento API di Forms
description: Riferimento completo per l’API Marketo Forms 2.0, con informazioni dettagliate sui metodi MktoForms2 e Form, parametri, callback e risultati per il caricamento e il rendering dei moduli.
feature: Forms, Javascript
exl-id: 0f8d242f-0b27-4087-b080-3d41ebaa25b3
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1345'
ht-degree: 1%

---

# Riferimento API di Forms

Esistono due oggetti principali con cui interagirai utilizzando l’API di Forms 2.0. Oggetto `MktoForms2` e oggetto `Form`. L&#39;oggetto `MktoForms2` è lo spazio dei nomi di primo livello pubblicamente visibile per la funzionalità di Forms2 e contiene funzioni per la creazione, il caricamento e il recupero di oggetti Form.

## Metodi MktoForms2

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Metodo</strong></td>
      <td><strong>Descrizione</strong></td>
      <td><strong>Parametri</strong></td>
      <td><strong>Restituisce</strong></td>
    </tr>
    <tr valign="top">
      <td>.loadForm(baseUrl, munchkinId, formId, callback)</td>
      <td>Carica un descrittore di modulo dai server Marketo e crea un nuovo oggetto Form.</td>
      <td> baseUrl(String) - URL dell’istanza del server Marketo per la sottoscrizione</td>
      <td>non definito</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>munchkinId (String) - ID Munchkin dell’abbonamento</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>formId (String o Number): l’ID versione modulo (Vid) del modulo da caricare</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (facoltativo) (funzione): funzione di callback a cui passare l'oggetto Form costruito dopo averlo caricato e inizializzato.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.lightbox(form, opts)</td>
      <td>Esegue il rendering di una finestra di dialogo modale in stile lightbox con l'oggetto Form al suo interno.</td>
      <td>form (oggetto Form) - Istanza di un oggetto Form di cui si desidera eseguire il rendering in un lightbox.</td>
      <td>Oggetto lightbox con i metodi .show() e .hide().</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (optional)(Object) - Oggetto di opzioni passato all'oggetto lightbox</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Function) - Callback attivato quando il modulo viene inviato.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn(Boolean) default true - Controlla se nella finestra di dialogo Lightbox viene visualizzato un pulsante di chiusura (X).</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, callback)</td>
      <td>Crea un nuovo oggetto Form da un oggetto JS del descrittore del modulo. Aggiunge una funzione di callback chiamata dopo il recupero di tutti i fogli di stile e delle informazioni sul lead noto e la creazione dell'oggetto Form.</td>
      <td>formData (oggetto descrittore modulo): un oggetto descrittore del modulo, come creato dall’editor di Marketo Forms V2</td>
      <td>non definito</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (facoltativo)(Function) - Questo callback viene chiamato con un singolo argomento, un'istanza appena creata dell'oggetto Form.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Ottiene un oggetto Form creato in precedenza tramite l'identificatore di modulo</td>
      <td> formId (numero o stringa) - Identificatore ID modulo.</td>
      <td>Oggetto modulo</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Recupera un array di tutti gli oggetti modulo creati in precedenza nella pagina.</td>
      <td>n/d</td>
      <td>Array di oggetto modulo</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Ottiene un oggetto JS contenente i dati dell'URL e del referente che possono essere interessanti a scopo di tracciamento.</td>
      <td>n/d</td>
      <td>Oggetto</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(callback)</td>
      <td>Aggiunge un callback che viene chiamato esattamente una volta per ogni modulo sulla pagina che diventa "pronto". La preparazione significa che il modulo esiste, è stato inizialmente renderizzato e i suoi callback iniziali sono stati chiamati. Se esiste già un modulo pronto al momento della chiamata di questa funzione, il callback passato viene chiamato immediatamente.</td>
      <td>callback(Function) - Al callback viene passato un singolo argomento, un oggetto form.</td>
      <td>Oggetto MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Aggiunge un callback chiamato ogni volta che viene eseguito il rendering di un modulo della pagina. Viene eseguito il rendering di Forms al momento della creazione iniziale, quindi ogni volta che le regole di visibilità modificano la struttura del modulo.</td>
      <td>callback (Function) - Al callback viene passato un singolo argomento, l'oggetto form del form di cui è stato eseguito il rendering.</td>
      <td>Oggetto MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(callback)</td>
      <td>Analogamente a onFormRender, questo comando aggiunge un callback che viene chiamato ogni volta che viene eseguito il rendering di un modulo. Inoltre, richiama immediatamente il callback per tutti i moduli di cui è già stato eseguito il rendering.</td>
      <td>callback(Function) - Al callback viene passato un singolo argomento, l'oggetto form del form di cui è stato eseguito il rendering.</td>
      <td></td>
    </tr>
</table>

## Metodi modulo

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Metodo</strong></td>
      <td><strong>Descrizione</strong></td>
      <td><strong>Parametri</strong></td>
      <td><strong>Restituisce</strong></td>
    </tr>
    <tr valign="top">
      <td>.render(formElem)</td>
      <td>Esegue il rendering di un oggetto modulo, restituendo un oggetto jQuery che racchiude un elemento modulo che contiene il modulo. Se viene passato un formElem, questo verrà utilizzato come elemento del form, altrimenti ne verrà creato uno nuovo.</td>
      <td>formElem (facoltativo): elemento form racchiuso in un oggetto jQuery in cui eseguire il rendering.</td>
      <td> Elemento form con wrapping di oggetti jQuery contenente il form sottoposto a rendering.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Ottiene l'ID del modulo.</td>
      <td>n/d</td>
      <td>Numero: l’ID dell’oggetto modulo rappresentato da questo modulo</td>
    </tr>
    <tr valign="top">
      <td>.getFormElem()</td>
      <td>Ottiene l'elemento form con wrapping jQuery di un form di cui è stato eseguito il rendering.</td>
      <td>n/d</td>
      <td>Elemento form con wrapping di oggetti jQuery oppure null se il form non è ancora stato sottoposto a rendering con il metodo render().</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Forza la convalida del modulo, evidenziando eventuali errori e restituendo il risultato. Non invia il modulo.</td>
      <td>n/d</td>
      <td>Booleano: restituisce true se tutte le convalide nel modulo sono state passate, in caso contrario restituisce false.</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(callback)</td>
      <td>Aggiunge un callback di convalida che verrà chiamato ogni volta che viene attivata la convalida.</td>
      <td>callback(Function): callback che verrà attivato ogni volta che si verifica la convalida. Al callback verrà trasmesso un parametro, un valore booleano che indica se la convalida è riuscita.</td>
      <td>Oggetto Form: lo stesso oggetto form in cui è stato chiamato il metodo, a scopo di concatenamento.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>Attiva l'evento di invio del modulo. In questo modo si avvia il flusso di invio da, si esegue la convalida, si attivano eventuali eventi onSubmit, si invia il modulo e si attivano eventuali eventi onSuccess se l’invio del modulo ha avuto esito positivo.</td>
      <td>n/d</td>
      <td>Oggetto Form: lo stesso oggetto form in cui è stato chiamato il metodo, a scopo di concatenamento.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Aggiunge un callback che verrà chiamato quando il modulo viene inviato. Viene attivato all’inizio dell’invio, prima che sia noto l’esito positivo o negativo della richiesta.</td>
      <td>callback: funzione che verrà chiamata quando il modulo verrà inviato. Questo callback riceverà un argomento, questo oggetto Form.</td>
      <td>Oggetto Form: lo stesso oggetto form in cui è stato chiamato il metodo, a scopo di concatenamento.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Aggiunge un callback che verrà chiamato quando il modulo verrà inviato correttamente ma prima che il lead venga inoltrato alla pagina di follow-up. Può essere utilizzato per impedire che il lead venga inoltrato alla pagina di follow-up dopo l’invio corretto.</td>
      <td>callback: funzione che verrà chiamata quando il modulo verrà inviato correttamente. A questo callback verranno passati due argomenti. Un oggetto JS contenente i valori inviati e un URL String della pagina di follow-up a cui l’utente verrà inoltrato, oppure una stringa nulla o vuota se non è presente una pagina di follow-up configurata. Comportamento speciale: se questo callback restituisce "false" (misurato con ===), il visitatore NON verrà inoltrato alla pagina di follow-up e la pagina NON verrà ricaricata. Questo consente all’implementatore di eseguire un’elaborazione aggiuntiva all’URL di follow-up o di intervenire sulla pagina utilizzando JavaScript invece di uscire dalla pagina.</td>
      <td>Oggetto Form: lo stesso oggetto form in cui è stato chiamato il metodo, a scopo di concatenamento.</td>
    </tr>
    <tr valign="top">
      <td>.submittable(canSubmit) <em>disponibile anche come:</em> <em>.submitable(canSubmit)</em></td>
      <td>Ottiene o imposta se è possibile inviare il modulo. Se chiamato senza argomenti, ottiene il valore, se chiamato con un argomento imposta il valore. Questo può essere utilizzato per impedire l'invio di una maschera mentre devono essere soddisfatti altri criteri al di fuori della maschera normale.</td>
      <td>canSubmit (facoltativo)(booleano) - Imposta il modulo in modo che sia inviabile o non inviabile.</td>
      <td>Booleano o oggetto modulo: se chiamato senza argomenti, restituisce un valore booleano che indica se il modulo è inviabile. Se chiamato con un argomento, restituisce questo oggetto Form a scopo di concatenamento. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFilled()</td>
      <td>Restituisce true se per tutti i campi del modulo sono impostati valori non vuoti.</td>
      <td>n/d</td>
      <td>Booleano: True se tutti i campi hanno valori non vuoti/vuoti/non impostati/nulli, in caso contrario false.</td>
    </tr>
    <tr valign="top">
      <td>.setValues(vals)</td>
      <td>Imposta i valori in uno o più campi del modulo.</td>
      <td>vals - Un oggetto JS. Per ogni coppia chiave/valore nell’oggetto, il campo modulo denominato chiave verrà impostato su valore.</td>
      <td>non definito</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Ottiene tutti i valori di tutti i campi del modulo.</td>
      <td>n/d</td>
      <td>Oggetto: oggetto JS contenente coppie chiave/valore che rappresentano i nomi e i valori dei campi nel modulo.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(values)</td>
      <td>Aggiunge al modulo il valore type=hidden fields.</td>
      <td>values - Oggetto JS contenente coppie chiave/valore che rappresentano i nomi e i valori dei campi nascosti da aggiungere al modulo.</td>
      <td>non definito</td>
    </tr>
    <tr valign="top">
      <td>.vals(values)</td>
      <td>Imposta/getter stile jQuery .vals(). Se viene chiamato senza argomenti, equivale a chiamare getValues(). Se viene chiamato con un argomento, equivale a chiamare setValues()</td>
      <td>Valori (facoltativi) - Oggetto</td>
      <td>non definito</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, elem)</td>
      <td>Visualizza un messaggio di errore che indica l'elemento.</td>
      <td>msg (String of HTML) - Stringa contenente il testo dell’errore che si desidera visualizzare.</td>
            <td>Oggetto modulo: questo oggetto modulo, per il concatenamento.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>elem (facoltativo)(jQuery Object): l’elemento a cui deve puntare l’errore. Se non viene impostato, viene utilizzato il pulsante di invio del modulo.</td>
<td></td>
    </tr>
  </tbody>
</table>
