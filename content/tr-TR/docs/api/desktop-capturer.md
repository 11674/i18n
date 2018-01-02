# desktopCapturer

> Access information about media sources that can be used to capture audio and video from the desktop using the [`navigator.mediaDevices.getUserMedia`] API.

Süreç:[ İşleyici](../glossary.md#renderer-process)

Aşağıdaki örnek, ` Electron` isimli masaüstü penceresinden nasıl ekran kaydedilebileceğini göstermektedir:

```javascript
// İşleme sürecinde.
const {desktopCapturer} = require('electron')

desktopCapturer.getSources({types: ['window', 'screen']}, (error, sources) => {
  if (error) throw error
  for (let i = 0; i < sources.length; ++i) {
    if (sources[i].name === 'Electron') {
      navigator.mediaDevices.getUserMedia({
        audio: false,
        video: {
          mandatory: {
            chromeMediaSource: 'desktop',
            chromeMediaSourceId: sources[i].id,
            minWidth: 1280,
            maxWidth: 1280,
            minHeight: 720,
            maxHeight: 720
          }
        }
      }, handleStream, handleError)
      return
    }
  }
})

function handleStream (stream) {
  document.querySelector('video').src = URL.createObjectURL(stream)
}

function handleError (e) {
  console.log(e)
}
```

To capture video from a source provided by `desktopCapturer` the constraints passed to [`navigator.mediaDevices.getUserMedia`] must include `chromeMediaSource: 'desktop'`, and `audio: false`.

Ses ve videoyu masaüstünün tamamından yakalamak için [`navigator.mediaDevices.getUserMedia`] içermelidir. `chromeMediaSource: 'desktop'`, her ikisi içinde `audio` and `video`, ancak yalnızca bir `chromeMediaSourceId`kısıtlama getirilmelidir.

```javascript
const constraints = {
  audio: {
    mandatory: {
      chromeMediaSource: 'desktop'
    }
  },
  video: {
    mandatory: {
      chromeMediaSource: 'desktop'
    }
  }
}
```

## Metodlar

The `desktopCapturer` module has the following methods:

### `desktopCapturer.getSources(options, callback)`

* `options` Nesne 
  * `types` String[] - An array of Strings that lists the types of desktop sources to be captured, available types are `screen` and `window`.
  * `thumbnailSize` [Size](structures/size.md) (optional) - The size that the media source thumbnail should be scaled to. Default is `150` x `150`.
* `callback` Fonksiyon 
  * `error` Hata 
  * `sources` [DesktopCapturerSource[]](structures/desktop-capturer-source.md)

Starts gathering information about all available desktop media sources, and calls `callback(error, sources)` when finished.

`sources` is an array of [`DesktopCapturerSource`](structures/desktop-capturer-source.md) objects, each `DesktopCapturerSource` represents a screen or an individual window that can be captured.