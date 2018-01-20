# autoUpdater

> Permite que las aplicaciones que se actualicen automáticamente.

Proceso: [Principal](../glossary.md#main-process)

El modulo `autoUpdater` provee una interfaz para el framework [Squirrel](https://github.com/Squirrel).

Puedes iniciar rápidamente un servidor multiplataforma para distribuir tu aplicación utilizando uno de estos proyectos:

* [nuts](https://github.com/GitbookIO/nuts): * Un servidor de lanzamiento inteligente para tus aplicaciones, usando GitHub como Backend. Se auto actualiza con Squirrel(Mac & Windows)*
* [electron-release-server](https://github.com/ArekSredzki/electron-release-server): * Un destacado destacado servidor de lanzamiento alojado para aplicaciones en electron, compatible con el auto actualizador*
* [squirrel-updates-server](https://github.com/Aluxian/squirrel-updates-server): * Un simple servidor en node.js para Squirrel.Mac y Squirrel.Windows el cual usa GitHub releases*
* [squirrel-release-server](https://github.com/Arcath/squirrel-release-server): *Una simple aplicacion PHP para Squirrel.Windows la cual lee las actualizaciones desde una carpeta. Admite las actualizaciones delta.*

## Noticias de plataforma

A pesar que `autoUpdater` provee una API uniforme para diferentes plataformas, aun algunas diferencias sutiles en cada plataforma.

### macOS

En macOS el modulo `autoUpdater` esta construido sobre [Squirrel.Mac](https://github.com/Squirrel/Squirrel.Mac) significando que no tienes que hacer alguna configuración especial para que funcione. Para los requerimientos del lado del servidor, puedes leer [Soporte de Servidor](https://github.com/Squirrel/Squirrel.Mac#server-support). Tenga en cuenta que [App Transport Security](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) (ATS) se aplica para todas las solicitudes creadas como parte del proceso de actualizaciones. Aplicaciones que necesitan para desactivar ATS pueden agregar la clave de `NSAllowsArbitraryLoads` a su .plist de su aplicación.

**Nota:** Su aplicación debe ser firmada para actualizaciones automáticas en macOS. Este es un requisito de `Squirrel.Mac`.

### Windows

On Windows, you have to install your app into a user's machine before you can use the `autoUpdater`, so it is recommended that you use the [electron-winstaller](https://github.com/electron/windows-installer), [electron-forge](https://github.com/electron-userland/electron-forge) or the [grunt-electron-installer](https://github.com/electron/grunt-electron-installer) package to generate a Windows installer.

Cuando usas [electron-winstraller](https://github.com/electron/windows-installer) o [electron-forge](https://github.com/electron-userland/electron-forge) no intentes actualizar tu aplicación [lo primero de correr](https://github.com/electron/windows-installer#handling-squirrel-events) (También ver [este tema para obtener más información](https://github.com/electron/electron/issues/7155)). se recomienda usar electron-squirrel-startup<0> para obtener acceso directo en el escritorio para su aplicación.</p> 

El instalador generado con [Squirrel](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378459(v=vs.85).aspx) se creara un acceso directo a un icono con una `Application User Model ID` en el formato igual a este ` com.squirrel.PACKAGE_ID.YOUR_EXE_WITHOUT_DOT_EXE` ejemplos: `com.squirrel.slack.Slack` y <1>com.squirrel.code.Code</1> Tu debes usar el mismo ID de tu aplicación con `app.setAppUserModelId` API, de lo contrario Windows no podría ejecutarlo correctamente en la barra de tareas.

A diferencia de Squirrel, Mac OS, Windows puede recibir actualizaciones sobre S3 o cualquier otro lhst de archivos estático, puedes leer la documentación de [Squirrel.Windows](https://github.com/Squirrel/Squirrel.Windows) para obtener más detalles sobre el funcionamiento de esto.

### Linux

Esto no esta construido para soportar auto-updater sobre Linux, es recomendable que hagas uso del administrador de paquete de tu distribución para tu app.

## Eventos

El objeto `app` emite los siguientes eventos:

### Evento: 'error'

Devuelve:

* `error` Error

Aparece cuando hay un error al actualizar.

### Evento: "comprobar si hay actualizaciones"

Aparece al comprobar si una actualización ya ha empezado.

### Evento: "actualización disponible"

Aparece cuando hay una actualización disponible. La actualización se descargará automáticamente.

### Evento: 'update-not-available'

Aparece cuando no hay una actualización disponible.

### Evento: "actualización descargada"

Devuelve:

* `event` Evento
* `releaseNotes` String
* `releaseName` String
* `releaseDate` Fecha
* `updateURL` String

Aparece cuando se ha descargado una actualización.

Solo esta disponible en Windows `releaseName`.

## Métodos

El objeto `autoUpdater` tiene los siguientes métodos:

### `autoUpdater.setFeedURL(url[, requestHeaders])`

* `url` String
* `requestHeaders` Object *macOS* (optional) - HTTP request headers.

Configura el `url` e inicializa la actualización automática.

### `autoUpdater.getFeedURL()`

Devuelve `String` - La actualización actual provee el URL.

### `autoUpdater.checkForUpdates()`

Solicita al servidor si hay actualizaciones. Se debe llamar a `setFeedURL` antes de utilizar esta API.

### `autoUpdater.quitAndInstall()`

Reinicia la aplicación e instala la actualización luego de que se haya descargado. Solo se debería llamar luego de que aparezca `update-downloaded`.

**Note:** `autoUpdater.quitAndInstall()` will close all application windows first and only emit `before-quit` event on `app` after that. This is different from the normal quit event sequence.