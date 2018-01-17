# Ang Teknikal na Pagkakaiba ng Elektron at NW.js (dating node-webkit)

**Paalala: Atom Shell ang dating tawag sa Electron.**

Katulad ng NW.js, ang Elektron ay nagbibigay disenyo upang makasulat sa aplikasyon ng desktop kasama ang Javascript at HTML na may pinagsama-samang Node na syang magiging daan upang makapasok sa mababang lebel ng sistema galing sa pahina ng web.

Ngunit mayroon ding mahalagang pagkakaiba ang dalawang proyekto kung saan ang Elektron ay tuluyang hihiwalay sa produkto na galing sa NW.js:

**1. Pagpasok sa Aplikasyon**

Sa NW.js, ang pangunahing pasukan ng aplikasyon ay ang pahina ng web o ng Javascript. You specify a html or js file in the `package.json` and it is opened in a browser window as the application's main window (in case of an html entrypoint) or the script is executed.

In Electron, the entry point is a JavaScript script. Instead of providing a URL directly, you manually create a browser window and load an HTML file using the API. You also need to listen to window events to decide when to quit the application.

Electron works more like the Node.js runtime. Electron's APIs are lower level so you can use it for browser testing in place of [PhantomJS](http://phantomjs.org/).

**2. Build System**

In order to avoid the complexity of building all of Chromium, Electron uses [`libchromiumcontent`](https://github.com/electron/libchromiumcontent) to access Chromium's Content API. `libchromiumcontent` is a single shared library that includes the Chromium Content module and all of its dependencies. Users don't need a powerful machine to build Electron.

**3. Node Integration**

In NW.js, the Node integration in web pages requires patching Chromium to work, while in Electron we chose a different way to integrate the libuv loop with each platform's message loop to avoid hacking Chromium. See the [`node_bindings`](https://github.com/electron/electron/tree/master/atom/common) code for how that was done.

**4. Multi-context**

If you are an experienced NW.js user, you should be familiar with the concept of Node context and web context. These concepts were invented because of how NW.js was implemented.

By using the [multi-context](https://github.com/nodejs/node-v0.x-archive/commit/756b622) feature of Node, Electron doesn't introduce a new JavaScript context in web pages.

Note: NW.js has optionally supported multi-context since 0.13.