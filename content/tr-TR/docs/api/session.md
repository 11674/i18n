# session

> Tarayıcı oturumları, çerezler, önbellek, proxy ayarlarını, vb. yönetin.

Süreç: [Ana](../glossary.md#main-process)

`oturum` modülü, yeni `Oturum` nesneleri oluşturmak için kullanılabilir.

Ayrıca mevcut sayfaların `oturum`larına `oturum` [`Webİçeriği`](web-contents.md) özelliğinden, yada `oturum` modülünden ulaşabilirsiniz.

```javascript
const {BrowserWindow} = require('electron')

let win = new BrowserWindow({width: 800, height: 600})
win.loadURL('http://github.com')

const ses = win.webContents.session
console.log(ses.getUserAgent())
```

## Yöntemler

`oturum` modülü aşağıdaki yöntemleri içerir:

### `session.fromPartition(partition[, options])`

* `partition` String
* `ayarlar` Nesne 
  * `cache` Boolean - Whether to enable cache.

`Oturum` Döndürür - `bölümden` bir oturum örneği metini. Aynı `partition`'a sahip olan `Session` varsa, döndürülecektir; aksi taktirde `Session` örneği `options` ile yaratılacaktır.

Eğer `bölüm`ile başla`sürdür`ile başlarsa, sayfa kalıcı bir oturum kullanacaktır uygulamanın tüm sayfalarına aynı şekilde erişilebilir `bölüm`. yoksa `sürdür` önekini kullandığınızda, sayfa bir bellek içi oturum kullanacaktır. Eğer `bölüm` boş ise, uygulamanın varsayılan oturumu kullanılıcaktır.

`options` ile bir `Session` yaratmadan önce `partition`'lı `Session`'ın daha önce hiç kullanılmadığından emin olmalısınız. Var olan bir `Session` nesnesinin `options`'ını değiştirmenin bir yolu yoktur.

## Özellikler

`session` modülü aşağıdaki yöntemleri içerir:

### `session.defaultSession`

Bir `Session` nesnesi, uygulamanın varsayılan oturum nesnesidir.

## Sınıf: oturum

> Bir oturumun özelliklerini alın ve ayarlayın.

Süreç: [Ana](../glossary.md#main-process)

`oturum` modülünde bir `Oturum` nesnesi oluşturabilirsiniz:

```javascript
const {session} = require('electron')
const ses = session.fromPartition('persist:name')
console.log(ses.getUserAgent())
```

### Örnek Olaylar

Aşağıdaki olaylar `Session` durumun da kullanılabilir:

#### Etkinlik: 'indirilecek'

* `olay` Olay
* `item` [DownloadItem](download-item.md)
* `webContents` [webİçerikleri](web-contents.md)

Elektron indirmek üzereyken ortaya çıkar `item` in `webContents`.

`event.preventDefault()` çağırmak indirmeyi iptal edecektir ve `item` işlemin bir sonraki işaretine kadar uygun olmayacaktır.

```javascript
const {session} = require('electron')
session.defaultSession.on('will-download', (event, item, webContents) => {
  event.preventDefault()
  require('request')(item.getURL(), (data) => {
    require('fs').writeFileSync('/somewhere', data)
  })
})
```

### Örnek yöntemler

Aşağıdaki yöntemler `Oturum` örnekleri üzerinde mevcuttur:

#### `ses.getCacheSize(callback)`

* `geri arama` Fonksiyon 
  * `boyut` Integer - Önbellek boyutu bayt cinsinden kullanılır.

Geri arama oturumun geçerli önbellek boyutu ile çağrılır.

#### `ses.clearCache(callback)`

* `geri çağırma` Fonksiyonu - İşlem tamamlandığında çağırılır

Oturumun HTTP önbelleğini temizler.

#### `ses.clearStorageData([options, callback])`

* `ayarlar` Obje (isteğe bağlı) 
  * `origin` String - (optional) Should follow `window.location.origin`’s representation `scheme://host:port`.
  * `storages` String[] - (optional) Temizlenecek depo türleri, aşağıdakileri içerebilir: `appcache`, `cookies`, `filesystem`, `indexdb`, `localstorage`, `shadercache`, `websql`, `serviceworkers`
  * `quotas` String[] - (optional) The types of quotas to clear, can contain: `temporary`, `persistent`, `syncable`.
* `callback` Function (isteğe bağlı) - İşlem bittiğinde çağırıldı.

Web depolama alanları verilerini siler.

#### `ses.flushStorageData()`

Yazılı olmayan herhangi bir DOM depolama verisini diske yazar.

#### `ses.setProxy(config, callback)`

* `konfigurasyon` Nesne 
  * `pacScript` String - PAC dosyasıyla ilişkilendirilmiş URL.
  * `proxyRules` String - Hangi proxy'lerin kullanılacağını belirten kurallar.
  * `proxyBypassRules` String - Rules indicating which URLs should bypass the proxy settings.
* `geri çağırma` Fonksiyonu - İşlem tamamlandığında çağırılır.

Proxy ayarlarını yap.

When `pacScript` and `proxyRules` are provided together, the `proxyRules` option is ignored and `pacScript` configuration is applied.

`proxyRules` aşağıdaki kurallara uymak zorundadır:

    proxyRules = schemeProxies[";"<schemeProxies>]
    schemeProxies = [<urlScheme>"="]<proxyURIList>
    urlScheme = "http" | "https" | "ftp" | "socks"
    proxyURIList = <proxyURL>[","<proxyURIList>]
    proxyURL = [<proxyScheme>"://"]<proxyHost>[":"<proxyPort>]
    

Örneğin:

* `http=foopy:80;ftp=foopy2` - Use HTTP proxy `foopy:80` for `http://` URLs, and HTTP proxy `foopy2:80` for `ftp://` URLs.
* `foopy:80` - Use HTTP proxy `foopy:80` for all URLs.
* `foopy:80,bar,direct://` - Use HTTP proxy `foopy:80` for all URLs, failing over to `bar` if `foopy:80` is unavailable, and after that using no proxy.
* `socks4://foopy` - Use SOCKS v4 proxy `foopy:1080` for all URLs.
* `http=foopy,socks5://bar.com` - Use HTTP proxy `foopy` for http URLs, and fail over to the SOCKS5 proxy `bar.com` if `foopy` is unavailable.
* `http=foopy,direct://` - Use HTTP proxy `foopy` for http URLs, and use no proxy if `foopy` is unavailable.
* `http=foopy;socks=foopy2` - Use HTTP proxy `foopy` for http URLs, and use `socks4://foopy2` for all other URLs.

`proxyBypassRules` yapısı aşşağıda açıklanan virgülle ayrılmış kurallar listesidir:

* `[ URL_SCHEME "://" ] HOSTNAME_PATTERN [ ":" <port> ]`
  
  HOSTNAME_PATTERN kalıbıyla eşleşen tüm ana makine adlarını eşleştirin.
  
  Examples: "foobar.com", "*foobar.com", "*.foobar.com", "*foobar.com:99", "https://x.*.y.com:99"
  
  * `"." HOSTNAME_SUFFIX_PATTERN [ ":" PORT ]`
    
    Belirli bir alanın son ekiyle eşleşir.
    
    Examples: ".google.com", ".com", "http://.google.com"

* `[ SCHEME "://" ] IP_LITERAL [ ":" PORT ]`
  
  IP adresi değişmez olan URL'leri eşleştirin.
  
  Examples: "127.0.1", "[0:0::1]", "[::1]", "http://[::1]:99"

* `IP_LITERAL "/" PREFIX_LENGHT_IN_BITS`
  
  Match any URL that is to an IP literal that falls between the given range. IP range is specified using CIDR notation.
  
  Examples: "192.168.1.1/16", "fefe:13::abc/33".

* `<local>`
  
  Match local addresses. The meaning of `<local>` is whether the host matches one of: "127.0.0.1", "::1", "localhost".

#### `ses.resolveProxy(url, callback)`

* `url` URL
* `geri arama` Fonksiyon 
  * `proxy` String

`url` Urlsinin proksi bilgisini çözümler. `callback`, `callback(proxy)` istek geldiğinde çağrılacaktır.

#### `ses.setDownloadPath(path)`

* `yol` String - İndirme konumu

İndirme, kaydetme dizini ayarlar. Varsayılan olarak, karşıdan yükleme dizini `İndirilenler` uygulama klasörü altındadır.

#### `ses.enableNetworkEmulation(options)`

* `ayarlar` Nesne 
  * `offline` Boolean (İsteğe Bağlı) - Ağ bağlantısının kopmasını taklit eder. Varsayılan değer False.
  * `latency` Double (İsteğe Bağlı) - RTT (ms cinsinden) Varsayılan değer 0, gecikmenin azaltılmasını devre dışı bırakır.
  * `downloadThroughput` Double (isteğe bağlı) - Bps' de indirme hızı. Varsayılan değer 0, indirme hız sınırlamalarını devre dışı bırakır.
  * `uploadThroughput` Double (isteğe bağlı) - Bps' de yükleme hızı. Varsayılan değer 0, yükleme sınırlamalarını devre dışı bırakır.

Emulates ağı için verilen yapılandırmayla `session`.

```javascript
// GPRS bağlantısını 50kbps çıkış ve 500 ms gecikme ile taklit etmek.
window.webContents.session.enableNetworkEmulation({
  latency: 500,
  downloadThroughput: 6400,
  uploadThroughput: 6400
})

// To emulate a network outage.
window.webContents.session.enableNetworkEmulation({offline: true})
```

#### `ses.disableNetworkEmulation()`

Ağbağlantısı emulasyonu `session` için zaten aktiftir. Orjinal ağ yapılandırmasını sıfırlar.

#### `ses.setCertificateVerifyProc(proc)`

* `proc` Fonksiyon 
  * `istek` Nesne 
    * `hostname` String
    * `certificate` [sertifika](structures/certificate.md)
    * `hata` Metin - Chromium doğrulama sonucu.
  * `geri arama` Fonksiyon 
    * `doğrulama Sonucu` Tamsayı: Değer sertifika hata kodlarından olabilir [buraya](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h). Sertifika hata kodlarından ayrı aşağıdaki özel kodlar da kullanılabilir. 
      * `` - Başarıyı belirtir ve Sertifika Şeffaflık doğrulamasını devre dışı bırakır.
      * `-2` - Arızayı gösterir.
      * `-3` - Doğrulama sonucunu Chromium'dan kullanır.

Sets the certificate verify proc for `session`, the `proc` will be called with `proc(request, callback)` whenever a server certificate verification is requested. Arama `geri çağırma(0)` sertfikayı kabul eder, arama `geri çağırma(-2)` reddeder.

Calling `setCertificateVerifyProc(null)` will revert back to default certificate verify proc.

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

* `halledici` Fonksiyon 
  * `webContents` [WebContents](web-contents.md) - WebContents izin istiyor.
  * `permission` String - Enum of 'media', 'geolocation', 'notifications', 'midiSysex', 'pointerLock', 'fullscreen', 'openExternal'.
  * `geri arama` Fonksiyon 
    * `permissionGranted` Boolean - İzin verme veya reddetme

Hallediciyi `session` tepki verecek şekilde ayarlar. Arama `geri çağırma(true)` izin verir ve `geri çağırma(false)` reddeder.

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

* Fonksiyon `geri çağırma` (isteğe bağlı) - İşlem tamamlandığında çağrılır.

Ana çözümleyici önbelleğini temizler.

#### `ses.allowNTLMCredentialsForDomains(domains)`

* `domains` String - A comma-seperated list of servers for which integrated authentication is enabled.

Dinamik olarak, HTTP, NTLM veya Müzakere kimlik doğrulaması için kimlik bilgilerini göndermeyi veya göndermemeyi ayarlar.

```javascript
const {session} = require('electron')
// consider any url ending with `example.com`, `foobar.com`, `baz`
// for integrated authentication.
session.defaultSession.allowNTLMCredentialsForDomains('*example.com, *foobar.com, *baz')

// consider all urls for integrated authentication.
session.defaultSession.allowNTLMCredentialsForDomains('*')
```

#### `ses.setUserAgent(userAgent[, acceptLanguages])`

* `userAgent` String
* `acceptLanguages` String (optional)

`userAgent` ve `acceptLanguages` modülünü bu oturum için geçersiz kılar.

The `acceptLanguages` must a comma separated ordered list of language codes, for example `"en-US,fr,de,ko,zh-CN,ja"`.

Bu mevcut `WebContents` yapısını etkilemez ve her `WebContents` yapısı `webContents.setUserAgent` yapısını oturum genelinde kullanıcı aracısını geçersiz kılmak için kullanabilir.

#### `ses.getUserAgent()`

`String` döndürür - Bu oturum için kullanıcı aracısı.

#### `ses.getBlobData(identifier, callback)`

* `identifier` String - Valid UUID.
* `geri arama` Fonksiyon 
  * `result` Buffer - Blob data.

Returns `Blob` - The blob data associated with the `identifier`.

#### `ses.createInterruptedDownload(options)`

* `ayarlar` Nesne 
  * `yol` String - İndirmenin kesin yolu.
  * `urlChain` String[] - Karşıdan yükleme için tam URL zinciri.
  * `mimeType` String (isteğe bağlı)
  * `offset` Integer - Karşıdan yükleme için başlangıç aralığı.
  * `uzunluk` Integer - Karşıdan yükleme toplam uzunluk.
  * `lastModified` String - Son değiştirilen başlık değeri.
  * `eTag` String - ETag başlık değeri.
  * `startTime` Double (optional) - Time when download was started in number of seconds since UNIX epoch.

Önceki `oturumdan` `iptal edilen` ya da `kesilen` indirmelerin devam etmesine izin verir. API [will-download](#event-will-download) eventi ile erişilebilecek bir [DownloadItem](download-item.md) oluşturacak. The [DownloadItem](download-item.md) will not have any `WebContents` associated with it and the initial state will be `interrupted`. The download will start only when the `resume` API is called on the [DownloadItem](download-item.md).

#### `ses.clearAuthCache(options[, callback])`

* `options` ([RemovePassword](structures/remove-password.md) | [RemoveClientCertificate](structures/remove-client-certificate.md))
* `geri çağırma` Fonksiyon (isteğe bağlı) - İşlem tamamlandığında çağrılır

Kullanıcı oturumunun HTTP kimlik doğrulama önbelleğini temizler.

### Örnek özellikleri

Aşağıdaki özellikler `Oturum` örnekleri üzerinde mevcuttur:

#### `ses.cookies`

Bu oturum için [çerezler](cookies.md) nesnesi.

#### `ses.webRequest`

Bu oturum için [Webistek](web-request.md) nesnesi.

#### `ses.protocol`

Bu oturum için bir [Protokol](protocol.md) nesnesi.

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