# ang protokol

> Irehistro ang isang ipinasadyang protokol at harangin ang umiiral na kahilingan para sa protokol.

Ang proseso: [Main](../glossary.md#main-process)

Ang isang halimbawa ng pagpapatupad ng isang protokol na may kaparehas na epekto katulad ng protokol ng `file://`:

```javascript
const {app, protocol} = kailangan('electron')
const path = require('path')

app.on('ready', () => {
  protocol.registerFileProtocol('atom', (request, callback) => {
    const url = request.url.substr(7)
    callback({path: path.normalize(`${__dirname}/${url}`)})
  }, (error) => {
    if (error) console.error('Failed to register protocol')
  })
})
```

**Note:** Ang lahat ng mga pamamaraan maliban kung tinukoy ay maaari lamang gamitin pagkatapos ng event ng `ready` sa modyul ng `app` ay lumabas.

## Pamamaraan

Ang modyul ng `protocol` ay mayroon ng mga sumusunod na mga pamamaraan:

### `ang protocol.registerStandardSchemes(schemes[, options])`

* `schemes` String[] - Ang pasadyang panukala na magiging rehistrado bilang mga standard na panukala.
* `mga pagpipilian` Mga bagay (opsyonal) 
  * `secure` Boolean (opsyonal) - `true` para irehistro ang panukala bilang ligtas. Ang default ay `false`.

Ang isang standard na panukala ay sumusunod sa kung tawagin ng RFC 3986 ay [generic URI syntax](https://tools.ietf.org/html/rfc3986#section-3). Halimbawa ang `http` at ang `https` ay mga standard na panukala, samantalang ang `file` ay hindi.

Ang pagpaparehistro ng panukala bilang standard, ay papayagan ang may kaugnayan at tiyak na mapagkukunan ay malulutas ng tama kapag isinilbi. Kung hindi man ang panukala ay kikilos ng kagaya ng protokol ng `file`, ngunit walang kakayahang lutasin ang may kaugnayang mga URL.

Halimbawa kapag iniload mo ang mga sumusunod na pahina na may pasadyang protokol na hindi inirerehistro ito bilang standard na panukala, ang imahe ay hindi mailoload sapagkat ang hindi standard na mga panukala ay hindi makakakilala ng may kaugnayang mga URL:

```html
<body>
  <img src='test.png'>
</body>
```

Ang pagrerehistro sa isang panukala bilang standard ay pinapayagan ang pagpunta sa mga file sa pamamagitan ng [FileSystemAPI](https://developer.mozilla.org/en-US/docs/Web/API/LocalFileSystem). Kung hindi man ang tagabigay ay magbabato ng isang pang-seguridad na pagkakamali para sa panukala.

By default web storage apis (localStorage, sessionStorage, webSQL, indexedDB, cookies) are disabled for non standard schemes. So in general if you want to register a custom protocol to replace the `http` protocol, you have to register it as a standard scheme:

```javascript
const {app, protocol} = require('electron')

protocol.registerStandardSchemes(['atom'])
app.on('ready', () => {
  protocol.registerHttpProtocol('atom', '...')
})
```

**Note:** This method can only be used before the `ready` event of the `app` module gets emitted.

### `protocol.registerServiceWorkerSchemes(schemes)`

* `schemes` String[] - Custom schemes to be registered to handle service workers.

### `protocol.registerFileProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `filePath` String (optional)
* `completion` Function (optional) 
  * `error` Error

Registers a protocol of `scheme` that will send the file as a response. The `handler` will be called with `handler(request, callback)` when a `request` is going to be created with `scheme`. `completion` will be called with `completion(null)` when `scheme` is successfully registered or `completion(error)` when failed.

To handle the `request`, the `callback` should be called with either the file's path or an object that has a `path` property, e.g. `callback(filePath)` or `callback({path: filePath})`.

When `callback` is called with nothing, a number, or an object that has an `error` property, the `request` will fail with the `error` number you specified. For the available error numbers you can use, please see the [net error list](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h).

By default the `scheme` is treated like `http:`, which is parsed differently than protocols that follow the "generic URI syntax" like `file:`, so you probably want to call `protocol.registerStandardSchemes` to have your scheme treated as a standard scheme.

### `protocol.registerBufferProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `buffer` (Buffer | [MimeTypedBuffer](structures/mime-typed-buffer.md)) (optional)
* `completion` Function (optional) 
  * `error` Error

Registers a protocol of `scheme` that will send a `Buffer` as a response.

The usage is the same with `registerFileProtocol`, except that the `callback` should be called with either a `Buffer` object or an object that has the `data`, `mimeType`, and `charset` properties.

Example:

```javascript
const {protocol} = require('electron')

protocol.registerBufferProtocol('atom', (request, callback) => {
  callback({mimeType: 'text/html', data: Buffer.from('<h5>Response</h5>')})
}, (error) => {
  if (error) console.error('Failed to register protocol')
})
```

### `protocol.registerStringProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `data` String (optional)
* `completion` Function (optional) 
  * `error` Error

Registers a protocol of `scheme` that will send a `String` as a response.

The usage is the same with `registerFileProtocol`, except that the `callback` should be called with either a `String` or an object that has the `data`, `mimeType`, and `charset` properties.

### `protocol.registerHttpProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `redirectRequest` Bagay 
      * `url` String
      * `method` String
      * `session` Object (optional)
      * `uploadData` Mga bagay (opsyonal) 
        * `contentType` String - MIME type of the content.
        * `data` String - Content to be sent.
* `completion` Function (optional) 
  * `error` Error

Registers a protocol of `scheme` that will send an HTTP request as a response.

The usage is the same with `registerFileProtocol`, except that the `callback` should be called with a `redirectRequest` object that has the `url`, `method`, `referrer`, `uploadData` and `session` properties.

By default the HTTP request will reuse the current session. If you want the request to have a different session you should set `session` to `null`.

For POST requests the `uploadData` object must be provided.

### `protocol.unregisterProtocol(scheme[, completion])`

* `scheme` Ang string
* `completion` Function (optional) 
  * `error` Error

Unregisters the custom protocol of `scheme`.

### `protocol.isProtocolHandled(scheme, callback)`

* `scheme` Ang string
* `tumawag muli` Punsyon 
  * `error` Error

The `callback` will be called with a boolean that indicates whether there is already a handler for `scheme`.

### `protocol.interceptFileProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `filePath` String
* `completion` Function (optional) 
  * `error` Error

Intercepts `scheme` protocol and uses `handler` as the protocol's new handler which sends a file as a response.

### `protocol.interceptStringProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `data` String (optional)
* `completion` Function (optional) 
  * `error` Error

Intercepts `scheme` protocol and uses `handler` as the protocol's new handler which sends a `String` as a response.

### `protocol.interceptBufferProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `buffer` Buffer (optional)
* `completion` Function (optional) 
  * `error` Error

Intercepts `scheme` protocol and uses `handler` as the protocol's new handler which sends a `Buffer` as a response.

### `protocol.interceptHttpProtocol(scheme, handler[, completion])`

* `scheme` Ang string
* `handler` Punsyon 
  * `kahilingan` Bagay 
    * `url` String
    * `referrer` String
    * `method` String
    * `uploadData` [UploadData[]](structures/upload-data.md)
  * `tumawag muli` Punsyon 
    * `redirectRequest` Bagay 
      * `url` String
      * `method` String
      * `session` Object (optional)
      * `uploadData` Mga bagay (opsyonal) 
        * `contentType` String - MIME type of the content.
        * `data` String - Content to be sent.
* `completion` Function (optional) 
  * `error` Error

Intercepts `scheme` protocol and uses `handler` as the protocol's new handler which sends a new HTTP request as a response.

### `protocol.uninterceptProtocol(scheme[, completion])`

* `scheme` Ang string
* `completion` Function (optional) 
  * `error` Error

Remove the interceptor installed for `scheme` and restore its original handler.