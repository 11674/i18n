# "Packaging" ng Aplikasyon

Para maiwasan ang [issues](https://github.com/joyent/node/issues/6960) sa paligid ng mahabang pangalan sa landas ng Windows, bahagyang mapapabilis ang `require` para itago ang iyong source code mula sa pinasimpleng inspeksyon, pwede kang mamili para I- package ang iyong app isang [asar](https://github.com/electron/asar) na aktibo sa maliit na pag babago sa iyong source code.

## Pagbuo ng `asar` na aktibo

Ang "[asar](https://github.com/electron/asar) archive" ay isang simpleng ayos tulad ng "tar" na dinudugtong sa mga payl upang maging isang payl. Ang Elektron ay maaaring basahin ang mga payl na arbitraryo galing dito nang hindi kinukuha sa buong payl.

Mga hakbang para mailagak ang iyong "app" sa "archive" ng `asar`:

### 1. Iinstol ang "asar Utility"

```sh
$ npm install -g asar
```

### 2. Ang ilang "software" kasama ang `asar pack`

```sh
$ asar pack your-app app.asar
```

## Gamit ang "Archives" ng `asar`

Sa Elektron, mayroong dalawang nakatakdang "APIs": "Node APIs" na galing sa Node.js at ang "Web APIs na galing sa " Chrilomium". Ang parehong "APIs" ay sumusuporta sa pagbasa ng mga payl galing sa mga "archive" ng `asar`.

### "Node API"

Kasama ang espesyal na "patches" sa Elektron, "Node APIs" tulad ng `fs.readFile` at `require` ay itinuturing ang "archive" ng `asar` bilang mga direktoryo ng birtwal, at ang mga payl dito at bilang mga normal na payl sa sistema nito.

Halimbawa, ating ipagpalagay na ang "archive" na `example.asar` sa ilalim ng `/path/to`:

```sh
$ asar list /path/to/example.asar
/app.js
/file.txt
/dir/module.js
/static/index.html
/static/main.css
/static/jquery.min.js
```

Basahin ang oayl sa "archive" ng `asar`:

```javascript
const fs = require('fs')
fs.readFileSync('/path/to/example.asar/file.txt')
```

Ang listahan ng lahat ng payl sa ilalim ng "root" ng "archive":

```javascript
const fs = require('fs')
fs.readdirSync('/path/to/example.asar')
```

Gumamit ng modyul galing sa "archive":

```javascript
require('/path/to/example.asar/dir/module.js')
```

Maaari ring i-display ang pahina ng "web" sa "archive" na `asar` kasama ang `BrowserWindow`:

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow({width: 800, height: 600})
win.loadURL('file:///path/to/example.asar/static/index.html')
```

### "Web API"

Sa pahina ng "web", ang mga payl na nasa "archive" ay maaaring hilingin sa protokol ng `file:`. Tulad ng "Node API", ang mga "archive" ng `asar` ay tinuturing bilang mga direktoryo.

Halimbawa, para makuha ang payl gamit ang `$.get`:

```html
<script>
let $ = require('./jquery.min.js')
$.get('file:///path/to/example.asar/file.txt', (data) => {
  console.log(data)
})
</script>
```

### Itinuturing ang "Archive" ng `asar` bilang Normal na Payl

Para sa ibang mga kaso tulad ng pagkumpirma sa "archive's checksum" ng `asar`, kinakailangan nating basahin ang nilalaman ng "archive" ng `asar` bilang payl. Para sa layuning ito, maaaring gumamit ng "built-in" na modyul ng `original-fs` na nagbibigay ng orihinal na `fs` APIs nang hindi kasama ang suporta ng `asar`:

```javascript
const originalFs = require('original-fs')
originalFs.readFileSync('/path/to/example.asar')
```

Maaari ring itakda ang `process.noAsar` sa `true` para hindi magamit an suporta para sa `asar` sa modyul ng `fs`:

```javascript
const fs = require('fs')
process.noAsar = true
fs.readFileSync('/path/to/example.asar')
```

## Mga Limitasyon ng Node API

Kahit pa ating subukan na gumawa ng "archive" ng `asar`, hangga't maaari sa loob ng "Node API" tulad ng mga direktoryo, mayroon pa ring mga limitasyon dahil sa natural mababang lebel ng ng "Node API".

### Ang mga "Archives" ay "Read-only"

Ang mga "archive" ay hindi maaaring mabago kaya ang lahat ng "Node APIs" na maaaring mabago ang mga payl ay hindi gagana sa "archive" ng `asar`.

### Ang Gumaganang Direktoryo ay Hindi Maaaring Itakda sa mga Direktoryo sa "Archive"

Bagaman, ang mga "archive" ng `asar` ay itinuturing bilang mga direktoryo, walang aktwal na mga direktoryo sa loob ng sistema ng payl, kaya kailanman ay hindi maaaring itakda ang tumtakbong direktoryo sa mga direktoryo sa mga "archive" ng `asar`. Ang pagpapadaan sa kanila bilang opsyon ng `cwd` ng ilang APIs ay magiging dahilan din ng mga mali.

### Dagdag na "Unpacking" sa Ilang APIs

Karamihan sa `fs` APIs ay nakakabasa ng payl o kumukuha ng impormasyon ng payl galing sa mga "archive" ng `asar` nang hindi kasama ang "unpacking", ngunit para sa ilang APIs na nakadepende sa pagdaan sa totoong "path" ng payl na pinagbabatayan ng pagtawag ng sistema, ang Elektron ay ililipat ang mga kailangang payl sa pansamantalang payl at dadaan sa "path" ng pansamantalang patungo sa "APIs" para sila ay gumana. Ito ay nagdadagdag ng kaunting "overhead" para sa mga APIs.

Ang APIs na nangangailangan ng karagdagang "unpacking" ay mga:

* `child_process.execFile`
* `child_process.execFileSync`
* `fs.open`
* `fs.openSync`
* `process.dlopen` - Used by `require` on native modules

### Fake Stat Information of `fs.stat`

Ang `Stats` ay bagay na bumalik batay sa `ft.stat` At sa magkalapit na mga pile ng `asar` Archives ay nabuo sa pamamagitan ng paghula, dahil ang ibang mga pileay hindi umiiral sa mga sistema ng pile. Kaya hindi dapat pagkatiwalaan ang `Stats` na bagay maliban para sa pagkuha ng pile Sukat at apula na uri ng pile.

### Magsagawa ng mga dobleng nilalaman `asar` Archive

There are Node APIs that can execute binaries like `child_process.exec`, `child_process.spawn` and `child_process.execFile`, but only `execFile` is supported to execute binaries inside `asar` archive.

This is because `exec` and `spawn` accept `command` instead of `file` as input, and `command`s are executed under shell. There is no reliable way to determine whether a command uses a file in asar archive, and even if we do, we can not be sure whether we can replace the path in command without side effects.

## Karagdagang Unpacked ng mga pile sa `asar` Archive

Katulad ng nakasaad sa itaas, ilan sa mga Node API ay nasa unpack ng pile para sa sistema ng pile kapag nagtatawag, bukod sa paggawa ng mga isyu, ito ay maaari din magbigay daan para sa mga maling alerto ng mga birus scanner.

To work around this, you can unpack some files creating archives by using the `--unpack` option, an example of excluding shared libraries of native modules is:

```sh
$ asar pack app app.asar --unpack *.node
```

After running the command, apart from the `app.asar`, there is also an `app.asar.unpacked` folder generated which contains the unpacked files, you should copy it together with `app.asar` when shipping it to users.