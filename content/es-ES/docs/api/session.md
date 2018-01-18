# session

> Administra las sesiones del navegador, cookies, cache, configuración del proxy, etc.

Proceso: [Principal](../glossary.md#main-process)

El módulo `sesion` puede ser usado para crear un nuevo objeto `sesion`.

También puede accesar la `sesión` de las páginas existentes usando la propiedad `sesión` de [`WebContents`](web-contents.md), o desde el módulo `sesión`.

```javascript
const {BrowserWindow} = require('electron')

let win = new BrowserWindow({width: 800, height: 600})
win.loadURL('http://github.com')

const ses = win.webContents.session
console.log(ses.getUserAgent())
```

## Métodos

El módulo `sesión` tiene los siguientes métodos:

### `session.fromPartition(partition[, options])`

* `Paritición` Cadena
* `options` Object 
  * `cache` Booleano - En el caso de activar la memoria cache.

Regresa `Sesión` - Una reunión de la cadena `partición`. Cuando hay una `Sesión` existente con la misma `partición`, se devolverá; de otra manera, una nueva instancia `Sesión` será creada con `opciones`.

Si la `partition` comienza con `persistir:`, la página usará una sesión persistente disponible a todas las páginas en la aplicación con la misma `partición`. si no hay un prefijo `persistir:`, la página usará una sesión en memoria. Si la `partición` está vacía entonces la sesión de la aplicación será usada por defecto.

Al crear una `Sesión` con `opciones`, tiene que asegurar la `Sesión` con la `partición` nunca ha sido usada antes. No hay manera de cambiar las `opciones` de un objeto `Sesión` existente.

## Propiedades

El módulo `sesión` tiene las siguientes propiedades:

### `session.defaultSession`

Un objeto `Sesión`, el objeto de sesión de la aplicación por defecto.

## Clase: Sesión

> Obtener y configurar las propiedades de una sesión.

Proceso: [Principal](../glossary.md#main-process)

Puede crear un objeto `Sesión` en el módulo `sesión`:

```javascript
const {session} = require('electron')
const ses = session.fromPartition('persist:name')
console.log(ses.getUserAgent())
```

### Eventos de Instancia

Los siguientes eventos están disponibles en instancias de `Sesión`:

#### Evento: "Se-descargará"

* `evento` Evento
* `item` [DownloadItem](download-item.md)
* `Contenidosweb` [Contenidosweb](web-contents.md)

Emitido cuando Electron está por descargar un `elemento` en `Contenido web`.

Llamando `event.preventDefault()` Se cancelará la descarga y el `elemento` no estará disponible para el siguiente tick del proceso.

```javascript
const {session} = require('electron')
session.defaultSession.on('will-download', (event, item, webContents) => {
  event.preventDefault()
  require('request')(item.getURL(), (data) => {
    require('fs').writeFileSync('/somewhere', data)
  })
})
```

### Métodos de Instancia

Los siguientes métodos están disponibles para instancias de `Sesión`:

#### `ses.getCacheSize(callback)`

* `llamada de vuelta` Función 
  * `tamaño` Entero - Tamaño de memoria caché usada en bites.

Llamar es invocado con el tamaño actual en memoria caché de la sesión.

#### `ses.clearCache(callback)`

* `Llamada` Función - llamada cuando se realiza la operación

Borra la memoria caché del HTTP de la sesión.

#### `ses.clearStorageData([options, callback])`

* `options` Objecto (opcional) 
  * `origen` Cadena - (opcional) Debe seguir la representación de `window.location.origin` `scheme://host:port`.
  * `storages` String[] - (optional) The types of storages to clear, can contain: `appcache`, `cookies`, `filesystem`, `indexdb`, `localstorage`, `shadercache`, `websql`, `serviceworkers`
  * `quotas` Cadena[] - (opcional) El tipo de acciones que borrar, puede contener: `temporary`, `persistent`, `syncable`.
* `Llamada` Función (opcional) - Llamada cuando se ha realizado la operación.

Borra los datos de almacenamiento web.

#### `ses.flushStorageData()`

Escribe cualquier dato DOMStorage que no lo haya sido en disco.

#### `ses.setProxy(config, callback)`

* `configuración` Objecto 
  * `pacScript` Cadena - El URL asociado con el archivo PAC.
  * `proxyRules` Cadena - Reglas indicando cual proxy utilizar.
  * `proxyBypassRules` Cadena - Reglas indicando cuál URL deben eludir la configuración del proxy.
* `Llamada` Funcion - Llamada cuando la operación está completada.

Configurar proxy.

Cuando `pacScript` y `proxyRules` están junto, la opción `proxyRules` es ignorada y le configuración `pacScript` es aplicada.

Las `proxyRules` tienen las siguientes reglas abajo:

    proxyRules = schemeProxies[";"<schemeProxies>]
    schemeProxies = [<urlScheme>"="]<proxyURIList>
    urlScheme = "http" | "https" | "ftp" | "socks"
    proxyURIList = <proxyURL>[","<proxyURIList>]
    proxyURL = [<proxyScheme>"://"]<proxyHost>[":"<proxyPort>]
    

Por ejemplo:

* `http=foopy:80;ftp=foopy2` - Use HTTP proxy `foopy:80` for `http://` URLs, and HTTP proxy `foopy2:80` for `ftp://` URLs.
* `foopy:80` - Use HTTP proxy `foopy:80` for all URLs.
* `foopy:80,bar,direct://` - Use HTTP proxy `foopy:80` for all URLs, failing over to `bar` if `foopy:80` is unavailable, and after that using no proxy.
* `socks4://foopy` - Use SOCKS v4 proxy `foopy:1080` for all URLs.
* `http=foopy,socks5://bar.com` - Use HTTP proxy `foopy` for http URLs, and fail over to the SOCKS5 proxy `bar.com` if `foopy` is unavailable.
* `http=foopy,direct://` - Use HTTP proxy `foopy` for http URLs, and use no proxy if `foopy` is unavailable.
* `http=foopy;socks=foopy2` - Use HTTP proxy `foopy` for http URLs, and use `socks4://foopy2` for all other URLs.

El `proxyBypassRules` es una lista separada por comas de las reglasa que se describen a continuación:

* `[ URL_SCHEME "://" ] HOSTNAME_PATTERN [ ":" <port> ]`
  
  Une todos los nombres que coinciden con el patrón HOSTNAME_PATTERN.
  
  Examples: "foobar.com", "*foobar.com", "*.foobar.com", "*foobar.com:99", "https://x.*.y.com:99"
  
  * `"." HOSTNAME_SUFFIX_PATTERN [ ":" PORT ]`
    
    Une sufijos de dominios particulares.
    
    Ejemplos: ".google.com", ".com", "http://.google.com"

* `[ SCHEME "://" ] IP_LITERAL [ ":" PORT ]`
  
  Une URLs que son literales de dirección IP.
  
  Ejemplos: "127.0.1", "[0:0::1]", "[::1]", "http://[::1]:99"

* `IP_LITERAL "/" PREFIX_LENGHT_IN_BITS`
  
  Une cualquier URL que es literal a una IP que cae en un rango determinado. El rango de IP es especificado usando la notación CIDR.
  
  Examples: "192.168.1.1/16", "fefe:13::abc/33".

* `<local>`
  
  Unir direcciones locales. El significado de `<local>` es el que el host determine que coincida entre: "127.0.0.1", "::1", "localhost".

#### `ses.resolveProxy(url, callback)`

* `url` URL
* `llamada de vuelta` Función 
  * `proxy` Cadena

Resuelve la información del proxy para una `url`. La `llamada` será hecha con `callback(proxy)` cuando se realice la solicitud.

#### `ses.setDownloadPath(path)`

* `ruta` Cadena - la ubicación de descarga

Configura el directorio de descargas. Por defecto, el directorio de descargas será `Descargas` en la carpeta respectiva de la aplicación.

#### `ses.enableNetworkEmulation(options)`

* `options` Object 
  * `fuera de linea` Booleano (opcional) - cuando la red emulada es interrumpida. por defecto es falso.
  * `Latencia` Doble (opcional) - RTT en ms. Por defecto es 0 lo cual deshabilitará la regulación de la latencia.
  * `downloadThroughput` Doble (opcional) - Velocidad de descarga en Bps. Por defecto es 0 que deshabilitará la regulación de descarga.
  * `uploadThroughput` Doble (opcional) - Velocidad de subida en Bps. por defecto es 0 lo cual deshabilitará la regulación de subida.

Emula la red con la configuración dada por la `sesión`.

```javascript
Para emular la conexión GPRS con rendimiento de 50kbps y latencia de 500ms.
window.webContents.session.enableNetworkEmulation({
  latency: 500,
  downloadThroughput: 6400,
  uploadThroughput: 6400
})

// To emulate a network outage.
window.webContents.session.enableNetworkEmulation({offline: true})
```

#### `ses.disableNetworkEmulation()`

Deshabilita cualquier emulación de red activa durante la `sesión`. Resetea a la configuración de red original.

#### `ses.setCertificateVerifyProc(proc)`

* `proc` Función 
  * `request` Object 
    * `hostname` Cadena
    * `certificate` [certificate](structures/certificate.md)
    * `error` Cadena - Verificación de resultado de Chromium.
  * `llamada de vuelta` Función 
    * `verificationResult` Entero - Valor que puede ser uno de los códigos de error certificado de [aquí](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h). Además de los códigos de error certificado, los siguientes códigos especiales pueden ser usados. 
      * `` - indiva el éxito y desactiva la certificación de la verificación de transparencia.
      * `-2` - Indica falla.
      * `-3` - Usa el resultado de verificación de chromium.

Establece el certificado de verificar proc de la `sesión`, el `proc` será cancelada con `proc(request, callback)` cuando sea solicitado una verificación del certificado del servidor. Llamando `callback(0)` se acepta el certificado, llamando `callback(-2)` se rechaza.

Llamando `setCertificateVerifyProc(null)` se reveritrá la verificación de certificado por defecto.

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow()

win.webContents.session.setCertificateVerifyProc((request, callback) => {
  const {hostname} = request
  if (hostname === 'github.com') {
    callback(0)
  } else {
    callback(-2)
  }
})
```

#### `ses.setPermissionRequestHandler(handler)`

* `handler` Función 
  * `contenido web` [contenido web](web-contents.md) - contenido web solicitando el permiso.
  * `permiso` cadena - Enumeración de 'medios', 'geolocalización', 'notificaciones', 'midiSysex', 'bloque de puntero', 'Pantalla completa', 'Apertura externa'.
  * `llamada de vuelta` Función 
    * `permiso concedido` Booleano - Permiso o denegado de permiso

Configurar el controlador que será usado para responder las peticiones de permisos para la `sesión`. Llamando `callback(true)` se permitirá el permiso y `callback(false)` se rechazará.

```javascript
const {session} = require('electron')
session.fromPartition('some-partition').setPermissionRequestHandler((webContents, permission, callback) => {
  if (webContents.getURL() === 'some-host' && permission === 'notifications') {
    return callback(false) // denied.
  }

  callback(true)
})
```

#### `ses.clearHostResolverCache([callback])`

* `Llamada` Función (opcional) - Llamada cuando se ha realizado la operación.

Borra la caché de resolución de host.

#### `ses.allowNTLMCredentialsForDomains(domains)`

* `dominio` Cadena - Una lista separada por coma de servidores para los cuales la autenticación integrada está habilitada.

Configura dinámicamente cada vez que se envíen credenciales para HTTP NTLM o negociaciones de autenticación.

```javascript
const {session} = require('electron')
// considera cualquier url que termine con `example.com`, `foobar.com`, `baz`
// para autenticación integrada.
session.defaultSession.allowNTLMCredentialsForDomains('*example.com, *foobar.com, *baz')

// consider all urls for integrated authentication.
session.defaultSession.allowNTLMCredentialsForDomains('*')
```

#### `ses.setUserAgent(userAgent[, acceptLanguages])`

* `userAgent` cadena
* `lenguajes aceptados` Cadena (opcional)

Reemplaza el `userAgent` y los `lenguajes aceptados` para esta sesión.

Los `lenguajes aceptados` deben estar ordenados en una lista separada por coma de códigos de lenguaje, por ejemplo `"en-US,fr,de,ko,zh-CN,ja"`.

Esto no afecta el `contenido web` existente, y cada `contenido web` puede usar `webContents.setUserAgent` para sobreescribir el agente de sesión de usuario.

#### `ses.getUserAgent()`

Devuelve `Cadena` - El agente usuario para esta sesión.

#### `ses.getBlobData(identifier, callback)`

* `identificador` Cadena - UUID válido.
* `llamada de vuelta` Función 
  * `resultado` Buffer - datos Blob.

Devuelve `Blob` - Los datos blob asociados con el `identificador`.

#### `ses.createInterruptedDownload(options)`

* `options` Object 
  * `ruta` Cadena - ruta de acceso absoluta de la descarga.
  * `Cadeba URL` Cadena[] - Cadena de URL completa para la descarga.
  * `mimeType` Cadena (opcional)
  * `offset` Entero - rango de inicio para la descarga.
  * `longitud` Entero - longitud total de la descarga.
  * `última modificación` Cadena - Último valor del encabezado modificado.
  * `eTag` Cadena - Valor Etag del encabezado.
  * `Tiempo de inicio` Doble (opcional) - Tiempo en que se inició la descarga en números de segundo desde epoch de UNIX.

Permite `cancelar` o `interrumpir` descargas de una `Sesión` previa. La API generará un [elemento de descarga](download-item.md) que puede ser accesado con el evento [se descargará](#event-will-download). El [Elemento de descarga](download-item.md) no tendrá ningún `contenido web` asociado con el y el estado inicial será `interrumpido`. La descarga empezará solo cuando la `reanudación` de la API sea llamada en el [elemento descargado](download-item.md).

#### `ses.clearAuthCache(options[, callback])`

* `options` ([RemovePassword](structures/remove-password.md) | [RemoveClientCertificate](structures/remove-client-certificate.md))
* `callback` Function (optional) - Called when operation is done

Clears the session’s HTTP authentication cache.

### Propiedades de Instancia

Las siguientes propiedades están disponibles en instancias de `Sesión`:

#### `ses.cookies`

Un objeto de [Cookies](cookies.md) para esta esión.

#### `ses.webRequest`

Un objeto [petición web](web-request.md) para esta sesión.

#### `ses.protocol`

Un objeto de [protocolo](protocol.md) para esta sesión.

```javascript
const {app, session} = require('electron')
const path = require('path')

app.on('ready', function () {
  const protocol = session.fromPartition('some-partition').protocol
  protocol.registerFileProtocol('atom', function (request, callback) {
    var url = request.url.substr(7)
    callback({path: path.normalize(`${__dirname}/${url}`)})
  }, function (error) {
    if (error) console.error('Failed to register protocol')
  })
})
```