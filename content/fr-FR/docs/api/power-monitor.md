# powerMonitor

> Surveille les changements d'état de puissance.

Processus : [Main](../glossary.md#main-process)

This module cannot be used until the `ready` event of the `app` module is emitted.

Par exemple :

```javascript
const { app, powerMonitor } = require('electron')

app.on('ready', () => {
  powerMonitor.on('suspend', () => {
    console.log('The system is going to sleep')
  })
})
```

## Événements

Le module `powerMonitor` émet les événements suivants :

### Événement : 'suspend'

Émis lorsque le système est suspendu.

### Événement : 'resume'

Émis lorsque le système reprend.

### Événement : 'on-ac' *Windows*

Émis lorsque le système est branché sur prise.

### Événement : 'on-battery' *Windows*

Émis lorsque le système passe sur batterie.

### Event: 'shutdown' *Linux* *macOS*

Emitted when the system is about to reboot or shut down. If the event handler invokes `e.preventDefault()`, Electron will attempt to delay system shutdown in order for the app to exit cleanly. If `e.preventDefault()` is called, the app should exit as soon as possible by calling something like `app.quit()`.

### Event: 'lock-screen' *macOS* *Windows*

Emitted when the system is about to lock the screen.

### Event: 'unlock-screen' *macOS* *Windows*

Emitted as soon as the systems screen is unlocked.

## Méthodes

The `powerMonitor` module has the following methods:

### `powerMonitor.getSystemIdleState(idleThreshold)`

* `idleThreshold` Integer

Returns `String` - The system's current state. Can be `active`, `idle`, `locked` or `unknown`.

Calculate the system idle state. `idleThreshold` is the amount of time (in seconds) before considered idle. `locked` is available on supported systems only.

### `powerMonitor.getSystemIdleTime()`

Returns `Integer` - Idle time in seconds

Calculate system idle time in seconds.