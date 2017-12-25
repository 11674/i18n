# Electron Belgeleme Stil Rehberi

Electron belgeleri yazmak için rehberler.

## Başlıklar

* Her sayfa en yukarıda `#` seviyesinde bir başlığa sahip olmalıdır.
* Aynı sayfadaki bölümler `##` seviyesinde başlığa sahip olmalıdır.
* Alt bölümler başlığın iç içe derinliğine göre `#` sayısını arttırmak zorundadır.
* "ve", "ile" gibi bağlaçlar dışında sayfanın başlığındaki tüm kelimeler büyük harfle başlamalıdır.
* Bölüm başlıklarının sadece ilk harfi büyük harfli olmalıdır.

Örnek olarak `Hızlı Başlangıç`:

```markdown
# Hızlı Başlangıç

...

## Ana Süreç

...

## Renderer Süreç

...

## Uygulamanızı başlatın

...

### Dağıtım olarak başlatın

...

### El ile indirilmiş Electron binary'si

...
```

API referansları için bazı istisnalar mevcut.

## Markdown kuralları

* Kod bloklarında `sh` yerine `cmd` kullanın. (Sözdizimi işaretleyicisinden dolayı).
* Satırlar 80. karakterde bitmelidir.
* İç içe listeleri 2'den fazla kademeyi listelemez (indirim işleyici nedeniyle).
* Tüm `js` ve `javascript` kod blokları [standard-markdown](http://npm.im/standard-markdown) ile kontrol edilir.

## Kelime seçimi

* Çıktıları tanımlarken "would" yerine "will" kullanın.
* "in the process" yerine "on" 'u tercih edin.

## API başvuruları

Aşağıdaki kurallar yalnızca API'ların belge işlemi için geçerlidir.

### Sayfa başlığı

Her sayfa başlık olarak `('electron') gerektirir`, tarafından döndürülen gerçek nesne adını kullanmalıdır `TarayıcıPenceresi`, `otomatikGüncelleyici` ve `oturum` gibi.

Sayfa başlığı altında `>` ile başlayan tek satırlık bir açıklama olmalıdır.

Örnek olarak `oturum` kullanma:

```markdown
#oturum
> Tarayıcı oturumlarını, çerezleri, önbelleği, proxy ayarlarını vb. Yönetin.
```

### Modül yöntem ve etkinlikleri

Sınıfı olmayan modüller için onların yöntemleri ve olayları `##Yöntem` ve `##Olaylar` bölümlerinin altında listelenmelidir.

`otomatikGüncelleme` ögesini örnek olarak kullanmak:

```markdown
#otomatikGüncelleyici
##Etkinlikler
###Etkinlik: 'hatası'
#Yöntemler
#otomatikGüncelleyici.BeslemeUrl'sini ayarla(url[,istekBaşlıkları])
```

### Sınıflar

* API sınıfları veya modüllerin bir parçası olan sınıflar bir `## Sınıf altında listelenmelidir; Sınıf İsmi` bölümü.
* Bir sayfa birden fazla sınıfa sahip olabilir.
* Kurucular `###` düzeyinde başlıklarla listelenmelidir.
* [Statik Yöntemler](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static) `### Statik Yöntemler` bölümünün altında listelenmelidir.
* [Örnek Yöntemler](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#Prototype_methods) bir `### Örnek Yöntemler` bölümünün altında listelenmelidir.
* Bir getiri değeri olan tüm yöntemlerin açıklamalarını "getiriler ile başlatması gerekir `[TYPE]` -Getiri tanımı" 
  * Eğer yöntem `Nesne`'ye dönerse, yapısı iki noktadan sonra bir satır sonu karakteriyle, ardından da işlev parametreleriyle aynı tarzda sırasız bir özellik listesi kullanarak belirlenebilir.
* Örnek Olayları `###Örnek Olaylar` bölümünün altında listelenmelidir.
* Örnek Özellikler aşağısında listelenmelidir `### Örnek Özellikleri` bölüm. 
  * Örnek özellikleri "Bir [Özellik Türü]..." ile başlamalıdır

Örnek olarak `Oturum` ve `Çerezler` sınıflarını kullanmak:

```markdown
# session

## Methods

### session.fromPartition(partition)

## Properties

### session.defaultSession

## Class: Session

### Instance Events

#### Event: 'will-download'

### Instance Methods

#### `ses.getCacheSize(callback)`

### Instance Properties

#### `ses.cookies`

## Class: Cookies

### Instance Methods

#### `cookies.get(filter, callback)`
```

### Metodlar

Metodların bölümü aşağıdaki formda olmak zorundadır:

```markdown
###'nesneİsmi.yöntemİsmi(gerekli[, opsiyonel]))'

* `gerekli` dizilim- Parametre açıklaması
*`isteğe bağlı 'Tamsayı (isteğe bağlı) - Başka bir parametre açıklaması

...
```

Bir modülün veya bir sınıfın bir metodu olup olmadığına bağlı olarak başlık `###` veya `####` olabilir.

Modüller için `nesneİsmi` modülün adıdır. Sınıflar için, sınıf örneğinin adı olmalı ve modül adı ile aynı olmamalıdır.

Örneğin, `Oturum`modülü altındaki `Oturum` oturum sınıfının yöntemleri `nesneAdı` olarak `ses` kullanmalıdır.

İsteğe bağlı bağımsız değişkenler isteğe bağlı bağımsız değişkeni çevreleyen köşeli parantezler `[]` ile ve isteğe bağlı bağımsız değişkenin başka bir bağımsız değişkeni izlemesi durumunda gerekli virgülle gösterilir:

```sh
gerekli[, isteğe bağlı]
```

Yöntemin altında her değişken hakkında daha ayrıntılı bilgi var. Bağımsız değişken türü, ortak türlerle belirtilir:

* [`Dize`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
* [`Sayı`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
* [`Nesne`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
* [`Dizi`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
* [`Boole değeri`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
* Ya da Electron'un [`WebContent`](api/web-contents.md) gibi özel bir tür

Bir bağımsız değişken veya yöntem belirli platformlara özgü ise, bu platformalar veri türünün ardında boşlukla sınırlanmış italikleşmiş bir liste kullanılarak ifade edilir. Değer `macOS`, `Windows` veya `Linux` olabilir.

```markdown
*'animasyon ekle' Boole (isteğe bağlı) _macOS_ _Windows_ - Animasyon.
```

`Dizilim` tür bağımsız değişkenleri, dizilimin hangi açıklayıcı öğeye dahil edilebileceğini belirtmelidir.

`İşlev` tür bağımsız değişkenleri için açıklamanın, nasıl çağrılabileceğini açıklığa kavuşturması ve ona iletilecek parametrelerin türlerini listelemesi gerekir.

### Olaylar

Olaylar bölümü aşşağıdaki form da olmak zorundadır:

```markdown
### Event: 'wake-up'

Returns:

* `time` String

...
```

Başlık bir modülün veya bir sınıfın bir etkinliğinin olup olmadığına bağlı olan `###` `####` düzeyleri olabilir.

Bir olayda ki argümanlar metodların takip ettiği kurallar ile aynıdır.

### Özellikler

Metodların bölümü aşağıdaki formda olmak zorundadır:

```markdown
### session.defaultSession

...
```

Başlığın bir modülün veya bir sınıfın özelliği olup olmadığına bağlı olan `###` `####` düzeyleri olabilir.

## Belge Çevirileri

Bakınız [electron/electron-i18n](https://github.com/electron/electron-i18n#readme)