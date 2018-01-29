# app

> Controlla il ciclo di vita degli eventi della tua applicazione.

Processo: [Principale](../glossary.md#main-process)

I seguenti esempi mostrano come uscire dall'applicazione quando l'ultima finestra è chiusa:

```javascript
const {app} = richiedi('electron')
app.on('finestra-tutta-chiusa', () => {
 app.esci()
})
```

## Eventi

L'oggetto `app` emette i seguenti eventi:

### Evento: 'finirà-lancio'

Emetti quando l'app ha finito la startup di base. Su Windows e Linux, l'evento `finirà-lancio` equivale all'evento `pronto`; su macOS questo evento rappresenta la notifica `applicazionefiniràlancio` di `NSApplication`. Spesso dovrai impostare ascoltatori per gli eventi `apri-file` e `apri-url` ed avviare il reporter dei crash e l'aggiornamento automatico.

In gran parte dei casi, dovresti solo fare tutto nel gestore eventi `pronto`.

### Evento: 'pronto'

Restituiti:

* `Infolanciò` Oggetto *macOS*

Emesso quando Electron ha concluso l'inizializzazione. Su macOS `Infolancio` deteiene le `Infouser` della `NSNotificazioneUtente` usata per aprire l'applicazione, se lanciata dal Centro Notifiche. Puoi chiamare `app.isPronta()` per controllare se gli eventi sono già stati generati.

### Evento: 'finestra-tutto-chiuso'

Emesso quando tutte le finestre sono state chiuse.

Se non sottoscrivi a questo evento e tutte le finestre sono chiuse il comportamento predefinito è di uscire dall'app; comunque sr sottoscrivi controlli se l'app deve uscire o no. Se l'utente ha premuto `Cmd+Q` o lo sviluppatore chiamato `app.esci()`, Electron proverà prima a chiudere tutte le finestre ed emettere l'evento `will-quit` e in questo caso l'evento `finestra-tutto-chiuso` non sarà emesso.

### Evento: 'prima-uscire'

Restituiti:

* `evento` Evento

Emesso prima che l'app inizi a chiudere le sue finestre. Chiamare `evento.previeniDefault()` impedirà il comportamento predefinito che sta terminando l'applicazione.

**Note:** Se l'uscita dall'app è avviata da `autoAggiornamento.esciEInstalla()` allora `prima-uscire` è emesso *dopo* aver emesso l'evento `chiuso` su tutte le finestre e chiudendole.

### Evento: 'uscirà'

Restituiti:

* `evento` Evento

Emesso quando tutte le finestre sono state chiuse e l'app uscirà. Chiamando `evento.previeniDefault` impedirà il comportamento predefinito che sta terminando l'app.

Vedi la descrizione dell'evento `finestra-tutto-chiuso` per le differenze tra gli eventi `uscirà` e `finestra-tutto-chiuso`.

### Evento: 'esci'

Restituiti:

* `evento` Evento
* `Codiceuscita` Numero Intero

Emesso quando l'app è in uscita.

### Evento: 'apri-file' *macOS*

Restituiti:

* `evento` Evento
* `percorso` Stringa

Emesso quando l'utente vuole aprire un file con l'app. L'evento `apri-file` è in genere emesso quando l'app è già aperta e l'OS vuole riutilizzare l'app per aprire il file. `apri-file` è anche emesso quando un file è rilasciato nel dock e l'app non è ancora in esecuzione. Assicurati di ascoltare l'evento `apri-file` molto presto all'avvio della tua app per gestire questo cado (anche prima dell'emissione dell'evento `pronto`).

Dovresti chiamare `evento.previeniDefault` se vuoi gestire questo evento.

Su Windows, devi analizzare `process.argv` (nel processo principale) per ottenere il percorso del file.

### Evento: 'apri-url' *macOS*

Restituiti:

* `evento` Evento
* `url` Stringa

Emesso quando l'utente vuole aprire un URL con l'l'applicazione. Il file `Info.plist` della tua applicazione definisce lo schema url compreso della chiave `CFBundleURLTipo` ed imposta la `NSClassePrincipale` ad `AtomApplicazione`.

Dovresti chiamare `evento.previeniDefault` se vuoi gestire questo evento.

### Evento: 'attiva' *macOS*

Restituiti:

* `evento` Evento
* `haFinestreVisibili` Booleano

Emesso quando l'app è attivata. Varie azioni possono generare questo evento, come il lancio dell'app per la prima volta, provare a rilanciare l'app quando già aperta o cliccare sul dock dell'applicazione o sull'icona della taskbar.

### Evento: 'continua-attività' *macOS*

Restituiti:

* `evento` Evento
* `tipo` Stringa - Una stringa che identifica l'l'attività. Mappa a [`NSUtenteAttività.attivitàTipo`](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSUserActivity_Class/index.html#//apple_ref/occ/instp/NSUserActivity/activityType).
* `utenteInfo` Oggetto - Contiene stati app specifici immagazzinati per attività su un altro dispositivo.

Emesso durante [Handoff](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html) quando un'attività da un altro dispositivo vuole essere ripristinata. Se vuoi gestire questo evento dovresti chiamare l'`evento.previeniDefault()`.

Un'attività dell'utente può essere continuata solo in un app con lo stesso Team ID dello sviluppatore come fonte app dell'attività e che supporti il tipo di attività. I tipi di attività supportati sono specificati nell'`Info.plist` dell'app sotto la chiave `NSTipoAttivitàUtente`.

### Evento: 'nuova-finestra-per-scheda' *macOS*

Restituiti:

* `evento` Evento

Emesso quando l'utente clicca il pulsante macOS nativo nuova scheda. Il pulsante nuova scheda è visibile solo se l'attuale `FinestraBrowser` ha un `Identificatoreschede`

### Evento: 'browser-finestra-sfocatura'

Restituiti:

* `evento` Evento
* `finestra` FinestraBrowser

Emesso quando una [Finestrabrowser](browser-window.md) è sfocata.

### Evento: 'browser-finestra-focalizza'

Restituiti:

* `evento` Evento
* `finestra` FinestraBrowser

Emesso quando una [Finestrabrowser](browser-window.md) è focalizzata.

### Evento: 'broser-finestra-creata'

Restituiti:

* `evento` Evento
* `finestra` FinestraBrowser

Emesso quando una [Finestrabrowser](browser-window.md) è creata.

### Evento: 'web-contenuto-creato'

Restituiti:

* `evento` Evento
* `Contenutiweb` ContenutiWeb

Emesso quando un nuovo [ContenutoWeb](web-contents.md) è creato.

### Evento: 'certificato-errore'

Restituiti:

* `evento` Evento
* `ContenutiWeb` [ContenutiWeb](web-contents.md)
* `url` Stringa
* `errore` Stringa - Il codice d'errore
* `certificato` [Certificato](structures/certificate.md)
* `callback` Funzione 
  * `èVerificato` Booleano - Se considerare il certificato come verificato

Emesso quando fallisce la verifica del`certificato` per`url`, per verificare il certificato puoi prevenire il comportamento predefinito con `evento.previeniDefault()` e chiamare `callback(vero)`.

```javascript
const {app} = richiedi('electron')


app.on ('certificato-errore', (eventi, Contenutiweb, url, errori, certificato, callback) => {
  se (url === 'https://github.com') {
      // Logica di verifica.
    evento.previeniDefault()
    callback(true)
  } altro {
    callback(false)
  }
})
```

### Evento: 'selezione-certificato-client'

Restituiti:

* `evento` Evento
* `ContenutiWeb` [ContenutiWeb](web-contents.md)
* `url` URL
* `Listacertificati` [Certificati[]](structures/certificate.md)
* `callback` Funzione 
  * `certificato` [Certificato](structures/certificate.md) (opzionale)

Emesso quando un certificato client è richiesto.

L'`url` corrisponde alla voce di navigazione richiedente il certificato client e `callback` può essere chiamato con una voce filtrata dalla lista. Usando `evento.previeniDefault()` si previene che l'app usi il primo certificato dal magazzino.

```javascript
const {app} = richiedi('electron')


app.on ('seleziona-certificato-client', evento, Contenutiweb, url, lista, callback) => {
 evento.previeniDefault()
 callback(lista[0])
})
```

### Evento: 'accedi'

Restituiti:

* `evento` Evento
* `ContenutiWeb` [ContenutiWeb](web-contents.md)
* `richiesta` Oggetto 
  * `metodo` Stringa
  * `url` URL
  * `prescrivente` URL
* `infoautore` Oggetto 
  * `èProxy` Booleano
  * `schema` Stringa
  * `ospite` Stringa
  * `porta` Numero Intero
  * `regno` Stringa
* `callback` Funzione 
  * `nomeutente` Stringa
  * `password` Stringa

Emesso quando i `Contenutiweb` vogliono fare un'autenticazione base.

Il comportamento predefinito è di cancellare tutte le autenticazioni, per evitare ciò puoi prevenire il comportamento predefinito con `evento.previeniDefault` e chiamare `callback(nomeutente, password)` con le credenziali.

```javascript
const {app} = richiedi('electron')


app.on('login', evento, Contenutiweb, richiesta, Infoaut, callback) => {
 evento.previeniDefault()
 callback('nomeutente', 'segreto')
})
```

### Evento: 'processi-gpu-crashati'

Restituiti:

* `evento` Evento
* `ucciso` Booleano

Emesso quando i processi gpu crashano o soni uccisi.

### Evento: 'accessibilità-supporto-cambiata' *macOS* *Windows*

Restituiti:

* `evento` Evento
* `SupportoAccessibilitàAbilitato` Booleano - `true` quando il supporto all'accessibilità a Chrome è abilitato, `false` altrimenti.

Emesso quando cambia il supporto accessibilità di Chrome. Questo evento avviene quando le tecnologie d'assistenza, come lettore schermo, sono abilitate o disabilitate. Vedi https://www.chromium.org/developers/design-documents/accessibility per altri dettagli.

## Metodi

L'oggetto `app` ha i seguenti metodi:

**Nota:** Alcuni metodi sono disponibili solo su sistemi operativi specifici e sono etichettati come tali.

### `app.esci()`

Prova a chiudere tutte le finestre. L'evento `esci-prima` sarà emesso prima. Se tutte le finestre sono chiuse con successo, l'evento `uscirà` sarà emesso e di default l'app sarà terminata.

Questo metodo garantisce che tutti i `precaricati` e `caricati` eventi gestionali siano correttamente eseguiti. È possibile che una finestra annulli l'uscita tornando `false` nell'evento gestionale `precaricato`.

### `app.esci([exitCode])`

* `Codiceuscita` Numero Intero (opzionale)

Esci immediatamente con `Codiceuscita`. Il `Codiceuscita` predefinito è 0.

Tutte le finestre saranno immediatamente chiuse senza richiesta all'utente e gli eventi `prima-esci` e `uscirà` non saranno emessi.

### `app.rilancio([options])`

* `opzioni` Oggetto (opzionale) 
  * `arg` Stringa[] - (opzionale)
  * `eseguiPercorso` Stringa (opzionale)

Rilancia l'app quando esiste la corrente istanza.

Di default la nuova istanza userà la stessa directory di lavoro e argomenti della linea di comando con la corrente istanza. Quando l'`arg` è specificato, l'`arg` sarà invece passato come argomento di linea di comando. Quando `eseguiPercorso` è specificato, `eseguiPercorso` sarà eseguito per rilanciare, invece, l'app corrente.

Nota che questo metodo non termina l'app quando eseguito, devi chiamare `app.esci` o `app.uscita` dopo aver chiamato `app.rilancia` per riavviare l'app.

Quando `app.rilancia` è chiamato ripetutamente, le istanze multiple sarannò avviate dopo l'istanza corrente sia uscita.

Un esempio di riavvio dell'istanza corrente immediato e aggiungendo un nuovo argomento della linea di comando alla nuova istanza:

```javascript
const {app} = richiedi('electron')


app.rilancia({args: processo.argv..slice(1).concat(['--rilancia'])})
app.esci(0)
```

### `app.isPronta()`

Restituisce `Booleano` - `true` se Electron ha finito l'inizializzazione, `falso` viceversa.

### `app.focalizza()`

Su Linux, focalizza sulla prima finestra visibile. Su macOS rende l'applicazione attiva. Su Windows, focalizza sulla prima finestra dell'applicazione.

### `app.nascondi()` *macOS*

Nasconde tutte le finestre dell'applicazione senza minimizzarle.

### `app.mostra()` *macOS*

Mostra le finestre dell'applicazione dopo che sono state nascoste. Non le focalizza automaticamente.

### `app.ottieniAppPercorso()`

Restituisce `Stringa` - La directory dell'app corrente.

### `app.ottieniPercorso(nome)`

* `nome` Stringa

Restituisce `Stringa` - Un percorso ad una directory speciale o ai file associati con `nome`. In caso di fallimento avviene un `Errore`.

Puoi richiedere i seguenti percorsi dal nome:

* `home` Directory della home utente.
* `appData` Dati della directory dell'app utente, con punti predefiniti a: 
  * `%APPDATA%` su Windows
  * `$XDG_CONFIG_HOME` o `~/.config` su Linux
  * `~/Libraria/Supporto Applicazione` su macOS
* `Datiutente` La directory per ammagazzinare i file di configurazione della tua app, che per valore predefinito è la directory `Datiapp` seguita dal nome della tua app.
* `temp` Directory temporanea.
* `exe` L'attuale file eseguibile.
* `modulo` La libreria `libchromiumcontent`.
* `desktop` L'attuale directory del desktop utente.
* `documenti` La directory per l'utente "I miei Documenti".
* `Scaricati` La directory per i file scaricati dall'utente.
* `musica` La directory per la musica dell'utente.
* `immagini` La directory per le immagini dell'utente.
* `video` La directory per i video dell'utente.
* `pepperFlashSystemPlugin` Percorso intero alla versione di sistema del plugin Pepper Flash.

### `app.ottieniIconaFile(percorso[, opxioni], callback)`

* `percorso` Stringa
* `opzioni` Oggetto (opzionale) 
  * `dimensioni` Stringa 
    * `piccola` - 16x16
    * `normale` - 32x32
    * `grande - 48x48 su <em>Linux</em>, 32x32 su <em>Windows</em>, non supportato su <em>macOS</em>.</li>
</ul></li>
</ul></li>
<li><code>callback` Funzione 
      * `errore` Errore
      * `icona` [ImmagineNativa](native-image.md)
    
    Recupera un'icona associata al percorso.
    
    Su *Windows* esistono 2 tipi di icone:
    
    * Icone associate con certe estensioni di file come `.mp3`, `.png`, etc.
    * Icone interne allo stesso file come `.exe`, `.dll`, `.ico`.
    
    Su *Linux* e *macOS* le icone dipendono dall'app associata con il tipo di file mimo.
    
    ### `app.impostaPercorso(nome, percorso)`
    
    * `nome` Stringa
    * `percorso` Stringa
    
    Sostituisce il `percorso` ad una directory speciale o ad un file associato con `nome`. Se il percorso specifica una directory che non esiste, la directory sarà creata da questo metodo. In caso di fallimento viene generato un `Errore`.
    
    Si possono sostituire solo i percorsi di un `nome` definiti in `app.ottieniPercorso`.
    
    Di default, i cookie e la cache delle pagine web saranno immagazzinate sotto la directory `Datiutente`. Se vuoi cambiare questa posizione devi sostituire al percorso `Datiutente` prima che l'evento `pronto` del modulo `app` venga emesso.
    
    ### `app.ottieniVersione()`
    
    Restituisce `Stringa` - La versione dell'app caricata. Se non viene trovata nessuna versione nel file dell'app `pacchetto-json`, la versione dell'attuale pacchetto o eseguibile è restituita.
    
    ### `app.ottieniNome()`
    
    Restituisce `Stringa`. Il nome attuale dell'app, che è il nome nel file dell'app `package.json`.
    
    Spesso il campo `nome` del `package.json` è un breve nome in minuscolo, in bae alla specifica dei moduli npm-. Di solito si dovrebbe anche specificare un campo `NomeProdotto`, che è il nome in maiuscolo della tua applicazione, e che sarà preferito al `nome` da Electron.
    
    ### `app.impostaNome(nome)`
    
    * `nome` Stringa
    
    Sostituisce l'attuale nome dell'app.
    
    ### `app.ottieniLocale()`
    
    Restituisce `Stringa` - L'attuale locale dell'app. Possibili valori restituiti sono documentati [qui](locales.md).
    
    **Note:** Quando distribuisci il tuo pacchetto app, devi anche navigare nelle cartelle `locali`.
    
    **Note:** Su Windows devi chiamarlo dopo che l'evento `pronto` è emesso.
    
    ### `app.aggoimgoRecenteDocumento(percorso)` *macOS* *Windows*
    
    * `percorso` Stringa
    
    Aggiungi `percorso` alla lista documenti recenti.
    
    Questa lista è gestita dall'OS. Su Windows puoi visitare la lista dalla taskbar e su macOS la puoi visitare dal menu dock.
    
    ### `app,pulisciRecentiDocumenti` *macOS* *Windows*
    
    Pulisce la lista documenti recenti.
    
    ### `app.impostacomeProtocolloClientDefault(protocol[, percorso,args])` *macOS* *Windows*
    
    * `protocollo` Stringa - Il nome del tuo protocollo, senza `://`. Se vuoi che la tua app gestisca i link `electron://` chiama questo metodo con `electron` come parametro.
    * `percorso` Stringa (opzionale) *Windows* - Di default a `process.eseguiPercorso`
    * `arg` Stringa[] (opzionale) *Windows* - Di default ad un insieme vuoto
    
    Restituisce `Booleano` - Se la chiamata ha avuto successo.
    
    Questo metodo imposta l'attuale eseguibile come gestionale di default per un protocollo (a. k. a. schema URI). Ti permette di integrare la tua app in profondità nel sistema operativo. Una volta registrati, tutti i link con `your-protocol://` saranno aperti con l'attuale eseguibile. L'intero link, incluso il protocollo, sarà passato all'app come parametro.
    
    Su Windows puoi fornire parametri di percorso opzionali, il percorso al tuo eseguibile e gli argomenti, un insieme di argomenti da passare al tuo eseguibile quando si lancia.
    
    **Nota:** Su macOS, puoi solo registrare protocolli aggiunti alla tua app `info.plist`, che non può essere modificato in esecuzione. Puoi comunque cambiare il file con un semplice editore di testo o script durante il momento di costruzione. Si prega di riferirsi alla [documentazione Apple](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-102207-TPXREF115) per i dettagli.
    
    L'API usa il Registro Windows e LSImpostaGestionaleDefaultPerSchemaURL internamente.
    
    ### `app.rimuoviComeProtocolloClientDefault(protocollo[, percorso, arg])` *macOS* *Windows*
    
    * `protocollo` Stringa - Il nome del tuo protocollo, senza `://`.
    * `percorso` Stringa (opzionale) *Windows* - Di default a `process.eseguiPercorso`
    * `arg` Stringa[] (opzionale) *Windows* - Di default ad un insieme vuoto
    
    Restituisce `Booleano` - Se la chiamata ha avuto successo.
    
    Questo metodo controlla se l'eseguibile attuale è come un gestionale di default per un protocollo (o schema URI). Se sì, rimuoverà l'app come gestionale predefinito.
    
    ### `app.isDefaultClientProtocollo(protocollo[, percorso, arg])` *macOS* *Windows*
    
    * `protocollo` Stringa - Il nome del tuo protocollo, senza `://`.
    * `percorso` Stringa (opzionale) *Windows* - Di default a `process.eseguiPercorso`
    * `arg` Stringa[] (opzionale) *Windows* - Di default ad un insieme vuoto
    
    Restituisci `Booleano`
    
    Questo metodo controlla se l'eseguibile attuale è come un gestionale per un protocollo (o schema URI). Se sì, restituirà true. Altrimenti, restituirà false.
    
    **Nota:** Su macOS puoi usare questo metodo per controllare se l'app è stata registrata come gestionale di protocolli di default per un protocollo. Puoi anche verificarlo controllando `~/Libreria/Preferenze/com.apple.LanciaServizi.plist` su computer macOS. Si prega di riferirsi alla [documentazione Apple](https://developer.apple.com/library/mac/documentation/Carbon/Reference/LaunchServicesReference/#//apple_ref/c/func/LSCopyDefaultHandlerForURLScheme) per i dettagli.
    
    L'API usa il Registro Windows e LSCopiaGestionaleDefaultPerSchemaURL internamente.
    
    ### `app.impostaTaskUtente(task)` *Windows*
    
    * `task` [Task[]](structures/task.md) - Insieme di oggetti `Task`
    
    Aggiungi `task` alla categoria [Task](http://msdn.microsoft.com/en-us/library/windows/desktop/dd378460(v=vs.85).aspx#tasks) della JumpList su Windows.
    
    `task` è un insieme di oggetti [`Task`](structures/task.md).
    
    Restituisce `Booleano` - Se la chiamata ha avuto successo.
    
    **Nota:** Se ti piacerebbe modificare la Jump List ecco altri usi, invece, `app.impostaJumpList(categorie)`.
    
    ### `app.ottieniImpostazioniJumpList` *Windows*
    
    Restituisci `Oggetto`:
    
    * `miniElementi` Numero intero - Il minimo numero di elementi che saranno mostrati nella JumpList (per una più dettagliata descrizione di questo valore vedere [MSDN docs](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378398(v=vs.85).aspx)).
    * `Elementirimossi` [ElementiJumpList[]](structures/jump-list-item.md) - Insieme degli oggetti `ElementiJumpList` che corrisponde agli elementi esplicitamente rimossi dall'utente dalle categorie modificate nella Jump List. Questi elementi non possono essere nuovamente aggiunti alla Jump List alla **prossima** chiamata a `app.impostaJumpList()`, Windows non mostrerà alcuna categoria personalizzata che contenga alcuni valori rimossi.
    ### `app.impostaJumpList(categorie)` *Windows*
    
    * `categorie` [CategoriaJumpList[]](structures/jump-list-category.md) o `nullo` - Insieme di oggetti `CategorieJumpList`.
    
    Imposta o rimuovi una JumpList personalizzata per l'app, e restituisci una delle seguenti strimghe:
    
    * `ok` - Nulla è andato storto.
    * `errore` - Uno o più errori sono avvenuti, abilita il log di esecuzione per mostrare la possibile causa.
    * `ErroreSeparatoreInvalido` - È stato fatto un tentativo di aggiungere un separatore ad una categoria personalizzata nella Jump List. I separatori sono permessi solo nella categoria `Task` standard.
    * `ErroreRegistrazioneTipofile` - È stato fatto un tentativo di aggiungere un link file alla Jump List per un tipo di file non gestibile dall'app.
    * `ErroreAccessoNegatoCategoriapersonalizzata` - Le categorie personalizzate non possono essere aggiunte alla Jump List per motivi di privacy dell'utente o per le impostazioni di privacy di gruppo.
    
    Se le `categorie` sono `nulle` la precedentemente impostata Jump List (se esistente) sarà rimpiazzata dalla Jump List standard per l'app (gestita da Windows).
    
    **Nota:** Se un oggetto `CategoriaJumpList` non ha nè la proprietà `tipo` nè quella `nome` le proprietà si impostano che il `tipo` sia sicuramente `task`. Se la proprietà `nome` è impostata ma la proprietà `tipo` è omessa il `tipo` sarà considerato `personalizzato`.
    
    **Note:** Gli utenti possono rimuovere gli elementi dalle categorie personalizzate, e Windows non permetterà ad un elemento rimosso di essere ri-aggiunto in una categoria personalizzata fino a **dopo** la successiva chiamata di successo a `app.impostaJumpList(categorie)`. Qualsiasi tentativo di aggiunta di un elemento rimosso ad una categoria personalizzata prima che questo risulterà nell'intera categoria personalizzata sarà omesso dalla Jump List. La lista degli elementi rimossi può essere ottenuta usando `app.ottieniImpostazioniJumpList()`.
    
    Questo è un esempio molto semplice di come creare una Jump List personalizzata:
    
    ```javascript
const {app} = richiedi('electron')

app.impostaJumpList([
  {
    tipo: 'personalizzata',
    nome: 'Progetti Recenti',
    elementi: [
      { tipo: 'file', percorso: 'C:\\Progetti\\progetto1.proj' },
      { tipe: 'file', percorso: 'C:\\Progetti\\progetto2.proj' }
    ]
  },
  { // ha un nome quindi 'tipo' è considerato essere "personalizzato"
    nome: 'Strumenti',
    elementi: [
      {
        tipo: 'task',
        titolo: 'Strumento A',
        programma: processo.eseguiPercorso,
        arg: '--esegui-strumento-a',
        icona: processo.eseguiPercorso,
        indiceIcona: 0,
        descrizione: 'Esegui Strumento A'
      },
      {
        tipo: 'task',
        titolo: 'Strumento B',
        programma: processo.eseguiPercorso,
        arg: '--esegiui-strumento-b',
        icona: processo.eseguiPercorso'
        Indiceicona: 0,
        descrizione: 'Esegui Strumento B'
      }
    ]
  },
  { tipo: 'frequente' },
  { // non ha nè nome nè tipo quindi il 'tipo' è considerato essere "task"
    strumenti: [
      {
        tipo: 'task',
        titolo: 'Nuoco Progetto',
        programma: processo.eseguiPercorso,
        arg: '--nuovo-progetto',
        descrizione: 'Crea un nuovo progetto.'
      },
      { tipo: 'separatore' },
      {
        tipo: 'task',
        titolo: 'Recupera Progetto',
        programma: processo.eseguiPercorso,
        arg: '--recupera-progetto',
        descrizione: 'Recupera Progetto'
      }
    ]
  }
])
```

### `app.compiSingolaIstanza(callback)`

* `callback` Funzione 
  * `argv` Stringa[] - Un insieme della linea di comando d'argomento della seconda istanza
  * `Directoryfunzionante` Stringa - La directory funzionante della seconda istanza

Restituisce `Booleano`.

Questo metodo rende la tua app una app a Singola Istanza - invece di permettere multiple istanze della tua app da eseguire, questo assicurerà che solo una singola istanza della tua app sia in esecuzione e che le altre istanze segnino questa ed escano.

`callback` sarà chiamato dalla prima istanza con `callback(argv, Directoryfunzionante` quando una seconda istanza è stata eseguita. `argv` è un insieme delle linee di comando degli argomenti della seconda istanza e la `Directoryfunzionante` è la sua attuale Directory funzionante. Di solito le app rispondono a questo focalizzando la loro finestra primaria e non minimizzata.

Il `callback` è garantito essere eseguito dopo che l'evento `pronto` dell'app è stato emesso.

Questo metodo restituisce `false` se il tuo processo è l'istanza primaria dell'applicazione e la tua app potrebbe continuare a caricare. E restituisce `true` se il tuo processo ha inviato i suoi parametri ad un'altra istanza e dovresti immediatamente uscire.

Su macOS il sistema fa rispettare l'istanza singola automaticamente quando l'utente prova ad aprirne un'altra della vostra app su Finder e per questo sono emessi gli eventi `apri-file` ed `apri-url`. Comunque quando un utente avvia la tua app nella linea di comando il meccanismo della singola istanza del sistema sarà bypassato e devi usare questo metodo per assicurare la singola istanza.

Un esempio dell'attivazione drll'istanza primaria quando se ne avvia una seconda:

```javascript
const {app} = richiedi('electron')
fai miaFinestra = nullo


const èSecondaIstanza =
app.faSingolaIstanza((Lineacomando, Directoryfunzionante) => {
 // Qualcuno ha provato ad eseguire una seconda istanza, dovremmo focalizzare la nostra finestra.
  se (miaFinestra) {
    se (miaFinestra.èMinimizzata()) miaFinestra.ristabilisci()
    miaFinestra.focalizza()
  }
})

se (èSecondaIstanza) {
  app.esci()
}

// Crea miaFinestra, carica il resto dell'app, etc...
app.su('pronto', () => {
})
```

### `app.rilasciaIstanzaSingola()`

Rilascia tutti i blocchi creati da `faIstanzaSingola`. Permetterà alle istanze multiple dell'app di essere eseguite di nuovo al contempo.

### `app.impostaUtenteAttività(tipo, userInfo[, Urlpaginaweb])` *macOS*

* `tipo` Stringa - Unicamente identifica l'attività. Mappa a [`NSUtenteAttività.attivitàTipo`](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSUserActivity_Class/index.html#//apple_ref/occ/instp/NSUserActivity/activityType).
* `userInfo` Oggetto - Stato app specifico al magazzino per usare da altro dispositivo.
* `Urlpaginaweb` Stringa (opzionale) - La pagina web da caricare nel browser se non sono installate app adatte nel dispositivo ripristinante. Lo schema deve essere `http` o `https`.

Crea un'`NSAttivitàUtente` e la imposta come attività corrente. L'attività è eleggibile per [Passarlo](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html) ad un altro dispositivo poi.

### `app.ottieniTipoAttivitàCorrente()` *macOS*

Restituisce `Stringa` - Il tipo di attività al momento in esecuzione.

### `app.impostaModelloIdAppUtente(id)` *Windows*

* `id` Stringa

Cambia il [Modello Id Applicazione Utente](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378459(v=vs.85).aspx) ad `id`.

### `app.importaCertificato(opzioni, callback)` *LINUX*

* `opzioni` Oggetto 
  * `certificato` Stringa - Percorso per il file pkcs12.
  * `password` Stringa - Frase d'accesso per il certificato.
* `callback` Funzione 
  * `risultato` Numero intero - Risultato dell'importo.

Importa il certificato in formato pkcs12 nel magazzino del certificato della piattaforma. `callback` è chiamato con il `risultato` dell'operazione di importazione, un valore di `` indica successo, mentre un altro valore indica un fallimento in base al chromium [net_errore_lista](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h).

### `app.disabilitaAccelerazioneHardware()`

Disabilita l'accelerazione hardware per l'app attuale.

Questo metodo può essere chiamato solo prima che l'app sia pronta.

### `app.disabilitaBloccaggioDominioPerAPI3D()`

Di default, Chromium disabilita le API 3D (come WebGL) fino al riavvio su una base per dominio se i processi GPU crashano troppo spesso. Questa funzione disabilita questo comportamento.

Questo metodo può essere chiamato solo prima che l'app sia pronta.

### `app.ottieniInfoMemoriaApp()` *Deprecato*

Restituisce [`ProcessoMetrico[]`](structures/process-metric.md): Insieme di oggetti `ProcessoMetrico` corrispondenti alle statistiche di utilizzo della memoria e della cpu di tutti i processi associati con l'app. **Nota:** Questo metodo è deprecato, usa invece `app.ottieniMetricheApp()`.

### `app.ottieniMetricheApp()`

Restituisce [`ProcessoMetrico[]`](structures/process-metric.md): Insieme di oggetti `ProcessoMetrico` corrispondenti alle statistiche di utilizzo della memoria e della cpu di tutti i processi associati con l'app.

### `app.ottieniStatoFunzioniGpu()`

Restituisce lo [`StatoFunzioneGPU`](structures/gpu-feature-status.md) - Lo Stato Funzioni Grafiche da `chrome://gpu/`.

### `app.impostaContaBadge(conta)` *Linux* *macOS*

* `conta` Numero Intero

Restituisce `Booleano` - Se la chiamata ha avuto successo.

Imposta il contatore badge per l'app attuale. Impostare il conto a `` nasconderà il badge.

Su macOS esso è mostrato sull'icona del dock. Su Linux lavora sol9 con il Launcher Unity,

**Nota:** Il launcher Unity richiede l'esistenza di un file `.desktop` per funzionare, per ulteriori informazioni leggere [Desktop Integrazione Ambiente](../tutorial/desktop-environment-integration.md#unity-launcher-shortcuts-linux).

### `app.ottieniContaBadge()` *Linux* *macOS*

Restituisce `Intero` - Il valore attuale è mostrato nel contatore di badge.

### `app.èUnityEsecuzione()` *Linux*

Restituisce `Booleano` - Se l'attuale ambiente desktop è il launcher Unity.

### `app.ottieniImpostazioniElementiAccesso([options])` *macOS* *Windows*

* `opzioni` Oggetto (opzionale) 
  * `percorso` Stringa (opzionale) *Windows* - Il percorso eseguibile a comparazione. Di default è `processo.eseguiPercorso`.
  * `arg` Stringa[] (opzionale) *Windows* - La linea di comando degli argomenti comparata. Di default è un insieme vuoto.

Se hai fornito le opzioni di `percorso` e di `arg` a `app.impostaImpostazioniElementiAccedi` dovrai passare gli stessi argomenti qui per `apriAdAccesso` per impostarlo correttamente.

Restituisci `Oggetto`:

* `apriAdAccesso` Booleano - `true` se l'app è impostata a aperta all'accesso.
* `apriComeNascosto` Booleano - `true` se l'app è impostata ad aprirsi come nascosta all'accesso. Impostazione supportata solo su macOS.
* `eraApertoAdAccesso` Booleano - `true` se l'app era aperta automaticamente all'all'accesso. Questa impostazione è supportata solo su macOS.
* `eraApertoComeNascosto` Booleano - `true` se l'app era aperta come un elemento di accesso nascosto. Questo indica che l'app potrebbe non aprire alcuna finestra all'avvio. Questa impostazione è supportata solo su macOS.
* `ripristinaStato` Booleano - `true` se l'app era aperta come elemento d'accesso che potrebbe ripristinare lo stato dalla sessione precedente. Questo indica che l'app potrebbe ripristinare le finestre aperte l'ultima volta che l'app è stata chiusa. Questa impostazione è supportata solo su macOS.

**Nota:** Questa API non ha effetto sulle [Costruzioni MAS](../tutorial/mac-app-store-submission-guide.md).

### `app.impostaImpostazioniElementoAccesso(impostazioni)` *macOS* *Windows*

* `impostazioni` Oggetto 
  * `apriAdAccesso` Booleano (opzionale) - `true` per aprire l'app all'accesso, `false` per rimuovere l'app come elemento di accesso. Di default a `false`.
  * `apriComeNascosto` Booleano (opzionale) - `true` per aprire l'app come nascosta. Di default `false`. L'utente può editare questa impostazione dalle Preferenze di Sistema quindi `app.ottieniStatoAccessoElementi().eraApertoComeNascosto` potrebbe essere controllato quando l'app è aperta per conoscere il valore attuale. Questa impostazione è supportata solo su macOS.
  * `percorso` Stringa (opzionale) *Windows*. L'eseguibile al lancio all'accesso. Di default a `processo.eseguiPercorso`.
  * `arg` Stringa[] (opzionale) *Windows* - La linea di comando dell'argomento per passare all'eseguibile. Di default ad un insieme vuoto. Stai attento ad avvolgere i percorsi in quote.

Imposta le impostazioni dell'elemento d'accesso all'app.

Per lavorare con l'`autoCaricatore` di Electron su Windows, che usa [Squirrel](https://github.com/Squirrel/Squirrel.Windows), vorrai impostare il percorso di lancio ad Update.exe e passare gli argomenti per specificare il nome della tua applicazione. Ad esempio:

```javascript
const Cartellaapp = percorso.dirnome(processo.eseguiPercorso)
const aggiornaExe = percorso.risolvi(Cartellaapp, '..', 'Aggiorna.exe')
const exeNome = percorso.basenome(processo.eseguiPercorso)

app.impostaImpostazioniElementoAccesso({
  apriAdAccssso: true,
  percorso: AggiornaExe,
  arg: [
    '--processoAvvio', `"${exeNome}"`,
    '--processo-avvio-arg', `"--nascosto"`
  ]
})
```

**Nota:** Questa API non ha effetto sulle [Costruzioni MAS](../tutorial/mac-app-store-submission-guide.md).

### `app.isAccessibilitySupportEnabled()` *macOS* *Windows*

Returns `Boolean` - `true` if Chrome's accessibility support is enabled, `false` otherwise. This API will return `true` if the use of assistive technologies, such as screen readers, has been detected. See https://www.chromium.org/developers/design-documents/accessibility for more details.

### `app.setAboutPanelOptions(options)` *macOS*

* `opzioni` Object 
  * `applicationName` String (optional) - The app's name.
  * `applicationVersion` String (optional) - The app's version.
  * `copyright` String (optional) - Copyright information.
  * `credits` String (optional) - Credit information.
  * `version` String (optional) - The app's build version number.

Set the about panel options. This will override the values defined in the app's `.plist` file. See the [Apple docs](https://developer.apple.com/reference/appkit/nsapplication/1428479-orderfrontstandardaboutpanelwith?language=objc) for more details.

### `app.commandLine.appendSwitch(switch[, value])`

* `switch` String - A command-line switch
* `value` String (optional) - A value for the given switch

Append a switch (with optional `value`) to Chromium's command line.

**Note:** This will not affect `process.argv`, and is mainly used by developers to control some low-level Chromium behaviors.

### `app.commandLine.appendArgument(value)`

* `value` String - The argument to append to the command line

Append an argument to Chromium's command line. The argument will be quoted correctly.

**Note:** This will not affect `process.argv`.

### `app.enableMixedSandbox()` *Experimental* *macOS* *Windows*

Enables mixed sandbox mode on the app.

Questo metodo può essere chiamato solo prima che l'app sia pronta.

### `app.dock.bounce([type])` *macOS*

* `type` String (optional) - Can be `critical` or `informational`. The default is `informational`

When `critical` is passed, the dock icon will bounce until either the application becomes active or the request is canceled.

When `informational` is passed, the dock icon will bounce for one second. However, the request remains active until either the application becomes active or the request is canceled.

Returns `Integer` an ID representing the request.

### `app.dock.cancelBounce(id)` *macOS*

* `id` Integer

Cancel the bounce of `id`.

### `app.dock.downloadFinished(filePath)` *macOS*

* `filePath` String

Bounces the Downloads stack if the filePath is inside the Downloads folder.

### `app.dock.setBadge(text)` *macOS*

* `text` String

Sets the string to be displayed in the dock’s badging area.

### `app.dock.getBadge()` *macOS*

Returns `String` - The badge string of the dock.

### `app.dock.hide()` *macOS*

Hides the dock icon.

### `app.dock.show()` *macOS*

Shows the dock icon.

### `app.dock.isVisible()` *macOS*

Returns `Boolean` - Whether the dock icon is visible. The `app.dock.show()` call is asynchronous so this method might not return true immediately after that call.

### `app.dock.setMenu(menu)` *macOS*

* `menu` [Menu](menu.md)

Sets the application's [dock menu](https://developer.apple.com/library/mac/documentation/Carbon/Conceptual/customizing_docktile/concepts/dockconcepts.html#//apple_ref/doc/uid/TP30000986-CH2-TPXREF103).

### `app.dock.setIcon(image)` *macOS*

* `image` ([NativeImage](native-image.md) | String)

Sets the `image` associated with this dock icon.