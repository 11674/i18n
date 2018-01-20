## Sınıf: TouchBarButton

> Yerel macOS uygulamaları için dokunmatik çubukta bir düğme oluşturun

Süreç: [Ana](../tutorial/quick-start.md#main-process)

### `new TouchBarButton(options)` *Deneysel*

* `options` Nesnesi 
  * `label` String (İsteğe bağlı) - Görüntülenecek metin.
  * `backgroundColor` String (isteğe bağlı) - Düğme arkaplan rengi hex formatında, i.e `#ABCDEF`.
  * `icon` [NativeImage](native-image.md) (isteğe bağlı) - Buton simgesi.
  * `iconPosition` String (isteğe bağlı) - `left`, `right` yada `overlay` olabilir.
  * `click` Fonksiyon (isteğe bağlı) - Tuşa tıklandığında aranan fonksiyon.

### Örnek özellikleri

Aşağıdaki özellikler `TouchBar` örneklerinde mevcuttur:

#### `touchBarButton.label`

Bir `String` düğmenin mevcut metnini temsil eder. Bu değeri değiştirmek hemen düğmeyi günceller Dokunmatik kaydırıcıda.

#### `touchBarButton.backgroundColor`

Düğmenin geçerli arka plan rengini temsil eden `String` hex kodu. Bu değeri değiştirmek dokunmatik bardaki butonu hemen günceller.

#### `touchBarButton.icon`

A `NativeImage` representing the button's current icon. Changing this value immediately updates the button in the touch bar.