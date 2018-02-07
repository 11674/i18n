# konten web

> Membuat dan mengontrol halaman web.

Proses: [Utama](../glossary.md#main-process)

`isi web` adalah [Pemancar acara](https://nodejs.org/api/events.html#events_class_eventemitter). Ini bertanggung jawab untuk kontrol dan mengendalikan halaman web dan properti objek [`Jendela Peramban`](browser-window.md). Contoh untuk mengakses objek `isi web`:

```javascript
const {BrowserWindow} = membutuhkan ('elektron')

let win = new BrowserWindow ({width: 800, height: 1500})
win.loadURL ('http://github.com')

biarkan isi = win.webContents
console.log (isi)
```

## Metode

Metode ini dapat diakses dari modul `isi web`:

```javascript
const {isi web} = memerlukan('electron') perintah.log(isi web)
```

### `isi wab.dapatkan Semua Web()`

`[isi web]` - mengembalikan array dari semua contoh `isi web`. Ini akan berisi isi web untuk semua windows, tampilan web , devtools terbuka, dan devtools ekstensi latar belakang halaman.

### `isi web.dapatkan Fokus Web isi()`

Kembali `isi web` - isi web yang terfokus dalam aplikasi ini, jika tidak kembali `batal`.

### `isi web dari Id(id)`

* `identitas` Integer

Mengembalikan `isi web` - Contoh isi web dengan INDETITAS yang diberikan.

## Kelas: isi web

> Membuat dan mengontrol isi sebuah contoh jendela peramban.

Proses: [utama](../glossary.md#main-process)

### Perihal contoh

#### Event: 'Apakah-selesai-load'

Dibunyikan apabila navigasi dilakukan, yakni pemintal tab telah berhenti berputar dan acara `pada beban` dikirim.

#### Peristiwa: 'Apakah-gagal-beban'

Kembali:

* `peristiwa` Peristiwa
* `kode kesalahan` Bilangan bulat
* `Deskripsi kesalahan` Tali
* `memvalidasi URL` Tali
* `adalah Bingkai Utama` Boolean

Acara ini seperti `Apakah-selesai-beban` tapi dipancarkan ketika beban gagal atau dibatalkan, misalnya `jendela.berhenti()` dipanggil. Daftar lengkap kode galat dan makna mereka tersedia [di sini](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h).

#### Peristiwa: 'Apakah-frame-selesai-beban'

Pengembalian:

* `peristiwa` Peristiwa
* `adalah Bingkai Utama` Boolean

Dibunyikan apabila bingkai telah melakukan navigasi.

#### Peristiwa: 'Apakah-mulai-pemuatan'

Sesuai dengan poin pada saat pemintal tab berhenti berputar.

#### Peristiwa: 'Apakah-stop-pemuatan'

Sesuai dengan poin pada saat pemintal tab berhenti berputar.

#### Peristiwa: 'Apakah-mendapatkan-tanggapan-rincian'

Kembali:

* `acara` Acara
* `status` Boolean
* `URL baru` Tali
* `URL asli` Tali
* `kode tanggapan http` Bilangan bulat
* `metode permintaan` Tali
* `pengarah` Tali
* `header` Obyek
* `Jenissumberdaya` Tali

dipancarkan ketika rincian tentang sumber daya yang diminta tersedia. `status` menunjukkan koneksi soket untuk mendownload sumber daya.

#### Peristiwa: 'apakah-mendapatkan-pengalihan-permintaan'

Pengembalian:

* `acara` Acara
* `URL lama` Tali
* `URL baru` Tali
* `adalah Bingkai Utama` Boolean
* `kode tanggapan http` Bilangan bulat
* `metode permintaan` Tali
* `pengarah` Tali
* `header` Obyek

dipancarkan ketika pengalihan diterima saat meminta sumber daya.

#### Peristiwa: 'lokal-siap'

Pengembalian:

* `acara` Acara

dipancarkan saat dokumen dalam bingkai yang diberikan dimuat.

#### Peristiwa: 'halaman-favicon-diperbarui '

Pengembalian:

* `acara` Acara
* `favicons` Tali [] - serangkaian URL

Dibunyikan saat halaman menerima url favicon.

#### Peristiwa: 'baru-jendela'

Pengembalian:

* `acara` Acara
* `url` Tali
* `nama bingkai` tali
* `disposisi` String - dapat `default`, `latar depan-tab`, `latar belakang-tab`, `jendela baru`, `Simpan ke disk` dan `lainnya`.
* `pilihan` Objek - pilihan yang akan digunakan untuk menciptakan baru `jendela peramban`.
* `fitur tambahan` String [] - fitur tidak-standar (fitur tidak ditangani oleh Kromium atau elektron) diberikan kepada `jendela terbuka()`.

Dibunyikan apabila halaman yang permintaan untuk membuka jendela baru `url`. Itu bisa saja diminta oleh `jendela terbuka` atau link eksternal seperti `<a target='_blank'>`.

Secara default baru `Jendela Peramban` akan diciptakan untuk `url`.

Memanggil `acara mencegah Default()` akan mencegah elektron dari secara otomatis menciptakan baru `Jendela Peramban`. Jika Anda menelepon `event.preventDefault()` dan manual membuat baru `Jeendela Peramban` maka Anda harus mengatur `event.new Tamu` ke referensi contoh `Jendela Peramban` baru, gagal untuk melakukannya dapat mengakibatkan perilaku tak terduga. Sebagai contoh:

```javascript
JendelaPerambansay isiweb.on ('window baru ', (acara, url) = > {peristiwa.mencegahDefault() const menang = baru JendelaPeramban({tunjukkan: salah}) menang (' siap-untuk-menunjukkan ', () = > menang.menunjukkan()) menang. memuatURL(url) acara.tamu baru = menang})
```

#### Peristiwa: 'akan navigasi'

Pengembalian:

* `acara` Acara
* ` url </ 0> Tali</li>
</ul>

<p>dipancarkan saat pengguna atau halaman ingin memulai navigasi. Hal itu bisa terjadi ketikaObjek <code> jendela.lokasi </ 0> diubah atau pengguna mengklik link di halaman.
</p>

<p>Peristiwa ini tidak akan memancarkan saat navigasi dimulai secara pemrograman
API seperti <code>isi web memuat URL` dan `isi web kembali`.</p> 
  Itu juga tidak dibunyikan untuk navigations di halaman, seperti mengklik anchor link atau memperbarui `jendela.lokasi.hash`. Menggunakan acara `melakukan-menavigasi-di Halaman` untuk tujuan ini.
  
  Memanggil `peristiwa.mencegah Default()` akan mencegah navigasi.
  
  #### Peristiwa: 'akan navigasi'
  
  Pengembalian:
  
  * `acara` Acara
  * `url` Tali
  
  Dibunyikan apabila navigasi dilakukan.
  
  Acara ini tidak dibunyikan untuk navigations di halaman, seperti mengklik anchor link atau memperbarui `window.location.hash`. Menggunakan acara `melakukan-menavigasi-di Halaman` untuk tujuan ini.
  
  #### peristiwa: 'Apakah-menavigasi-di halaman'
  
  Pengembalian:
  
  * `acara` Acara
  * `url` Tali
  * `adalah Bingkai Utama` Boolean
  
  Dibunyikan saat navigasi dalam halaman terjadi.
  
  Saat navigasi dalam halaman terjadi, perubahan URL halaman tidak menyebabkan navigasi di luar halaman. Contoh dari hal ini adalah ketika jangkar link diklik atau saat peristiwa hash `perubahan hash` dipicu.
  
  #### Peristiwa: 'akan-mencegah-membongkar'
  
  Pengembalian:
  
  * `acara` Acara
  
  Dibunyikan apabila `sebelumnya` event handler adalah mencoba untuk membatalkan halaman membongkar.
  
  Memanggil `event.preventDefault()` akan mengabaikan `beforeunload` event handler dan memungkinkan halaman harus dibongkar.
  
  ```javascript
const {BrowserWindow, dialog} = require ('electron') const win = new BrowserWindow ({width: 800, height: 600}) win.webContents.on ('akan-mencegah-membongkar', (event) = > { const choice = dialog.showMessageBox (menang, {type: 'question', buttons: ['Leave', 'Stay'], title: 'Apakah Anda ingin meninggalkan situs ini?', pesan: 'Perubahan yang Anda buat mungkin tidak disimpan. ', defaultId: 0, cancelId: 1}) const leave = (pilihan === 0) if (leave) {event.preventDefault ()}})
```

#### Peristiwa: 'jatuh'

Pengembalian:

* `acara` Acara
* `terbunuh` Boolean

Dipancarkan ketika proses perender penembak atau terbunuh.

#### Peristiwa: 'plugin-jatuh'

Pengembalian:

* `acara` Acara
* `nama` Tali
* `Versi` String

Dibunyikan ketika proses plugin telah jatuh.

#### Event: 'menghancurkan'

Dibunyikan apabila `webContents` dihancurkan.

#### Acara: 'sebelum-masukan-event'

Pengembalian:

* `peristiwa` Peristiwa
* `masukan` Obyek - Input properti 
  * `jenis` String - baik `keyUp` atau `keyDown`
  * `kunci` String - setara dengan [KeyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `kode` String - setara dengan [KeyboardEvent.code](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `isAutoRepeat` Boolean - setara dengan [KeyboardEvent.repeat](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `pergeseran` Boolean - setara dengan [KeyboardEvent.shiftKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `kontrol` Boolean - setara dengan [KeyboardEvent.controlKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `Alt` Boolean - setara dengan [KeyboardEvent.altKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)
  * `meta` Boolean - setara dengan [KeyboardEvent.metaKey](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)

Dipancarkan sebelum membuat acara `keydown` dan `keyup` di halaman. Memanggil `event.preventDefault` akan mencegah halaman `keydown` / `keyup` peristiwa dan menu cara pintas.

Untuk hanya mencegah menu cara pintas, menggunakan [`setIgnoreMenuShortcuts`](#contentssetignoremenushortcuts):

```javascript
const {BrowserWindow} = require ('electron') 

misalkan win = new BrowserWindow ({width: 800, height: 600}) 

win.webContents.on ('before-input-event', (event, input) => { // Sebagai contoh, aktifkan pintasan keyboard menu aplikasi saat // Ctrl/Cmd sedang down.
  win.webContents.setIgnoreMenuShortcuts(!input.control && !input.meta)
})
```

#### Event: 'devtools-dibuka'

Emitted saat DevTools dibuka.

#### Event: 'devtools-ditutup'

Emitted saat DevTools ditutup.

#### Event: 'fokus devtools'

Emitted saat DevTools difokuskan / dibuka.

#### Acara: 'sertifikat-kesalahan'

Pengembalian:

* `peristiwa` Peristiwa
* ` url </ 0> String</li>
<li><code>error` String - Kode kesalahan
* `sertifikat` [Sertifikat](structures/certificate.md)
* `callback` Fungsi 
  * `Terpercaya` Boolean -Menunjukkan apakah sertifikat bisa dianggap terpercaya

Emitted ketika gagal untuk memverifikasi `sertifikat` untuk `url`.

Penggunaannya sama dengan [the `certificate-error` event of `app`](app.md#event-certificate-error).

#### Acara: 'pilih-klien-sertifikat'

Pengembalian:

* `peristiwa` Peristiwa
* `url` URL
* `certificateList` [Sertifikat[]](structures/certificate.md)
* `callback` Fungsi 
  * `sertifikat` [Sertifikat](structures/certificate.md) - Harus berupa sertifikat dari daftar yang diberikan

Emitted ketika sertifikat klien diminta.

Penggunaannya sama dengan [the `pilih-sertifikat-klien` acara `app`](app.md#event-select-client-certificate).

#### Acara: 'login'

Pengembalian:

* `peristiwa` Peristiwa
* `permintaan` Obyek 
  * `method` String
  * `url` URL
  * `perujuk` URL
* `authInfo` Obyek 
  * ` isProxy </ 0>  Boolean</li>
<li><code>skema` String
  * `host` String
  * `port` Integer
  * `realm` String
* `callback` Fungsi 
  * `namapengguna` String
  * `katasandi` String

Emitted ketika `webContents` ingin melakukan auth dasar.

Penggunaannya sama dengan [the `masuk` event of `app`](app.md#event-login).

#### Event: 'ditemukan-di-halaman'

Pengembalian:

* `peristiwa` Peristiwa
* `hasil` Obyek 
  * `requestId` Bilangan bulat
  * `activeMatchOrdinal` Bulat - posisi pertandingan aktif.
  * `pertandingan` Bulat - jumlah pertandingan.
  * `selectionArea` Objek - koordinat pertama pertandingan wilayah.
  * `finalUpdate` Boolean

Dipancarkan saat hasilnya tersedia [`webContents.findInPage`] permintaan.

#### Event: 'media-mulai-bermain''

Emitted saat media mulai diputar.

#### Event: 'media-berhenti'

Emitted saat media dijeda atau dilakukan bermain.

#### Event: 'apakah-ganti-tema-warna'

Emitted ketika warna tema halaman berubah. Hal ini biasanya karena bertemu sebuah meta tag:

```html
<meta name='theme-color' content='#ff0000'>
```

Pengembalian:

* `peristiwa` Peristiwa
* `color` (String | null) - Theme color is in format of '#rrggbb'. It is `null` when no theme color is set.
#### Event: 'update-target-url'

Pengembalian:

* `peristiwa` Peristiwa
* ` url </ 0> String</li>
</ul>

<p>Emitted saat mouse bergerak di atas sebuah link atau keyboard memindahkan fokus ke sebuah link.</p>

<h4>Event: 'kursor-berubah'</h4>

<p>Pengembalian:</p>

<ul>
<li><code>peristiwa` Peristiwa
* `type` String
* `image` NativeImage (optional)
* `scale` Float (optional) - scaling factor for the custom cursor
* `size` [Size](structures/size.md) (optional) - the size of the `image`
* `hotspot` [Point](structures/point.md) (optional) - coordinates of the custom cursor's hotspot

Emitted when the cursor's type changes. The `type` parameter can be `default`, `crosshair`, `pointer`, `text`, `wait`, `help`, `e-resize`, `n-resize`, `ne-resize`, `nw-resize`, `s-resize`, `se-resize`, `sw-resize`, `w-resize`, `ns-resize`, `ew-resize`, `nesw-resize`, `nwse-resize`, `col-resize`, `row-resize`, `m-panning`, `e-panning`, `n-panning`, `ne-panning`, `nw-panning`, `s-panning`, `se-panning`, `sw-panning`, `w-panning`, `move`, `vertical-text`, `cell`, `context-menu`, `alias`, `progress`, `nodrop`, `copy`, `none`, `not-allowed`, `zoom-in`, `zoom-out`, `grab`, `grabbing`, `custom`.

If the `type` parameter is `custom`, the `image` parameter will hold the custom cursor image in a `NativeImage`, and `scale`, `size` and `hotspot` will hold additional information about the custom cursor.

#### Event: 'menu konteks'

Pengembalian:

* `peristiwa` Peristiwa
* `params` Obyek 
  * `x` Integer - x coordinate
  * `y` Integer - y coordinate
  * `linkURL` String - URL of the link that encloses the node the context menu was invoked on.
  * `linkText` String - Text associated with the link. May be an empty string if the contents of the link are an image.
  * `pageURL` String - URL of the top level page that the context menu was invoked on.
  * `frameURL` String - URL of the subframe that the context menu was invoked on.
  * `srcURL` String - Source URL for the element that the context menu was invoked on. Elements with source URLs are images, audio and video.
  * `mediaType` String - Type of the node the context menu was invoked on. Can be `none`, `image`, `audio`, `video`, `canvas`, `file` or `plugin`.
  * `hasImageContents` Boolean - Whether the context menu was invoked on an image which has non-empty contents.
  * `isEditable` Boolean - Whether the context is editable.
  * `selectionText` String - Text of the selection that the context menu was invoked on.
  * `titleText` String - Title or alt text of the selection that the context was invoked on.
  * `misspelledWord` String - The misspelled word under the cursor, if any.
  * `frameCharset` String - The character encoding of the frame on which the menu was invoked.
  * `inputFieldType` String - If the context menu was invoked on an input field, the type of that field. Possible values are `none`, `plainText`, `password`, `other`.
  * `menuSourceType` String - Input source that invoked the context menu. Can be `none`, `mouse`, `keyboard`, `touch`, `touchMenu`.
  * `mediaFlags` Object - The flags for the media element the context menu was invoked on. 
    * `inError` Boolean - Whether the media element has crashed.
    * `isPaused` Boolean - Whether the media element is paused.
    * `isMuted` Boolean - Whether the media element is muted.
    * `hasAudio` Boolean - Whether the media element has audio.
    * `isLooping` Boolean - Whether the media element is looping.
    * `isControlsVisible` Boolean - Whether the media element's controls are visible.
    * `canToggleControls` Boolean - Whether the media element's controls are toggleable.
    * `canRotate` Boolean - Whether the media element can be rotated.
  * `editFlags` Object - These flags indicate whether the renderer believes it is able to perform the corresponding action. 
    * `canUndo` Boolean - Whether the renderer believes it can undo.
    * `canRedo` Boolean - Whether the renderer believes it can redo.
    * `canCut` Boolean - Whether the renderer believes it can cut.
    * `canCopy` Boolean - Whether the renderer believes it can copy
    * `canPaste` Boolean - Whether the renderer believes it can paste.
    * `canDelete` Boolean - Whether the renderer believes it can delete.
    * `canSelectAll` Boolean - Whether the renderer believes it can select all.

Emitted when there is a new context menu that needs to be handled.

#### Event: 'Pilih--perangkat bluetooth'

Pengembalian:

* `peristiwa` Peristiwa
* `devices` [BluetoothDevice[]](structures/bluetooth-device.md)
* `callback` Fungsi 
  * `IdentitasAlat` String

Emitted when bluetooth device needs to be selected on call to `navigator.bluetooth.requestDevice`. To use `navigator.bluetooth` api `webBluetooth` should be enabled. If `event.preventDefault` is not called, first available device will be selected. `callback` should be called with `deviceId` to be selected, passing empty string to `callback` will cancel the request.

```javascript
const {app, webContents} = require('electron') app.commandLine.appendSwitch('enable-web-bluetooth') app.on ('siap', () = > {webContents.on (' perangkat pilih bluetooth', (acara, deviceList, callback) = > {event.preventDefault() membiarkan hasil = deviceList.find((device) = > {kembali device.deviceName === 'test'}) jika (! hasil) {callback('')} lain {callback(result.deviceId)}})})
```

#### Event: 'cat'

Pengembalian:

* `peristiwa` Peristiwa
* `dirtyRect` [Rectangle](structures/rectangle.md)
* `image` [NativeImage](native-image.md) - The image data of the whole frame.

Emitted when a new frame is generated. Only the dirty area is passed in the buffer.

```javascript
const {BrowserWindow} = require('electron') membiarkan memenangkan = BrowserWindow baru ({webPreferences: {offscreen: true}}) win.webContents.on ('cat', (acara, kotor, gambar) = > {/ / updateBitmap (kotor, image.getBitmap())}) win.loadURL ('http://github.com')
```

#### Event: 'devtools-reload-halaman'

Emitted when the devtools window instructs the webContents to reload

#### Event: 'akan-melampirkan-webview'

Pengembalian:

* `peristiwa` Peristiwa
* `webPreferences` Object - The web preferences that will be used by the guest page. This object can be modified to adjust the preferences for the guest page.
* `params` Object - The other `<webview>` parameters such as the `src` URL. This object can be modified to adjust the parameters of the guest page.

Emitted when a `<webview>`'s web contents is being attached to this web contents. Calling `event.preventDefault()` will destroy the guest page.

This event can be used to configure `webPreferences` for the `webContents` of a `<webview>` before it's loaded, and provides the ability to set settings that can't be set via `<webview>` attributes.

**Note:** The specified `preload` script option will be appear as `preloadURL` (not `preload`) in the `webPreferences` object emitted with this event.

#### Event: 'did-attach-webview'

Pengembalian:

* `peristiwa` Peristiwa
* `webContents` WebContents - The guest web contents that is used by the `<webview>`.

Emitted when a `<webview>` has been attached to this web contents.

#### Event: 'console-message'

Pengembalian:

* `level` Integer
* ` pesan </ 0> String</li>
<li><code>line` Integer
* `sourceId` String

Emitted when the associated window logs a console message. Will not be emitted for windows with *offscreen rendering* enabled.

### Metode Instance

#### `contents.loadURL(url[, options])`

* ` url </ 0> String</li>
<li><code>pilihan` Objek (opsional) 
  * `httpReferrer` String (opsional) - url perujuk HTTP.
  * `userAgent` String (opsional) - agen pengguna berasal permintaan.
  * `extraHeaders` String (opsional) - header tambahan yang dipisahkan oleh "\n"
  * `postData` ([[UploadRawData]](structures/upload-raw-data.md) | [[UploadFile]](structures/upload-file.md) | [[UploadFileSystem]](structures/upload-file-system.md) | [[UploadBlob]](structures/upload-blob.md)) -(opsional)
  * `baseURLForDataURL` String (opsional) - url dasar (dengan trailing pemisah path) untuk file yang akan diambil oleh data url. Hal ini diperlukan hanya jika ditentukan `url` data url dan perlu memuat file lainnya.

Loads the `url` in the window. The `url` must contain the protocol prefix, e.g. the `http://` or `file://`. If the load should bypass http cache then use the `pragma` header to achieve it.

```javascript
const {webContents} = require('electron') opsi const = {extraHeaders: ' pragma: no-cache\n'} webContents.loadURL ('https://github.com', opsi)
```

#### `contents.downloadURL(url)`

* ` url </ 0> String</li>
</ul>

<p>Initiates a download of the resource at <code>url` without navigating. The `will-download` event of `session` will be triggered.</p> 
  #### `contents.getURL()`
  
  Returns `String` - The URL of the current web page.
  
  ```javascript
const {BrowserWindow} = require('electron') membiarkan memenangkan = baru BrowserWindow({width: 800, height: 600}) win.loadURL ('http://github.com') Biarkan currentURL = win.webContents.getURL() console.log(currentURL)
```

#### `contents.getTitle()`

Returns `String` - The title of the current web page.

#### `contents.isDestroyed()`

Returns `Boolean` - Whether the web page is destroyed.

#### `contents.focus()`

Focuses the web page.

#### `contents.isFocused()`

Returns `Boolean` - Whether the web page is focused.

#### `contents.isLoading()`

Returns `Boolean` - Whether web page is still loading resources.

#### `contents.isLoadingMainFrame()`

Returns `Boolean` - Whether the main frame (and not just iframes or frames within it) is still loading.

#### `contents.isWaitingForResponse()`

Returns `Boolean` - Whether the web page is waiting for a first-response from the main resource of the page.

#### `contents.stop()`

Menghentikan navigasi yang tertunda.

#### `contents.reload()`

Reloads the current web page.

#### `contents.reloadIgnoringCache()`

Reloads current page and ignores cache.

#### `contents.canGoBack()`

Returns `Boolean` - Whether the browser can go back to previous web page.

#### `contents.canGoForward()`

Returns `Boolean` - Whether the browser can go forward to next web page.

#### `contents.canGoToOffset(offset)`

* `offset` Integer

Returns `Boolean` - Whether the web page can go to `offset`.

#### `contents.clearHistory()`

Menghapus sejarah navigasi.

#### `contents.goBack()`

Makes the browser go back a web page.

#### `contents.goForward()`

Makes the browser go forward a web page.

#### `contents.goToIndex(index)`

* `indeks` Integer

Navigates browser to the specified absolute web page index.

#### `contents.goToOffset(offset)`

* `offset` Integer

Arahkan ke offset yang ditentukan dari "entri saat ini".

#### `contents.isCrashed()`

Mengembalikan `Boolean` - Apakah proses renderer telah jatuh.

#### `contents.setUserAgent(userAgent)`

* `userAgent` String

Overrides the user agent for this web page.

#### `contents.getUserAgent()`

Returns `String` - The user agent for this web page.

#### `contents.insertCSS(css)`

* `css` String

Injects CSS into the current web page.

#### `contents.executeJavaScript(code[, userGesture, callback])`

* ` kode </ 0> String</li>
<li><code>userGesture` Boolean (opsional) - Default adalah `false`.
* `callback` Fungsi (opsional) - Dipanggil setelah script telah dieksekusi. 
  * `hasil` Ada

Mengembalikan `Janji` - Janji yang diselesaikan dengan hasil kode yang dijalankan atau ditolak jika hasil dari kode tersebut adalah janji yang ditolak.

Evaluasi `kode` di halaman.

Di jendela browser beberapa API HTML seperti `requestFullScreen` hanya bisa dipanggil oleh isyarat dari pengguna. Setting `userGesture` ke `true` akan dihapus keterbatasan ini.

If the result of the executed code is a promise the callback result will be the resolved value of the promise. We recommend that you use the returned Promise to handle code that results in a Promise.

```js
isi.executeJavaScript('ambil("https://jsonplaceholder.typicode.com/users/1"). kemudian (resp => resp.json())', true)
  .Kemudian ((hasil) => {
    console.log (result) // Akan menjadi objek JSON dari fetch call
  })
```

#### `contents.setIgnoreMenuShortcuts(ignore)` *Experimental*

* `abaikan` Boolean

Ignore application menu shortcuts while this web contents is focused.

#### `contents.setAudioMuted(muted)`

* `dibungkam` Boolean

Mute the audio on the current web page.

#### `contents.isAudioMuted()`

Returns `Boolean` - Whether this page has been muted.

#### `contents.setZoomFactor(factor)`

* `faktor` Angka - Faktor zoom.

Mengubah faktor pembesaran ke faktor yang ditentukan. Faktor zoom adalah zoom persen dibagi dengan 100, sehingga 300% = 3,0.

#### `contents.getZoomFactor(callback)`

* `callback` Fungsi 
  * `zoomFactor` Number

Sends a request to get current zoom factor, the `callback` will be called with `callback(zoomFactor)`.

#### `contents.setZoomLevel(level)`

* `level` Angka - level zoom

Mengubah tingkat zoom ke tingkat tertentu. Ukuran aslinya adalah 0 dan masing-masing Peningkatan atas atau di bawah mewakili zoom 20% lebih besar atau lebih kecil ke default batas 300% dan 50% dari ukuran aslinya, berurutan.

#### `contents.getZoomLevel(callback)`

* `callback` Fungsi 
  * `zoomLevel` Number

Sends a request to get current zoom level, the `callback` will be called with `callback(zoomLevel)`.

#### `contents.setZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimumLevel` Nomor
* `minimumLevel` Nomor

**Tidak berlaku lagi:**Panggil`setVisualZoomLevelLimits` untuk mengatur zoom visual batas tingkat Metode ini akan dihapus di Electron 2.0.

#### `contents.setVisualZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimumLevel` Nomor
* `minimumLevel` Nomor

Menetapkan maksimum dan minimum tingkat mencubit-to-zoom.

#### `contents.setLayoutZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimumLevel` Nomor
* `minimumLevel` Nomor

Menetapkan tingkat zoom maksimal dan minimal berbasis tata letak (yaitu bukan-visual).

#### `contents.undo()`

Executes the editing command `undo` in web page.

#### `contents.redo()`

Executes the editing command `redo` in web page.

#### `contents.cut()`

Executes the editing command `cut` in web page.

#### `contents.copy()`

Executes the editing command `copy` in web page.

#### `contents.copyImageAt(x, y)`

* `x` Integer
* `y` Integer

Copy the image at the given position to the clipboard.

#### `contents.paste()`

Executes the editing command `paste` in web page.

#### `contents.pasteAndMatchStyle()`

Executes the editing command `pasteAndMatchStyle` in web page.

#### `contents.delete()`

Executes the editing command `delete` in web page.

#### `contents.selectAll()`

Executes the editing command `selectAll` in web page.

#### `contents.unselect()`

Executes the editing command `unselect` in web page.

#### `contents.replace(text)`

* ` teks </ 0>  String</li>
</ul>

<p>Executes the editing command <code>replace` in web page.</p> 
  #### `contents.replaceMisspelling(text)`
  
  * ` teks </ 0>  String</li>
</ul>

<p>Executes the editing command <code>replaceMisspelling` in web page.</p> 
    #### `contents.insertText(text)`
    
    * ` teks </ 0>  String</li>
</ul>

<p>Sisipan <code>teks` ke elemen yang terfokus.</p> 
      #### `contents.findInPage(text[, options])`
      
      * `text` String - Konten yang akan dicari, tidak boleh kosong.
      * `pilihan` Objek (opsional) 
        * `forward` Boolean - (opsional) Baik untuk mencari maju atau mundur, default ke `true`.
        * `findNext` Boolean - (opsional) Apakah operasi tersebut merupakan permintaan pertama atau tindak lanjut, default ke `false`.
        * `matchCase` Boolean - (opsional) Apakah pencarian harus sensitif huruf, default ke `false`.
        * `wordStart` Boolean - (opsional) Baik untuk melihat hanya pada awal kata-kata. default ke `false`.
        * `medialCapitalAsWordStart` Boolean - (opsional) Bila digabungkan dengan `wordStart`, menerima sebuah pertandingan di tengah sebuah kata jika pertandingan dimulai dengan sebuah huruf besar diikuti huruf kecil atau huruf non. Menerima beberapa kecocokan intra-kata lainnya, defaultnya adalah `false`.
      
      Returns `Integer` - The request id used for the request.
      
      Starts a request to find all matches for the `text` in the web page. The result of the request can be obtained by subscribing to [`found-in-page`](web-contents.md#event-found-in-page) event.
      
      #### `contents.stopFindInPage(action)`
      
      * `tindakan` String - Specifies the action to take place when ending [`webContents.findInPage`] request. 
        * `clearSelection` - jelas pilihan.
        * `keepSelection` - menerjemahkan pemilihan menjadi sebuah pilihan yang normal.
        * `activateSelection` - fokus dan klik seleksi simpul.
      
      Stops any `findInPage` request for the `webContents` with the provided `action`.
      
      ```javascript
const {webContents} = require('electron') webContents.on (' ditemukan-di-halaman ', (acara, hasil) = > {jika webContents.stopFindInPage('clearSelection') (result.finalUpdate)}) const requestId = webContents.findInPage('api') console.log(requestId)
```
  
  #### `contents.capturePage([rect, ]callback)`
  
  * `rect` [Persegi panjang](structures/rectangle.md) (opsional) - daerah halaman untuk ditangkap
  * `callback` Fungsi 
    * ` gambar </ 0>  <a href="native-image.md"> gambar asli </ 1></li>
</ul></li>
</ul>

<p>Captures a snapshot of the page within <code>rect`. Upon completion `callback` will be called with `callback(image)`. The `image` is an instance of [NativeImage](native-image.md) that stores data of the snapshot. Omitting `rect` will capture the whole visible page.</p> 
      #### `contents.hasServiceWorker(callback)`
      
      * `callback` Fungsi 
        * `hasWorker` Boolean
      
      Checks if any ServiceWorker is registered and returns a boolean as response to `callback`.
      
      #### `contents.unregisterServiceWorker(callback)`
      
      * `callback` Fungsi 
        * `success` Boolean
      
      Unregisters any ServiceWorker if present and returns a boolean as response to `callback` when the JS promise is fulfilled or false when the JS promise is rejected.
      
      #### `contents.getPrinters()`
      
      Get the system printer list.
      
      Returns [`PrinterInfo[]`](structures/printer-info.md)
      
      #### `contents.print([options], [callback])`
      
      * `pilihan` Objek (opsional) 
        * `diam` Boolean (opsional) - Jangan tanya pengguna untuk pengaturan cetak. Defaultnya adalah `false`.
        * `printBackground` Boolean (opsional) - Juga mencetak warna latar belakang dan gambar halaman web Defaultnya adalah `false`.
        * `deviceName` String (opsional) - mengatur printer nama perangkat untuk menggunakan. Default adalah `"`.
      * `callback` Fungsi (opsional) 
        * success` Boolean - Indicates success of the print call.
      
      Prints window's web page. When `silent` is set to `true`, Electron will pick the system's default printer if `deviceName` is empty and the default settings for printing.
      
      Calling `window.print()` in web page is equivalent to calling `webContents.print({silent: false, printBackground: false, deviceName: ''})`.
      
      Use `page-break-before: always;` CSS style to force to print to a new page.
      
      #### `contents.printToPDF(options, callback)`
      
      * `pilihan` Obyek 
        * `marginType` Integer - (opsional) Menentukan jenis margin yang akan digunakan. Menggunakan 0 untuk margin default, 1 tanpa margin, dan 2 untuk margin minimum.
        * `pageSize` String - (opsional) Tentukan ukuran halaman PDF yang dihasilkan. Can be`A3`,`A4`,`A5`,` Legal `,`Letter`,`Tabloid` or an Object containing `height` and `width` in microns.
        * `printBackground` Boolean - (opsional) Baik untuk mencetak latar belakang CSS.
        * `printSelectionOnly` Boolean - (opsional) Baik untuk mencetak pilihan saja.
        * `landscape` Boolean - (opsional) `true` untuk landscape, `false` untuk potret.
      * `callback` Fungsi 
        * Kesalahan `kesalahan`
        * `data` nomor
      
      Prints window's web page as PDF with Chromium's preview printing custom settings.
      
      The `callback` will be called with `callback(error, data)` on completion. The `data` is a `Buffer` that contains the generated PDF data.
      
      The `landscape` will be ignored if `@page` CSS at-rule is used in the web page.
      
      By default, an empty `options` will be regarded as:
      
      ```javascript
{marginsType: 0, printBackground: false, printSelectionOnly: false, landscape: false}
```
  
  Use `page-break-before: always;` CSS style to force to print to a new page.
  
  An example of `webContents.printToPDF`:
  
  ```javascript
const {BrowserWindow} = require ('electron') const fs = require ('fs') let win = new BrowserWindow ({width: 800, height: 600}) win.loadURL ('http://github.com') win.webContents.on ('did-finish-load', () = > {// Use default printing options win.webContents.printToPDF ({}, (error, data) = > {if (error) throw error fs.writeFile ('/ tmp / print.pdf', data, (error) = > {if (error) throw error console.log ('Write PDF successfully.')})})})
```

#### `contents.addWorkSpace(path)`

* `path` String

Adds the specified path to DevTools workspace. Must be used after DevTools creation:

```javascript
const {BrowserWindow} = require ('electron') let win = new BrowserWindow () win.webContents.on ('devtools-opened', () = > {win.webContents.addWorkSpace (__ dirname)})
```

#### `contents.removeWorkSpace(path)`

* `path` String

Removes the specified path from DevTools workspace.

#### `contents.openDevTools([options])`

* `pilihan` Objek (opsional) 
  * `mode` String - Opens the devtools with specified dock state, can be `right`, `bottom`, `undocked`, `detach`. Defaults to last used dock state. In `undocked` mode it's possible to dock back. In `detach` mode it's not.

Opens the devtools.

#### `contents.closeDevTools()`

Closes the devtools.

#### `contents.isDevToolsOpened()`

Returns `Boolean` - Whether the devtools is opened.

#### `contents.isDevToolsFocused()`

Returns `Boolean` - Whether the devtools view is focused .

#### `contents.toggleDevTools()`

Toggles the developer tools.

#### `contents.inspectElement(x, y)`

* `x` Integer
* `y` Integer

Starts inspecting element at position (`x`, `y`).

#### `contents.inspectServiceWorker()`

Opens the developer tools for the service worker context.

#### `contents.send(channel[, arg1][, arg2][, ...])`

* ` saluran </ 0>  String</li>
<li><code> ... args </ 0> ada []</li>
</ul>

<p>Kirim pesan asinkron ke proses renderer melalui <code>channel`, Anda juga bisa mengirim argumen sewenang wenang. Argumen akan diserialkan di JSON secara internal dan karenanya tidak ada fungsi atau rantai prototipe yang akan disertakan.</p> 
  The renderer process can handle the message by listening to `channel` with the `ipcRenderer` module.
  
  An example of sending messages from the main process to the renderer process:
  
  ```javascript
// Dalam proses utamanya.
const {app, BrowserWindow} = require('electron')
let win = null

app.on('ready', () => {
  win = new BrowserWindow({width: 800, height: 600})
  win.loadURL(`file://${__dirname}/index.html`)
  win.webContents.on('did-finish-load', () => {
    win.webContents.send('ping', 'whoooooooh!')
  })
})
```

```html
<!-- index.html -->
<html>
<body>
  <script>
    require('electron').ipcRenderer.on('ping', (event, message) => {
      console.log(message)  // Prints 'whoooooooh!'
    })
  </script>
</body>
</html>
```

#### `contents.enableDeviceEmulation(parameters)`

* `parameters` Obyek 
  * `screenPosition` String - Specify the screen type to emulate (default: `Desktop`) 
    * `desktop` - Desktop screen type
    * `mobile` - Mobile screen type
  * `screenSize` [Size](structures/size.md) - Set the emulated screen size (screenPosition == mobile)
  * `viewPosition` [Point](structures/point.md) - Position the view on the screen (screenPosition == mobile) (default: `{x: 0, y: 0}`)
  * `deviceScaleFactor` Integer - Set the device scale factor (if zero defaults to original device scale factor) (default: ``)
  * `viewSize` [Size](structures/size.md) - Set the emulated view size (empty means no override)
  * `fitToView` Boolean - Whether emulated view should be scaled down if necessary to fit into available space (default: `false`)
  * `offset` [Point](structures/point.md) - Offset of the emulated view inside available space (not in fit to view mode) (default: `{x: 0, y: 0}`)
  * `scale` Float - Scale of emulated view inside available space (not in fit to view mode) (default: `1`)

Enable device emulation with the given parameters.

#### `contents.disableDeviceEmulation()`

Disable device emulation enabled by `webContents.enableDeviceEmulation`.

#### `contents.sendInputEvent(event)`

* `peristiwa` Obyek 
  * `type` String (**required**) - The type of the event, can be `mouseDown`, `mouseUp`, `mouseEnter`, `mouseLeave`, `contextMenu`, `mouseWheel`, `mouseMove`, `keyDown`, `keyUp`, `char`.
  * `modifiers` String[] - An array of modifiers of the event, can include `shift`, `control`, `alt`, `meta`, `isKeypad`, `isAutoRepeat`, `leftButtonDown`, `middleButtonDown`, `rightButtonDown`, `capsLock`, `numLock`, `left`, `right`.

Sends an input `event` to the page. **Note:** The `BrowserWindow` containing the contents needs to be focused for `sendInputEvent()` to work.

For keyboard events, the `event` object also have following properties:

* `keyCode` String (**required**) - The character that will be sent as the keyboard event. Should only use the valid key codes in [Accelerator](accelerator.md).

For mouse events, the `event` object also have following properties:

* `x` Integer (**required**)
* `y` Integer (**required**)
* `button` String - The button pressed, can be `left`, `middle`, `right`
* `globalX` Integer
* `globalY` Integer
* `movementX` Integer
* `movementY` Integer
* `clickCount` Integer

For the `mouseWheel` event, the `event` object also have following properties:

* `deltaX` Integer
* `deltaY` Integer
* `wheelTicksX` Integer
* `wheelTicksY` Integer
* `accelerationRatioX` Integer
* `accelerationRatioY` Integer
* `hasPreciseScrollingDeltas` Boolean
* `canScroll` Boolean
#### `contents.beginFrameSubscription([onlyDirty ,]callback)`

* `onlyDirty` Boolean (optional) - Defaults to `false`
* `callback` Fungsi 
  * `frameBuffer` Buffer
  * `dirtyRect` [Rectangle](structures/rectangle.md)

Begin subscribing for presentation events and captured frames, the `callback` will be called with `callback(frameBuffer, dirtyRect)` when there is a presentation event.

The `frameBuffer` is a `Buffer` that contains raw pixel data. On most machines, the pixel data is effectively stored in 32bit BGRA format, but the actual representation depends on the endianness of the processor (most modern processors are little-endian, on machines with big-endian processors the data is in 32bit ARGB format).

The `dirtyRect` is an object with `x, y, width, height` properties that describes which part of the page was repainted. If `onlyDirty` is set to `true`, `frameBuffer` will only contain the repainted area. `onlyDirty` defaults to `false`.

#### `contents.endFrameSubscription()`

End subscribing for frame presentation events.

#### `contents.startDrag(item)`

* `item` Obyek 
  * `file` String or `files` Array - The path(s) to the file(s) being dragged.
  * `icon` [NativeImage](native-image.md) - The image must be non-empty on macOS.

Sets the `item` as dragging item for current drag-drop operation, `file` is the absolute path of the file to be dragged, and `icon` is the image showing under the cursor when dragging.

#### `contents.savePage(fullPath, saveType, callback)`

* `fullPath` String - The full file path.
* `saveType` String - Specify the save type. 
  * `HTMLOnly` - Save only the HTML of the page.
  * `HTMLComplete` - Save complete-html page.
  * `MHTML` - Save complete-html page as MHTML.
* `callback` Function - `(error) => {}`. 
  * Kesalahan `kesalahan`

Returns `Boolean` - true if the process of saving page has been initiated successfully.

```javascript
const {BrowserWindow} = require ('electron') let win = new BrowserWindow () win.loadURL ('https://github.com') win.webContents.on ('did-finish-load', () = > {win.webContents.savePage ('/ tmp / test.html', 'HTMLComplete', (error) = > {if (! error) console.log ('Selamatkan halaman berhasil')})})
```

#### `contents.showDefinitionForSelection()` *macOS*

Menampilkan kamus pop-up yang mencari kata yang dipilih pada halaman.

#### `contents.setSize(options)`

Set the size of the page. This is only supported for `<webview>` guest contents.

* `pilihan` Obyek 
  * `normal` Object (optional) - Normal size of the page. This can be used in combination with the [`nonaktifkanukurkembalitamu`](web-view-tag.md#disableguestresize) attribute to manually resize the webview guest contents. 
    * ` width </ 0>  Integer</li>
<li><code> tinggi </ 0>  Integer</li>
</ul></li>
</ul></li>
</ul>

<h4><code>contents.isOffscreen()`</h4> 
      Returns `Boolean` - Indicates whether *offscreen rendering* is enabled.
      
      #### `contents.startPainting()`
      
      If *offscreen rendering* is enabled and not painting, start painting.
      
      #### `contents.stopPainting()`
      
      If *offscreen rendering* is enabled and painting, stop painting.
      
      #### `contents.isPainting()`
      
      Returns `Boolean` - If *offscreen rendering* is enabled returns whether it is currently painting.
      
      #### `contents.setFrameRate(fps)`
      
      * `fps` Integer
      
      If *offscreen rendering* is enabled sets the frame rate to the specified number. Only values between 1 and 60 are accepted.
      
      #### `contents.getFrameRate()`
      
      Returns `Integer` - If *offscreen rendering* is enabled returns the current frame rate.
      
      #### `contents.invalidate()`
      
      Schedules a full repaint of the window this web contents is in.
      
      If *offscreen rendering* is enabled invalidates the frame and generates a new one through the `'paint'` event.
      
      #### `contents.getWebRTCIPHandlingPolicy()`
      
      Returns `String` - Returns the WebRTC IP Handling Policy.
      
      #### `contents.setWebRTCIPHandlingPolicy(policy)`
      
      * `policy` String - Specify the WebRTC IP Handling Policy. 
        * `default` - Exposes user's public and local IPs. This is the default behavior. When this policy is used, WebRTC has the right to enumerate all interfaces and bind them to discover public interfaces.
        * `default_public_interface_only` - Exposes user's public IP, but does not expose user's local IP. When this policy is used, WebRTC should only use the default route used by http. This doesn't expose any local addresses.
        * `default_public_and_private_interfaces` - Exposes user's public and local IPs. When this policy is used, WebRTC should only use the default route used by http. This also exposes the associated default private address. Default route is the route chosen by the OS on a multi-homed endpoint.
        * `disable_non_proxied_udp` - Does not expose public or local IPs. When this policy is used, WebRTC should only use TCP to contact peers or servers unless the proxy server supports UDP.
      
      Setting the WebRTC IP handling policy allows you to control which IPs are exposed via WebRTC. See [BrowserLeaks](https://browserleaks.com/webrtc) for more details.
      
      #### `contents.getOSProcessId()`
      
      Returns `Integer` - The `pid` of the associated renderer process.
      
      ### Instance Properties
      
      #### `contents.id`
      
      A `Integer` representing the unique ID of this WebContents.
      
      #### `contents.session`
      
      A [`Session`](session.md) used by this webContents.
      
      #### `contents.hostWebContents`
      
      A [`WebContents`](web-contents.md) instance that might own this `WebContents`.
      
      #### `contents.devToolsWebContents`
      
      A `WebContents` of DevTools for this `WebContents`.
      
      **Note:** Users should never store this object because it may become `null` when the DevTools has been closed.
      
      #### `contents.debugger`
      
      A [Debugger](debugger.md) instance for this webContents.