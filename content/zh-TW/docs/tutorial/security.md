# 安全性、原生功能及你的責任

身為網頁開發人員，我們常常受惠於瀏覽器建立的強大安全網，我們的程式再怎麼搞，所能引起的風險都微乎其微。 我們的網站被限制在獨立的沙盒環境中運作，我們相信使用者都習慣於享受由龐大工程師團隊開發維護的瀏覽器，能在第一時間快速處理新發現的安全性威脅。

而在使用 Electron 時，千萬要記住一點: Electron 並不是網頁瀏覽器。 它讓你能用熟悉的網頁技術打造出功能完善的桌面應用程式，只不過你的程式碼具有更大的能力。 JavaScript 能存取檔案系統，使用者 Shell 等等。 你能做出高品質的原生應用程式，但是你的程式碼被賦與的能力越強，相對的安全性問題也會越重。

請謹記在心，顯示不能完全信任的來源產生的任意內容會有極大的安全風險，Electron 也沒打算處理。 事實上，流行的 Electron 應用程式 (Atom, Slack, Visual Studio Code 等) 幾乎都只顯示本機的內容 (或受信任、安全且沒跟 Node 整合的遠端內容) ，如果你的應用程式會執行網路上抓來的程式碼，你要負責確保那些程式碼中不會有惡意內容。

## 回報安全性問題

如何正確揭露 Electron 漏洞的相關資訊可參考 [SECURITY.md](https://github.com/electron/electron/tree/master/SECURITY.md)

## Chromium 安全性議題及升級

雖然 Electron 致力於盡快支援新版的 Chromium，但開發人員都應該知道，升級是件嚴肅的事，可能要手動修改數十，甚至上百個檔案。 以現有的資源及貢獻程度來看，Electron 通常沒辦法跟到最新版的 Chromium，可能會落後數日或數週。

我們認為現在的 Chromium 元件升級機制，已經在有限資源及多數桌面應用程式需求之間取得平衡。 我們真的很想聽到大家又把 Electron 應用在什麼特別的地方。 隨時歡迎各位提出 Pull Request 或是其他貢獻。

## 先不管上面那些建議

由遠端取得程式碼並在本機執行就會有安全議題。 假設在 [`BrowserWindow`](../api/browser-window.md) 中顯示一個遠端網站， 如果攻擊者有辦法改變網站內容 (可能是直接攻擊來源，或是藏在你的應用程式與伺服器中間)，他們就有機會在使用者的機器上執行原生程式。

> :warning: 無論如何，你都不該在啟用 Node.js 整合的情況下，由遠端載入並執行程式碼。 如果需要執行 Node.js 程式碼，請只用本機檔案 (跟你的應用程式打包在一起的那些)。 如果要顯示遠端內容，請用 [`webview`](../api/web-view) 標籤，並確定已經停掉 `nodeIntegration`。

## Electron 安全性警告

由 Electron 2.0 版開始，開發者會在開發主控台中看到警告訊息及建議事項。 只限於二進位檔名是 Electron 時才會顯示這些訊息，因為這代表看主控台的就是開發者。

你也可以在 `process.env` 或 `window` 中設定 `ELECTRON_ENABLE_SECURITY_WARNINGS` 或 `ELECTRON_DISABLE_SECURITY_WARNINGS` 來強制開啟或關閉這些警告訊息。

## 檢查清單: 安全性建議事項

This is not bulletproof, but at the least, you should follow these steps to improve the security of your application.

1) [Only load secure content](#only-load-secure-content) 2) [Disable the Node.js integration in all renderers that display remote content](#disable-node.js-integration-for-remote-content) 3) [Enable context isolation in all renderers that display remote content](#enable-context-isolation-for-remote-content) 4) [Use `ses.setPermissionRequestHandler()` in all sessions that load remote content](#handle-session-permission-requests-from-remote-content) 5) [Do not disable `webSecurity`](#do-not-disable-websecurity) 6) [Define a `Content-Security-Policy`](#define-a-content-security-policy) and use restrictive rules (i.e. `script-src 'self'`) 7) [Override and disable `eval`](#override-and-disable-eval) , which allows strings to be executed as code. 8) [Do not set `allowRunningInsecureContent` to `true`](#do-not-set-allowRunningInsecureContent-to-true) 9) [Do not enable experimental features](#do-not-enable-experimental-features) 10) [Do not use `blinkFeatures`](#do-not-use-blinkfeatures) 11) [WebViews: Do not use `allowpopups`](#do-not-use-allowpopups) 12) [WebViews: Verify the options and params of all `<webview>` tags](#verify-webview-options-before-creation)

## 1) 只載入安全的內容

Any resources not included with your application should be loaded using a secure protocol like `HTTPS`. In other words, do not use insecure protocols like `HTTP`. Similarly, we recommed the use of `WSS` over `WS`, `FTPS` over `FTP`, and so on.

### 為什麼?

`HTTPS` has three main benefits:

1) It authenticates the remote server, ensuring your app connects to the correct host instead of an impersonator. 2) It ensures data integrity, asserting that the data was not modified while in transit between your application and the host. 3) It encrypts the traffic between your user and the destination host, making it more difficult to eavesdrop on the information sent between your app and the host.

### 怎麼做?

```js
// 錯誤示範
browserWindow.loadURL('http://my-website.com')

// 正確寫法
browserWindow.loadURL('https://my-website.com')
```

```html
<!-- 錯誤示範 -->
<script crossorigin src="http://cdn.com/react.js"></script>
<link rel="stylesheet" href="http://cdn.com/style.css">

<!-- 正確寫法 -->
<script crossorigin src="https://cdn.com/react.js"></script>
<link rel="stylesheet" href="https://cdn.com/style.css">
```

## 2) 針對遠端內容停用 Node.js 整合功能

It is paramount that you disable Node.js integration in any renderer ([`BrowserWindow`](../api/browser-window.md), [`BrowserView`](../api/browser-view.md), or [`WebView`](../api/web-view)) that loads remote content. The goal is to limit the powers you grant to remote content, thus making it dramatically more difficult for an attacker to harm your users should they gain the ability to execute JavaScript on your website.

After this, you can grant additional permissions for specific hosts. For example, if you are opening a BrowserWindow pointed at `https://my-website.com/", you can give that website exactly the abilities it needs, but no more.

### 為什麼?

A cross-site-scripting (XSS) attack is more dangerous if an attacker can jump out of the renderer process and execute code on the user's computer. Cross-site-scripting attacks are fairly common - and while an issue, their power is usually limited to messing with the website that they are executed on. Disabling Node.js integration helps prevent an XSS from being escalated into a so-called "Remote Code Execution" (RCE) attack.

### 怎麼做?

```js
// 錯誤示範
const mainWindow = new BrowserWindow()
mainWindow.loadURL('https://my-website.com')
```

```js
// 正確用法
const mainWindow = new BrowserWindow({
  webPreferences: {
    nodeIntegration: false,
    preload: './preload.js'
  }
})

mainWindow.loadURL('https://my-website.com')
```

```html
<!-- 錯誤示範 -->
<webview nodeIntegration src="page.html"></webview>

<!-- 正確寫法 -->
<webview src="page.html"></webview>
```

When disabling Node.js integration, you can still expose APIs to your website that do consume Node.js modules or features. Preload scripts continue to have access to `require` and other Node.js features, allowing developers to expose a custom API to remotely loaded content.

In the following example preload script, the later loaded website will have access to a `window.readConfig()` method, but no Node.js features.

```js
const { readFileSync } = require('fs')

window.readConfig = function () {
  const data = readFileSync('./config.json')
  return data
}
```

## 3) Enable Context Isolation for Remote Content

Context isolation is an Electron feature that allows developers to run code in preload scripts and in Electron APIs in a dedicated JavaScript context. In practice, that means that global objects like `Array.prototype.push` or `JSON.parse` cannot be modified by scripts running in the renderer process.

Electron uses the same technology as Chromium's [Content Scripts](https://developer.chrome.com/extensions/content_scripts#execution-environment) to enable this behavior.

### 為什麼?

Context isolation allows each the scripts on running in the renderer to make changes to its JavaScript environment without worrying about conflicting with the scripts in the Electron API or the preload script.

While still an experimental Electron feature, context isolation adds an additional layer of security. It creates a new JavaScript world for Electron APIs and preload scripts.

At the same time, preload scripts still have access to the `document` and `window` objects. In other words, you're getting a decent return on a likely very small investment.

### 怎麼做?

```js
// 主執行序
const mainWindow = new BrowserWindow({
  webPreferences: {
    contextIsolation: true,
    preload: 'preload.js'
  }
})
```

```js
// Preload script

// Set a variable in the page before it loads
webFrame.executeJavaScript('window.foo = "foo";')

// The loaded page will not be able to access this, it is only available
// in this context
window.bar = 'bar'

document.addEventListener('DOMContentLoaded', () => {
  // Will log out 'undefined' since window.foo is only available in the main
  // context
  console.log(window.foo)

  // Will log out 'bar' since window.bar is available in this context
  console.log(window.bar)
})
```

## 4) Handle Session Permission Requests From Remote Content

You may have seen permission requests while using Chrome: They pop up whenever the website attempts to use a feature that the user has to manually approve ( like notifications).

The API is based on the [Chromium permissions API](https://developer.chrome.com/extensions/permissions) and implements the same types of permissions.

### 為什麼?

By default, Electron will automatically approve all permission requests unless the developer has manually configured a custom handler. While a solid default, security-conscious developers might want to assume the very opposite.

### 怎麼做?

```js
const { session } = require('electron')

session
  .fromPartition('some-partition')
  .setPermissionRequestHandler((webContents, permission, callback) => {
    const url = webContents.getURL()

    if (permission === 'notifications') {
      // 准許權限請求
      callback(true)
    }

    if (!url.startsWith('https://my-website.com')) {
      // 拒絕權限請求
      return callback(false)
    }
  })
```

## 5) 不用停用 WebSecurity

*Recommendation is Electron's default*

You may have already guessed that disabling the `webSecurity` property on a renderer process ([`BrowserWindow`](../api/browser-window.md), [`BrowserView`](../api/browser-view.md), or [`WebView`](../api/web-view)) disables crucial security features.

Do not disable `webSecurity` in production applications.

### 為什麼?

Disabling `webSecurity` will disable the same-origin policy and set `allowRunningInsecureContent` property to `true`. In other words, it allows the execution of insecure code from different domains.

### 怎麼做?

```js
// 錯誤示範
const mainWindow = new BrowserWindow({
  webPreferences: {
    webSecurity: false
  }
})
```

```js
// 正確寫法
const mainWindow = new BrowserWindow()
```

```html
<!-- 錯誤示範 -->
<webview disablewebsecurity src="page.html"></webview>

<!-- 正確寫法 -->
<webview src="page.html"></webview>
```

## 6) 定義內容安全性原則

內容安全性原則 (Content Security Policy; 縮寫 CSP) 是用來防止跨網站指令碼 (Cross-Site Scripting; 縮寫 XSS) 攻擊及資料注入攻擊的機制。 我們建議你在所有會由 Electron 載入的網站都啟用這個機制。

### 為什麼?

CSP allows the server serving content to restrict and control the resources Electron can load for that given web page. `https://your-page.com` should be allowed to load scripts from the origins you defined while scripts from `https://evil.attacker.com` should not be allowed to run. Defining a CSP is an easy way to improve your applications security.

### 怎麼做?

Electron 會遵照 [`Content-Security-Policy` HTTP 標頭](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) 及相應的 `<meta>` 標籤規範。

The following CSP will allow Electron to execute scripts from the current website and from `apis.mydomain.com`.

```txt
// 錯誤示範
Content-Security-Policy: '*'

// 正確寫法
Content-Security-Policy: script-src 'self' https://apis.mydomain.com
```

## 7) 覆寫並停用 `eval`

`eval()` is a core JavaScript method that allows the execution of JavaScript from a string. Disabling it disables your app's ability to evaluate JavaScript that is not known in advance.

### 為什麼?

The `eval()` method has precisely one mission: To evaluate a series of characters as JavaScript and execute it. It is a required method whenever you need to evaluate code that is not known ahead of time. While legitimate use cases exist, just like any other code generators, `eval()` is difficult to harden.

Generally speaking, it is easier to completely disable `eval()` than to make it bulletproof. Thus, if you do not need it, it is a good idea to disable it.

### 怎麼做?

```js
// ESLint will warn about any use of eval(), even this one
// eslint-disable-next-line
window.eval = global.eval = function () {
  throw new Error(`Sorry, this app does not support window.eval().`)
}
```

## 8) 不要將 `allowRunningInsecureContent` 設為 `true`

*Recommendation is Electron's default*

By default, Electron will now allow websites loaded over `HTTPS` to load and execute scripts, CSS, or plugins from insecure sources (`HTTP`). Setting the property `allowRunningInsecureContent` to `true` disables that protection.

Loading the initial HTML of a website over `HTTPS` and attempting to load subsequent resources via `HTTP` is also known as "mixed content".

### 為什麼?

Simply put, loading content over `HTTPS` assures the authenticity and integrity of the loaded resources while encrypting the traffic itself. See the section on [only displaying secure content](#only-display-secure-content) for more details.

### 怎麼做?

```js
// 錯誤示範
const mainWindow = new BrowserWindow({
  webPreferences: {
    allowRunningInsecureContent: true
  }
})
```

```js
// 正確寫法
const mainWindow = new BrowserWindow({})
```

## 9) Do Not Enable Experimental Features

*Recommendation is Electron's default*

Advanced users of Electron can enable experimental Chromium features using the `experimentalFeatures` and `experimentalCanvasFeatures` properties.

### 為什麼?

Experimental features are, as the name suggests, experimental and have not been enabled for all Chromium users. Futhermore, their impact on Electron as a whole has likely not been tested.

Legitimate use cases exist, but unless you know what you are doing, you should not enable this property.

### 怎麼做?

```js
// 錯誤示範
const mainWindow = new BrowserWindow({
  webPreferences: {
    experimentalFeatures: true
  }
})
```

```js
// 正確寫法
const mainWindow = new BrowserWindow({})
```

## 10) 不要用 `blinkFeatures`

*Recommendation is Electron's default*

Blink is the name of the rendering engine behind Chromium. As with `experimentalFeatures`, the `blinkFeatures` property allows developers to enable features that have been disabled by default.

### 為什麼?

Generally speaking, there are likely good reasons if a feature was not enabled by default. Legitimate use cases for enabling specific features exist. As a developer, you should know exactly why you need to enable a feature, what the ramifications are, and how it impacts the security of your application. Under no circumstances should you enable features speculatively.

### 怎麼做?

```js
// 錯誤示範
const mainWindow = new BrowserWindow({
  webPreferences: {
    blinkFeatures: ['ExecCommandInJavaScript']
  }
})
```

```js
// 正確寫法
const mainWindow = new BrowserWindow()
```

## 11) 不要用 `allowpopups`

*Recommendation is Electron's default*

If you are using [`WebViews`](web-view), you might need the pages and scripts loaded in your `<webview>` tag to open new windows. The `allowpopups` attribute enables them to create new [`BrowserWindows`](browser-window) using the `window.open()` method. `WebViews` are otherwise not allowed to create new windows.

### 為什麼?

If you do not need popups, you are better off not allowing the creation of new [`BrowserWindows`](browser-window) by default. This follows the principle of minimally required access: Don't let a website create new popups unless you know it needs that feature.

### 怎麼做?

```html
<!-- 錯誤示範 -->
<webview allowpopups src="page.html"></webview>

<!-- 正確寫法 -->
<webview src="page.html"></webview>
```

## 12) 建立 WebView 前先檢查選項

A WebView created in a renderer process that does not have Node.js integration enabled will not be able to enable integration itself. However, a WebView will always create an independent renderer process with its own `webPreferences`.

It is a good idea to control the creation of new [`WebViews`](web-view) from the main process and to verify that their webPreferences do not disable security features.

### 為什麼?

Since WebViews live in the DOM, they can be created by a script running on your website even if Node.js integration is otherwise disabled.

Electron enables developers to disable various security features that control a renderer process. In most cases, developers do not need to disable any of those features - and you should therefore not allow different configurations for newly created [`<WebView>`](web-view) tags.

### 怎麼做?

Before a [`<WebView>`](web-view) tag is attached, Electron will fire the `will-attach-webview` event on the hosting `webContents`. Use the event to prevent the creation of WebViews with possibly insecure options.

```js
app.on('web-contents-created', (event, contents) => {
  contents.on('will-attach-webview', (event, webPreferences, params) => {
    // 拿掉用不著的預載腳本，或是確認它們的位置是安全正確的
    delete webPreferences.preload
    delete webPreferences.preloadURL

    // 停用 Node.js 整合
    webPreferences.nodeIntegration = false

    // 驗證將要載入的 URL
    if (!params.src.startsWith('https://yourapp.com/')) {
      event.preventDefault()
    }
  })
})
```

再次強調，這份清單只能幫你降低風險，並沒辦法完全將風險排除。如果你的目標只是要顯示網站，那麼瀏覽器會是比較安全的選項。