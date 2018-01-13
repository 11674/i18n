# screen

> Ekran boyutu, ekranlar, imleç konumu vb. hakkında bilgi alın.

Process: [Main](../glossary.md#main-process), [Renderer](../glossary.md#renderer-process)

You cannot require or use this module until the `ready` event of the `app` module is emitted.

`screen` is an [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter).

**Note:** In the renderer / DevTools, `window.screen` is a reserved DOM property, so writing `let {screen} = require('electron')` will not work.

Tüm ekranı kaplayan bir pencere oluşturmanın örneği:

```javascript
const electron = require('electron')
const {app, BrowserWindow} = electron

let win

app.on('ready', () => {
  const {width, height} = electron.screen.getPrimaryDisplay().workAreaSize
  win = new BrowserWindow({width, height})
  win.loadURL('https://github.com')
})
```

Harici ekranda bir pencere oluşturmanın bir diğer örneği:

```javascript
const electron = require('electron')
const {app, BrowserWindow} = require('electron')

let win

app.on('ready', () => {
  let displays = electron.screen.getAllDisplays()
  let externalDisplay = displays.find((display) => {
    return display.bounds.x !== 0 || display.bounds.y !== 0
  })

  if (externalDisplay) {
    win = new BrowserWindow({
      x: externalDisplay.bounds.x + 50,
      y: externalDisplay.bounds.y + 50
    })
    win.loadURL('https://github.com')
  }
})
```

## Olaylar

`ekran` modülü aşağıdaki olayları yayar:

### Etkinlik: 'görünüm-eklendi'

Dönüşler:

* `olay` Olay
* `newDisplay` [Display](structures/display.md)

`newDisplay` eklendiğinde ortaya çıkar.

### Olay: 'Görünüm-kaldırıldı'

Dönüşler:

* `olay` Olay
* `oldDisplay` [Display](structures/display.md)

`oldDisplay` kaldırıldığında yayılır.

### Event: 'display-metrics-changed'

Dönüşler:

* `olay` Olay
* `display` [Display](structures/display.md)
* `changedMetrics` String[]

Emitted when one or more metrics change in a `display`. The `changedMetrics` is an array of strings that describe the changes. Possible changes are `bounds`, `workArea`, `scaleFactor` and `rotation`.

## Metodlar

`ekran` modülü aşağıdaki olayları içerir:

### `screen.getCursorScreenPoint()`

[`Point`](structures/point.md) geri alır

Fare işaretçisinin geçerli mutlak konumu.

### `screen.getMenuBarHeight()` *macOS*

`Tamsayı` Dödürür - Menü çubuğunun piksel olarak yüksekliği.

### `screen.getPrimaryDisplay()`

[`Ekran`](structures/display.md) Dödürür - Birincil görüntü.

### `screen.getAllDisplays()`

[`Görüntü[]`](structures/display.md) Dödürür - Şu anda mevcut ekran görüntüleri dizisi.

### `screen.getDisplayNearestPoint(point)`

* `point` [Point](structures/point.md)

[`Görüntü`](structures/display.md) Dödürür - Belirtilen noktaya en yakın ekran.

### `screen.getDisplayMatching(rect)`

* `rect` [Rectangle](structures/rectangle.md)

[`Görüntü`](structures/display.md) - En yakından izlenen ekran verilen sınırları kesişir.