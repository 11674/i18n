# powerSaveBlocker

> Memblokir sistem agar tidak masuk modus daya-rendah (tidur).

Proses:  Utama </ 0></p> 

Sebagai contoh:

```javascript
const {powerSaveBlocker} = require('electron') 

const id = powerSaveBlocker.start('mencegah-tampilan-tidur') 
console.log(powerSaveBlocker.isStarted(id)) 

powerSaveBlocker.stop(id)
```

## Methods

Modul `powerSaveBlocker` mempunyai metods sebagai berikut:

### `powerSaveBlocker.start(type)`

* `type` String - jenis Power save blocker. 
  * `prevent-app-suspension` Mencegah aplikasi agar tidak ditangguhkan/dibekukan. Menjaga sistem tetap aktif tetapi mengizinkan layar untuk dimatikan. Contoh kasus penggunaannya: mengunduh sebuah file atau memainkan audio.
  * `prevent-display-sleep` - Mencegah tampilan agar tidak tidur (gelap). Menjaga sitem dan layar tetap aktif. Contoh kasus penggunaannya: memainkan video.

Returns `Integer` - ID bloker yang ditetapkan untuk pemblokir daya ini

Mulai mencegah sistem agar tidak memasuki mode daya-rendah. Mengembalikan sebuah integer yang mengidentifikasi pemblokir hemat daya.

**Catatan:** `prevent-display-sleep` mempunyai prioritas lebih tinggi dari `prevent-app-suspension`. Hanya jenis prioritas tertinggi yang akan berpengaruh. Dengan kata lain, `prevent-display-sleep` selalu mengambil prioritas lebih tinggi dari `prevent-app-suspension`.

Sebagai contoh, sebuah API memanggil A untuk meminta `prevent-app-suspension`, dan yang lainnya memanggil B untuk meminta `prevent-display-sleep`. `prevent-display-sleep` akan digunakan sampai B menghentikan permintaannya. Setelah itu, `prevent-app-suspension` digunakan.

### `powerSaveBlocker.stop(id)`

* `id` Integer - Id pemblokir hemat daya dikembalikan dengan `powerSaveBlocker.start`.

Menghentikan pemblokir hemat daya yang ditetapkan.

### `powerSaveBlocker.isStarted(id)`

* `id` Integer - Id pemblokir hemat daya dikembalikan dengan `powerSaveBlocker.start`.

Returns `Boolean` - Whether the corresponding `powerSaveBlocker` has started.