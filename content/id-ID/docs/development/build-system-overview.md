# Membangun Sistem Tinjauan

Menggunakan Electron [gyp](https://gyp.gsrc.io/) proyek generasi dan [ninja](https://ninja-build.org/) untuk bangunan. Proyek konfigurasi dapat ditemukan di `.gyp` dan `.gypi` file.

## File Gyp

Setelah `gyp` file berisi aturan utama untuk membangun Electron:

* `electron.gyp` Definisikan bagaimana Electron itu sendiri dibangun.
* `common.gypi` menyesuaikan konfigurasi membangun Node untuk membuatnya dibangun bersama dengan Chromium.
* `brightray/brightray.gyp` mendefinisikan bagaimana `brightray` dibuat dan mencakup konfigurasi default untuk menghubungkan dengan Chromium.
* `brightray/brightray.gypi` termasuk konfigurasi umum membangun tentang bangunan.

## Membangun Komponen

Karena Chromium cukup merupakan proyek besar, tahap penghubung terakhir bisa terjadi cukup beberapa menit, yang membuat sulit untuk pembangunan. Untuk memecahkannya Ini, Chromium memperkenalkan "komponen bangunan", yang membangun setiap komponen sebagai perpustakaan bersama yang terpisah, membuat hubungan sangat cepat namun mengorbankan ukuran file dan kinerja.

Di Electron kami mengambil pendekatan yang sangat mirip: untuk `Debug` membangun, binari akan dikaitkan dengan versi komponen Chromium bersama untuk dikembangkan bersama waktu menghubungkan cepat; untuk `melepaskan` membangun, binari akan dihubungkan ke static versi perpustakaan, jadi kita bisa memiliki ukuran dan kinerja bineri sebaik mungkin.

## Minimal Bootstrapping

Semua binari masa membangun Chromium (`libchromiumcontent`) diunduh saat menjalankan skrip bootstrap. Secara sederhana kedua perpustakaan statis dan pembagian perpustakaan akan didownload dan ukuran akhirnya harus antara 800MB dan 2GB tergantung pada platform.

Secara sederhana, `libchromiumcontent` diunduh dari Amazon Web Services. Jika `LIBCHROMIUMCONTENT_MIRROR` variabel lingkungan disetel, bootstrap script akan mendownload dari itu. [`libchromiumcontent-qiniu-mirror`](https://github.com/hokein/libchromiumcontent-qiniu-mirror) adalah kaca untuk `libchromiumcontent`. Jika Anda kesulitan mengakses AWS, Anda bisa ganti alamat downloadnya via `export LIBCHROMIUMCONTENT_MIRROR=http://7xk3d2.dl1.z0.glb.clouddn.com/`

Jika Anda hanya ingin membangun Electron dengan cepat untuk pengujian atau pengembangan, Anda dapat mendownload hanya versi pembagian pustaka dengan melewatkan `--dev` parameter:

```sh
$ ./script/bootstrap.py --dev
$ ./script/build.py -c D
```

## Generasi Proyek Dua Tahap

Electron link dengan berbagai tetapan perpustakaan di `Pelepasan` dan `Debug` membangun. `gyp`, bagaimanapun, tidak mendukung konfigurasi pengaturan tautan yang berbeda konfigurasi yang berbeda.

Untuk mengolah Electron ini menggunakan `gyp` variabel `libchromiumcontent_component` untuk mengontrol pengaturan link yang akan digunakan dan hanya menghasilkan satu target saat menjalankan `gyp`.

## Target Nama

Tidak seperti kebanyakan proyek yang menggunakan `Melepaskan` dan `Debug` sebagai nama target, Electron menggunakan `R` dan `D` sebagai gantinya. Ini karena `gyp` secara tabrakan jika ada hanya satu `Melepaskan` atau `Debug` membangun konfigurasi yang ditentukan, dan hanya Electron untuk menghasilkan satu target pada satu waktu seperti yang dinyatakan di atas.

Ini hanya mempengaruhi pengembang, jika Anda hanya membangun Electron untuk rebranding Anda tidak terpengaruh.

## Pengujian

Menguji perubahan sesuai dengan proyek gaya pengkodean menggunakan:

```sh
$ npm run lint
```

Menguji fungsionalitas menggunakan:

```sh
$ npm test
```

Whenever you make changes to Electron source code, you'll need to re-run the build before the tests:

```sh
$ npm run build && npm test
```

You can make the test suite run faster by isolating the specific test or block you're currently working on using Mocha's [exclusive tests](https://mochajs.org/#exclusive-tests) feature. Just append `.only` to any `describe` or `it` function call:

```js
describe.only('some feature', function () {
  // ... only tests in this block will be run
})
```

Alternatively, you can use mocha's `grep` option to only run tests matching the given regular expression pattern:

```sh
$ npm test -- --grep child_process
```

Tests that include native modules (e.g. `runas`) can't be executed with the debug build (see [#2558](https://github.com/electron/electron/issues/2558) for details), but they will work with the release build.

To run the tests with the release build use:

```sh
$ npm test -- -R
```