# Seguridad, Katutubong Kakayahan, at Iyong Responsibilidad

Bilang isang web developer, karaniwan nating matamasa ang malakas na seguridad sa net ng browser - ang panganib na kaugnay sa ang code naisulat ay mahigit kumulang na kaunti lamang. Ang aming websites ay ipinagkakaloob ng limitadong kakayahan sa isang sandbox, at nagtitiwala na ang mga gumagamit ay nag-ienjoy sa browser na itinayo ng mga mamalaking koponan nga inhinyero na mabilis na makatugon sa bagong tuklas na banta ng seguridad.

Kapang nagtatrabaho sa Elektron, importanteng maintindihan na ang Elektron ay hindi web brwoser. Ito ay nagpapahintulot sa iyo na magtayo ng saganang-tampok na desktop applications na may pamilyar na teknolohiyang web, ngunit ang iyong code ay gumagamit ng mas higit na kakayahan. JavaScript ay pwedeng ma-access ang filesystem, user shell, at iba pa. Ito ay nagpapahintulot sayo na magtayo ng mataas na kalidad na native applications, pero ang likas na antas sa panganib ng seguridad na may dagdag na kakayahan ay ipinagkaloob sa iyong code. 

Nasasaisip iyan, dapat maging alerto sa mga nagpapakitang hindi makatwirang nilalaman sa mga di mapagkakatiwalaang pinanggalingan ay nagmumungkahi ng mahigpit na panganib sa seguridad na ang Elektron ay hindi nilalayon na maghawak. Sa katunayan, ang pinaka-popyular na Electron apps (Atom, Slack, Visual Studio Code, at iba pa) ay pangunahing nagpapakita ng lokal na nilalaman ( o pinagkakatiwalaan, ligtas na nilalaman na walang Node integration)- kung ang iyong aplikasyon ay nagsasagawa ng code na mula sa online, responsibilidad mo na tiyaking na ang code ay hindi malisyoso.

## Reporting Security Issues

For information on how to properly disclose an Electron vulnerability, see [SECURITY.md](https://github.com/electron/electron/tree/master/SECURITY.md)

## Chromium Security Issues and Upgrades

While Electron strives to support new versions of Chromium as soon as possible, developers should be aware that upgrading is a serious undertaking - involving hand-editing dozens or even hundreds of files. Given the resources and contributions available today, Electron will often not be on the very latest version of Chromium, lagging behind by either days or weeks.

We feel that our current system of updating the Chromium component strikes an appropriate balance between the resources we have available and the needs of the majority of applications built on top of the framework. We definitely are interested in hearing more about specific use cases from the people that build things on top of Electron. Pull requests and contributions supporting this effort are always very welcome.

## Ignoring Above Advice

A security issue exists whenever you receive code from a remote destination and execute it locally. As an example, consider a remote website being displayed inside a browser window. If an attacker somehow manages to change said content (either by attacking the source directly, or by sitting between your app and the actual destination), they will be able to execute native code on the user's machine.

> :warning: Under no circumstances should you load and execute remote code with Node integration enabled. Instead, use only local files (packaged together with your application) to execute Node code. To display remote content, use the `webview` tag and make sure to disable the `nodeIntegration`.

#### Checklist

This is not bulletproof, but at the least, you should attempt the following:

* Only display secure (https) content
* Disable the Node integration in all renderers that display remote content (setting `nodeIntegration` to `false` in `webPreferences`)
* Enable context isolation in all renderers that display remote content (setting `contextIsolation` to `true` in `webPreferences`)
* Use `ses.setPermissionRequestHandler()` in all sessions that load remote content
* Do not disable `webSecurity`. Disabling it will disable the same-origin policy.
* Define a [`Content-Security-Policy`](http://www.html5rocks.com/en/tutorials/security/content-security-policy/) , and use restrictive rules (i.e. `script-src 'self'`)
* [Override and disable `eval`](https://github.com/nylas/N1/blob/0abc5d5defcdb057120d726b271933425b75b415/static/index.js#L6-L8) , which allows strings to be executed as code.
* Do not set `allowRunningInsecureContent` to true.
* Do not enable `experimentalFeatures` or `experimentalCanvasFeatures` unless you know what you're doing.
* Do not use `blinkFeatures` unless you know what you're doing.
* WebViews: Do not add the `nodeintegration` attribute.
* WebViews: Do not use `disablewebsecurity`
* WebViews: Do not use `allowpopups`
* WebViews: Do not use `insertCSS` or `executeJavaScript` with remote CSS/JS.
* WebViews: Verify the options and params of all `<webview>` tags before they get attached using the `will-attach-webview` event:

```js
app.on('web-contents-created', (event, contents) => {
  contents.on('will-attach-webview', (event, webPreferences, params) => {
    // Strip away preload scripts if unused or verify their location is legitimate
    delete webPreferences.preload
    delete webPreferences.preloadURL

    // Disable node integration
    webPreferences.nodeIntegration = false

    // Verify URL being loaded
    if (!params.src.startsWith('https://yourapp.com/')) {
      event.preventDefault()
    }
  })
})
```

Again, this list merely minimizes the risk, it does not remove it. If your goal is to display a website, a browser will be a more secure option.