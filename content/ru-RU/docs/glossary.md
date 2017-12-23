# Глоссарий

Данная страница посвящена терминам, которые обычно используются при разработке на Electron.

### ASAR

ASAR это Atom Shell Archive Format. [Asar](https://github.com/electron/asar) это простой архив `tar`-like формата, который содержит в себе все файлы проекта. Electron может работать с файлами в архиве без распаковки оного.

ASAR был создан для повышения производительности в среде Windows

### Brightray

Brightray [была](https://github.com/electron-archive/brightray) статической библиотекой, созданной для того чтобы [libchromiumcontent](#libchromiumcontent) было проще использовать в приложениях. Сейчас она уже является устаревшей и была объединена с кодовой базой Electron.

### CRT

Библиотека C времени выполнения (CRT) является частью стандартной библиотеки C ++, которая включает стандартную библиотеку ISO C99. Библиотеки Visual C++, которые реализуют CRT поддерживают развитие машинного кода и смешанного собственного и управляемого кода и чисто управляемого кода для разработки .NET.

### DMG

Apple Disk Image (DMG) это пакетный формат, который используется в macOS. DMG файлы обычно используются для распространения приложения «установщика». [electron-builder](https://github.com/electron-userland/electron-builder) поддерживает `dmg` как целевой объект компиляции.

### IME

Input Method Editor. A program that allows users to enter characters and symbols not found on their keyboard. Например это позволяет пользователям Латинской клавиатуры для ввода символов китайского, японского, корейского и хинди.

### IPC

IPC стенды для взаимодействия между процессами. Electron использует IPC для отправки сериализованных сообщений JSON, между [основными](#main-process) и [визуализационными](#renderer-process) процессами.

### libchromiumcontent

Общая библиотека, которая включает в себя [Chromium Content module](https://www.chromium.org/developers/content-module) и все его зависимости. (Например, Blink, [V8](#v8) и т.д.). Также именуется "libcc".

- [github.com/electron/libchromiumcontent](https://github.com/electron/libchromiumcontent)

### Главный (Main) процесс

Основной процесс, обычно в файл с именем `main.js`, является точкой входа для каждого приложения Electron. Он контролирует жизнь приложения, от его открытия до закрытия. It also manages native elements such as the Menu, Menu Bar, Dock, Tray, etc. The main process is responsible for creating each new renderer process in the app. The full Node API is built in.

Every app's main process file is specified in the `main` property in `package.json`. This is how `electron .` knows what file to execute at startup.

В хром этот процесс называется «процесс браузера». Он переименовывается в электрон, чтобы избежать путаницы с визуализации процессов.

См. также: [process](#process), [renderer process](#renderer-process)

### MAS

Акроним к Apple's Mac App Store. Для более подробных сведений см. [Mac App Store Submission Guide](tutorial/mac-app-store-submission-guide.md).

### собственные модули

Собственные модули (также называемые [addons](https://nodejs.org/api/addons.html) в Node.js) являются модули, написанные на C или C++, которые могут быть загружены в Node.js или Electron с помощью функции require() и используется, как обычные модули Node.js. Они используются главным образом для предоставления интерфейса между скриптов JavaScript, выполняющийся в Node.js и C/C++ библиотеках.

Native Node modules are supported by Electron, but since Electron is very likely to use a different V8 version from the Node binary installed in your system, you have to manually specify the location of Electron’s headers when building native modules.

См. также [Using Native Node Modules](tutorial/using-native-node-modules.md).

### NSIS

Nullsoft Scriptable Install System is a script-driven Installer authoring tool for Microsoft Windows. It is released under a combination of free software licenses, and is a widely-used alternative to commercial proprietary products like InstallShield. [electron-builder](https://github.com/electron-userland/electron-builder) supports NSIS as a build target.

### OSR

OSR (Off-screen rendering) can be used for loading heavy page in background and then displaying it after (it will be much faster). It allows you to render page without showing it on screen.

### процесс

A process is an instance of a computer program that is being executed. Electron apps that make use of the [main](#main-process) and one or many [renderer](#renderer-process) process are actually running several programs simultaneously.

In Node.js and Electron, each running process has a `process` object. This object is a global that provides information about, and control over, the current process. As a global, it is always available to applications without using require().

See also: [main process](#main-process), [renderer process](#renderer-process)

### renderer process

The renderer process is a browser window in your app. Unlike the main process, there can be multiple of these and each is run in a separate process. They can also be hidden.

В нормальных браузерах, веб-страницы обычно выполняются в изолированной среде и им не разрешается доступ к нативным ресурсам. Electron пользователей, однако, имеют право использовать API Node.js на веб-страницах, позволяя взаимодействия нижнего уровня операционной системы.

See also: [process](#process), [main process](#main-process)

### Squirrel

Squirrel is an open-source framework that enables Electron apps to update automatically as new versions are released. See the [autoUpdater](api/auto-updater.md) API for info about getting started with Squirrel.

### пользовательское пространство

This term originated in the Unix community, where "userland" or "userspace" referred to programs that run outside of the operating system kernel. More recently, the term has been popularized in the Node and npm community to distinguish between the features available in "Node core" versus packages published to the npm registry by the much larger "user" community.

Like Node, Electron is focused on having a small set of APIs that provide all the necessary primitives for developing multi-platform desktop applications. This design philosophy allows Electron to remain a flexible tool without being overly prescriptive about how it should be used. Userland enables users to create and share tools that provide additional functionality on top of what is available in "core".

### V8

V8 is Google's open source JavaScript engine. It is written in C++ and is used in Google Chrome. V8 can run standalone, or can be embedded into any C++ application.

Electron builds V8 as part of Chromium and then points Node to that V8 when building it.

V8's version numbers always correspond to those of Google Chrome. Chrome 59 includes V8 5.9, Chrome 58 includes V8 5.8, etc.

- [developers.google.com/v8](https://developers.google.com/v8)
- [nodejs.org/api/v8.html](https://nodejs.org/api/v8.html)
- [docs/development/v8-development.md](development/v8-development.md)

### webview

`webview` tags are used to embed 'guest' content (such as external web pages) in your Electron app. They are similar to `iframe`s, but differ in that each webview runs in a separate process. It doesn't have the same permissions as your web page and all interactions between your app and embedded content will be asynchronous. This keeps your app safe from the embedded content.