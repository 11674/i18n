# aplikasi

> Kontrol aplikasi Anda acara siklus hidup.

Proses:  Utama </ 0></p> 

Contoh berikut menunjukkan bagaimana cara menghentikan aplikasi saat jendela terakhir ditutup:

```javascript
const {app} = require ('electron') app.on ('window-all-closed', () = & gt; {
   app.quit ()})
```

## Acara

The ` aplikasi </ 0> objek memancarkan peristiwa berikut:</p>

<h3>Acara : 'will-finish-launching'</h3>

<p>Emitted saat aplikasi sudah selesai basic startup. Pada Windows dan Linux, <code> akan-selesai-launching </ 0>  acara adalah sama dengan <code> siap </ 0>  event ; di macos , acara ini mewakili aplikasi <code> applicationWillFinishLaunching </ 0> dari
 <code> NSApplication </ 0> . Anda biasanya akan menyiapkan pendengar untuk acara <code> open-file </ 0> dan
 <code> open-url </ 0> di sini, dan memulai reporter kecelakaan dan updater otomatis.</p>

<p>Dalam kebanyakan kasus, Anda hanya harus melakukan semuanya dalam pengendali event <code> siap </ 0>  .</p>

<h3>Acara : 'siap'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> launchInfo </ 0> Objek <em> macOS </ 1></li>
</ul>

<p>Emitted ketika Elektron selesai menginisialisasi. Di macos , <code> launchInfo </ 0> memegang <code> userInfo </ 0> dari <code> NSUserNotification </ 0> yang digunakan untuk membuka aplikasi, jika diluncurkan dari Notification Center. Anda dapat menghubungi <code> app.isReady () </ 0> untuk memeriksa apakah acara ini telah dipecat.</p>

<h3>Acara : 'window-all-closed'</h3>

<p>Emitted ketika semua jendela telah ditutup.</p>

<p>Jika Anda tidak berlangganan acara ini dan semua jendela ditutup, perilaku defaultnya adalah berhenti dari aplikasi; Namun, jika Anda berlangganan, Anda mengontrol apakah aplikasi berhenti atau tidak. Jika pengguna menekan <code> Cmd + Q </ 0> , atau pengembang yang disebut
 <code> app.quit () </ 0> , Elektron pertama akan mencoba untuk menutup semua jendela dan kemudian memancarkan
 <code> akan- berhenti </ 0>  event , dan dalam hal ini <code> jendela-semua-ditutup </ 0>  acara tidak akan dipancarkan.</p>

<h3>Acara : 'sebelum-berhenti'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
</ul>

<p>Emitted sebelum aplikasi mulai menutup jendela-jendelanya. Memanggil <code> event.preventDefault () </ 0> akan mencegah perilaku default, yang mengakhiri aplikasi.</p>

<p><strong> Catatan: </ 0> Jika aplikasi berhenti diprakarsai oleh <code> autoUpdater.quitAndInstall () </ 1> 
lalu <code> sebelum-berhenti </ 1> dipancarkan <em> setelah </ 2> memancarkan < 1> dekat </ 1>  acara pada semua jendela dan menutup mereka.</p>

<h3>Acara : 'akan-berhenti'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
</ul>

<p>Emitted ketika semua jendela telah ditutup dan aplikasi akan berhenti. Memanggil <code> event.preventDefault () </ 0> akan mencegah perilaku default, yang mengakhiri aplikasi.</p>

<p>Lihat deskripsi <code> jendela-semua-ditutup </ 0>  acara untuk perbedaan antara <code> akan-berhenti </ 0> dan <code> jendela-semua-ditutup </ 0> peristiwa.</p>

<h3>Acara : 'berhenti'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> exitCode </ 0>  Integer</li>
</ul>

<p>Emitted saat aplikasi berhenti.</p>

<h3>Event : 'open-file' <em> macos </ 0></h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> path </ 0>  String</li>
</ul>

<p>Emitted saat pengguna ingin membuka file dengan aplikasi. The <code> open-file yang </ 0> 
event biasanya dipancarkan saat aplikasi sudah terbuka dan OS ingin menggunakan kembali aplikasi untuk membuka file. <code> open-file </ 0> juga dipancarkan saat sebuah file diturunkan ke dok dan aplikasi belum berjalan. Pastikan untuk mendengarkan <code> open-file yang </ 0>  acara sangat awal di startup aplikasi Anda untuk menangani kasus ini (bahkan sebelum <code> siap </ 0>  acara dipancarkan).</p>

<p>Anda harus menghubungi <code> event .preventDefault () </ 0> jika Anda ingin menangani acara ini .</p>

<p>Pada Windows , Anda harus mengurai <code> process.argv </ 0> (dalam proses utama) untuk mendapatkan filepath.</p>

<h3>Acara : 'buka-url' <em> macos </ 0></h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> url </ 0>  String</li>
</ul>

<p>Emitted saat pengguna ingin membuka URL dengan aplikasi. File <code> Info.plist <code> aplikasi Anda
 harus menentukan skema url di dalam kunci <code> CFBundleURLTypes </ 0> , dan set <code> NSPrincipalClass </ 0> ke <0> AtomApplication </ 0> .</p>

<p>Anda harus menghubungi <code> event .preventDefault () </ 0> jika Anda ingin menangani acara ini .</p>

<h3>Acara : 'aktifkan' <em> macOS </ 0></h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> hasVisibleWindows </ 0>  Boolean</li>
</ul>

<p>Emitted saat aplikasi diaktifkan. Berbagai tindakan dapat memicu acara ini , seperti meluncurkan aplikasi untuk pertama kalinya, mencoba meluncurkan ulang aplikasi saat sudah berjalan, atau mengklik ikon dok atau ikon taskbar.</p>

<h3>Acara : 'lanjutkan aktivitas' <em> macOS </ 0></h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> ketik </ 0> String - String yang mengidentifikasi aktivitas. Maps ke
 <a href="https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSUserActivity_Class/index.html#//apple_ref/occ/instp/NSUserActivity/activityType"><code> NSUserActivity.activityType </ 0>.</li>
<li><code> userInfo </ 0> Objek - Berisi status spesifik aplikasi yang disimpan oleh aktivitas di perangkat lain.</li>
</ul>

<p>Emitted selama <a href="https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html"> Handoff </ 0> saat aktivitas dari perangkat lain ingin dilanjutkan. Anda harus menghubungi <code> event .preventDefault () </ 0> jika Anda ingin menangani acara ini .</p>

<p>Aktivitas pengguna hanya dapat dilanjutkan di aplikasi yang memiliki ID Tim pengembang yang sama dengan aplikasi sumber aktivitas dan yang mendukung jenis aktivitas.
Jenis aktivitas yang didukung ditentukan di aplikasi <code> Info.plist </ 0> di bawah tombol
 <code> NSUserActivityTypes </ 0> .</p>

<h3>Event : 'new-window-for-tab' <em> macOS </ 0></h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
</ul>

<p>Emitted saat pengguna mengklik tombol tab baru macOS asli . Tombol tab baru hanya terlihat jika arus <code> BrowserWindow </ 0> memiliki
 <code> tabbingIdentifier </ 0></p>

<h3>Acara : 'browser-window-blur'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> jendela </ 0> Jendela Peramban</li>
</ul>

<p>Emitted ketika <a href="browser-window.md"> browserWindow </ 0> menjadi kabur.</p>

<h3>Acara : 'browser-window-focus'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> jendela </ 0> Jendela Peramban</li>
</ul>

<p>Emitted ketika <a href="browser-window.md"> browserWindow </ 0> terpusat.</p>

<h3>Acara : 'browser-window-created'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> jendela </ 0> Jendela Peramban</li>
</ul>

<p>Emitted ketika baru <a href="browser-window.md"> browserWindow </ 0> dibuat.</p>

<h3>Acara : 'isi web-dibuat'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> webContents </ 0> Konten Web</li>
</ul>

<p>Emitted ketika baru <a href="web-contents.md"> webContents </ 0> dibuat.</p>

<h3>Acara : 'sertifikat-kesalahan'</h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> webContents </ 0>  <a href="web-contents.md"> WebContents </ 1></li>
<li><code> url </ 0>  String</li>
<li><code> error </ 0>  String - Kode kesalahan</li>
<li><code> sertifikat </ 0>  <a href="structures/certificate.md"> Sertifikat </ 1></li>
<li><code>callback` Fungsi 

* ` isTrusted </ 0>  Boolean - Apakah akan mempertimbangkan sertifikat sebagai terpercaya</li>
</ul></li>
</ul>

<p>Emitted ketika gagal untuk memverifikasi <code> certificate </ 0> untuk <code> url </ 0> , untuk mempercayai sertifikat Anda harus mencegah perilaku default dengan
 <code> event.preventDefault () </ 0> dan memanggil < 0> callback (true) </ 0> .</p>

<pre><code class="javascript">const {app} = require ('electron') app.on ('certificate-error', ( event , webContents, url, error, certificate, callback) = & gt; {
   if (url === 'https: // github .com ') {
     // Verifikasi logika.
    event.preventDefault ()
     callback (true)
   } else {
     callback (false)
   }})
`</pre> 
  ### Acara : 'pilih-klien-sertifikat'
  
  Pengembalian:
  
  * ` event </ 0>  Acara</li>
<li><code> webContents </ 0>  <a href="web-contents.md"> WebContents </ 1></li>
<li><code> url </ 0> URL</li>
<li><code> certificateList </ 0>  <a href="structures/certificate.md"> Sertifikat [] </ 1></li>
<li><code>callback` Fungsi 
    * ` sertifikat </ 0>  <a href="structures/certificate.md"> Sertifikat </ 1> (opsional)</li>
</ul></li>
</ul>

<p>Emitted ketika sertifikat klien diminta.</p>

<p>The <code> url </ 0> sesuai dengan entri navigasi meminta sertifikat klien dan <code> callback </ 0> bisa disebut dengan entri disaring dari daftar. Menggunakan
 <code> event.preventDefault () </ 0> mencegah aplikasi menggunakan sertifikat pertama dari toko.</p>

<pre><code class="javascript">const {app} = require ('electron') app.on ('select-client-certificate', ( event , webContents, url, list, callback) = & gt; {
 event .preventDefault ()
 callback (daftar [0] ) })    
`</pre> 
      ### Acara : 'login'
      
      Pengembalian:
      
      * ` event </ 0>  Acara</li>
<li><code> webContents </ 0>  <a href="web-contents.md"> WebContents </ 1></li>
<li><code>permintaan` Obyek 
        * ` method </ 0>  String</li>
<li><code> url </ 0> URL</li>
<li><code> perujuk </ 0> URL</li>
</ul></li>
<li><code>authInfo` Obyek 
          * ` isProxy </ 0>  Boolean</li>
<li><code> skema </ 0>  String</li>
<li><code> host </ 0>  String</li>
<li><code> port </ 0>  Integer</li>
<li><code> realm </ 0>  String</li>
</ul></li>
<li><code>callback` Fungsi 
            * ` nama pengguna </ 0>  String</li>
<li><code> kata sandi </ 0>  String</li>
</ul></li>
</ul>

<p>Emitted ketika <code> webContents </ 0> ingin melakukan auth dasar.</p>

<p>Perilaku default adalah membatalkan semua otentikasi, untuk menimpa ini Anda harus mencegah perilaku default dengan <code> event.preventDefault () </ 0> dan panggil
 <code> callback (nama pengguna, kata sandi) </ 0> dengan kredensial.</p>

<pre><code class="javascript">const {app} = require ('electron') app.on ('login', ( event , webContents, request, authInfo, callback) = & gt; {
 event .preventDefault ()
 callback ('username', 'secret')} )    
`</pre> 
              ### Acara : 'proses gpu-jatuh'
              
              Pengembalian:
              
              * ` event </ 0>  Acara</li>
<li><code> terbunuh </ 0>  Boolean</li>
</ul>

<p>Emitted saat proses gpu macet atau terbunuh.</p>

<h3>Event : 'aksesibilitas-support-changed' <em> macOS </ 0>  <em> Windows </ 0></h3>

<p>Pengembalian:</p>

<ul>
<li><code> event </ 0>  Acara</li>
<li><code> aksesibilitasSupportEnabled </ 0>  Boolean - <code> true </ 0> saat dukungan aksesibilitas Chrome diaktifkan, <code> false </ 0> sebaliknya.</li>
</ul>

<p>Emitted saat dukungan aksesibilitas Chrome berubah. Peristiwa ini terjadi saat teknologi bantu, seperti pembaca layar, diaktifkan atau dinonaktifkan.
Lihat https://www.chromium.org/developers/design-documents/accessibility untuk lebih jelasnya.</p>

<h2>Metode</h2>

<p>The <code> aplikasi </ 0> objek memiliki metode berikut:</p>

<p><strong> Catatan: </ 0> Beberapa metode hanya tersedia pada sistem operasi tertentu dan diberi label seperti itu.</p>

<h3><code>app.quit ()`</h3> 
                Cobalah untuk menutup semua jendela. The ` sebelum-berhenti </ 0>  acara akan dipancarkan pertama. Jika semua jendela berhasil ditutup, <code> akan-berhenti </ 0>  acara akan dipancarkan dan secara default aplikasi akan mengakhiri.</p>

<p>Metode ini menjamin bahwa semua <code> beforeunload </ 0> dan <code> unload </ 0>  event handlers dijalankan dengan benar. Ada kemungkinan bahwa sebuah jendela membatalkan berhenti dengan mengembalikan <code> false </ 0> pada pengendali event < i > Beforeunload </ 0>  .</p>

<h3><code>app.exit ( [exitCode] )`</h3> 
                
                * ` exitCode </ 0>  Integer (opsional)</li>
</ul>

<p>Keluar segera dengan <code> exitCode </ 0> .  <code> exitCode </ 0> default ke 0.</p>

<p>Semua jendela akan ditutup segera tanpa meminta pengguna dan <code> sebelum-berhenti </ 0> 
dan <code> akan-berhenti </ 0> tidak akan dipancarkan.</p>

<h3><code>app.relaunch ( [options] )`</h3> 
                  * `pilihan` Objek (opsional) 
                    * ` args </ 0>  String [] - (opsional)</li>
<li><code> execPath </ 0>  String (opsional)</li>
</ul></li>
</ul>

<p>Luncurkan ulang aplikasi saat instance saat ini keluar.</p>

<p>Secara default, contoh baru akan menggunakan direktori kerja dan argumen baris perintah yang sama dengan instance saat ini. Bila <code> args </ 0> ditentukan, <code> args </ 0> akan dilewatkan sebagai argumen baris perintah. Ketika <code> execPath </ 0> dispesifikasikan,
 <code> execPath </ 0> akan dieksekusi untuk diluncurkan kembali alih-alih aplikasi saat ini.</p>

<p>Perhatikan bahwa metode ini tidak berhenti dari aplikasi saat dijalankan, Anda harus memanggil
 <code> app.quit </ 0> atau <code> app.exit </ 0> setelah memanggil <code> app.relaunch </ 0> ke buat aplikasi restart</p>

<p>Saat <code> app.relaunch </ 0> dipanggil berkali-kali, beberapa contoh akan dimulai setelah instance saat ini keluar.</p>

<p>Contoh untuk me-restart instance saat ini segera dan menambahkan argumen baris perintah baru ke instance baru:</p>

<pre><code class="javascript">const {app} = require ('electron') app.relaunch ({args: process.argv.slice (1) .concat (['- relaunch'])}) app.exit (0)
`</pre> 
                      ### `app.isReady ()`
                      
                      Mengembalikan ` Boolean </ 0> - <code> true </ 0> jika Elektron selesai menginisialisasi, <code> false </ 0> sebaliknya.</p>

<h3><code>app.focus ()`</h3> 
                      
                      Di Linux, fokus pada jendela yang pertama terlihat. Di macos , buat aplikasi yang aktif. Pada Windows , fokus pada jendela pertama aplikasi.
                      
                      ### ` app.hide () </ 0>  <em> macos </ 1></h3>

<p>Menyembunyikan semua jendela aplikasi tanpa meminimalkannya.</p>

<h3><code> app.show () </ 0>  <em> macos </ 1></h3>

<p>Menunjukkan jendela aplikasi setelah disembunyikan. Tidak secara otomatis memfokuskannya.</p>

<h3><code>app.getAppPath ()`
                      
                      Mengembalikan ` String </ 0> - Direktori aplikasi saat ini.</p>

<h3><code>app.getPath (nama)`</h3> 
                      
                      * ` nama </ 0>  String</li>
</ul>

<p>Mengembalikan <code> String </ 0> - Path ke direktori khusus atau file yang terkait dengan <code> nama </ 0> . Pada kegagalan sebuah <code> Error </ 0> dilempar.</p>

<p>Anda dapat meminta jalur berikut dengan namanya:</p>

<ul>
<li><code> home </ 0> Direktori home pengguna.</li>
<li><code>data aplikasi` Direktori data aplikasi per pengguna, yang secara default menunjuk ke: 
                        * ` % APPDATA% </ 0> di Windows</li>
<li><code> $ XDG_CONFIG_HOME </ 0> atau <code> ~ / .config </ 0> di Linux</li>
<li><code> ~ / Library / Application Support </ 0> di macos</li>
</ul></li>
<li><code> userData </ 0> Direktori untuk menyimpan file konfigurasi aplikasi Anda, yang secara default merupakan direktori <code> appData </ 0> yang ditambahkan dengan nama aplikasi Anda.</li>
<li><code> temp </ 0> Direktori sementara.</li>
<li><code> exe </ 0> File eksekusi saat ini.</li>
<li><code> modul </ 0> The <code> libchromiumcontent </ 0> perpustakaan.</li>
<li><code> desktop </ 0> Direktori Desktop pengguna saat ini.</li>
<li><code> dokumen </ 0> Direktori untuk "My Documents" pengguna.</li>
<li><code> download </ 0> Direktori untuk download pengguna.</li>
<li><code> musik </ 0> Direktori untuk musik pengguna.</li>
<li><code> gambar </ 0> Direktori untuk gambar pengguna.</li>
<li><code> video </ 0> Direktori untuk video pengguna.</li>
<li><code> pepperFlashSystemPlugin </ 0>   Path lengkap ke versi sistem plugin Pepper Flash.</li>
</ul>

<h3><code>app.getFileIcon (path [, options], callback)`</h3> 
                          * ` path </ 0>  String</li>
<li><code>pilihan` Objek (opsional) 
                            * `ukuran` Tali 
                              * ` kecil </ 0> - 16x16</li>
<li><code> normal </ 0> - 32x32</li>
<li><code> besar </ 0> - 48x48 di <em> Linux </ 1> , 32x32 pada <em> Windows </ 1> , tidak didukung di <em> macOS </ 1> .</li>
</ul></li>
</ul></li>
<li><code>callback` Fungsi 
                                * ` error </ 0> Kesalahan</li>
<li><code> ikon </ 0>  <a href="native-image.md"> NativeImage </ 1></li>
</ul></li>
</ul>

<p>Mengambil ikon terkait jalur.</p>

<p>Pada <em> Windows </ 0> , ada 2 macam ikon:</p>

<ul>
<li>Ikon terkait dengan ekstensi file tertentu, seperti <code> .mp3 </ 0> , <code> .png </ 0> , dll.</li>
<li>Ikon di dalam file itu sendiri, seperti <code> .exe </ 0> , <code> .dll </ 0> , <code> .ico </ 0> .</li>
</ul>

<p>Pada <em> Linux </ 0> dan <em> macOS </ 0> , ikon bergantung pada aplikasi yang terkait dengan jenis file mime.</p>

<h3><code>app.setPath (nama, path)`</h3> 
                                  * ` nama </ 0>  String</li>
<li><code> path </ 0>  String</li>
</ul>

<p>Menimpa <code> path </ 0> ke direktori khusus atau file yang terkait dengan <code> nama </ 0> . Jika path menentukan direktori yang tidak ada, direktori akan dibuat dengan metode ini. Pada kegagalan sebuah <code> Error </ 0> dilempar.</p>

<p>Anda hanya dapat menimpa jalur dari <code> nama </ 0> didefinisikan dalam <code> app.getPath </ 0> .</p>

<p>Secara default, cookie dan cache halaman web akan disimpan di bawah 
direktori <code> userData </ 0> . Jika Anda ingin mengubah lokasi ini, Anda harus mengganti
 path <code> userData </ 0> sebelum event <code> ready </ 0>  dari modul <code> app </ 0> dipancarkan.</p>

<h3><code>app.getVersion ()`</h3> 
                                    Mengembalikan ` String </ 0> - Versi aplikasi yang dimuat. Jika tidak ada versi yang ditemukan di file <code> package.json </ 0> aplikasi, versi dari paket saat ini atau yang dapat dijalankan akan dikembalikan.</p>

<h3><code>app.getName ()`</h3> 
                                    
                                    Mengembalikan ` String </ 0> - Nama aplikasi saat ini, yang merupakan nama di file <code> package.json </ 0> aplikasi
 .</p>

<p>Biasanya <code> nama </ 0> bidang <code> package.json </ 0> adalah nama lowercased singkat, menurut NPM modul spec. Anda juga harus menentukan bidang <code> productName </ 0> 
, yang merupakan nama lengkap kapitalisasi aplikasi Anda, dan mana yang lebih disukai dari <code> nama </ 0> oleh Elektron .</p>

<h3><code>app.setName (nama)`</h3> 
                                    
                                    * ` nama </ 0>  String</li>
</ul>

<p>Ganti nama aplikasi saat ini.</p>

<h3><code>app.getLocale ()`</h3> 
                                      Mengembalikan `` String </ 0> - Lokal aplikasi saat ini. Nilai pengembalian yang mungkin didokumentasikan
 <a href="locales.md"> di sini </ 1> .</p>

<p><strong> Catatan: </ 0> Saat mendistribusikan aplikasi yang dikemas, Anda juga harus mengirimkan
 map <code> locales </ 1> .</p>

<p><strong> Catatan: </ 0> Pada Windows Anda harus meneleponnya setelah <code> ready </ 1> dipancarkan.</p>

<h3><code> app.addRecentDocument (path) </ 0>  <em> macos </ 1>  <em> Windows </ 1></h3>

<ul>
<li><code> path </ 0>  String</li>
</ul>

<p>Menambahkan <code> path </ 0> ke daftar dokumen terbaru.</p>

<p>Daftar ini dikelola oleh OS. Pada Windows Anda bisa mengunjungi daftar dari task bar, dan di macos Anda bisa mengunjunginya dari menu dock .</p>

<h3><code> app.clearRecentDocuments () </ 0>  <em> macos </ 1>  <em> Windows </ 1></h3>

<p>Menghapus daftar dokumen terbaru</p>

<h3><code> app.setAsDefaultProtocolClient (protokol [, path, args]) </ 0>  <em> macOS </ 1>  <em> Windows </ 1></h3>

<ul>
<li><code> protocol </ 0>  String - Nama protokol Anda, tanpa <code> : // </ 0> . Jika Anda ingin aplikasi Anda menangani tautan <code> elektron : // </ 0> , hubungi metode ini dengan <code> elektron </ 0> sebagai parameternya.</li>
<li><code> path </ 0>  String (opsional) <em> Windows </ 1> - Default ke <code> process.execPath </ 0></li>
<li><code> args </ 0>  String [] (opsional) <em> Windows </ 1> - Default ke array kosong</li>
</ul>

<p>Mengembalikan <code> Boolean </ 0> - Apakah panggilan berhasil.</p>

<p>Metode ini menetapkan executable saat ini sebagai pengendali default untuk sebuah protokol (alias skema URI). Ini memungkinkan Anda mengintegrasikan aplikasi Anda lebih dalam ke dalam sistem operasi. Setelah terdaftar, semua link dengan <code> your-protocol: // </ 0> akan dibuka dengan executable saat ini. Seluruh link, termasuk protokol, akan diteruskan ke aplikasi Anda sebagai parameter.</p>

<p>Pada Windows Anda dapat menyediakan jalur parameter opsional, jalur ke executable Anda, dan args, serangkaian argumen yang akan dikirimkan ke executable Anda saat diluncurkan.</p>

<p><strong> Catatan: </ 0> Pada macos , Anda hanya dapat mendaftarkan protokol yang telah ditambahkan ke aplikasi <code> info.plist </ 1> , yang tidak dapat diubah saat runtime. Namun Anda dapat mengubah file dengan editor teks sederhana atau skrip selama waktu pembuatan.
Silahkan lihat <a href="https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-102207-TPXREF115"> dokumentasi Apple </ 0> untuk rincian.</p>

<p>The API menggunakan Windows Registry dan LSSetDefaultHandlerForURLScheme internal.</p>

<h3><code> app.removeAsDefaultProtocolClient (protokol [, path, args]) </ 0>  <em> macos </ 1>  <em> Windows </ 1></h3>

<ul>
<li><code> protocol </ 0>  String - Nama protokol Anda, tanpa <code> : // </ 0> .</li>
<li><code> path </ 0>  String (opsional) <em> Windows </ 1> - Default ke <code> process.execPath </ 0></li>
<li><code> args </ 0>  String [] (opsional) <em> Windows </ 1> - Default ke array kosong</li>
</ul>

<p>Mengembalikan <code> Boolean </ 0> - Apakah panggilan berhasil.</p>

<p>Metode ini memeriksa apakah saat ini dapat dieksekusi sebagai pengendali default untuk sebuah protokol (alias skema URI). Jika demikian, itu akan menghapus aplikasi sebagai penangan default.</p>

<h3><code> app.isDefaultProtocolClient (protokol [, path, args]) </ 0>  <em> macos </ 1>  <em> Windows </ 1></h3>

<ul>
<li><code> protocol </ 0>  String - Nama protokol Anda, tanpa <code> : // </ 0> .</li>
<li><code> path </ 0>  String (opsional) <em> Windows </ 1> - Default ke <code> process.execPath </ 0></li>
<li><code> args </ 0>  String [] (opsional) <em> Windows </ 1> - Default ke array kosong</li>
</ul>

<p>Mengembalikan <code> Boolean </ 0></p>

<p>Metode ini memeriksa apakah executable saat ini adalah default handler untuk sebuah protokol (alias skema URI). Jika demikian, itu akan kembali benar. Jika tidak, itu akan kembali salah.</p>

<p><strong> Catatan: </ 0> Pada macos , Anda dapat menggunakan metode ini untuk memeriksa apakah aplikasi telah terdaftar sebagai pengendali protokol default untuk sebuah protokol. Anda juga dapat memverifikasi ini dengan memeriksa <code> ~ / Library / Preferences / com.apple.LaunchServices.plist </ 0> pada
 mesin macos . Silahkan lihat
 <a href="https://developer.apple.com/library/mac/documentation/Carbon/Reference/LaunchServicesReference/#//apple_ref/c/func/LSCopyDefaultHandlerForURLScheme"> dokumentasi Apple </ 0> untuk rincian.</p>

<p>The API menggunakan Windows Registry dan LSCopyDefaultHandlerForURLScheme internal.</p>

<h3><code> app.setUserTasks (tugas) </ 0>  <em> Windows </ 1></h3>

<ul>
<li><code> tugas </ 0>  <a href="structures/task.md"> Tugas [] </ 1> - Array dari <code> Tugas </ 0> objek</li>
</ul>

<p>Tambahkan <code> tugas </ 0> ke kategori <a href="http://msdn.microsoft.com/en-us/library/windows/desktop/dd378460(v=vs.85).aspx#tasks"> Tugas </ 1> JumpList di Windows .</p>

<p><code> tugas </ 0> adalah berbagai dari <a href="structures/task.md"><code> Tugas </ 1> benda.</p>

<p>Mengembalikan <code> Boolean </ 0> - Apakah panggilan berhasil.</p>

<p><strong> Catatan: </ 0> Jika Anda ingin menyesuaikan Daftar Langsung gunakan lebih banyak lagi
 <code> app.setJumpList (categories) </ 1> .</p>

<h3><code> app.getJumpListSettings () </ 0>  <em> Windows </ 1></h3>

<p>Mengembalikan <code> Objek </ 0> :</p>

<ul>
<li><code> minItems </ 0>  Integer - The minimum jumlah item yang akan ditampilkan dalam Daftar Langsung (untuk penjelasan lebih rinci tentang nilai ini melihat
 <a href="https://msdn.microsoft.com/en-us/library/windows/desktop/dd378398(v=vs.85).aspx"> MSDN docs </ 1> ).</li>
<li><code> removedItems </ 0>  <a href="structures/jump-list-item.md"> JumpListItem [] </ 1> - Array dari <code> JumpListItem </ 0> objek yang sesuai dengan item yang telah dihapus pengguna dari kategori khusus dalam Daftar Langsung. Item ini tidak boleh ditambahkan kembali ke Daftar Langsung di 
panggilan <strong> berikutnya </ 0> ke <code> app.setJumpList () </ 1> , Windows tidak akan menampilkan kategori khusus yang berisi salah satu dari yang dihapus item.</li>
</ul>

<h3><code> app.setJumpList (kategori) </ 0>  <em> Windows </ 1></h3>

<ul>
<li><code> kategori </ 0>  <a href="structures/jump-list-category.md"> JumpListCategory [] </ 1> atau <code> nol </ 0> - Array of <code> JumpListCategory </ 0> benda.</li>
</ul>

<p>Mengatur atau menghapus Daftar Langsung kustom untuk aplikasi, dan mengembalikan salah satu dari string berikut:</p>

<ul>
<li><code> ok </ 0> - Tidak ada yang salah.</li>
<li><code> error </ 0> - Satu atau beberapa kesalahan terjadi, aktifkan logging runtime untuk mengetahui kemungkinan penyebabnya.</li>
<li><code> invalidSeparatorError </ 0> - Upaya dilakukan untuk menambahkan pemisah ke kategori khusus dalam Daftar Langsung. Pemisah hanya diperbolehkan dalam kategori <code> Tugas </ 0> standar .</li>
<li><code> fileTypeRegistrationError </ 0> - Upaya dilakukan untuk menambahkan tautan file ke Daftar Langsung untuk jenis file yang tidak terdaftar dalam aplikasi.</li>
<li><code> customCategoryAccessDeniedError </ 0> - Kategori khusus tidak dapat ditambahkan ke Daftar Langsung karena pengaturan kebijakan privasi atau grup pengguna.</li>
</ul>

<p>Jika <code> kategori </ 0> adalah <code> null </ 0> daftar Jump kustom yang telah ditetapkan sebelumnya (jika ada) akan diganti oleh Daftar Langsung standar untuk aplikasi (dikelola oleh Windows ).</p>

<p><strong> Catatan: </ 0> Jika objek <code> JumpListCategory </ 1> tidak memiliki <code> tipe </ 1> atau <code> nama </ 1> 
properti yang ditetapkan maka <code> tipe < / 1> diasumsikan <code> tugas </ 1> . Jika <code> nama </ 0> properti diatur tetapi <code> ketik </ 0> properti dihilangkan maka <code> ketik </ 0> diasumsikan
 <code> kustom </ 0> .</p>

<p><strong> Catatan: </ 0> Pengguna dapat menghapus item dari kategori khusus, dan Windows tidak mengizinkan item yang dihapus ditambahkan ke dalam kategori khusus sampai <strong> setelah </ 0> 
panggilan sukses berikutnya ke <code> app.setJumpList (kategori) </ 1> . Setiap usaha untuk menambahkan kembali item yang dihapus ke kategori khusus lebih awal dari pada itu akan mengakibatkan keseluruhan kategori khusus dihilangkan dari Daftar Langsung. Daftar item yang dihapus dapat diperoleh dengan menggunakan <code> app.getJumpListSettings () </ 0> .</p>

<p>Berikut adalah contoh sederhana untuk membuat Daftar Langsung kustom:</p>

<pre><code class="javascript">const {app} = require ('electron') app.setJumpList ([
   {
     type: 'custom',
     name: 'Proyek Terbaru',
     item: [
       {type: 'file', path: 'C: \\ Projects \\ project1.proj '},
       {type:' file ', path:' C: \\ Projects \\ project2.proj '}
     ]
   },
   {// memiliki nama jadi `type` diasumsikan sebagai     nama " custom "
 : 'Tools',
     item: [
       {
         type: 'task',
         title: 'Tool A',
         program: process.execPath,
         args: '--run-tool-a',
         icon: process.execPath,
         iconIndex: 0,
         deskripsi : 'Runs Tool A'
       },
       {
         type: 'task',
        judul: 'Alat B',
         program: process.execPath,
        args: '--run-tool-b',
         icon: process.execPath,
         iconIndex: 0,
         description: 'Runs Tool B'
       }
     ]
   },
 {type: 'frequent'} ,
 {// tidak memiliki nama dan tipe tidak ada Jadi `tipe` diasumsikan sebagai item " tugas "
 : [
 {
 type: 'task',
 title: 'New Project',
 program: process.execPath,
 args: '--new-project',
 deskripsi: 'Buat yang baru proyek.'
},
 {type: 'separator'} ,
 {
 type: 'task',
 title: 'Recover Project',
 program: process.execPath,
 args: '--recover-project',
 deskripsi: '
                                                                                                                            
``</pre> 
                                      
                                      ### `app.makeSingleInstance (callback)`
                                      
                                      * `callback` Fungsi 
                                        * ` argv </ 0>  String [] - Sebuah array dari argumen baris perintah kedua</li>
<li><code> workingDirectory </ 0>  String - Direktori kerja contoh kedua</li>
</ul></li>
</ul>

<p>Mengembalikan <code> Boolean </ 0> .</p>

<p>Metode ini membuat aplikasi Anda menjadi Aplikasi Instan Tunggal - alih-alih membiarkan beberapa contoh aplikasi Anda berjalan, ini akan memastikan bahwa hanya satu contoh aplikasi Anda yang berjalan, dan contoh lainnya memberi isyarat contoh ini dan keluar.</p>

<p><code> callback </ 0> akan dipanggil oleh instance pertama dengan <code> callback (argv, workingDirectory) </ 0> 
ketika instance kedua telah dieksekusi. <code> argv </ 0> adalah argumen argumen baris kedua dari Array , dan <code> workingDirectory </ 0> adalah direktori kerja saat ini. Biasanya aplikasi merespon hal ini dengan membuat jendela utama mereka fokus dan tidak diminimalisir.</p>

<p>The <code> callback </ 0> dijamin akan dieksekusi setelah <code> siap </ 0>  acara dari <code> aplikasi </ 0> 
akan dipancarkan.</p>

<p>Metode ini mengembalikan <code> false </ 0> jika proses Anda adalah contoh utama aplikasi dan aplikasi Anda harus terus dimuat. Dan mengembalikan <code> true </ 0> jika proses Anda telah mengirimkan parameternya ke instance lain, dan Anda harus segera berhenti.</p>

<p>Pada macOS , sistem memberlakukan instance tunggal secara otomatis saat pengguna mencoba membuka instance kedua aplikasi Anda di Finder, dan acara <code> open-file </ 0> dan <code> open-url </ 0> 
akan dipancarkan untuk bahwa. Namun saat pengguna memulai aplikasi Anda di jalur perintah mekanisme contoh tunggal sistem akan dilewati dan Anda harus menggunakan metode ini untuk memastikan satu contoh.</p>

<p>Contoh mengaktifkan jendela contoh utama saat instance kedua dimulai:</p>

<pre><code class="javascript">const {app} = require ('electron') biarkan myWindow = null const isSecondInstance = app.makeSingleInstance ((commandLine, workingDirectory) = & gt; {
   // Seseorang mencoba untuk menjalankan instance kedua, kita harus memusatkan jendela kita.
  jika (myWindow) {
     if (myWindow.isMinimized ()) myWindow.restore ()
     myWindow.focus ()
   }}) if (isSecondInstance) {
   app.quit ()} // buat myWindow, muat sisa aplikasi, dll. ...
app.on ('siap', () = & gt; {})
`</pre> 
                                          ### `app.releaseSingleInstance ()`
                                          
                                          Rilis semua kunci yang diciptakan oleh ` makeSingleInstance </ 0> . Ini akan memungkinkan beberapa contoh aplikasi sekali lagi berjalan berdampingan.</p>

<h3><code> app.setUserAktivitas (ketik, userInfo [, webpageURL]) </ 0>  <em> macos </ 1></h3>

<ul>
<li><code> ketik </ 0>  String - Unik mengidentifikasi aktivitas. Maps ke
 <a href="https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSUserActivity_Class/index.html#//apple_ref/occ/instp/NSUserActivity/activityType"><code> NSUserActivity.activityType </ 0> .</li>
<li><code> userInfo </ 0> Objek - Negara khusus aplikasi untuk disimpan untuk digunakan oleh perangkat lain.</li>
<li><code> webpageURL </ 0>  String (opsional) - Halaman web dimuat di browser jika tidak ada aplikasi yang sesuai untuk dipasang pada perangkat yang dilanjutkan. Skema ini harus <code> http </ 0> atau <code> https </ 0> .</li>
</ul>

<p>Membuat <code> NSUserActivity </ 0> dan menetapkannya sebagai aktivitas saat ini. Aktivitas ini memenuhi syarat untuk <a href="https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html"> Handoff </ 0> ke perangkat lain sesudahnya.</p>

<h3><code> app.getCurrentActivityType () </ 0>  <em> macos </ 1></h3>

<p>Mengembalikan <code> String </ 0> - Jenis aktivitas yang sedang berjalan.</p>

<h3><code> app.setAppUserModelId (id) </ 0>  <em> Windows </ 1></h3>

<ul>
<li><code> id </ 0>  String</li>
</ul>

<p>Ubah < ID > User ID Model Aplikasi </ 0> menjadi <code> id </ 1> .</p>

<h3><code> app.importCertificate (opsi, callback) </ 0>  <em> LINUX </ 1></h3>

<ul>
<li><code>pilihan` Obyek 
                                          
                                          * ` sertifikat </ 0>  String - Path untuk berkas pkcs12.</li>
<li><code> kata sandi </ 0>  String - Passphrase untuk sertifikat.</li>
</ul></li>
<li><code>callback` Fungsi 
                                            * ` hasil </ 0>  Integer - Hasil impor.</li>
</ul></li>
</ul>

<p>Impor sertifikat dalam format pkcs12 ke toko sertifikat platform.
<code> callback </ 0> dipanggil dengan <code> hasil </ 0> dari operasi impor, nilai <code> 0 </ 0> 
menunjukkan keberhasilan sementara nilai lainnya mengindikasikan kegagalan menurut kromium  <a href="https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h"> net_error_list </ 1> .</p>

<h3><code>app.disableHardwareAcceleration ()`</h3> 
                                              Nonaktifkan akselerasi perangkat keras untuk aplikasi saat ini.
                                              
                                              Metode ini hanya bisa dipanggil sebelum aplikasi sudah siap.
                                              
                                              ### `app.disableDomainBlockingFor3DAPIs ()`
                                              
                                              Secara default, Chromium menonaktifkan API 3D (misalnya WebGL) sampai dimulai ulang per basis domain jika proses GPU mogok terlalu sering. Fungsi ini menonaktifkan perilaku itu.
                                              
                                              Metode ini hanya bisa dipanggil sebelum aplikasi sudah siap.
                                              
                                              ### ` app.getAppMemoryInfo () </ 0>  <em> Tidak berlaku lagi </ 1></h3>

<p>Pengembalian <a href="structures/process-metric.md"><code> ProcessMetric [] </ 0> :   Array dari <code> ProcessMetric </ 1> benda-benda yang sesuai dengan memori dan penggunaan cpu statistik dari semua proses yang terkait dengan aplikasi.
<strong> Catatan: </ 0> Metode ini tidak berlaku lagi, gunakan <code> app.getAppMetrics () </ 1> .</p>

<h3><code>app.getAppMetrics ()`
                                              
                                              Pengembalian ` ProcessMetric [] </ 0> :   Array dari <code> ProcessMetric </ 1> benda-benda yang sesuai dengan memori dan penggunaan cpu statistik dari semua proses yang terkait dengan aplikasi.</p>

<h3><code>app.getGpuFeatureStatus ()`</h3> 
                                              
                                              Mengembalikan ` GPUFeatureStatus </ 0> - Status Fitur Gambar dari <code> chrome: // gpu / </ 1> .</p>

<h3><code> app.setBadgeCount (count) </ 0>  <em> Linux </ 1>  <em> macos </ 1></h3>

<ul>
<li><code> hitung </ 0>  Integer</li>
</ul>

<p>Mengembalikan <code> Boolean </ 0> - Apakah panggilan berhasil.</p>

<p>Menetapkan lencana penghitung untuk aplikasi saat ini. Menetapkan hitungan ke<code>0` akan menyembunyikan lencana</p> 
                                              
                                              Di macos itu terlihat di ikon dermaga. Di Linux hanya bekerja untuk Unity launcher,
                                              
                                              **Note:** Unity launcher mensyaratkan adanya a `.desktop` file untuk bekerja, untuk informasi lebih lanjut silahkan baca[Desktop Environment Integration](../tutorial/desktop-environment-integration.md#unity-launcher-shortcuts-linux).
                                              
                                              ### `app.getBadgeCount()` *Linux* *macOS*
                                              
                                              Returns `Integer` - The current value displayed in the counter badge.
                                              
                                              ### `app.isUnityRunning()` *Linux*
                                              
                                              Returns `Boolean` - Whether the current desktop environment is Unity launcher.
                                              
                                              ### `app.getLoginItemSettings([options])` *macOS* *Windows*
                                              
                                              * `pilihan` Objek (opsional) 
                                                * `path` String (optional) *Windows* - The executable path to compare against. Defaults to `process.execPath`.
                                                * `args` String[] (optional) *Windows* - The command-line arguments to compare against. Defaults to an empty array.
                                              
                                              If you provided `path` and `args` options to `app.setLoginItemSettings` then you need to pass the same arguments here for `openAtLogin` to be set correctly.
                                              
                                              Mengembalikan ` Objek </ 0> :</p>

<ul>
<li><code>openAtLogin` Boolean - `true` if the app is set to open at login.</li> 
                                              
                                              * `openAsHidden` Boolean - `true` if the app is set to open as hidden at login. This setting is only supported on macOS.
                                              * `wasOpenedAtLogin` Boolean - `true` if the app was opened at login automatically. This setting is only supported on macOS.
                                              * `wasOpenedAsHidden` Boolean - `true` if the app was opened as a hidden login item. This indicates that the app should not open any windows at startup. This setting is only supported on macOS.
                                              * `restoreState` Boolean - `true` if the app was opened as a login item that should restore the state from the previous session. This indicates that the app should restore the windows that were open the last time the app was closed. This setting is only supported on macOS.</ul> 
                                              
                                              **Note:** This API has no effect on [MAS builds](../tutorial/mac-app-store-submission-guide.md).
                                              
                                              ### `app.setLoginItemSettings(settings)` *macOS* *Windows*
                                              
                                              * `settings` Object 
                                                * ` openAtLogin </ 0>  Boolean (opsional) - <code> true </ 0> untuk membuka aplikasi saat masuk, <code> false </ 0> untuk menghapus aplikasi sebagai item masuk. Default ke <code> false </ 0> .</li>
<li><code> openAsHidden </ 0>  Boolean (opsional) - <code> true </ 0> untuk membuka aplikasi sebagai tersembunyi. Default ke
 <code> false </ 0> . Pengguna dapat mengedit setelan ini dari Preferensi Sistem jadi
 <code> app.getLoginItemStatus (). BeenOpenedAsHidden </ 0> harus diperiksa saat aplikasi dibuka untuk mengetahui nilai saat ini. Pengaturan ini hanya didukung pada
 macos .</li>
<li><code>path` String (optional) *Windows* - The executable to launch at login. Defaults to `process.execPath`.
                                                * `args` String[] (optional) *Windows* - The command-line arguments to pass to the executable. Defaults to an empty array. Take care to wrap paths in quotes.
                                              
                                              Set the app's login item settings.
                                              
                                              To work with Electron's `autoUpdater` on Windows, which uses [Squirrel](https://github.com/Squirrel/Squirrel.Windows), you'll want to set the launch path to Update.exe, and pass arguments that specify your application name. For example:
                                              
                                              ```javascript
const appFolder = path.dirname(process.execPath)
const updateExe = path.resolve(appFolder, '..', 'Update.exe')
const exeName = path.basename(process.execPath)

app.setLoginItemSettings({
  openAtLogin: true,
  path: updateExe,
  args: [
    '--processStart', `"${exeName}"`,
    '--process-start-args', `"--hidden"`
  ]
})
```
                                          
                                          **Note:** This API has no effect on [MAS builds](../tutorial/mac-app-store-submission-guide.md).
                                          
                                          ### `app.isAccessibilitySupportEnabled()` *macOS* *Windows*
                                          
                                          Returns `Boolean` - `true` if Chrome's accessibility support is enabled, `false` otherwise. This API will return `true` if the use of assistive technologies, such as screen readers, has been detected. See https://www.chromium.org/developers/design-documents/accessibility for more details.
                                          
                                          ### `app.setAboutPanelOptions(options)` *macOS*
                                          
                                          * `pilihan` Object 
                                            * `applicationName` String (optional) - The app's name.
                                            * `applicationVersion` String (optional) - The app's version.
                                            * `copyright` String (optional) - Copyright information.
                                            * `credits` String (optional) - Credit information.
                                            * `version` String (optional) - The app's build version number.
                                          
                                          Set the about panel options. This will override the values defined in the app's `.plist` file. See the [Apple docs](https://developer.apple.com/reference/appkit/nsapplication/1428479-orderfrontstandardaboutpanelwith?language=objc) for more details.
                                          
                                          ### `app.commandLine.appendSwitch(switch[, value])`
                                          
                                          * `switch` String - A command-line switch
                                          * `value` String (optional) - A value for the given switch
                                          
                                          Append a switch (with optional `value`) to Chromium's command line.
                                          
                                          **Note:** This will not affect `process.argv`, and is mainly used by developers to control some low-level Chromium behaviors.
                                          
                                          ### `app.commandLine.appendArgument(value)`
                                          
                                          * `value` String - The argument to append to the command line
                                          
                                          Append an argument to Chromium's command line. The argument will be quoted correctly.
                                          
                                          **Note:** This will not affect `process.argv`.
                                          
                                          ### `app.enableMixedSandbox()` *Experimental* *macOS* *Windows*
                                          
                                          Enables mixed sandbox mode on the app.
                                          
                                          Metode ini hanya bisa dipanggil sebelum aplikasi sudah siap.
                                          
                                          ### `app.dock.bounce([type])` *macOS*
                                          
                                          * `type` String (optional) - Can be `critical` or `informational`. The default is `informational`
                                          
                                          When `critical` is passed, the dock icon will bounce until either the application becomes active or the request is canceled.
                                          
                                          When `informational` is passed, the dock icon will bounce for one second. However, the request remains active until either the application becomes active or the request is canceled.
                                          
                                          Returns `Integer` an ID representing the request.
                                          
                                          ### `app.dock.cancelBounce(id)` *macOS*
                                          
                                          * `id` Integer
                                          
                                          Cancel the bounce of `id`.
                                          
                                          ### `app.dock.downloadFinished(filePath)` *macOS*
                                          
                                          * `filePath` String
                                          
                                          Bounces the Downloads stack if the filePath is inside the Downloads folder.
                                          
                                          ### `app.dock.setBadge(text)` *macOS*
                                          
                                          * `text` String
                                          
                                          Sets the string to be displayed in the dock’s badging area.
                                          
                                          ### `app.dock.getBadge()` *macOS*
                                          
                                          Returns `String` - The badge string of the dock.
                                          
                                          ### `app.dock.hide()` *macOS*
                                          
                                          Sembunyikan ikon dok.
                                          
                                          ### `app.dock.show()` *macOS*
                                          
                                          Tampilkan ikon dok.
                                          
                                          ### `app.dock.isVisible()` *macOS*
                                          
                                          Returns `Boolean` - Whether the dock icon is visible. The `app.dock.show()` call is asynchronous so this method might not return true immediately after that call.
                                          
                                          ### `app.dock.setMenu(menu)` *macOS*
                                          
                                          * `menu` [Menu](menu.md)
                                          
                                          Sets the application's [dock menu](https://developer.apple.com/library/mac/documentation/Carbon/Conceptual/customizing_docktile/concepts/dockconcepts.html#//apple_ref/doc/uid/TP30000986-CH2-TPXREF103).
                                          
                                          ### `app.dock.setIcon(image)` *macOS*
                                          
                                          * `image` ([NativeImage](native-image.md) | String)
                                          
                                          Sets the `image` associated with this dock icon.