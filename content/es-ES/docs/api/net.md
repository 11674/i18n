# net

> Emitir solicitudes HTTP/HTTPS usando la biblioteca de red nativa de Chromium

Proceso: [Principal](../glossary.md#main-process)

El módulo `net` es un lado del cliente API para tratar pedidos HTTP(S). Si es similar a los módulos [HTTP](https://nodejs.org/api/http.html) y [HTTPS](https://nodejs.org/api/https.html) de Node.js pero usa la biblioteca de la red nativa de Chromium en vez de las aplicaciones Node.js, ofreciendo un mejor soporte a los proxies de la web.

La siguiente es una lista no completa de por qué debería considerar usar el módulo `net` en vez de los módulos nativos Node.js:

* Gestión automática del sistema de configuración de proxy, soporte del protocolo wpad y el paquete de archivos de configuración del proxy.
* Túnel automático para peticiones HTTPS.
* Soportar los proxies de autentificación usando basic, digest, NTLM, Kerberos, o negociar esquemas de autentificación.
* Soporta proxies para monitoreo de tráfico: Fiddler como proxies usados para el acceso, el control y el monitoreo.

The `net` module API has been specifically designed to mimic, as closely as possible, the familiar Node.js API. The API components including classes, methods, properties and event names are similar to those commonly used in Node.js.

For instance, the following example quickly shows how the `net` API might be used:

```javascript
const {app} = require('electron')
app.on('ready', () => {
  const {net} = require('electron')
  const request = net.request('https://github.com')
  request.on('response', (response) => {
    console.log(`STATUS: ${response.statusCode}`)
    console.log(`HEADERS: ${JSON.stringify(response.headers)}`)
    response.on('data', (chunk) => {
      console.log(`BODY: ${chunk}`)
    })
    response.on('end', () => {
      console.log('No more data in response.')
    })
  })
  request.end()
})
```

By the way, it is almost identical to how you would normally use the [HTTP](https://nodejs.org/api/http.html)/[HTTPS](https://nodejs.org/api/https.html) modules of Node.js

The `net` API can be used only after the application emits the `ready` event. Trying to use the module before the `ready` event will throw an error.

## Métodos

The `net` module has the following methods:

### `net.request(options)`

* `options` (Object | String) - The `ClientRequest` constructor options.

Returns [`ClientRequest`](./client-request.md)

Creates a [`ClientRequest`](./client-request.md) instance using the provided `options` which are directly forwarded to the `ClientRequest` constructor. The `net.request` method would be used to issue both secure and insecure HTTP requests according to the specified protocol scheme in the `options` object.