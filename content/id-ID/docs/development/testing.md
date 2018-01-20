# Pengujian

Kami bertujuan untuk menjaga cakupan kode dalam Elektron tinggi. Kami meminta agar semua menarik Permintaan tidak hanya lulus semua tes yang sudah ada, tapi idealnya juga menambah tes baru untuk menutupi perubahan kode dan skenario skenario baru. Memastikan bahwa kita mengambarkan sebagai banyak jalur kode dan penggunaan kasus Elektron yang mungkin memastikan bahwa kita semua aplikasi berjalan dengan lebih sedikit bug.

Repositori ini dilengkapi dengan aturan linting untuk JavaScript dan C ++ - serta tes unit dan berintegrasi. Untuk mempelajari lebih lanjut tentang elektron coding style, silakan lihat dokumen [coding-style (coding-style.md).

## Linting

Untuk memastikan bahwa JavaScript Anda sesuai dengan gaya pengkodean Elektron, jalankan  npm jalankan lint-js </ 0>, yang akan menjalankan <code> standar </ 0> terhadap kedua Elektron itu sendiri sekaligus sebagai bagian tes. Jika Anda menggunakan pengedit
Dengan sistem plugin / addon, Anda mungkin ingin menggunakan salah satu dari sekian banyak
<a href="standard-addons"> StandardJS addons </ 0> untuk mengetahui informasi dari pengcodingan
pelanggaran sebelum Anda melakukannya.</p>

<p>Untuk menjalankan <code> standar </ 0> dengan parameter-parameternya, jalankan <code> npm run lint-js - </ 0> lalu diikuti oleh
argumen yang Anda inginkan untuk melewati ke <code> standar </ 0>.</p>

<p>Untuk memastikan bahwa C ++ Anda sesuai dengan gaya pengkodean Elektron,
jalankan <code> npm jalankan lint-cpp </ 0>, yang menjalankan script <code> cpplint </ 0>. Kami merekomendasikan 
Anda menggunakan <code> clang-format </ 0> dan menyiapkan <a href="clang-format.md"> sebuah tutorial singkat </ 1>.</p>

<p>Tidak banyak Python dalam repositori ini, tapi juga diatur
dengan aturan gaya pengkodean. <code> npm jalanakan lint-py </ 0> dan akan memeriksa semua Python, menggunakan
<code> pylint </ 0> untuk melakukannya.</p>

<h2>Pengujian unit</h2>

<p>Untuk menjalankan semua unit test, jalankan <code> npm jalankan test </ 0>. Tes unitnya adalah Elektron
aplikasi (surprise!) yang bisa ditemukan di folder <code> spasi </ 0>. Perhatikan bahwa itu mempunyai miliknya sendiri
<code> package.json </ 0> dan karena itu dependensinya tidak didefinisikan
di level atas <code> package.json </ 0>.</p>

<p>Untuk menjalankan hanya sejumlah tes yang dipilih, jalankan <code> npm run test -match = NAMA </ 0>,
ganti dengan <code> NAMA </ 0> dengan nama file dari tes suite yang anda inginkan
untuk dijalankan. Sebagai contoh: Jika Anda hanya ingin menjalankan suite IPC, Anda harus menjalankannya
<code> npm jalankan tes - kecocokan = ipc </ 0>.</p>