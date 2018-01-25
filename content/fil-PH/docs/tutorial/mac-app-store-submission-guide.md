# Gabay sa pagsmite sa Mac App Store

Simula v0.34.0, pinahintulutan ng Electron ang pagsumite ng packaged apps sa Mac App Store (IMAS). Ang gabay na ito ay nagbibigay impormasyon sa: paano magsumite ng iyong app at ang limitasyon ng MAS build.

**Palatandaan:** Ang pagsumite ng app sa Mac App Store ay nangangailangan ng pag-enrol ng [Apple Developer Program](https://developer.apple.com/support/compare-memberships/), kung saan nagkakahalaga ng pera.

## Paano mag-sumite ng Iyong App

Ang sumusunod na hakbang ay nagpapakilala ng simpleng paraan upang isumite ang iyong app sa Mac App Store. Ngunit, ang mga hakbang na ito ay hindi matitiyak na maapruban ng Apple ang iyong app; kailangan mo parin basahin ang hakbang ng [Pagsumite ng Iyong App](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/SubmittingYourApp/SubmittingYourApp.html) ng Apple upang matugunan ang pangangailangan ng Mac App Store.

### Kumuha ng Sertipiko

Para isumite ang iyong app sa Mac App Store, una ay kailangan mong kumuha ng sertipiko mula sa Apple. Maaari mong sundin ang mga [nananatili na gabay](https://github.com/nwjs/nw.js/wiki/Mac-App-Store-%28MAS%29-Submission-Guideline#first-steps) sa web.

### Kumuha ng Team ID

Bago lagdaan ang iyong app, kailangan mong malaman ang Team ID ng iyong account. Para mahanap ang Team ID, mag-sign in sa [Apple Developer Center](https://developer.apple.com/account/), at i-klik ang Membership sa sidebar. Ang iyong Team ID ay makikita sa Membership impormasyon na seksyon ilalim ng pangalan ng team.

### Lagdaan ang Iyong App

Matapos magtapos ang paghahanda ng gawain, maaari mong i-package ang iyong app sa pamamagitan ng pagsunod sa [Application Distribution](application-distribution.md), at pagkatapos ipagpatuloy lagdaan ang iyong app.

Una, kailangan mong magdagdag ng isang `ElectronTeamID` na key sa app ng iyong `Info.plist`, kung saan may halaga ang iyong Team ID:

```xml
<plist version="1.0">
<dict>
  ...
  <key>ElectronTeamID</key>
  <string>TEAM_ID</string>
</dict>
</plist>
```

Tapos, dapat mong ihanda ang tatlong mga file na may karapatan.

`child.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>com.apple.security.app-sandbox</key>
    <true/>
    <key>com.apple.security.inherit</key>
    <true/>
  </dict>
</plist>
```

`parent.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>com.apple.security.app-sandbox</key>
    <true/>
    <key>com.apple.security.application-groups</key>
    <string>TEAM_ID.your.bundle.id</string>
  </dict>
</plist>
```

`loginhelper.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>com.apple.security.app-sandbox</key>
    <true/>
  </dict>
</plist>
```

Dapat mong palitan ang `TEAM_ID` sa iyong Team ID, at palitan ang `your.bundle.id` sa Bundle ID ng iyong app.

At pagkatapos, lagdaan ang iyong app sa sumusunod na mga iskrip:

```sh
#!/bin/bash

# Pangalan ng iyong app.
APP="YourApp"
# Ang landas na mag-sign ang iyong app.
APP_PATH="/path/to/YourApp.app"
# Ang landas sa lokasyon na gusto mong ilagay ang signed package.
RESULT_PATH="~/Desktop/$APP.pkg"
# Ang pangalan ng mga sertipikong hiniling mo.
APP_KEY="3rd Party Mac Developer Application: Pangalan ng Kompanya (APPIDENTITY)"
INSTALLER_KEY="3rd Party Mac Developer Installer: Pangalan ng Kompanya (APPIDENTITY)"
# Ang landas ng iyong plist na mga file.
CHILD_PLIST="/path/to/child.plist"
PARENT_PLIST="/path/to/parent.plist"
LOGINHELPER_PLIST="/path/to/loginhelper.plist"

FRAMEWORKS_PATH="$APP_PATH/Contents/Frameworks"

codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/Electron Framework.framework/Versions/A/Electron Framework"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/Electron Framework.framework/Versions/A/Libraries/libffmpeg.dylib"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/Electron Framework.framework/Versions/A/Libraries/libnode.dylib"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/Electron Framework.framework"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/$APP Helper.app/Contents/MacOS/$APP Helper"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/$APP Helper.app/"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/$APP Helper EH.app/Contents/MacOS/$APP Helper EH"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/$APP Helper EH.app/"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/$APP Helper NP.app/Contents/MacOS/$APP Helper NP"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$FRAMEWORKS_PATH/$APP Helper NP.app/"
codesign -s "$APP_KEY" -f --entitlements "$LOGINHELPER_PLIST" "$APP_PATH/Contents/Library/LoginItems/$APP Login Helper.app/Contents/MacOS/$APP Login Helper"
codesign -s "$APP_KEY" -f --entitlements "$LOGINHELPER_PLIST" "$APP_PATH/Contents/Library/LoginItems/$APP Login Helper.app/"
codesign -s "$APP_KEY" -f --entitlements "$CHILD_PLIST" "$APP_PATH/Contents/MacOS/$APP"
codesign -s "$APP_KEY" -f --entitlements "$PARENT_PLIST" "$APP_PATH"
productbuild --component "$APP_PATH" /Applications --sign "$INSTALLER_KEY" "$RESULT_PATH"
```

Kung baguhan ka sa app sandboxing ng macOS, dapat mo rin basahin sa pamamagitan ng [Enabling App Sandbox](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html) ng Apple para magkaroon ng pangunahing ideya, tapos magdagdag ng mga ideya na kailangan upang pahintulutan ang iyong app ng mga entitlement file. 

Bukod sa pag mano-manong pag-sign ng iyong app, maaari ka ring pumili na gumamit ng [electron-osx-sign](https://github.com/electron-userland/electron-osx-sign) na modyul para gawin ang trabaho.

#### Mga sign ng mga Native na Modyul

Ang mga native na modyul na ginagamit ng iyong app ay dapat ring lagdaan. Kung gagamit ng electron-osx-sign, siguraduhing isama ang landas ng built binaries sa lista ng argumento:

```sh
electron-osx-sign YourApp.app YourApp.app/Contents/Resources/app/node_modules/nativemodule/build/release/nativemodule
```

Tandaan din na ang native na mga modyul ay maaaring may intermediate na mga file na binuo na hindi sana kabilang (na dapat rin nilagdaan). Kung gumamit ka ng [electron-packager](https://github.com/electron-userland/electron-packager) bago ang bersyon 8.1.0, magdagdag ng `--ignore=.+\.o$` sa iyong build na hakbang para maiwasan ang mga file. Mga bersyon 8.1.0 at luma pa ay iniiwasan ang mga file na iyon sa pamamagitan ng default.

### I-upload aang iyong app

Matapos lagdaan ang iyong app, maaari kang gumamit ng Application Loader para mag-upload sa iTunes Connect upang ma proseso, sinisigurado na ikaw ay [nakalikha ng record](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/CreatingiTunesConnectRecord.html) bago mag-upload.

### I-sumite ang Iyong App upang Suriin

Matapos ang mga hakbang na ito, maaari mong [i-sumite ang iyong app para sa pagsusuri](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/SubmittingTheApp.html).

## Mga limitasyon ng MAS Build

Upang matugunan ang pangangailangan ng app sandboxing, ang sumusunod na modyul ay hindi pinagana sa MAS build:

* `crashReporter`
* `autoUpdater`

at ang mga sumusunod na paguugali ay binago:

* Ang bidyo capture ay maaaring hindi gumana sa ibang mga makina.
* Siguradong accessibility features ay maaaring hindi gumana.
* Ang mga app ay hindi makakaalam sa mga pagbabago ng DNS.

Tsaka, dahil sa paggagamit ng app sandboxing, ang mga sagguniang maaaring ma-access sa pamamagitan ng app ay striktong limitado; maaari mong basahin ang [App Sandboxing](https://developer.apple.com/app-sandboxing/) para sa karagdagang impormasyon.

### Additional Entitlements

Depending on which Electron APIs your app uses, you may need to add additional entitlements to your `parent.plist` file to be able to use these APIs from your app's Mac App Store build.

#### Network Access

Enable outgoing network connections to allow your app to connect to a server:

```xml
<key>com.apple.security.network.client</key>
<true/>
```

Enable incoming network connections to allow your app to open a network listening socket:

```xml
<key>com.apple.security.network.server</key>
<true/>
```

See the [Enabling Network Access documentation](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW9) for more details.

#### dialog.showOpenDialog

```xml
<key>com.apple.security.files.user-selected.read-only</key>
<true/>
```

See the [Enabling User-Selected File Access documentation](https://developer.apple.com/library/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW6) for more details.

#### dialog.showSaveDialog

```xml
<key>com.apple.security.files.user-selected.read-write</key>
<true/>
```

See the [Enabling User-Selected File Access documentation](https://developer.apple.com/library/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW6) for more details.

## Known issues

### `shell.openItem(filePath)`

This will fail when the app is signed for distribution in the Mac App Store. Subscribe to [#9005](https://github.com/electron/electron/issues/9005) for updates.

#### Workaround

`shell.openExternal('file://' + filePath)` will open the file in the default application as long as the extension is associated with an installed app.

## Cryptographic Algorithms Used by Electron

Depending on the country and region you are located, Mac App Store may require documenting the cryptographic algorithms used in your app, and even ask you to submit a copy of U.S. Encryption Registration (ERN) approval.

Electron uses following cryptographic algorithms:

* AES - [NIST SP 800-38A](https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf), [NIST SP 800-38D](https://csrc.nist.gov/publications/nistpubs/800-38D/SP-800-38D.pdf), [RFC 3394](https://www.ietf.org/rfc/rfc3394.txt)
* HMAC - [FIPS 198-1](https://csrc.nist.gov/publications/fips/fips198-1/FIPS-198-1_final.pdf)
* ECDSA - ANS X9.62–2005
* ECDH - ANS X9.63–2001
* HKDF - [NIST SP 800-56C](https://csrc.nist.gov/publications/nistpubs/800-56C/SP-800-56C.pdf)
* PBKDF2 - [RFC 2898](https://tools.ietf.org/html/rfc2898)
* RSA - [RFC 3447](http://www.ietf.org/rfc/rfc3447)
* SHA - [FIPS 180-4](https://csrc.nist.gov/publications/fips/fips180-4/fips-180-4.pdf)
* Blowfish - https://www.schneier.com/cryptography/blowfish/
* CAST - [RFC 2144](https://tools.ietf.org/html/rfc2144), [RFC 2612](https://tools.ietf.org/html/rfc2612)
* DES - [FIPS 46-3](https://csrc.nist.gov/publications/fips/fips46-3/fips46-3.pdf)
* DH - [RFC 2631](https://tools.ietf.org/html/rfc2631)
* DSA - [ANSI X9.30](https://webstore.ansi.org/RecordDetail.aspx?sku=ANSI+X9.30-1%3A1997)
* EC - [SEC 1](http://www.secg.org/sec1-v2.pdf)
* IDEA - "On the Design and Security of Block Ciphers" book by X. Lai
* MD2 - [RFC 1319](https://tools.ietf.org/html/rfc1319)
* MD4 - [RFC 6150](https://tools.ietf.org/html/rfc6150)
* MD5 - [RFC 1321](https://tools.ietf.org/html/rfc1321)
* MDC2 - [ISO/IEC 10118-2](https://wiki.openssl.org/index.php/Manual:Mdc2(3))
* RC2 - [RFC 2268](https://tools.ietf.org/html/rfc2268)
* RC4 - [RFC 4345](https://tools.ietf.org/html/rfc4345)
* RC5 - http://people.csail.mit.edu/rivest/Rivest-rc5rev.pdf
* RIPEMD - [ISO/IEC 10118-3](https://webstore.ansi.org/RecordDetail.aspx?sku=ISO%2FIEC%2010118-3:2004)

On how to get the ERN approval, you can reference the article: [How to legally submit an app to Apple’s App Store when it uses encryption (or how to obtain an ERN)](https://carouselapps.com/2015/12/15/legally-submit-app-apples-app-store-uses-encryption-obtain-ern/).