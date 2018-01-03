## Kelas: Utama

> Buat menu aplikasi asli dan menu konteks.

Proses:  Utama </ 0></p> 

### `menu Menu()`

Membuat menu baru.

### Metode Statis

Kelas ` BrowserWindow ` memiliki metode statis berikut:

#### `Menu.set Aaplikasi Menu (menu)`

* ` menu </ 0> Menu</li>
</ul>

<p>Set <code> menu </ 0> sebagai menu aplikasi pada macOS. Pada Windows dan Linux,
 <code> menu </ 0> akan ditetapkan sebagai menu atas setiap jendela.</p>

<p>Melewati <code>null` akan menghapus menu bar pada Windows dan Linux tetapi tidak memiliki efek pada macOS.</p> 
  ** Catatan: ** API ini tidak dapat dipanggil sebelum event ` ready ` dari modul ` app `dipancarkan.
  
  #### `Menu.getApplicationMenu()`
  
  Mengembalikan `Menu` - menu aplikasi, jika set, atau `null`, jika tidak ditetapkan.
  
  **Catatan:** Contoh `Menu` kembali tidak mendukung dinamis penambahan atau penghapusan item menu.  Instance properti </ 0> masih dapat dimodifikasi secara dinamis.</p> 
  
  #### ` Menu.kirim aksi pertama ke Responder (tindakan) </ 0> <em> macos </ 1></h4>

<ul>
<li><code> aksi </ 0>  Tali</li>
</ul>

<p>Mengirimkan <code> action </ 0> ke responder pertama dari aplikasi. Ini digunakan untuk meniru perilaku menu macos default. Biasanya Anda hanya akan menggunakan
 <a href="menu-item.md#roles"><code> peran </ 0> properti dari <a href="menu-item.md"><code> MenuItem </ 1>.</p>

<p>Lihat <a href="https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/EventArchitecture/EventArchitecture.html#//apple_ref/doc/uid/10000060i-CH3-SW7"> MacOS Kakao Acara Penanganan Panduan </ 0> 
untuk informasi lebih lanjut tentang MacOS tindakan asli '.</p>

<h4><code>Menu.membangun dari Template (template)`
  
  * `template` MenuItemConstructorOptions[]
  
  Mengembalikan `menu`
  
  Umumnya, `template` hanyalah sebuah array dari `options` untuk membangun a [MenuItem](menu-item.md). Penggunaannya bisa diacu di atas.
  
  Anda juga bisa melampirkan bidang lain ke elemen `template` dan mereka akan menjadi properti dari item menu yang dibangun.
  
  ### Metode Instance
  
  The `menu` object has the following instance methods:
  
  #### `menu.popup([browserWindow, options])`
  
  * `browserWindow` BrowserWindow (opsional) - Default adalah jendela yang terfokus.
  * `pilihan` Objek (opsional) 
    * `x` minor (options) - Default adalah posisi kursor mouse saat ini. harus dinyatakan jika `y<\0> dinyatakan.</li>
<li><code>y` Nomor (opsional) - Default adalah posisi kursor mouse saat ini. Harus dinyatakan jika `x` dinyatakan.
    * `async` Boolean (opsional) - Atur ke `true` agar metode ini segera dipanggil, `false` untuk kembali setelah menu dipilih atau ditutup. Default ke ` false </ 0>.</li>
<li><code>positioningItem`Nomor (opsional) *macOS* - Indeks item menu ke diposisikan di bawah kursor mouse pada koordinat yang ditentukan. Default adalah -1.
  
  Menutup menu konteks di `browserWindow`.
  
  #### `menu.closePopup([browserWindow])`
  
  * `browserWindow` BrowserWindow (opsional) - Default adalah jendela yang terfokus.
  
  Menutup menu konteks di `browserWindow`.
  
  #### `menu.append(menuItem) menuItem`
  
  * `menuItem` MenuItem
  
  Appends the `menuItem` to the menu.
  
  #### `menu.insert(pos, menuItem)`
  
  * `pos` Integer
  * `menu` Menu
  
  Sisipkan `menuItem` ke posisi `pos` pada menu.
  
  ### Instance Properties
  
  `menu` objek juga memiliki properti berikut:
  
  #### `menu.items`
  
  A `MenuItem[]` array containing the menu's items.
  
  Setiap `Menu` terdiri dari beberapa [`MenuItem`](menu-item.md)s dan masing-masing `MenuItem` bisa punya submenu.
  
  ## Contoh
  
  Kelas `Utama` hanya tersedia dalam proses utama, namun Anda juga dapat menggunakannya dalam proses render melalui modul[`remote`](remote.md).
  
  ### Proses utama
  
  Contoh pembuatan menu aplikasi pada proses utama dengan API template sederhana:
  
  ```javascript
const menu = Menu.buildFromTemplate(template)
Menu.setApplicationMenu(menu)
```

### Proses renderer

Dibawah ini adalah contoh membuat menu di halaman web secara dinamis (render proses) dengan menggunakan modul [`remote`](remote.md), dan menunjukkan kapan pengguna menggunakan klik kanan pada halaman:

```html
<!-- index.html -->
<script>
const {remote} = require('electron')
const {Menu, MenuItem} = remote

const menu = new Menu()
menu.append(new MenuItem({label: 'MenuItem1', click() { console.log('item 1 clicked') }}))
menu.append(new MenuItem({type: 'separator'}))
menu.append(new MenuItem({label: 'MenuItem2', type: 'checkbox', checked: true}))

window.addEventListener('contextmenu', (e) => {
  e.preventDefault()
  menu.popup(remote.getCurrentWindow())
}, false)
</script>
```

## Catatan pada Menu Aplikasi MacOS

macos memiliki gaya menu aplikasi yang sama sekali berbeda dari Windows dan Linux. Berikut adalah beberapa catatan tentang cara membuat menu aplikasi Anda lebih mirip dengan asli.

### Menu Standar

Di macos terdapat banyak menu standar yang ditentukan oleh sistem, seperti menu ` Services </ 0> dan
 <code> Windows </ 0>. Untuk membuat menu Anda menu standar, Anda harus mengatur menu Anda
 <code> peran </ 0> ke salah satu dari berikut dan elektron akan mengenali mereka dan membuat mereka menjadi menu standar:</p>

<ul>
<li><code>jendela`</li> 

* `bantuan`
* `layanan`</ul> 

### Tindakan Item Menu Standar

macos telah memberikan tindakan standar untuk beberapa item menu, seperti ` Tentang xxx </ 0> ,
 <code> Sembunyikan xxx </ 0> , dan <code> Sembunyikan Lainnya </ 0> . Untuk mengatur tindakan item menu ke tindakan standar, Anda harus mengatur atribut <code> role </ 0> dari item menu.</p>

<h3>Nama Menu Utama</h3>

<p>Pada macos label item pertama menu aplikasi selalu nama aplikasi Anda, tidak peduli label apa yang Anda tetapkan. Untuk mengubahnya, modifikasi berkas <code> Info.plist < file > aplikasi Anda
 . Lihat
 <a href="https://developer.apple.com/library/ios/documentation/general/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html"> About Information Property List Files </ 0> 
untuk informasi lebih lanjut.</p>

<h2>Setting Menu untuk Jendela Peramban Tertentu (<em> Linux </em> <em> Windows </em>)</h2>

<p>Metode <a href="https://github.com/electron/electron/blob/master/docs/api/browser-window.md#winsetmenumenu-linux-windows"><code> setMenu`metode </a> pencarian windows dapat mengatur menu tertentu Pencarian windows.

## Posisi Item Menu

Anda dapat menggunakan `posisi` dan `id` untuk mengontrol bagaimana item akan ditempatkan ketika membangun sebuah menu dengan `Menu.buildFromTemplate`.

Atribut `posisi` dari `MenuItem` memiliki form ` [placement] = [id] `, di mana `penempatan` adalah salah satu dari `sebelum`, `setelah`, atau ` endof` dan` id ` adalah unik ID dari item yang ada di menu:

* `sebelum` - Menyisipkan item ini sebelum item yang diacu id. Jika Item yang direferensikan tidak ada barang akan disisipkan pada akhir menu.
* `setelah` - Menyisipkan item ini setelah item id direferensikan. Jika direferensikan item tidak ada item akan disisipkan di akhir menu.
* `endof` - Menyisipkan item ini di akhir kelompok logis yang berisi item yang diacu id (grup dibuat oleh item pemisah). Jika Item yang direferensikan tidak ada, grup pemisah baru dibuat dengan id yang diberikan dan item ini dimasukkan setelah pemisah tersebut.

Bila item diposisikan, semua item yang tidak diposisikan dimasukkan setelah item baru diposisikan. Jadi jika Anda ingin memposisikan sekelompok item menu di lokasi yang sama Anda hanya perlu menentukan posisi untuk item pertama.

### Contoh

Template:

```javascript
[
  {label: '4', id: '4'},
  {label: '5', id: '5'},
  {label: '1', id: '1', position: 'before=4'},
  {label: '2', id: '2'},
  {label: '3', id: '3'}
]
```

Menu:

    <br />- 1
    - 2
    - 3
    - 4
    - 5
    

Template:

```javascript
[
  {label: 'a', posisi: 'endof = letters'},
  {label: '1', posisi: 'endof = numbers'},
  {label: 'b', posisi: 'endof = letters'},
  {label: '2', posisi: 'endof = numbers'},
  {label: 'c', posisi: 'endof = letters'},
  {label: '3', posisi: 'endof = numbers'}
]
```

Menu:

    <br />- ---
    - Sebuah
    - b
    - c
    - ---
    - 1
    - 2
    - 3