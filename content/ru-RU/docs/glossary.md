# Глоссарий

Данная страница посвящена терминам, которые обычно используются при разработке на Electron.

### ASAR

ASAR это Atom Shell Archive Format. [Asar](https://github.com/electron/asar) это простой архив `tar`-подобного формата, который содержит в себе все файлы проекта. Electron может работать с файлами в архиве без распаковки оного.

ASAR был создан для повышения производительности в среде Windows

### Brightray

Brightray [была](https://github.com/electron-archive/brightray) статической библиотекой, созданной для того чтобы [libchromiumcontent](#libchromiumcontent) было проще использовать в приложениях. Сейчас она уже является устаревшей и была объединена с кодовой базой Electron.

### CRT

Библиотека C времени выполнения (CRT) является частью стандартной библиотеки C ++, которая включает стандартную библиотеку ISO C99. Библиотеки Visual C++, которые реализуют CRT поддерживают развитие машинного кода и смешанного собственного и управляемого кода и чисто управляемого кода для разработки .NET.

### DMG

Apple Disk Image (DMG) это пакетный формат, который используется в macOS. DMG файлы обычно используются для распространения приложения «установщика». [electron-builder](https://github.com/electron-userland/electron-builder) поддерживает `dmg` как целевой объект компиляции.

### IME

Input Method Editor. Эта программа позволяет пользователям вводить символы, которые отсутствуют на клавиатуре. Например, это позволяет пользователям Латинской клавиатуры вводить Китайские, Японские, Корейские и Хинди символы.

### IPC

IPC стенды для взаимодействия между процессами. Electron использует IPC для отправки сериализованных сообщений JSON, между [main](#main-process) и [renderer](#renderer-process) процессами.

### libchromiumcontent

Общая библиотека, которая включает в себя [Chromium Content module](https://www.chromium.org/developers/content-module) и все его зависимости. (Например, Blink, [V8](#v8) и т.д.). Также именуется "libcc".

- [github.com/electron/libchromiumcontent](https://github.com/electron/libchromiumcontent)

### основной (main) процесс

Основной процесс, обычно файл с именем `main.js`, является точкой входа для каждого приложения Electron. Он контролирует жизнь приложения, от его открытия до закрытия. Также управляет нативными элементами, такими как Menu, Menu Bar, Dock, Tray, и др. Основной процесс отвечает за создание каждого нового процесса в приложение. Полностью встроено Node API.

Каждый файл основного процесса приложения указывается в свойство `main` в `package.json`. Так `electron .` узнает какой файл выполнять при запуске.

В хром этот процесс называется «процесс browser». Он переименовывается в Electron, чтобы избежать путаницы с renderer процессами.

См. также: [процесс](#process), [renderer процесс](#renderer-process)

### MAS

Акроним к Mac App Store компании Apple. Для более подробных сведений см. [Mac App Store Submission Guide](tutorial/mac-app-store-submission-guide.md).

### нативные модули

Нативными модулями (также называемые [addons](https://nodejs.org/api/addons.html) в Node.js) являются модули, написанные на C или C++, которые могут быть загружены в Node.js или Electron с помощью функции require() и используются, как обычные модули Node.js. Они используются главным образом для предоставления интерфейса между скриптами JavaScript, выполняющихся в Node.js и C/C++ библиотеках.

Нативные модуля Node поддерживаются в Electron, но учитывая, что Electron предпочитает использовать разные версии V8 для Node установленного на Вашем компьютере, вы должны вручную указать расположение заголовков Electron'а, когда собираете нативные модули.

См. также [Использование нативных модулей Node](tutorial/using-native-node-modules.md).

### NSIS

Nullsoft Scriptable Install System это управляемое скриптом средство установки для Microsoft Windows. Оно выпускается под сочетанием различных лицензий на свободное программное обеспечение и является широко используемой альтернативой коммерческим проприетарным продуктам, таким как например InstallShield. [electron-builder](https://github.com/electron-userland/electron-builder) поддерживает NSIS как целевой объект компиляции.

### OSR

OSR (закадровый рендеринг) может использоваться для загрузки тяжелых страниц в фоновом режиме и затем отобразит её после (это будет намного быстрее). Это позволяет Вам рендерить страницу без отображения на экране.

### процесс

Процесс является экземпляром компьютерной программы, который выполняется. Electron приложения используют [main](#main-process) процесс и один или более [renderer](#renderer-process) процессы, на самом деле работает несколько программ одновременно.

В Node.js и Electron, каждый запущенный процесс имеет объект `process`. Этот объект является глобальным, предоставляющий информацию о текущем процессе и контроль над ним. Он всегда доступен глобально в приложение без использования require().

См. также: [основной (main) процесс](#main-process), [renderer процесс](#renderer-process)

### renderer процесс

Renderer процесс является окном браузера в Вашем приложении. В отличие основного процесса, их может быть несколько и каждый запускается в отдельном процессе. Они также могут быть скрыты.

В нормальных браузерах, веб-страницы обычно выполняются в изолированной среде и им не разрешается доступ к нативным ресурсам. Electron пользователи, однако, имеют право использовать API Node.js на веб-страницах, позволяя взаимодействовать на нижнем уровне операционной системы.

См. также: [процесс](#process), [основной (main) процесс](#main-process)

### Squirrel

Squirrel является фреймворком с открытым исходным кодом, который позволяет Electron приложениям автоматически обновляться, когда выпустится новая версия. См. [autoUpdated](api/auto-updater.md) API для информации о начале работы с Squirrel.

### пользовательское пространство

This term originated in the Unix community, where "userland" or "userspace" referred to programs that run outside of the operating system kernel. More recently, the term has been popularized in the Node and npm community to distinguish between the features available in "Node core" versus packages published to the npm registry by the much larger "user" community.

Like Node, Electron is focused on having a small set of APIs that provide all the necessary primitives for developing multi-platform desktop applications. This design philosophy allows Electron to remain a flexible tool without being overly prescriptive about how it should be used. Userland enables users to create and share tools that provide additional functionality on top of what is available in "core".

### V8

V8 - JavaScript движок с открытым исходным кодом компании Google. Он написан на C++ и используется в Google Chrome. V8 может быть запущен автономно или может быть встроен в любое C++ приложение.

Electron компилирует V8 как часть Chromium а затем указывает Node использовать этот V8 при сборке.

V8's version numbers always correspond to those of Google Chrome. Chrome 59 includes V8 5.9, Chrome 58 includes V8 5.8, etc.

- [developers.google.com/v8](https://developers.google.com/v8)
- [nodejs.org/api/v8.html](https://nodejs.org/api/v8.html)
- [docs/development/v8-development.md](development/v8-development.md)

### webview

`webview` tags are used to embed 'guest' content (such as external web pages) in your Electron app. They are similar to `iframe`s, but differ in that each webview runs in a separate process. It doesn't have the same permissions as your web page and all interactions between your app and embedded content will be asynchronous. This keeps your app safe from the embedded content.