# sesyon

> Pamahalaan ang mga sesyon ng browser, mga cookie, cache, mga setting ng proxy, etc.

Ang Proseso: [Pangunahin](../glossary.md#main-process)

Ang `sesyon` modyul ay maaaring gamitin para gumawa ng bagong `Sesyon` ng mga layunin.

Maaari mo rin ma-akses ang `sesyon` ng umiiral na mga pahina sa pamamagitan ng paggamit ng `sesyon` uri ng [`NilalamanngmgaWeb`](web-contents.md), o galing sa `sesyon` na modyul.

```javascript
const {BrowserWindow} = require('electron')

let win = bagong BrowserWindow({width: 800, height: 600})
win.loadURL('http://github.com')

const ses = win.webContents.session
console.log(ses.getUserAgent())
```

## Mga pamamaraan

Ang `sesyon` ng modyul ay ang mga sumusunod na pamamaraan:

### `sesyon.galingPartisyon(partisyon[, mga opsyon])`

* `partisyon` String
* `mga opsyon` Layunin 
  * `cache` Boolean - Maaaring paganahin ang cache.

Ibalik ang `Sesyon` - Isang mungkahi ng sesyon mula sa `partisyon` ng string. Kapag merong umiiral sa `sesyon` ng may kaparehong `partisyon`, ito ay maaaring bumalik: sa kabilang banda ay maging bago `Sesyon` ang instansya ay maaaring gumawa kasama ang `mga opsyon`.

Kung ang `partisyon` ay nagsisimula kasama ang `pananatili:`, ang pahina ay gumagamit ng isang sesyon sa pananatili Maaari magamit ito sa lahat ng mga pahina sa mga app na may kaparehong `partisyon`. kung walang ditong `paninindigan:` prefix, ang pahina ay magagamit bilang sesyon ng memorya. Kung ang `partisyon` ay walang laman kung gayoon ang sesyon ng default ng app ay ibabalik.

Para gumawa ng isang `sesyon` kasama ng `mga option`, siguraduhin mo rin ang `Sesyon` kasama ang `partisyon` na hindi pa ginamit nuon. Walang ibang paraan para baguhin ang `mga opsyon` bilang isang umiiiral na `Sesyon` ng layunin.

## Mga katangian

Ang `sesyon` ng module ay may sinusunod na katangian:

### `sesyon.defaultngsesyon`

Isang `sesyon` ng layunin, ang depult ng sesyon na layunin ng app.

## Klase: ng Sesyon

> Kumuwa at magtakda ng mga katangian ng isang sesyon.

Ang proseso ng: [Main](../glossary.md#main-process)

Maaari kang gumawa ng isang `Sesyon` ng layunin sa `sesyon` ng module:

```javascript
const {session} = kinakailangang('electron')
const ses = sesyon.galingpartisyon('persist:name')
console.log(ses.getUserAgent())
```

### Halimbawa ng mga Kaganapan

Ang mga sumusunod na mga kaganapan ay magagamit sa mga pagkakataon ng `Sesyon`:

#### Kaganapan: 'Ay-madadownload'

* `kaganapan` Kaganapan
* `aytem` [I-downloadangaytem](download-item.md)
* `mga nilalaman ng web` [Mga nilalaman ng Web](web-contents.md)

Kung saan ang electron ay napalabas tungkol sa download `aytem` sa `Mga nilalaman ng web`.

Pagtawag sa `event.preventDefault()` ay makakansela ang dinadownload at ang `aytem` ay hindi maaaring magamit hanggang sa susunod na tik ng proseso.

```javascript
const {session} = require('electron')
session.defaultSession.on('will-download', (event, item, webContents) => {
  event.preventDefault()
  require('request')(item.getURL(), (data) => {
    require('fs').writeFileSync('/somewhere', data)
  })
})
```

### Mga halimbawa ng pamamaraan

Nag sumusunod na pamamaraan ay magagamit para sa halimbawa ng `Session`:

#### `ses.getCacheSize(callback)`

* `tumawag muli` Function 
  * `size` Integer - Nagamit na laki cache sa bytes.

Pagwatag-muli ay nananawagan sa mga sesyons sa kasalukuyang laki ng cache.

#### `ses.clearCache(callback)`

* `callback` Function - Tinatawag kung ang operason ay tapos na

Nililimas ang sesyon ng HTTP cache.

#### `ses.clearStorageData([options, callback])`

* `mga pagpipilian` Mga bagay (opsyonal) 
  * `origin` String - (optional) Should follow `window.location.origin`’s representation `scheme://host:port`.
  * `storages` String[] - (optional) The types of storages to clear, can contain: `appcache`, `cookies`, `filesystem`, `indexdb`, `localstorage`, `shadercache`, `websql`, `serviceworkers`
  * `quotas` String[] - (optional) The types of quotas to clear, can contain: `temporary`, `persistent`, `syncable`.
* `callback` Function (opsyonal) - Tinatawag kung ang operasyon ay tapos na.

Nililimas and datos ng web na imbikan.

#### `ses.flushStorageData()`

Pagsulat nag anumang di-nakusulat na DOMStorage na datos para sa disk.

#### `ses.setProxy(config, callback)`

* `kumpuni` Bagay 
  * `pacScript` String - Ang URL na kasama na may PAC file.
  * `proxyRules` String - Mga panuntunan na nagsasad kung no lang mga proxies na gagamitan.
  * `proxyBypassRules` String - Mga panuntunan na nagpapahiwatig kung saan ang URLs ay dapat i-bypass ang mga setting ng proxy.
* `callback` Function - Tinatawag kung ang operasyon ay tapos na.

Nagtatakda ng mga settings na proxy.

Kung ang `pacScript` at `proxyRules` ay kasabay na ibinigay, ang `proxyRules` na opsyon ay hindi pinansin at `pacScript` ang pagsasaayos ay inilipat.

Ang `proxyRules` ay dapat sumunod sa panuntunan:

    proxyRules = schemeProxies[";"<schemeProxies>]
    schemeProxies = [<urlScheme>"="]<proxyURIList>
    urlScheme = "http" | "https" | "ftp" | "socks"
    proxyURIList = <proxyURL>[","<proxyURIList>]
    proxyURL = [<proxyScheme>"://"]<proxyHost>[":"<proxyPort>]
    

Halimbawa:

* `http=foopy:80;ftp=foopy2` - Use HTTP proxy `foopy:80` for `http://` URLs, and HTTP proxy `foopy2:80` for `ftp://` URLs.
* `foopy:80` - Gamitin ang HTTP proxy `foopy:80` sa lahat ng URLs.
* `foopy:80,bar,direct://` - Gumamit ng HTTP proxy `foopy:80` sa lahat ng URLs, failing sa lahat para 0>bar</code> if `foopy:80` ay hindi magagamit, at matapos gamitin ang walang proxy.
* `socks4://foopy` - Gumamit ng SOCKS v4 proxy `foopy:1080` sa lahat ng URLs.
* `http=foopy,socks5://bar.com` - Gumamit ng HTTP na proxy `foopy` para sa hhtp URLs, at nabigo higit para sa SOCKS5 proxy `bar.com` if `foopy` ay hindi magagamit.
* `http=foopy,direct://` - Gumamit ng HTTP na proxy `foopy` para sa http URLs, at gamitin ang no-proxy kung `foopy` ay hindi magagamit.
* `http=foopy;socks=foopy2` - Gumamit ng HTTP na proxy `foopy` para sa http URLs, at gamitin ang `socks4://foopy2` para sa lahat ng ibnag URLs.

Ang `proxyBypassRules` ay comma na hiwalay na listahan ng panuntunan na inilirawan sa baba:

* `[ URL_SCHEME "://" ] HOSTNAME_PATTERN [ ":" <port> ]`
  
  Itugma ang lahat ng hostnames na nakatugma sa pattern ng HOSTNAME_PATTERN.
  
  Mga halimbawa: "foobar.com", "*foobar.com", "*.foobar.com", "*foobar.com:99", "https://x.*.y.com:99"
  
  * `"." HOSTNAME_SUFFIX_PATTERN [ ":" PORT ]`
    
    Itugma ang partikular na domain suffix.
    
    Mga halimbawa: ".google.com", ".com", "http://.google.com"

* `[ SCHEME "://" ] IP_LITERAL [ ":" PORT ]`
  
  Itugma ang URLs kung saan ang IP address na literals.
  
  Mga halimbawa: "127.0.1", "[0:0::1]", "[::1]", "http://[::1]:99"

* `IP_LITERAL "/" PREFIX_LENGHT_IN_BITS`
  
  Itugma ang anumang URL na para sa IP literal na bumaba sa pagitan ng binigay na saklaw. IP range ay tinukoy gamit ang CIDR notation.
  
  Examples: "192.168.1.1/16", "fefe:13::abc/33".

* `<local>`
  
  Itugma ang mga lokal na mga address. Ang ibig sabihin ng `<local>` ay kung ang host ay matutugma sa isang: "127.0.0.1", "::1", "localhost".

#### `ses.resolveProxy(url, callback)`

* `url` Ang URL
* `tumawag muli` Function 
  * `proxy` String

Malulutas ang impormasyon para sa `url`. Ang `callback` ay tatawagin na may `callback(proxy)` kung saan ang hiling ay gagawin.

#### `ses.setDownloadPath(path)`

* `path` String - Ang lokasyon ng pag-download

Nagtatakda ng download saving na diktoryo. Bilang default, ang download na diktoryo ay magiging `Downloads` sa ilalim ng kaukulang app na folder.

#### `ses.enableNetworkEmulation(opsyons)`

* `mga pagpipilian` Mga Bagay 
  * `offline` Boolean (opsyonal) - Kung saan i-emulate ang network outage. Defaults sa huwad.
  * `latency` Double (opsyonal) - RTT in ms. Defaults sa 0 kung saan ito ay hindi pinagana ng latency throttling.
  * `downloadThroughput` Double (opsyonal) - Pag-download ng rate sa Bps. Defaults sa 0 kung saan hindi pinagana ang download throttling.
  * `uploadThroughput` Double (opsyonal) - Mag-upload ng rate sa Bps. Dafaults sa 0 kung saan hidni pinagana ang upload throttling.

Pag-emulate ng network na may nakabigay na konfigurasyon para sa `session`.

```javascript
// Para i-emulate ang GPRS na koneksyon na may 50kbps na throughput at 500 ms na latency.
window.webContents.session.enableNetworkEmulation({
  latency: 500,
  downloadThroughput: 6400,
  uploadThroughput: 6400
})

// Para i-emulate ang network na outage.
window.webContents.session.enableNetworkEmulation({offline: true})
```

#### `ses.disableNetworkEmulation()`

Hindi pinapagana ang anumang network emulation ay na aktibo para sa `session`. Ni-rereset para sa orihinal na network na konfigurasyon.

#### `ses.setCertificateVerifyProc(proc)`

* `proc` Function 
  * `kahilingan` Mga Bagay 
    * `hostname` String
    * `certificate` [Certificate](structures/certificate.md)
    * `error` String - Pagpapatunay na result galing sa chromium.
  * `callback` Function 
    * `verificationResult` Integer - Balyo ay maaring isang sertipiko na error codes galing sa [dito](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h)Bukod sa sertifikong error codes, ang mga sumusunod na espesyal na codes ay magagamit. 
      * `` - Ay nagpapahiwatig sa tagumpay at pag-disable sa Certificate Transperancy verification.
      * `-2` - Nagpapahiwatig sa kabigu-an.
      * `-3` - Gumagamit ng pagpapatunay galing sa chromium.

Nagtatakda ng sertifikong verify proc para sa `session`, ang `proc` ay tinatawag na may `proc(request, callback)` sa tuwing ang server certificate ay hinihiling. Calling `callback(0)` tinatanggap ang sertifiko, calling `callback(-2)` tinatangihan ito.

Ang pagtawag `setCertificateVerifyProc(null)` ay i-rerevert pabalik sa default ang sertifiko verify proc.

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

* `ang tagahawak` Function 
  * `webContents` [WebContents](web-contents.md) - WebContents pag-request ng pahintulot.
  * `permission` String - Enum of 'media', 'geolocation', 'notifications', 'midiSysex', 'pointerLock', 'fullscreen', 'openExternal'.
  * `callback` Function 
    * `permissionGranted` Boolean - Pagpayag o pag-tanggi sa pahintulot

Nagtatakda sa handler kung saan magagamit upang tumugon sa pahintulot na kahilingan para sa `session`. Calling `callback(true)` ay maaring bigyan pahintulot ang `callback(false)` ay tatangihan ito.

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

* `callback` Function (opsyonal) - Tinatawag kung ang operasyon ay tapos na.

Nililimas ang host resolver cache.

#### `ses.allowNTLMCredentialsForDomains(domains)`

* `domains` String - Ang comma-separated na listahan ng servers para sa integrated authentication ay pinagana.

Dynamically ay nagtatakda kung para sa laging magpadala sa credentials para sa HTTP NTLM o Negotiate authentication.

```javascript
const {session} = require('electron')
// consider any url ending with `example.com`, `foobar.com`, `baz`
// para sa pagsasama ng pagpapatunay.
session.defaultSession.allowNTLMCredentialsForDomains('*example.com, *foobar.com, *baz')

// isa-alangalang ang lahat ng urls para sa pagsasama ng pagpapatunay.
session.defaultSession.allowNTLMCredentialsForDomains('*')
```

#### `ses.setUserAgent(userAgent[, acceptLanguages])`

* `userAgent` String
* `acceptLanguages` String (opsyonal)

I-override ang `userAgent` at `acceptLanguages` para sa sesyong ito.

Ang `acceptLanguages` ay dapat may kuwit na hiwalay na ordered list sa language codes, para sa halimbawa `"en-US,fr,de,ko,zh-CN,ja"`.

Ito ay hindi makakapekto sa umiiral `WebContents`, at bawat-isa `WebContents` ay magagamit `webContents.setUserAgent` para i-override ang sesyon-wide ng ahente na gumagamit.

#### `ses.getUserAgent()`

Nagbabalik `String` - Ang gugamit na ahente para sa sesyon na ito.

#### `ses.getBlobData(identifier, callback)`

* `identifier` String - Valid UUID.
* `tumawag muli` Function 
  * `result` Buffer - Blob data.

Nagbabalik `Blob` - The blob na datos ay na-uugnay na may `identifier`.

#### `ses.createInterruptedDownload(opsyons)`

* `mga pagpipilian` Mga Bagay 
  * `path` String - Ganap na path para sa download.
  * `urlChain` String[] - Completong URL chain para sa download.
  * `mimeType` String (opsyonal)
  * `offset` Integer - Pagsimula sa range para sa download.
  * `length` Integer - Kabuuhan ng haba para sa download.
  * `lastModified` String - Last-Modified header value.
  * `eTag` String - ETag header balyo.
  * `startTime` Double (opsyonal) - Ang oras kung saan ang download ay nagsimula na sa numero ng segundo since UNIX epoch.

Allows resuming `cancelled` or `interrupted` downloads from previous `Session`. The API will generate a [DownloadItem](download-item.md) that can be accessed with the [will-download](#event-will-download) event. The [DownloadItem](download-item.md) will not have any `WebContents` associated with it and the initial state will be `interrupted`. The download will start only when the `resume` API is called on the [DownloadItem](download-item.md).

#### `ses.clearAuthCache(options[, callback])`

* `options` ([RemovePassword](structures/remove-password.md) | [RemoveClientCertificate](structures/remove-client-certificate.md))
* `callback` Function (optional) - Called when operation is done

Clears the session’s HTTP authentication cache.

### Humahalimbawa sa bahagi nito

The following properties are available on instances of `Session`:

#### `ses.cookies`

A [Cookies](cookies.md) object for this session.

#### `ses.webRequest`

A [WebRequest](web-request.md) object for this session.

#### `ses.protocol`

A [Protocol](protocol.md) object for this session.

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