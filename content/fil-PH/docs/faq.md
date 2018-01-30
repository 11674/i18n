# Electron FAQ

## Bakit ako nagkakaproblema sa pag-install ng Electron?

Habang pinatatakbo and `npm install electron`, ang ibang user ay kadalasang nakakasalubong ng error sa pag-install.

Sa maraming pagkakataon, ang mga problemang ito ay resulta ng problema sa network at hindit talaga aktwal na isyu ng `electron` npm package. Ang problema gaya ng `ELIFECYCLE`,`EAI_AGAIN`, `ECONNRESET`, and `ETIMEDOUT` ay mga indikasyon na mayroon problema sa iyong network. Ang pinaka mainam na solusyon ay subukang pagpalitin ang mga network o hindi kaya ay maghintay ng kaunti at subukang iinstall muli.

Maari mo ring subukan i download ang Electron ng direkta mula sa [electron/electron/releases](https://github.com/electron/electron/releases)kung ang pag-iinstall sa pamamagitan ng `npm` ay hindi nagtatagumpay.

## Kailan ang Electron mag-aupgrade sa pinakabagong Chrome?

Ang Chrome version ng Electron ay kadalasang inilalabas sa loob ng isa o dalawang linggo pagkatapos mailabas ang bagong maayos na Chrome version. Ang tayang ito ay hindi garantisado at depende parin sa dami ng trabahong kailangang gawin na nakapaloob sa pag-a upgrade.

Tanging ang matibay na tsanel ng Chrome ang gagamitin, kung ang importanteng pag-aayos ay nasa beta o dev channel, ito ay aming i ba back-port.

Para sa karagdagang impormasyon, mangyaring tignan lamang ang [ security introduction](tutorial/security.md).

## Kailan ang Electron mag-aupgrade sa pinakabagong Node.js?

Kapag ang bagong bersyon ng Node.js ay nailabas, kadalasan ay naghihintay kami ng isang buwan bago i-upgrade ang isang nasa Electron. Para maiwasan naming maapektuhan ng mga bugs na kasama sa bagong bersyon ng Node.js, na nangyayari ng madalas.

Ang bagong tampok ng Node.js ay kadalasang dala ng V8 upgrade, dahil ang Electron ay gumagamit ng V8 na dala ni Chrome browser, ang makinang at bagong tampok mula sa JavaScript ng bagong bersyon ng Node.js ay kadalasang kasama na sa Electron.

## Paano magbahagi ng data sa pagitan ng mga pahina ng web?

Para magbahagi ng mga datos sa pagitan ng pahina ng web ( ang nagbabahagi ay nagpoproseso) ang pinaka simpleng paraan ay ang paggamit ng HTML5 API na maari nang gamitin sa mga browser. Ang mabuting kandidato ay [Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Storage), [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage), [`sessionStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage), and [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API).

O maaari mo ring gamitin ang IPC system, na partikular na sa Electron, upang itabi ang mga bagay sa pangunahing proseso bilang isang pandaigdigang variable, at pagkatapos para ma access ang mga ito mula sa mga renderers sa pamamagitan ng `remote`property `electron`module:

```javascript
Sa mga pangunahing proseso. global.sharedObject = {
  someProperty: 'default value'
}
```

```javascript
Sa pahina 1.require('electron').remote.getGlobal ('sharedObject').someProperty = 'bagong halaga'
```

```javascript
// In page 2.
console.log(require('electron').remote.getGlobal('sharedObject').someProperty)
```

## My app's window/tray disappeared after a few minutes.

This happens when the variable which is used to store the window/tray gets garbage collected.

If you encounter this problem, the following articles may prove helpful:

* [Memory Management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)
* [Variable Scope](https://msdn.microsoft.com/library/bzt2dkta(v=vs.94).aspx)

If you want a quick fix, you can make the variables global by changing your code from this:

```javascript
const {app, Tray} = require('electron')
app.on('ready', () => {
  const tray = new Tray('/path/to/icon.png')
  tray.setTitle('hello world')
})
```

to this:

```javascript
const {app, Tray} = require('electron')
let tray = null
app.on('ready', () => {
  tray = new Tray('/path/to/icon.png')
  tray.setTitle('hello world')
})
```

## I can not use jQuery/RequireJS/Meteor/AngularJS in Electron.

Due to the Node.js integration of Electron, there are some extra symbols inserted into the DOM like `module`, `exports`, `require`. This causes problems for some libraries since they want to insert the symbols with the same names.

To solve this, you can turn off node integration in Electron:

```javascript
// In the main process.
const {BrowserWindow} = require('electron')
let win = new BrowserWindow({
  webPreferences: {
    nodeIntegration: false
  }
})
win.show()
```

But if you want to keep the abilities of using Node.js and Electron APIs, you have to rename the symbols in the page before including other libraries:

```html
<head>
<script>
window.nodeRequire = require;
delete window.require;
delete window.exports;
delete window.module;
</script>
<script type="text/javascript" src="jquery.js"></script>
</head>
```

## `require('electron').xxx` is undefined.

When using Electron's built-in module you might encounter an error like this:

```sh
> require('electron').webFrame.setZoomFactor(1.0)
Uncaught TypeError: Cannot read property 'setZoomLevel' of undefined
```

This is because you have the [npm `electron` module](https://www.npmjs.com/package/electron) installed either locally or globally, which overrides Electron's built-in module.

To verify whether you are using the correct built-in module, you can print the path of the `electron` module:

```javascript
console.log(require.resolve('electron'))
```

and then check if it is in the following form:

```sh
"/path/to/Electron.app/Contents/Resources/atom.asar/renderer/api/lib/exports/electron.js"
```

If it is something like `node_modules/electron/index.js`, then you have to either remove the npm `electron` module, or rename it.

```sh
npm i-tanggalin ang electron npm tanggalin ang -g electron
```

Subalit kung ikaw ay gumagamit ng mga built-in module at nakakakuha ka parin ng mga ganitong error, ito ay malamang na ang ginagamit mong module ay nasa maling proseso. Halimbawa`electron.app`ay maari lamang magamit sa pangunahing proseso, habang ang `electron.webFrame`ay tanging magagamit sa proseso ng tagasalin.