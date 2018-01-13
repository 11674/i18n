# Konu Özeti

> Node.js ve Electron API'leri nasıl kullanılır.

All of [Node.js's built-in modules](https://nodejs.org/api/) are available in Electron and third-party node modules also fully supported as well (including the [native modules](../tutorial/using-native-node-modules.md)).

Electron ayrıca yerli masaüstü uygulamaları üretiminin geliştirilmesi için ekstra yerleşik modüller de sağlamaktadır. Bazı modüller yalnızca ana süreçte bulunur; bazıları yalnızca işleyici sürecinde (internet sayfası) mevcuttur ve bazıları her iki işlemde de kullanılabilir.

Temek kural şudur: eğer modül [GUI](https://en.wikipedia.org/wiki/Graphical_user_interface) yada düşük-seviyeli bir sistemle alakalı ise, o zaman sadece ana işlemde olmalıdır. You need to be familiar with the concept of [main process vs. renderer process](../tutorial/quick-start.md#main-process) scripts to be able to use those modules.

Ana işlem komut dosyası normal bir Node.js komut dosyası gibidir:

```javascript
const {app, BrowserWindow} = require('electron')
let win = null

app.on('ready', () => {
  win = new BrowserWindow({width: 800, height: 600})
  win.loadURL('https://github.com')
})
```

Oluşturma işlemi, düğüm modüllerini kullanmanın ekstra yeteneği haricinde normal bir internet sayfasından farklı değildir:

```html
<!DOCTYPE html>
<html>
<body>
<script>
  const {app} = require('electron').remote
  console.log(app.getVersion())
</script>
</body>
</html>
```

Uygulamanızı çalıştırmak için,[Run your app](../tutorial/quick-start.md#run-your-app).

## Destructuring assignment

As of 0.37, you can use [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to make it easier to use built-in modules.

```javascript
const {app, BrowserWindow} = require('electron')

let win

app.on('ready', () => {
  win = new BrowserWindow()
  win.loadURL('https://github.com')
})
```

If you need the entire `electron` module, you can require it and then using destructuring to access the individual modules from `electron`.

```javascript
const electron = require('electron')
const {app, BrowserWindow} = electron

let win

app.on('ready', () => {
  win = new BrowserWindow()
  win.loadURL('https://github.com')
})
```

Bu, aşağıdaki koda eşdeğerdir:

```javascript
const electron = require('electron')
const app = electron.app
const BrowserWindow = electron.BrowserWindow
let win

app.on('ready', () => {
  win = new BrowserWindow()
  win.loadURL('https://github.com')
})
```