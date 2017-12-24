## Sınıf: DownloadItem

> Kontrol dosyası uzak kaynaklardan yükleme yapar.

Süreç: [Ana](../glossary.md#main-process)

`DownloadItem` elektron içindeki indirme öğesini temsil eden bir `EventEmitter`'dir. O `oturumun` `will-download` olayı içinde kullanılır ve kullanıcıların indirilen öğeyi kontrol etmesine izin verir.

```javascript
// Ana işlem içinde.
const {BrowserWindow} = require('electron')
let win = new BrowserWindow()
win.webContents.session.on('will-download', (event, item, webContents) => {
Kaydetme yolunu ayarlayın ve Electron'un bir kaydetme istememesi için yol gösterin.
  item.setSavePath('/tmp/save.pdf')

  item.on('updated', (event, state) => {
    if (state === 'interrupted') {
      console.log('Download is interrupted but can be resumed')
    } else if (state === 'progressing') {
      if (item.isPaused()) {
        console.log('Download is paused')
      } else {
        console.log(`Received bytes: ${item.getReceivedBytes()}`)
      }
    }
  })
  item.once('done', (event, state) => {
    if (state === 'completed') {
      console.log('Download successfully')
    } else {
      console.log(`Download failed: ${state}`)
    }
  })
})
```

### Örnek Events

#### Event: 'updated'

Returns:

* `event` Event
* `state` String

İndirme güncellendiğinde ve bitmediğinde yayınlanır.

`Durum` aşağıdakilerden biri olabilir:

* `progressing` - İndirme devam ediyor.
* `interrupted` - İndirme kesildi ama devam edilebilir.

#### Event: 'done'

Returns:

* `event` Event
* `state` String

İndirme işlemi terminal durumundayken yayımlanır. Bu bitmiş indirme, (via `downloadItem.cancel()`) ile iptal edilmiş bir indirme ve devam edilemeyen kesmeye uğramış indirme içerir.

`Durum` aşağıdakilerden biri olabilir:

* `completed` - İndirme başarıyla tamamlandı.
* `cancelled` - İndirme iptal edildi.
* `interrupted` - İndirme kesildi ve devam edemez.

### Örnek yöntemler

`downloadItem` nesnesi aşağıdaki yöntemleri içerir:

#### `downloadItem.setSavePath(path)`

* `path` String - indirme öğesinin dosya kaydetme yolunu ayarlayın.

API, yalnızca oturumun `will-download` geri arama işlevinde kullanılabilir. Kullanıcı API aracılığıyla kaydetme yolunu ayarlamazsa, Electron kaydetme yolunu belirlemek için orijinal rutinleri kullanacaktır. (Genellikle bir kaydetme diyaloğı çıkarır).

#### `downloadItem.getSavePath()`

İndirilen öğenin kaydedilecek yolunu `String` olarak döndürür. Bu ya `downloadItem.setSavePath(path)` ile ayarlanmış yol olacak yada kaydetme diyaloğunda görünen seçilmiş yol olacak.

#### `downloadItem.pause()`

İndirmeyi duraklatır.

#### `downloadItem.isPaused()`

İndirilmenin durdurulup durdurulmadığına dair `Boolean` döndürür.

#### `downloadItem.resume()`

Durdurulmuş indirmeyi devam ettirir.

**Not:** Devamlı indirmeleri etkinleştirmek için, indirdiğiniz sunucunun aralık isteklerini desteklemesi gerekir ve `Last-Modified` and `ETag` başlık değerlerinin ikisini de sağlamalı. Aksi takdirde, `resume()`, daha önce alınan baytları atlayacak ve indirmeyi baştan başlatacaktır.

#### `downloadItem.canResume()`

İndirmenin devam edip edemeyeceğine dair `Boolean` döndürür.

#### `downloadItem.cancel()`

İndirme işlemini iptal eder.

#### `downloadItem.getURL()`

Öğenin nereden indirildiğine dair orjinal url'sini `String` olarak döndürür.

#### `downloadItem.getMimeType()`

Dosyaların Mime türünü `String` olarak döndürür.

#### `downloadItem.hasUserGesture()`

İndirme işleminin kullanıcı hareketi olup olmadığına dair `Boolean` döndürür.

#### `downloadItem.getFilename()`

İndirilen öğenin ismini `String` olarak döndürür.

**Not:** Dosya adı her zaman yerel diskte kaydedilen dosya adıyla aynı değildir. Kullanıcı, istenen bir indirme kaydetme iletişim kutusunda dosya adını değiştirirse, kaydedilen dosyanın gerçek adı farklı olacaktır.

#### `downloadItem.getTotalBytes()`

İndirilen öğenin toplam byte boyutunu `Integer` olarak döndürür.

Eğer boyutu bilinmiyorsa 0 değerini döndürür.

#### `downloadItem.getReceivedBytes()`

İndirilen öğenin indirilen byte kadarını `Integer` türünde döndürür.

#### `downloadItem.getContentDisposition()`

Cevabın başlığından İçerik-Hazırlama alanını `String` türünde döndürür.

#### `downloadItem.getState()`

Geçerli durumu `String` türünde döndürür. `progressing`, `completed`, `cancelled` veya `interrupted` olabilir.

**Not:** Aşağıdaki metodlar oturup yeniden başlatıldığı zaman `iptal edilmiş` öğelerin devamı için oldukça kullanışlıdır.

#### `downloadItem.getURLChain()`

Herhangi bir yeniden yönlendirme de dahil olmak üzere öğenin tam url zincirini `String[]` döndürür.

#### `downloadItem.getLastModifiedTime()`

Son değiştirilen başlık değerini `String` olarak döndürür.

#### `downloadItem.getETag()`

ETag başlık değerini `String` olarak döndürür.

#### `downloadItem.getStartTime()`

UNIX zaman başlangıcından indirmenin başlatıldığı zamana kadar geçen saniye sayısını `Double` türünde döndürür.