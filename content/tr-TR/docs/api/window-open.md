# `window.open` Fonksiyonu

> Yeni bir pencere aç ve URL yükle.

Web sayfasında yeni bir pencere açmak için `window.open` çağırıldığında, `url` için yeni bir `BrowserWindow` örneği oluşturulur ve sayfanın üzerinde sınırlı kontrole sahip olması için proxy `window.open`'a döndürülür.

Proxy, geleneksel web sayfalarıyla uyumlu olması için uygulanan sınırlı standart işlevselliğe sahiptir. Yeni pencerenin tam kontrolü için doğrudan bir `BrowserWindow` oluşturmanız gerekmektedir.

Yeni oluşturulan `BrowserWindow`, varsayılan olarak ana pencerenin seçeneklerini miras alır. Miras alınan seçeneklerin üstüne yazmak için `features` karakter dizisini kullanabilirsiniz.

### `window.open(url[, frameName][, features])`

* `url` Dize
* `frameName` String (opsiyonel)
* `features` String (opsiyonel)

[`BrowserWindowProxy`](browser-window-proxy.md) Döndürür - Yeni bir pencere oluşturur ve `BrowserWindowProxy` sınıfının bir örneğini döndürür.

`features` karakter dizisi standart tarayıcının biçimi izler, fakat her bir özellik `BrowserWindow` seçeneğinin bir alanı olmalıdır.

**Notlar:**

* Ana pencerede devre dışı bırakılmış ise, açılan `window`'de Node entegrasyonu devre dışı bırakılacaktır.
* Eğer Ortam izolasyonu, ana pencerede etkinleştirilmiş ise, açılan `window`'da daima etkinleştirilir.
* Eğer JavaScript, ana pencerede devre dışı bırakılmış ise, açılan `window`'da daima devre dışı bırakılır.
* `features`'ta verilen standart olmayan özellikler (Chromium veya Electron tarafından ele alınmaz) `additionalFeatures` argümanı içindeki kayıtlı `webContent`'in `new-windows` olay işleyicisine geçirilir.

### `window.opener.postMessage(message, targetOrigin)`

* `message` String
* `targetOrigin` String

Sends a message to the parent window with the specified origin or `*` for no origin preference.

### Using Chrome's `window.open()` implementation

Chrome'un dahili `window.open()` uygulamasını kullanmak istiyorsanız, `webPreferences` seçenekler nesnesinde `nativeWindowOpen` öğesini `true` olarak ayarlayın.

Yerel `window.open()`, açılan pencerelere eşzamanlı erişime izin verir; Bir iletişim kutusu veya tercihler penceresi açmanız gerekiyorsa uygun bir seçim.

Bu seçenek aynı zamanda `<webview>` etiketlerinde de ayarlanabilir:

```html
<webview webpreferences="nativeWindowOpen=yes"></webview>
```

`BrowserWindow`'un oluşturulması, `WebContents`'in `new-window` etkinliği aracılığıyla özelleştirilebilir.

```javascript
// main process
const mainWindow = new BrowserWindow({
  width: 800,
  height: 600,
  webPreferences: {
    nativeWindowOpen: true
  }
})
mainWindow.webContents.on('new-window', (event, url, frameName, disposition, options, additionalFeatures) => {
  if (frameName === 'modal') {
    // open window as modal
    event.preventDefault()
    Object.assign(options, {
      modal: true,
      parent: mainWindow,
      width: 100,
      height: 100
    })
    event.newGuest = new BrowserWindow(options)
  }
})
```

```javascript
// renderer process (mainWindow)
let modal = window.open('', 'modal')
modal.document.write('<h1>Hello</h1>')
```