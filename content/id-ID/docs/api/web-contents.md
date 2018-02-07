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

## Methods

Metode ini dapat diakses dari modul `isi web`:

```javascript
const {webContents} = require('electron')
console.log(webContents)
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

Process: [Main](../glossary.md#main-process)

### Perihal contoh

#### Event: 'Apakah-selesai-load'

Dibunyikan apabila navigasi dilakukan, yakni pemintal tab telah berhenti berputar dan acara `pada beban` dikirim.

#### Peristiwa: 'Apakah-gagal-beban'

Kembali:

* `event` Event
* `kode kesalahan` Bilangan bulat
* `Deskripsi kesalahan` Tali
* `memvalidasi URL` Tali
* `adalah Bingkai Utama` Boolean

Acara ini seperti `Apakah-selesai-beban` tapi dipancarkan ketika beban gagal atau dibatalkan, misalnya `jendela.berhenti()` dipanggil. Daftar lengkap kode galat dan makna mereka tersedia [di sini](https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h).

#### Peristiwa: 'Apakah-frame-selesai-beban'

Pengembalian:

* `event` Event
* `adalah Bingkai Utama` Boolean

Dibunyikan apabila bingkai telah melakukan navigasi.

#### Peristiwa: 'Apakah-mulai-pemuatan'

Sesuai dengan poin pada saat pemintal tab berhenti berputar.

#### Peristiwa: 'Apakah-stop-pemuatan'

Sesuai dengan poin pada saat pemintal tab berhenti berputar.

#### Peristiwa: 'Apakah-mendapatkan-tanggapan-rincian'

Pengembalian:

* `event</ 0> Acara</li>
<li><code>status` Boolean
* `URL baru` Tali
* `URL asli` Tali
* `kode tanggapan http` Bilangan bulat
* `metode permintaan` Tali
* `pengarah` Tali
* `header` Obyek
* `TipeSumberdaya` String

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

* `event` Acara

dipancarkan saat dokumen dalam bingkai yang diberikan dimuat.

#### Peristiwa: 'halaman-favicon-diperbarui '

Pengembalian:

* `event` Acara
* `favicons` Tali [] - serangkaian URL

Dibunyikan saat halaman menerima url favicon.

#### Peristiwa: 'baru-jendela'

Pengembalian:

* `event</ 0> Acara</li>
<li><code>url` String
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
* `url` String

dipancarkan saat pengguna atau halaman ingin memulai navigasi. Hal itu bisa terjadi ketikaObjek ` jendela.lokasi </ 0> diubah atau pengguna mengklik link di halaman.
</p>

<p>Peristiwa ini tidak akan memancarkan saat navigasi dimulai secara pemrograman
API seperti <code>isi web memuat URL` dan `isi web kembali`.

Itu juga tidak dibunyikan untuk navigations di halaman, seperti mengklik anchor link atau memperbarui `jendela.lokasi.hash`. Menggunakan acara `melakukan-menavigasi-di Halaman` untuk tujuan ini.

Memanggil `peristiwa.mencegah Default()` akan mencegah navigasi.

#### Peristiwa: 'akan navigasi'

Pengembalian:

* `acara` Acara
* `url` String

Dibunyikan apabila navigasi dilakukan.

Acara ini tidak dibunyikan untuk navigations di halaman, seperti mengklik anchor link atau memperbarui `window.location.hash`. Menggunakan acara `melakukan-menavigasi-di Halaman` untuk tujuan ini.

#### peristiwa: 'Apakah-menavigasi-di halaman'

Pengembalian:

* `acara` Acara
* ` url </ 0> String</li>
<li><code>adalah Bingkai Utama` Boolean

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
* ` nama </ 0>  String</li>
<li><code>Versi` String

Dibunyikan ketika proses plugin telah jatuh.

#### Event: 'menghancurkan'

Dibunyikan apabila `webContents` dihancurkan.

#### Acara: 'sebelum-masukan-event'

Pengembalian:

* `acara` Acara
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

* `event</ 0> Acara</li>
<li><code>url` String
* `error` String - Kode kesalahan
* `sertifikat` [Sertifikat](structures/certificate.md)
* `callback` Fungsi 
  * `Terpercaya` Boolean -Menunjukkan apakah sertifikat bisa dianggap terpercaya

Emitted ketika gagal untuk memverifikasi `sertifikat` untuk `url`.

Penggunaannya sama dengan [the `certificate-error` event of `app`](app.md#event-certificate-error).

#### Acara: 'pilih-klien-sertifikat'

Pengembalian:

* `acara` Acara
* `url` URL
* `certificateList` [Sertifikat[]](structures/certificate.md)
* `callback` Fungsi 
  * `sertifikat` [Sertifikat](structures/certificate.md) - Harus berupa sertifikat dari daftar yang diberikan

Emitted ketika sertifikat klien diminta.

Penggunaannya sama dengan [the `pilih-sertifikat-klien` acara `app`](app.md#event-select-client-certificate).

#### Acara: 'login'

Pengembalian:

* `acara` Acara
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

* `acara` Acara
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

* `acara` Acara
* `color` (String | null) - Theme color is in format of '#rrggbb'. It is `null` when no theme color is set.

#### Event: 'update-target-url'

Pengembalian:

* `event</ 0> Acara</li>
<li><code> url </ 0> String</li>
</ul>

<p>Emitted saat mouse bergerak di atas sebuah link atau keyboard memindahkan fokus ke sebuah link.</p>

<h4>Event: 'kursor-berubah'</h4>

<p>Pengembalian:</p>

<ul>
<li><code>acara` Acara
* `jenis` String
* `gambar` NativeImage (opsional)
* `skala` Mengambang (opsional) - skala faktor untuk kursor kustom
* `ukuran` [Ukuran](structures/size.md) (opsional) - ukuran `gambar`
* `hotspot` [Titik](structures/point.md) (opsional) - koordinat kursor kustom Hotspot

Emitted saat tipe kursor berubah. The `type` parameter can be `default`, `crosshair`, `pointer`, `text`, `wait`, `help`, `e-resize`, `n-resize`, `ne-resize`, `nw-resize`, `s-resize`, `se-resize`, `sw-resize`, `w-resize`, `ns-resize`, `ew-resize`, `nesw-resize`, `nwse-resize`, `col-resize`, `row-resize`, `m-panning`, `e-panning`, `n-panning`, `ne-panning`, `nw-panning`, `s-panning`, `se-panning`, `sw-panning`, `w-panning`, `move`, `vertical-text`, `cell`, `context-menu`, `alias`, `progress`, `nodrop`, `copy`, `none`, `not-allowed`, `zoom-in`, `zoom-out`, `grab`, `grabbing`, `custom`.

Jika `jenis` parameternya `custom`, itu `gambar` Parameter akan menahan custom gambar kursor dalam `GambarAsli`, dan `skala`, `size` and `hotspot` akan memegang informasi tambahan tentang kursor khusus.

#### Event: 'menu konteks'

Pengembalian:

* `acara` Acara
* `params` Obyek 
  * `x` koordinat Integer - x
  * ` y </ 0>  Koordinat integer</li>
<li><code> linkURL </ 0>  String - URL tautan yang membungkus node menu konteks dipanggil.</li>
<li><code> linkText </ 0>  String - Teks yang terkait dengan tautan. Mungkin berupa string kosong
 jika isi link adalah gambar.</li>
<li><code> pageURL ` String - URL halaman tingkat atas yang diikuti menu konteks.
  * `frameURL` String - URL subframe yang diikuti menu konteks.
  * `srcURL` String - URL Sumber untuk elemen yang menu konteksnya dipanggil. Elemen dengan URL sumber adalah gambar, audio dan video.
  * `mediaType` String - jenis node menu konteks dipanggil pada. Bisa `none`, ` gambar`, `audio`, `video`, `kanvas`, `file` atau `plugin`.
  * `hasImageContents` Boolean - Apakah menu konteks dipanggil pada gambar yang isinya tidak kosong.
  * `isEditable` Boolean - Apakah konteks dapat diedit.
  * `selectionText` String - Teks pilihan bahwa menu konteks dipanggil.
  * `titleText` String - Judul atau teks alt dari pilihan yang konteksnya dipanggil.
  * `salah eja` String - Kata salah eja di bawah kursor, jika ada.
  * `frameCharset` String - Pengkodean karakter dari bingkai tempat menu dipanggil.
  * `inputFieldType` String - Jika menu konteks dipanggil pada bidang masukan, jenis bidang itu. Nilai yang mungkin adalah `tidak ada` `plainText`, `sandi`, `lain`.
  * `menuSourceType` String - sumber Input yang dipanggil menu konteks. Bisa `tidak`, `mouse`, `keyboard`, `menyentuh`, `touchMenu`.
  * `mediaFlags` Objek - Bendera untuk elemen media menu konteksnya dipanggil di. 
    * `inError` Boolean - Apakah elemen media telah jatuh.
    * `isPaused` Boolean - Apakah elemen media dijeda.
    * `isMuted` Boolean - Apakah elemen media dimatikan.
    * `hasAudio` Boolean - Apakah elemen media memiliki audio.
    * `isLooping` Boolean - Apakah elemen media adalah perulangan.
    * `isControlsVisible` Boolean - Apakah kontrol elemen media terlihat.
    * `canToggleControls` Boolean - Apakah kontrol elemen media dapat dialihkan.
    * `canRotate` Boolean - Apakah elemen media dapat diputar.
  * `editFlags` Objek - Bendera ini menunjukkan apakah penyair mempercayainya mampu melakukan tindakan yang sesuai. 
    * `canUndo` Boolean - Apakah renderer percaya itu dapat membatalkan.
    * `canRedo` Boolean - Apakah renderer percaya itu dapat mengulang.
    * `canCut` Boolean - Apakah renderer percaya dapat memotong.
    * `canCopy` Boolean - Apakah renderer percaya itu dapat menyalin
    * `canPaste` Boolean - Apakah renderer percaya itu dapat menyisipkan.
    * `canDelete` Boolean - Apakah renderer percaya itu dapat menghapus.
    * `canSelectAll` Boolean - Apakah renderer percaya itu dapat memilih semua.

Emitted saat ada menu konteks baru yang perlu ditangani.

#### Event: 'Pilih--perangkat bluetooth'

Pengembalian:

* `acara` Acara
* `perangkat` [[BluetoothDevice]](structures/bluetooth-device.md)
* `callback` Fungsi 
  * `IdentitasAlat` String

Dipancarkan saat perangkat bluetooth perlu dipilih saat dihubungi `navigator.bluetooth.requestDevice`. Menggunakan `navigator.bluetooth` api `webBluetooth` harus diaktifkan. Jika `event.preventDefault` tidak disebut, perangkat tersedia pertama akan dipilih. `callback` harus disebut dengan `deviceId` untuk dipilih, melewati string kosong ke `callback` akan membatalkan permintaan.

```javascript
const {app, webContents} = require('electron') app.commandLine.appendSwitch('enable-web-bluetooth') app.on ('siap', () = > {webContents.on (' perangkat pilih bluetooth', (acara, deviceList, callback) = > {event.preventDefault() membiarkan hasil = deviceList.find((device) = > {kembali device.deviceName === 'test'}) jika (! hasil) {callback('')} lain {callback(result.deviceId)}})})
```

#### Event: 'cat'

Pengembalian:

* `acara` Acara
* `dirtyRect` [Persegi panjang](structures/rectangle.md)
* `gambar` [NativeImage](native-image.md) - Data gambar dari keseluruhan frame.

Emitted ketika bingkai baru dihasilkan. Hanya area kotor yang dilewati di penyangga.

```javascript
const {BrowserWindow} = require('electron') membiarkan memenangkan = BrowserWindow baru ({webPreferences: {offscreen: true}}) win.webContents.on ('cat', (acara, kotor, gambar) = > {/ / updateBitmap (kotor, image.getBitmap())}) win.loadURL ('http://github.com')
```

#### Event: 'devtools-reload-halaman'

Dibunyikan apabila jendela devtools memerintahkan webContents untuk reload

#### Event: 'akan-melampirkan-webview'

Pengembalian:

* `event</ 0> Acara</li>
<li><code>webPreferences` Objek - preferensi web yang akan digunakan oleh semua halaman. Objek ini dapat dimodifikasi untuk menyesuaikan preferensi untuk semua halaman.
* `params` Obyek - `<webview>`parameter lain seperti `src` URL. Objek ini dapat dimodifikasi untuk menyesuaikan parameter halaman tamu.

Dipancarkan ketika `<webview>`isi web yang melekat pada isi web ini. Memanggil `event.preventDefault()` akan menghancurkan semua halaman.

Acara ini dapat digunakan untuk mengkonfigurasi `webPreferences` untuk `webContents` dari `<webview>`sebelum dimuat, dan menyediakan kemampuan untuk mengatur pengaturan yang tidak dapat diatur melalui `<webview>`atribut.

**Catatan:** Opsi script tertentu `preload` akan muncul sebagai `preloadURL` (tidak `preload`) di objek `webPreferences` yang dipancarkan dengan acara ini.

#### Event: 'did-attach-webview'

Pengembalian:

* `acara` Acara
* `webContents` WebContents - The guest web contents that is used by the `<webview>`.

Emitted when a `<webview>` has been attached to this web contents.

#### Event: 'console-message'

Pengembalian:

* `level` Integer
* `pesan` String
* `line` Integer
* `sourceId` String

Emitted when the associated window logs a console message. Will not be emitted for windows with *offscreen rendering* enabled.

### Metode Instance

#### `contents.loadURL (url [, opsi])`

* ` url </ 0> String</li>
<li><code>pilihan` Objek (opsional) 
  * ` httpReferrer </ 0>  String (opsional) - url Referrer HTTP.</li>
<li><code> userAgent </ 0>  String (opsional) - Agen pengguna yang berasal dari permintaan.</li>
<li><code> extraHeaders ` String (opsional) - Header ekstra yang dipisahkan oleh " \n "
  * ` postData </ 0> ( <a href="structures/upload-raw-data.md"> UploadRawData [] </ 1> | <a href="structures/upload-file.md"> UploadFile [] </ 2> | <a href="structures/upload-file-system.md"> UploadFileSystem [] </ 3> | <a href="structures/upload-blob.md"> UploadBlob [] </ 4> ) - (opsional)</li>
<li><code> baseURLForDataURL </ 0>  String (opsional) - URL dasar (dengan pemisah jalur trailing) untuk file yang akan dimuat oleh url data. Hal ini diperlukan hanya jika ditentukan <code>url` data url dan perlu memuat file lainnya.

Beban `url` di jendela. `Url` harus mengandung prefiks protokol, misalnya `http://` atau `file://`. Jika beban harus mem-bypass http cache kemudian menggunakan `pragma` header untuk mencapainya.

```javascript
const {webContents} = require('electron') opsi const = {extraHeaders: ' pragma: no-cache\n'} webContents.loadURL ('https://github.com', opsi)
```

#### `contents.downloadURL(url)`

* `url` String

Memulai download dari sumber daya di `url` tanpa menavigasi. Acara `akan-download` `sesi` akan dipicu.

#### `contents.getURL()`

Mengembalikan `String` - URL laman web saat ini.

```javascript
const {BrowserWindow} = require('electron') membiarkan memenangkan = baru BrowserWindow({width: 800, height: 600}) win.loadURL ('http://github.com') Biarkan currentURL = win.webContents.getURL() console.log(currentURL)
```

#### `contents.getTitle()`

Mengembalikan `String` - judul halaman web sekarang.

#### `contents.isDestroyed()`

Kembali `Boolean` - Apakah halaman web dihancurkan.

#### `contents.Focus()`

Berfokus halaman web.

#### `contents.isFocused()`

Kembali `Boolean` - Apakah halaman web yang terfokus.

#### `contents.isLoading()`

Kembali `Boolean` - Apakah halaman web masih sedang loading sumber daya.

#### `contents.isLoadingMainFrame()`

Kembali `Boolean` - Apakah bingkai utama (dan bukan hanya iframes atau bingkai di dalamnya) masih sedang loading.

#### `contents.isWaitingForResponse()`

Mengembalikan `Boolean` - Apakah halaman web menunggu tanggapan pertama dari utama sumber halaman.

#### `contents.stop()`

Menghentikan navigasi yang tertunda.

#### `isi.reload()`

Muat ulang halaman web saat ini.

#### `contents.reloadIgnoringCache()`

Muat ulang halaman ini dan mengabaikan cache.

#### `contents.canGoBack()`

Mengembalikan `Boolean` - Apakah browser dapat kembali ke halaman web sebelumnya.

#### `contents.canGoForward()`

Mengembalikan `Boolean` - Apakah browser dapat maju ke halaman web berikutnya.

#### `contents.canGoToOffset(offset)`

* `offset` Integer

Mengembalikan `Boolean` - Apakah halaman web bisa masuk ke `offset`.

#### `contents.clearHistory()`

Menghapus sejarah navigasi.

#### `isi.goBack()`

Membuat browser kembali menjadi halaman web.

#### `isi.goForward()`

Membuat browser maju ke depan halaman web.

#### `contents.goToIndex(indeks)`

* `indeks` Integer

Menavigasi browser ke indeks halaman web absolut yang ditentukan.

#### `contents.goToOffset (offset)`

* `offset` Integer

Arahkan ke offset yang ditentukan dari "entri saat ini".

#### `contents.isCrashed()`

Mengembalikan `Boolean` - Apakah proses renderer telah jatuh.

#### `contents.setUserAgent (userAgent)`

* `userAgent` String

Mengganti agen pengguna untuk halaman web ini.

#### `contents.getUserAgent()`

Mengembalikan `String` - Agen pengguna untuk halaman web ini.

#### `isi.insertCSS(css)`

* `css` String

Menyuntikkan CSS ke dalam halaman web saat ini.

#### `contents.executeJavaScript(kode[, userGesture, callback])`

* ` kode </ 0> String</li>
<li><code>userGesture` Boolean (opsional) - Default adalah `false`.
* `callback` Fungsi (opsional) - Dipanggil setelah script telah dieksekusi. 
  * `hasil` Ada

Mengembalikan `Janji` - Janji yang diselesaikan dengan hasil kode yang dijalankan atau ditolak jika hasil dari kode tersebut adalah janji yang ditolak.

Evaluasi `kode` di halaman.

Di jendela browser beberapa API HTML seperti `requestFullScreen` hanya bisa dipanggil oleh isyarat dari pengguna. Setting `userGesture` ke `true` akan dihapus keterbatasan ini.

Jika hasil dari kode yang dieksekusi adalah janji maka hasil callback akan menjadi terselesaikan nilai dari janji. Sebaiknya gunakan Janji yang dikembalikan untuk menangani kode yang menghasilkan Janji.

```js
isi.executeJavaScript('ambil("https://jsonplaceholder.typicode.com/users/1"). kemudian (resp => resp.json())', true)
  .Kemudian ((hasil) => {
    console.log (result) // Akan menjadi objek JSON dari fetch call
  })
```

#### `contents.setIgnoreMenuShortcuts (abaikan)` *Eksperimental*

* `mengabaikan` Boolean

Abaikan shortcut menu aplikasi sementara konten web ini difokuskan.

#### `contents.setAudioMuted(dibungkam)`

* `dibungkam` Boolean

Sesuaikan render halaman web saat ini.

#### `isi.isAudioMuted()`

Mengembalikan `Boolean` - Apakah halaman ini telah dibungkam.

#### `contents.setZoomFactor(faktor)`

* `faktor` Angka - Faktor zoom.

Mengubah faktor pembesaran ke faktor yang ditentukan. Faktor zoom adalah zoom persen dibagi dengan 100, sehingga 300% = 3,0.

#### `contents.getZoomFactor(callback)`

* `callback` Fungsi 
  * `zoomFactor` Nomor

Mengirim permintaan untuk mendapatkan faktor pembesaran saat ini, `panggilan balik` akan dipanggil `callback (zoomFactor)`.

#### `contents.setZoomLevel(level)`

* `level` Angka - level zoom

Mengubah tingkat zoom ke tingkat tertentu. Ukuran aslinya adalah 0 dan masing-masing Peningkatan atas atau di bawah mewakili zoom 20% lebih besar atau lebih kecil ke default batas 300% dan 50% dari ukuran aslinya, berurutan.

#### `contents.getZoomLevel(callback)`

* `callback` Fungsi 
  * `zoomLevel` Nomor

Mengirimkan permintaan untuk mendapatkan tingkat pembesaran saat ini, panggilan balik ` `akan dipanggil dengan `callback(zoomLevel)`.

#### `contents.setZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimalLevel` Nomor
* `maksimalLevel` Nomor

**Tidak berlaku lagi:**Panggil`setVisualZoomLevelLimits` untuk mengatur zoom visual batas tingkat Metode ini akan dihapus di Electron 2.0.

#### `contents.setVisualZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimalLevel` Nomor
* `maksimalLevel` Nomor

Menetapkan maksimum dan minimum tingkat mencubit-to-zoom.

#### `contents.setLayoutZoomLevelLimits(minimumLevel, maximumLevel)`

* `minimalLevel` Nomor
* `maksimalLevel` Nomor

Menetapkan tingkat zoom maksimal dan minimal berbasis tata letak (yaitu bukan-visual).

#### `contents.undo()`

Jalankan perintah pengeditan `undo` di halaman web.

#### `konten.redo()`

Jalankan perintah pengeditan `ulangi` di halaman web.

#### `konten.potong()`

Jalankan perintah pengeditan `potong` di halaman web.

#### `konten.mengkopi()`

Jalankan perintah pengeditan `copy` di halaman web.

#### `contents.copyImageAt(x, y)`

* `x` Integer
* `y` Integer

Salin gambar pada posisi yang diberikan ke clipboard.

#### `contents.paste()`

Jalankan perintah pengeditan `paste` di halaman web.

#### `contents.pasteAndMatchStyle()`

Jalankan perintah pengeditan `pasteAndMatchStyle` di halaman web.

#### `konten.menghapus()`

Jalankan perintah pengeditan `hapus` di halaman web.

#### `konten.memilihsemua()`

Jalankan perintah pengeditan `selectAll` di halaman web.

#### `konten.tidakmemilih()`

Jalankan perintah pengeditan `batalkan pilihan` di halaman web.

#### `isi.replace(teks)`

* ` teks </ 0>  String</li>
</ul>

<p>Jalankan perintah pengeditan <code>ganti` di halaman web.</p> 
  #### `contents.replaceMisspelling(teks)`
  
  * `teks` String
  
  Jalankan perintah pengeditan `replaceMisspelling` di halaman web.
  
  #### `konten.mencaritek()`
  
  * `teks` String
  
  Sisipan `teks` ke elemen yang terfokus.
  
  #### `contents.findInPage(teks[, pilihan])`
  
  * `text` String - Konten yang akan dicari, tidak boleh kosong.
  * `pilihan` Objek (opsional) 
    * `forward` Boolean - (opsional) Baik untuk mencari maju atau mundur, default ke `true`.
    * `findNext` Boolean - (opsional) Apakah operasi tersebut merupakan permintaan pertama atau tindak lanjut, default ke `false`.
    * `matchCase` Boolean - (opsional) Apakah pencarian harus sensitif huruf, default ke `false`.
    * `wordStart` Boolean - (opsional) Baik untuk melihat hanya pada awal kata-kata. default ke `false`.
    * `medialCapitalAsWordStart` Boolean - (opsional) Bila digabungkan dengan `wordStart`, menerima sebuah pertandingan di tengah sebuah kata jika pertandingan dimulai dengan sebuah huruf besar diikuti huruf kecil atau huruf non. Menerima beberapa kecocokan intra-kata lainnya, defaultnya adalah `false`.
  
  Returns `Integer` - The request id used for the request.
  
  Starts a request to find all matches for the `text` in the web page. The result of the request can be obtained by subscribing to [`found-in-page`](web-contents.md#event-found-in-page) event.
  
  #### `contents.stopFindInPage(tindakan)`
  
  * `tindakan` String - Menentukan tindakan yang akan dilakukan saat diakhiri [`webContents.findInPage`] permintaan. 
    * `clearSelection` - jelas pilihan.
    * `keepSelection` - menerjemahkan pemilihan menjadi sebuah pilihan yang normal.
    * `activateSelection` - fokus dan klik seleksi simpul.
  
  Berhenti setiap permintaan `findInPage` untuk `webContents` dengan disediakan `tindakan`.
  
  ```javascript
const {webContents} = require('electron') webContents.on (' ditemukan-di-halaman ', (acara, hasil) = > {jika webContents.stopFindInPage('clearSelection') (result.finalUpdate)}) const requestId = webContents.findInPage('api') console.log(requestId)
```

#### `contents.capturePage ([rect,] callback)`

* `rect` [Persegi panjang](structures/rectangle.md) (opsional) - daerah halaman untuk ditangkap
* `callback` Fungsi 
  * ` gambar </ 0>  <a href="native-image.md"> gambar asli </ 1></li>
</ul></li>
</ul>

<p>Menangkap sebuah snapshot dari halaman dalam <code>rect`. Setelah menyelesaikan `callback` yang akan disebut dengan `callback(image)`. `Gambar` adalah instance dari [NativeImage](native-image.md) yang menyimpan data dari snapshot. Menghilangkan `rect` akan menangkap halaman seluruh terlihat.</p> 
    #### `isi.hasServiceWorker(callback)`
    
    * `callback` Fungsi 
      * `hasWorker` Boolean
    
    Memeriksa apakah ada ServiceWorker yang terdaftar dan mengembalikan boolean sebagai respon terhadap `callback`.
    
    #### `contents.unregisterServiceWorker(callback)`
    
    * `callback` Fungsi 
      * `sukses` Boolean
    
    Unregisters ServiceWorker jika ada dan mengembalikan boolean sebagai respon terhadap `callback` ketika janji JS terpenuhi atau salah saat janji JS ditolak.
    
    #### `konten.mendapatkanpercetakan()`
    
    Dapatkan daftar printer sistem.
    
    Mengembalikan [`membuatinfo[]`](structures/printer-info.md)
    
    #### `contents.print([options], [callback])`
    
    * `pilihan` Objek (opsional) 
      * `diam` Boolean (opsional) - Jangan tanya pengguna untuk pengaturan cetak. Defaultnya adalah `false`.
      * `printBackground` Boolean (opsional) - Juga mencetak warna latar belakang dan gambar halaman web Defaultnya adalah `false`.
      * `deviceName` String (opsional) - Tetapkan nama perangkat printer yang akan digunakan. Defaultnya adalah `''`.
    * `callback` Fungsi (opsional) 
      * success` Boolean - Indicates success of the print call.
    
    Mencetak halaman web jendela. Bila `diam` diatur ke `true`, Elektron akan memilih printer default sistem jika `deviceName` kosong dan pengaturan default untuk dicetak.
    
    Memanggil `window.print()` di halaman web sama dengan memanggil `webContents.print ({silent: false, printBackground: false, deviceName: ''})`.
    
    Gunakan `halaman-break-before: always;` Gaya CSS untuk memaksa mencetak ke halaman baru.
    
    #### `contents.printToPDF(pilihan, callback)`
    
    * `pilihan` Obyek 
      * `marginType` Integer - (opsional) Menentukan jenis margin yang akan digunakan. Menggunakan 0 untuk margin default, 1 tanpa margin, dan 2 untuk margin minimum.
      * `pageSize` String - (opsional) Tentukan ukuran halaman PDF yang dihasilkan. Can be`A3`,`A4`,`A5`,` Legal `,`Letter`,`Tabloid` or an Object containing `height` and `width` in microns.
      * `printBackground` Boolean - (opsional) Baik untuk mencetak latar belakang CSS.
      * `printSelectionOnly` Boolean - (opsional) Baik untuk mencetak pilihan saja.
      * `landscape` Boolean - (opsional) `true` untuk landscape, `false` untuk potret.
    * `callback` Fungsi 
      * Kesalahan `kesalahan`
      * `data` nomor
    
    Mencetak halaman web jendela sebagai PDF dengan custom printing preview Chromium pengaturan.
    
    The `callback` akan dipanggil dengan `callback(error, data)` saat selesai. Itu `data` adalah `Buffer` yang berisi data PDF yang dihasilkan.
    
    The `landscape` will be ignored if `@page` CSS at-rule is used in the web page.
    
    By default, an empty `options` will be regarded as:
    
    ```javascript
{marginsType: 0, printBackground: false, printSelectionOnly: false, landscape: false}
```

Gunakan `halaman-break-before: always;` Gaya CSS untuk memaksa mencetak ke halaman baru.

An example of `webContents.printToPDF `:

```javascript
const {BrowserWindow} = require ('electron') const fs = require ('fs') let win = new BrowserWindow ({width: 800, height: 600}) win.loadURL ('http://github.com') win.webContents.on ('did-finish-load', () = > {// Use default printing options win.webContents.printToPDF ({}, (error, data) = > {if (error) throw error fs.writeFile ('/ tmp / print.pdf', data, (error) = > {if (error) throw error console.log ('Write PDF successfully.')})})})
```

#### `contents.addWorkSpace (path)`

* ` path </ 0>  String</li>
</ul>

<p>Adds the specified path to DevTools workspace. Must be used after DevTools creation:</p>

<pre><code class="javascript">const {BrowserWindow} = require ('electron') let win = new BrowserWindow () win.webContents.on ('devtools-opened', () = > {win.webContents.addWorkSpace (__ dirname)})
`</pre> 
  #### `konten.memindahkanruankerja(jalur)`
  
  * ` path </ 0>  String</li>
</ul>

<p>Menghapus jalur yang ditentukan dari ruang kerja DevTools.</p>

<h4><code>konten.membukaDevAlat([options])`</h4> 
    * `pilihan` Objek (opsional) 
      * `mode` String - Membuka devtool dengan status dermaga tertentu, bisa `kanan`, `bawah`, `undocked`, `lepas`. Default untuk terakhir digunakan dermaga negara. Pada mode `undocked`, mungkin untuk kembali ke dermaga. Di dalam `melepaskan` bukan mode itu.
    
    Membuka devtools.
    
    #### `konten.menutupDevAlat()`
    
    Menutup devtools.
    
    #### `konten.apakahalatDevTerbuka()`
    
    Mengembalikan `boolean` - apakah alatdev sudah terbuka.
    
    #### `konten.apakahAlatDevsudahTerfokus()`
    
    Mengembalikan `Boolean` - Apakah tampilan devtools terfokus.
    
    #### `konten.mematikanAlatDev()`
    
    Toggles alat pengembang.
    
    #### `contents.inspectElement (x, y)`
    
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
// In the main process.
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
  * `dirtyRect` [Persegi panjang](structures/rectangle.md)

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
      
      ### Contoh properti
      
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