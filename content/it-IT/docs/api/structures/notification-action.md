# Oggetto AzioneNotifica

* `tipo` Stringa - Il tipo di azione, può essere `pulsante`.
* `testo` Stringa - (opzionale) L'etichetta per l'azione data.

## Supporto Piattaforma / Azione

| Tipo di Azione | Supporto Piattaforma | Uso del `testo`                      | `testo` predefinito | Limitazioni                                                                                                                                                           |
| -------------- | -------------------- | ------------------------------------ | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `pulsante`     | macOS                | Usato come etichetta per il pulsante | "Mostra"            | Massimo di un pulsante, se ne sono forniti molti è usato solo l'ultimo. Questa azione è anche incompatibile con `haRisposto` e sarà ignorata se `haRisposto` è`true`. |

### Pulsante supportato su macOS

Per far funzionare i pulsanti di extra notificazione su macOS la tua app deve incontrare i seguenti criteri.

* L'App è firmata
* La App ha il suo `NSStileSuoneriaNotificaUtente` impostata su `allerta` nell'`info.plist`.

Se uno dei questi criteri non sono presenti il pulsante semplicemente non apparirà.