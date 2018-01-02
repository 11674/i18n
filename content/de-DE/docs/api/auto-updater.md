# automatischerUpdater

> Aktivieren Sie Apps, um sich automatisch zu aktualisieren.

Prozess: [Haupt](../glossary.md#main-process)

Das ` autoUpdater </ 0> -Modul bietet eine Schnittstelle für das
 <a href="https://github.com/Squirrel"> Squirrel </ 1> -Framewor</p>

<p>Sie können schnell einen Multi-Plattform-Release-Server zum Verteilen Ihrer Anwendung mithilfe eines dieser Projekte starten:</p>

<ul>
<li><a href="https://github.com/GitbookIO/nuts"> Nüsse </ 0> : <em> Ein intelligenter Freigabeserver für Ihre Anwendungen, der GitHub als Backend verwendet. Automatische Updates mit Squirrel (Mac & amp;  Windows ) </ 1></li>
<li><a href="https://github.com/ArekSredzki/electron-release-server"> electron -release-server </ 0> : <em> Ein voll ausgestatteter, selbst gehosteter Release-Server für elektronische Anwendungen, kompatibel mit Auto-Updater </ 1></li>
<li><a href="https://github.com/Aluxian/squirrel-updates-server"> squirrel-updates-server </ 0> : <em> Ein einfacher node.js-Server für Squirrel.Mac und Squirrel.Windows, der GitHub-Releases verwendet </ 1></li>
<li><a href="https://github.com/Arcath/squirrel-release-server"> squirrel-release-server </ 0> : <em> Eine einfache PHP-Anwendung für Squirrel.Windows, die Updates von einem Ordner liest. Unterstützt Delta-Updates. </ 0></li>
</ul>

<h2>Plattformhinweise</h2>

<p>Obwohl <code> autoUpdater </ 0> eine einheitliche API für verschiedene Plattformen bietet, gibt es auf jeder Plattform noch einige feine Unterschiede.</p>

<h3>macOS</h3>

<p>macOS , die <code> AutoUpdater </ 0> Modul wird auf gebaut <a href="https://github.com/Squirrel/Squirrel.Mac"> Squirrel.Mac </ 1> , Sie keine spezifische Softwareinstallation erforderlich bedeutet es funktioniert. Für serverseitige Anforderungen können Sie <a href="https://github.com/Squirrel/Squirrel.Mac#server-support"> Serverunterstützung </ 0> lesen . Beachten Sie, dass <a href="https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35"> App-Transportsicherheit </ 0> (ATS) für alle Anforderungen gilt, die im Rahmen des Aktualisierungsprozesses vorgenommen werden. Apps, die ATS deaktivieren müssen, können den Schlüssel <code> NSAllowsArbitraryLoads </ 0> zu ihrer App hinzufügen
 .</p>

<p><strong> Hinweis: </ 0> Ihre Anwendung muss für automatische Updates auf macOS signiert sein . Dies ist eine Voraussetzung von <code> Squirrel.Mac </ 1> .</p>

<h3>Windows</h3>

<p>Unter Windows müssen Sie Ihre App auf dem Computer eines Benutzers installieren, bevor Sie den <code> autoUpdater </ 0> verwenden können. Daher wird empfohlen, das
 <a href="https://github.com/electron/windows-installer"> Elektron-winstaller </ 1> , <a href="https://github.com/electron-userland/electron-forge"> Elektron zu verwenden -forge </ 2> oder das <a href="https://github.com/electron/grunt-electron-installer"> grunt-electron-installer </ 3> -Paket, um ein Windows- Installationsprogramm zu generieren .</p>

<p>Wenn Sie <a href="https://github.com/electron/windows-installer"> electron-winstaller </ 0> oder <a href="https://github.com/electron-userland/electron-forge"> electron-forge </ 1> verwenden , versuchen Sie nicht, Ihre App <a href="https://github.com/electron/windows-installer#handling-squirrel-events"> beim ersten Mal zu aktualisieren </ 2> (Siehe auch <3 > dieses Problem für weitere Informationen </ 3> ).
 Es wird auch empfohlen, <a href="https://github.com/mongodb-js/electron-squirrel-startup"> electron-squirrel-startup </ 0> zu verwenden, um Desktop-Verknüpfungen für Ihre App zu erhalten.</p>

<p>Das mit Squirrel erstellte Installationsprogramm erstellt ein Verknüpfungssymbol mit einer
 <a href="https://msdn.microsoft.com/en-us/library/windows/desktop/dd378459(v=vs.85).aspx"> Anwendungsbenutzermodell-ID </ 0> im Format
 <code> com.squirrel.PACKAGE_ID.YOUR_EXE_WITHOUT_DOT_EXE </ 1> , Beispiele sind
 <code> com.squirrel .slack.Schwarz </ 1> und <code> com.squirrel.code.Code </ 1> . Sie müssen dieselbe ID für Ihre App mit der <code> app.setAppUserModelId </ 0>  API verwenden , da Windows sonst Ihre App nicht ordnungsgemäß in der Taskleiste anheften kann .</p>

<p>Im Gegensatz zu Squirrel.Mac kann Windows Updates auf S3 oder einem anderen statischen Dateihost hosten.
Sie können die Dokumente von <a href="https://github.com/Squirrel/Squirrel.Windows"> Squirrel.Windows </ 0> lesen , um mehr über die Funktionsweise von Squirrel.Windows zu erfahren.</p>

<h3>Linux</h3>

<p>Es gibt keine integrierte Unterstützung für automatische Aktualisierung unter Linux. Daher wird empfohlen, den Paketmanager der Distribution zu verwenden, um Ihre App zu aktualisieren.</p>

<h2>Veranstaltungen</h2>

<p>Das <code> autoUpdater </ 0> -Objekt gibt die folgenden Ereignisse aus:</p>

<h3>Ereignis : "Fehler</h3>

<p>Kehrt zurück:</p>

<ul>
<li><code> Fehler </ 0> Fehler</li>
</ul>

<p>Wird gesendet, wenn beim Aktualisieren ein Fehler auftritt.</p>

<h3>Ereignis : "Nach Updates suchen"</h3>

<p>Wird gesendet, wenn geprüft wird, ob ein Update gestartet wurde.</p>

<h3>Ereignis : 'Update-verfügbar'</h3>

<p>Wird gesendet, wenn ein Update verfügbar ist. Das Update wird automatisch heruntergeladen.</p>

<h3>Ereignis : "Update nicht verfügbar"</h3>

<p>Wird gesendet, wenn kein Update verfügbar ist.</p>

<h3>Ereignis : 'Update-Download'</h3>

<p>Kehrt zurück:</p>

<ul>
<li><code> Ereignis </ 0>  Ereignis</li>
<li><code> Release Notes </ 0>  String</li>
<li><code> releaseName </ 0>  String</li>
<li><code> releaseDate </ 0> Datum</li>
<li><code> updateURL </ 0>  String</li>
</ul>

<p>Wird gesendet, wenn ein Update heruntergeladen wurde.</p>

<p>Unter Windows ist nur <code> releaseName </ 0> verfügbar.</p>

<h2>Methoden</h2>

<p>Das Objekt <code> autoUpdater </ 0> verfügt über die folgenden Methoden:</p>

<h3><code>autoUpdater.setFeedURL (url [, requestHeaders])`</h3> 

* `url` String
* `requestHeaders` Object *macOS* (optional) - HTTP request headers.

Sets the `url` and initialize the auto updater.

### `autoUpdater.getFeedURL()`

Returns `String` - The current update feed URL.

### `autoUpdater.checkForUpdates()`

Asks the server whether there is an update. You must call `setFeedURL` before using this API.

### `autoUpdater.quitAndInstall()`

Restarts the app and installs the update after it has been downloaded. It should only be called after `update-downloaded` has been emitted.

**Note:** `autoUpdater.quitAndInstall()` will close all application windows first and only emit `before-quit` event on `app` after that. This is different from the normal quit event sequence.