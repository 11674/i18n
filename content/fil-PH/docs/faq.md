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

## Ang aking window app's/tray na nawawala pagkatapos ng ilang minuto.

Nangyayari ito kapag ang variable na ginagamit upang i-imbak ang window/tray ay nakakakuha nakolekta ang basura.

Kung nakatagpo ka ng problemang ito, maaaring makatulong ang mga sumusunod na artikulo:

* [Pamamahala ng kaisipan](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)
* [Baryanteng Saklaw](https://msdn.microsoft.com/library/bzt2dkta(v=vs.94).aspx)

Kung gusto mo ng isang mabilis na ayusin, maaari mong gawin ang mga variable global sa pamamagitan ng pagpapalit ng iyong code mula dito:

```javascript
const {app, Tray} =kailangan ('electron')
app.on('ready', () => {
  const tray = new Tray('/path/to/icon.png')
  tray.setTitle('hello world')
})
```

sa ganito:

```javascript
const {app, Tray} = kailangan('electron')
hayaan ang tray = wala
app.on('ready', () => {
  tray = new Tray('/path/to/icon.png')
  tray.setTitle('hello world')
})
```

## Hindi ko magamit ang jQuery/RequireJS/Meteor/AngularJS sa Electron.

Dahil sa pagsasama ng Node.js ng Electron, mayroong ilang dagdag na simbolo ipinasok sa DOM tulad ng `module`,`exports`, `require`. Ito ay nagiging sanhi ng mga problema para sa ilang mga aklatan dahil gusto nilang ipasok ang mga simbolo na may parehong mga pangalan.

Upang malutas ito, maaari mong i-off ang pagsasama ng node sa Electron:

```javascript
// Sa pangunahing proseso. 
const {BrowserWindow} = require('electron')
let win = new BrowserWindow({
  webPreferences: {
    nodeIntegration: false
  }
})
win.show()
```

Ngunit kung nais mong panatilihin ang mga kakayahan ng paggamit ng Node.js at Electron API, ikaw kailangang palitan ang pangalan ng mga simbolo sa pahina bago isama ang iba pang mga library:

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