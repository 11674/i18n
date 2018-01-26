# Pamamahagi ng aplikasyon

Upang maipamahagi ang iyong app sa elektron, kailangan mong i-download ang elektron mga binary ng prebuilt<0/>. Sunod, ang foloder na naglalaman ng iyong app ay dapat nakapangalan sa `app` at iniligay sa mga pinagkukunan ng Elektron Ang direktoryo ay ipinapakita sa mga sumusunod na halimbawa. Tandaan na ang mga lokasyon ng mga binary sa Elektron prebuilt ay ipinapahiwatig ng `elektron/<0> sa mga halimbawa sa ibaba.</p>

<p>Sa macOs:</p>

<pre><code class="text">electron/Electron.app/Contents/Resources/app/
├── package.json
├── main.js
└── index.html
`</pre> 

Sa windows at linux:

```text
electron/resources/app
├── package.json
├── main.js
└── index.html
```

Pagkatapos magsagawa ng `Electron.app` (o `elektron` sa Linux,`electron.exe` sa windows), at ang elektron ay magsisimula bilang iyong app. Ang `elektron`direktoryo ay magiging iyong ma-i-pamamahagi upang ihatid sa mga huling gumagamit.

## Pagkaging ng iyong App sa isang File

Bukod sa pagpapadala sa iyong app sa pamamagitan ng pag-kopya sa lahat ng mga pinagkukunan ng file, maari mong i-package ang iyong app sa [asar](https://github.com/electron/asar) archive upang maiwasan ang paglalantad sa iyong app source sa mga gumagamit.

Para gamitin ang `asar` i-archive para palitan ang `app` na folder, kailangan mo ring palitan ang pangalan ang archive sa `app.asar`, at ilagay ito sa ilalim ng mga mapagkukunan sa Electron na direktoryo kagaya ng nasa ilalim at ang Electron ay susubukin ulit basahin ang archive at simulan ulit ito.

Sa macOs:

```text
electron/Electron.app/Contents/Resources/
└── app.asar
```

Sa windows at linux:

```text
electron/resources/
└── app.asar
```

Sa mga karagdagang detalye ay matatagpuan sa [Aplikasyon ng packaging](application-packaging.md).

## Rebranding sa na-download ng mga binary

Pagkatapos mong pag-bundle ng iyong app sa Elektron, gusto mong i-rebrand ang elektron bago ipamamahagi ito sa mga gumagamit.

### Windows

Maari mong palitan ang `electron.exe` sa anumang pangalan na gusto mo, at i-edit ang icon nito at iba pang impormasyong sa mga kagamitang [credit](https://github.com/atom/rcedit).

### macOS

Maari mong palitan ng pangalan ang `Electron.app` sa anumang gusto mo, at maari mo ring palitan ng pangalan anh `CFBundleDisplayName`,</code>CFBundleIdentifier</code> at `CFBundleName</0> sa mga larangan sa mga sumusunod na file:</p>

<ul>
<li><code>Electron.app/Contents/Info.plist`</li> 

* `Electron.app/Contents/Frameworks/Electron Helper.app/Contents/Info.plist`</ul> 

Maari mo ring palitan ng pangalan ang tumutulong na app upang maiwasan ang pagpapakita `Electron Helper` sa aktibidad ng monitor, ngunit siguraduhin na palitan mo nang pangalan ang helper app's para mapalitan ng pangalan ang mga file.

Ang istraktura na pinalitang pangalan ng app ay magiging tulad ng:

```text
AngAkingApp.app/Mga Nilalaman
├── Info.plist
├── MacOS/
│   └── MyApp
└── Frameworks/
    ├── MyApp Helper EH.app
    |   ├── Info.plist
    |   └── MacOS/
    |       └── MyApp Helper EH
    ├── MyApp Helper NP.app
    |   ├── Info.plist
    |   └── MacOS/
    |       └── MyApp Helper NP
    └── MyApp Helper.app
        ├── Info.plist
        └── MacOS/
            └── MyApp Helper
```

### Linux

Maari mong palitan ng pangalan ang `elektron` maaring palitan sa anumang gusto mo.

## Mga kagamitan sa packaging

Bukod sa pagmano mano ng packaging sa iyong app, pwede ka ring pumili para gumamit ng ikatlong partido na kagamitan ng packaging para gawin ang trabaho para sayo:

* [pagpipilit ng elektron](https://github.com/electron-userland/electron-forge)
* [pagbubuo ng elektron](https://github.com/electron-userland/electron-builder)
* [pagpapackage ng elektron](https://github.com/electron-userland/electron-packager)

## Rebranding sa pamamagitan ng pagbubuo muli ng Elekron mula sa pinagmulan

Posible rin ito na i-rebrand ang elektron sa pamamagitan ng pagbabago ng pangalan sa produkto at binuong ito mula sa pinagmulan. Upang magawa ito kailangan mong baguhin ang `atom.gyp` ng file at malinis itong binubuo ulit.

### Lumikha ng Pasadyang Elektron Fork

Lumikha ng pasadyang fork sa elektron ay halos tiyak na walang iba kinakailngan mong gumawa sa pagkasunod-sunod upang mabuo ang iyong app, pantay para sa "Level ng produksyon" ng mga aplikasyon. Gamit ng kagamitan tulad ng `packager ng elekron` o `elektron-forge` ay nagbibigay daan sayo upang "i-Rebrand" ang elektron kahit walang pakakaroon nitong ng mga hakbang.

Kailangan mong mag fork Electron kapag ikaw ay mag pasadyang C++ na kodigo na iyong ipinatched direkta sa Electron, na alinmang hindi pwedeng i-upstream, o na tinanggihan mula sa opisyal na bersyon. Bilang mga maintainer ng Elektron, napakagusto naming gawin ang sitwasyon ng iyong trabaho, kaya mangyaring subukan ng maigi hanggang makukuha mo ang mga pagbabago sa opisyal na bersyon ng Elektron, mas madali para sayo, at pinahahalagahan namin ang iyong tulong.

#### Paglikha ng isa pasadyang release sa pagbuo ng surf

1. I-install ang [Surf](https://github.com/surf-build/surf), via npm: `npm install -g surf-build@latest`

2. Gumawa ng bagong S3 bucket at gumawa ng sumusunod na walang laman na direktoryong istraktura:
    
    ```sh
- atom-shell/
  - simbolo/
  - dist/
```

3. Itakda ang mga sumusunod na baryabol ng kapaligiran:

* `ELEKTRON_GITHUB_TOKEN` - ay isang token na maaring lumikha ng mga release sa GitHub.
* `ELECTRON_S3_ACCESS_KEY`, `ELECTRON_S3_BUCKET`, `ELECTRON_S3_SECRET_KEY` - ang lugar kung saan maari kang mag-upload ng mga header sa node.js pati na rin ang mga simbolo
* `ELECTRON_RELEASE` - Itakda sa `tama` at ang parte ng pag-upload ay gagana, iwanan ang i-unset At `surf-bumuo` gagawin namin ang mga pagsusuri ng CI-type, nararapat patakbuhin sa bawat pull ng kahilingan.
* `CI<code> - Itakda sa <0>tama` o kung hindi ito ay mabibigo
* `GITHUB_TOKEN` - itakda ito sa kaparehong `ELECTRON_GITHUB_TOKEN`
* `SURF_TEMP` - itakda sa `C:\Temp` sa windows upang maiwasan ang mga landas na isyu sa mahabang isyu
* `TARGET_ARCH` - itakda sa `ia32` o `x64`

1. Sa `script/upload.py`, maari *kang* magtakda nf `ELECTRON_REPO` sa iyong fork (`MYORG/electron`), lalo na kung ikaw ay isang kontribyutor sa Elektron proper.

2. `surf-build -r https://github.com/MYORG/electron -s YOUR_COMMIT -n 'surf-PLATFORM-ARCH'`

3. Maghintay ng napaka, napakataas na panahon para makompleto ang build.