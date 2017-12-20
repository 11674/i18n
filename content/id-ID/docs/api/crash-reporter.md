# kecelakaan reporter

> Kirim laporan kerusakan ke server jauh.

Proses:  Utama </ 0> ,  Renderer </ 1></p> 

Berikut ini adalah contoh untuk secara otomatis mengirimkan laporan kerusakan ke server jauh:

```javascript
const {kecelakaan Reporter} = require ('electron') kecelakaan Reporter.mulai ({nama produk: 'NamaAnda'nama perusahaan: 'PerusahaanAnda', submitURL: 'https://your-domain.com/url-to-submit', unggah keServer: benar})
```

Untuk menyiapkan server untuk menerima dan memproses laporan kerusakan, Anda dapat menggunakan proyek berikut ini:

* [socorro](https://github.com/mozilla/socorro)
* [mini-istirahat pad-server](https://github.com/electron/mini-breakpad-server)

Laporan kerusakan disimpan secara lokal di folder direktori khusus aplikasi. Untuk `nama produk </ 0> dari <code> nama kamu </ 0> , laporan kerusakan akan disimpan dalam folder bernama <code> nama Crash kamu </ 0> di dalam direktori temp. Anda dapat menyesuaikan lokasi direktori sementara ini untuk aplikasi Anda dengan memanggil <code> app.setPath ( 'temp', '/ my / custom / temp') </ 0> 
API sebelum memulai reporter kecelakaan.</p>

<h2>Metode</h2>

<p>The <code> kecelakaan Reporter </ 0> modul memiliki metode berikut:</p>

<h3><code>kecelakaan Reporter.mulai (pilihan)`</h3> 

* `pilihan` Objek 
  * `` nama perusahaan </ 0>  String (opsional)</li>
<li><code> submitURL </ 0>  String - URL bahwa laporan kerusakan akan dikirim ke POST.</li>
<li><code> nama product</ 0>  String (opsional) - Default ke <code> app.getName () </ 0> .</li>
<li><code> ungkah ke Server </ 0>  Boolean (opsional) - Apakah laporan kerusakan harus dikirim ke server Default adalah <code> true </ 0> .</li>
<li><code> mengabaikan Sistem jatuh Handler </ 0>  Boolean (opsional) - Default adalah <code> false </ 0> .</li>
<li><code> ekstra </ 0> Objek (opsional) - Objek yang dapat Anda tentukan yang akan dikirim bersamaan dengan laporan. Hanya properti string yang dikirim dengan benar. Objek bersarang tidak didukung dan nama dan nilai properti harus panjangnya kurang dari 64 karakter.</li>
</ul></li>
</ul>

<p>Anda diminta untuk memanggil metode ini sebelum menggunakan API <code> crashReporter </ 0> lainnya dan dalam setiap proses (utama / perender) yang ingin Anda kumpulkan laporan kerusakan.
Anda bisa melewati pilihan yang berbeda untuk <code> kecelakaan Reporter.mulai </ 0> saat memanggil dari berbagai proses.</p>

<p><strong> Catatan </ 0> Proses anak yang dibuat melalui modul <code> child_process </ 1> tidak akan memiliki akses ke modul Elektron .
Oleh karena itu, untuk mengumpulkan laporan kerusakan dari mereka, gunakan <code> process.crashReporter.start </ 0> . Lewati pilihan yang sama seperti di atas dan yang tambahan yang disebut <code> crash Direktori</ 0> yang seharusnya mengarah ke direktori untuk menyimpan laporan kerusakan sementara. Anda bisa menguji ini dengan memanggil <code> process.crash () </ 0> untuk menabrak proses anak.</p>

<p><strong> Catatan: </ 0> Untuk mengumpulkan laporan kerusakan dari proses anak di Windows , Anda perlu menambahkan kode tambahan ini juga.
Ini akan memulai proses yang akan memantau dan mengirim laporan kecelakaan. Ganti <code> submit Url </ 0> , <code> nama produk</ 0> 
dan <code> crash Direktori</ 0> dengan nilai yang sesuai.</p>

<p><strong> Catatan: </ 0> Jika Anda memerlukan parameter tambahan / update <code> ekstra </ 1> setelah panggilan pertama <code> mulai </ 1> Anda dapat memanggil <code> set Extra Parameter </ 1> di macOS atau panggil <code> mulai </ 1> 
lagi dengan parameter <code> ekstra </ 1> baru di-update di Linux dan Windows .</p>

<pre><code class="js"> const args = [
    `--reporter-url = $ {submitURL} `,
    `--application-name = $ {productName} `,
    `--crashes-directory = $ {crashesDirectory} `
  ]
  const env = {
    ELECTRON_INTERNAL_CRASH_SERVICE: 1
  }
  bertelur (process.execPath, args, {
    env: env,
    dilepas: true
  })
``</pre> 
    ** Catatan: </ 0> Pada macos , Electron menggunakan klien ` crashpad </ 1> baru untuk pengumpulan dan pelaporan kecelakaan.
Jika Anda ingin mengaktifkan laporan kerusakan, menginisialisasi <code> crashpad </ 0> dari proses utama menggunakan <code> crashReporter.start </ 0> diperlukan terlepas dari proses mana yang ingin Anda kumpulkan. Setelah diinisialisasi dengan cara ini, pengendara crashpad mengumpulkan crash dari semua proses. Anda masih harus menghubungi <code> crashReporter.start </ 0> dari proses renderer atau child, jika tidak crash dari mereka akan dilaporkan tanpa <code> companyName </ 0> , <code> productName </ 0> atau salah satu dari informasi <code> ekstra </ 0> .</p>

<h3><code>kecelakaan Reporter.dapatkan terakhir kecelakaan Reporter ()`</h3> 
    
    Mengembalikan ` kecelakaan Report </ 0> :</p>

<p>Mengembalikan tanggal dan ID dari laporan kerusakan terakhir Jika tidak ada laporan kerusakan yang dikirim atau reporter kecelakaan belum dimulai, <code> null </ 0> dikembalikan.</p>

<h3><code>kecelakaan reporter.dapatkan unggahan repoter ()`</h3> 
    
    Mengembalikan ` kecelakaan Report [] </ 0> :</p>

<p>Mengembalikan semua laporan kerusakan yang diupload. Setiap laporan berisi tanggal dan upload ID.</p>

<h3><code>kecelakaan Reporter.dapatkan unggahan ke Server () </ 0>  <em> Linux </ 1>  <em> macos </ 1></h3>

<p>Mengembalikan <code> Boolean </ 0> - Apakah laporan harus diserahkan ke server. Tetapkan melalui <code> start </ 0> method atau <code> set unggahan ke Server </ 0> .</p>

<p><strong> Catatan: </ 0> Ini API hanya dapat dipanggil dari proses utama.</p>

<h3><code> kecelakaan Reporter.dapatkan unggahan ke Server () </ 0> <em> Linux </ 1> <em> macos </ 1></h3>

<ul>
<li><code> unggah ke Server </ 0>  Boolean  <em> macOS </ 1> - Apakah laporan harus diserahkan ke server</li>
</ul>

<p>Ini biasanya dikendalikan oleh preferensi pengguna. Ini tidak berpengaruh jika dipanggil sebelum <code> mulai </ 0> dipanggil.</p>

<p><strong> Catatan: </ 0> Ini API hanya dapat dipanggil dari proses utama.</p>

<h3><code> kecelakaan Reporter.set Extra Parameter (kunci, nilai) </ 0>  <em> macos </ 1></h3>

<ul>
<li><code> kunci </ 0>  String - Kunci parameter, harus panjangnya kurang dari 64 karakter.</li>
<li><code> nilai </ 0>  String - Nilai parameter, harus panjangnya kurang dari 64 karakter. Menentukan <code> null </ 0> atau <code> undefined </ 0> akan menghapus kunci dari parameter tambahan.</li>
</ul>

<p>Tetapkan parameter tambahan untuk dikirim dengan laporan kerusakan. Nilai yang ditetapkan di sini akan dikirim di samping nilai-nilai yang ditetapkan melalui <code> ekstra </ 0>  option 
ketika <code> mulai </ 0> dipanggil. Ini API hanya tersedia pada MacOS , jika Anda perlu menambahkan / update parameter tambahan pada Linux dan Windows setelah panggilan pertama Anda untuk
 <code> mulai </ 0> Anda dapat menghubungi <code> mulai </ 0> lagi dengan unggah < 0> ekstra </ 0> pilihan.</p>

<h2>Laporan Kecelakaan Payload</h2>

<p>Reporter kecelakaan akan mengirimkan data berikut ke <code> submitURL </ 0> sebagai <code> multipart / form-data </ 0>  <code> POST </ 0> :</p>

<ul>
<li><code> ver </ 0>  String - Versi Elektron .</li>
<li><code> platform </ 0>  String - misal 'win32'.</li>
<li><code> proses_tipe </ 0>  String - misalnya 'renderer'.</li>
<li><code> guid </ 0>  String - misal '5e1286fc-da97-479e-918b-6bfb0c3d1c72'</li>
<li><code> _version </ 0>  String - Versi di <code> package.json </ 0> .</li>
<li><code>_productName` String - The product name in the `crashReporter` `options` object.</li> 
    
    * `prod` String - Name of the underlying product. In this case Electron.
    * `_companyName` String - The company name in the `crashReporter` `options` object.
    * `upload_file_minidump` File - The crash report in the format of `minidump`.
    * All level one properties of the `extra` object in the `crashReporter` `options` object.</ul>