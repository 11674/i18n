자신의 Electron 버전과 일치하는 문서를 사용했는지 확인하세요. 버전 번호는 페이지 URL 의 일부여야 합니다. 그렇지 않은 경우, Electron 버전과 호환되지 않는 API 변경 사항이 포함될 수 있는 개발 브랜치의 문서를 사용하고 있을 수 있습니다. 문서의 이전 버전을 보려면, GitHub 에서 "Switch branches/tags" 드롭다운을 열고 버전과 일치하는 태그를 선택하여 [태그로 찾아](https://github.com/electron/electron/tree/v1.4.0) 볼 수 있습니다.

## 자주 묻는 질문

꽤 자주 묻는 질문이 있습니다. 이슈를 생성하기 전에 다음을 확인하세요:

* [Electron 자주 묻는 질문](faq.md)

## Guides and Tutorials

* [Setting up the Development Environment](tutorial/development-environment.md) 
  * [Setting up macOS](tutorial/development-environment.md#setting-up-macos)
  * [Setting up Windows](tutorial/development-environment.md#setting-up-windows)
  * [Setting up Linux](tutorial/development-environment.md#setting-up-linux)
  * [Choosing an Editor](tutorial/development-environment.md#a-good-editor)
* [Creating your First App](tutorial/first-app.md) 
  * [Installing Electron](tutorial/first-app.md#installing-electron)
  * [Electron Development in a Nutshell](tutorial/first-app.md#electron-development-in-a-nutshell)
  * [Running Your App](tutorial/first-app.md#running-your-app)
* [Boilerplates and CLIs](tutorial/boilerplates-and-clis.md) 
  * [Boilerplate vs CLI](tutorial/boilerplates-and-clis.md#boilerplate-vs-cli)
  * [electron-forge](tutorial/boilerplates-and-clis.md#electron-forge)
  * [electron-builder](tutorial/boilerplates-and-clis.md#electron-builder)
  * [electron-react-boilerplate](tutorial/boilerplates-and-clis.md#electron-react-boilerplate)
  * [Other Tools and Boilerplates](tutorial/boilerplates-and-clis.md#other-tools-and-boilerplates)
* [Application Architecture](tutorial/application-architecture.md) 
  * [Main and Renderer Processes](tutorial/application-architecture.md#main-and-renderer-processes)
  * [Using Electron's APIs](tutorial/application-architecture.md#using-electron-apis)
  * [Using Node.js APIs](tutorial/application-architecture.md#using-node.js-apis)
  * [Using Native Node.js Modules](tutorial/using-native-node-modules.md)
  * [Inter-Process Communication](tutorial/application-architecture.md#)
* Adding Features to Your App 
  * [알림](tutorial/notifications.md)
  * [Recent Documents](tutorial/desktop-environment-integration.md#recent-documents-windows-mac-os)
  * [Application Progress](tutorial/progress-bar.md)
  * [Custom Dock Menu](tutorial/desktop-environment-integration.md#custom-dock-menu-mac-os)
  * [Custom Windows Taskbar](tutorial/windows-taskbar.md)
  * [Custom Linux Desktop Actions](tutorial/linux-desktop-actions.md)
  * [키보드 단축기](tutorial/keyboard-shortcuts.md)
  * [Offline/Online Detection](tutorial/online-offline-events.md)
  * [Represented File for macOS BrowserWindows](tutorial/represented-file.md)
  * [Native File Drag & Drop](tutorial/native-file-drag-drop.md)
* [Application Accessibility](tutorial/accessibility.md) 
  * [Spectron](tutorial/accessibility.md#spectron)
  * [Devtron](tutorial/accessibility.md#devtron)
  * [접근성 활성화](tutorial/accessibility.md#enabling-accessibility)
* [Application Testing and Debugging](tutorial/application-debugging.md) 
  * [메인 프로세스 디버깅하기](tutorial/debugging-main-process.md)
  * [Selenium 과 WebDriver 사용하기](tutorial/using-selenium-and-webdriver.md)
  * [헤드리스 CI 시스템 (트래비스, 젠킨스) 테스트](tutorial/testing-on-headless-ci.md)
  * [DevTools 확장](tutorial/devtools-extension.md)
* [응용 프로그램 배포](tutorial/application-distribution.md) 
  * [지원되는 플랫폼](tutorial/supported-platforms.md)
  * [Mac App Store](tutorial/mac-app-store-submission-guide.md)
  * [Windows Store](tutorial/windows-store-guide.md)
  * [Snapcraft](tutorial/snapcraft.md)
* [Application Security](tutorial/security.md) 
  * [보안 문제 제보](tutorial/security.md#reporting-security-issues)
  * [Chromium 보안 문제와 업그레이드](tutorial/security.md#chromium-security-issues-and-upgrades)
  * [Electron 보안 경고](tutorial/security.md#electron-security-warnings)
  * [Security Checklist](tutorial/security.md#checklist-security-recommendations)
* [Application Updates](tutorial/updates.md) 
  * [Deploying an Update Server](tutorial/updates.md#deploying-an-update-server)
  * [Implementing Updates in Your App](tutorial/updates.md#implementing-updates-in-your-app)
  * [Applying Updates](tutorial/updates.md#applying-updates)

## Detailed Tutorials

These individual tutorials expand on topics discussed in the guide above.

* [In Detail: Installing Electron](tutorial/installation.md) 
  * [Global versus Local Installation](tutorial/installation.md#global-versus-local-installation)
  * [Proxies](tutorial/installation.md#proxies)
  * [Custom Mirrors and Caches](tutorial/installation.md#custom-mirrors-and-caches)
  * [문제 해결](tutorial/installation.md#troubleshooting)
* [In Detail: Electron's Versioning Scheme](tutorial/electron-versioning.md) 
  * [semver](tutorial/electron-versioning.md#semver)
  * [안정화 브랜치](tutorial/electron-versioning.md#stabilization-branches)
  * [베타 출시와 버그 수정](tutorial/electron-versioning.md#beta-releases-and-bug-fixes)
* [In Detail: Packaging App Source Code with asar](tutorial/application-packaging.md) 
  * [Generating asar Archives](tutorial/application-packaging.md#generating-asar-archives)
  * [asar 아카이브 사용하기](tutorial/application-packaging.md#using-asar-archives)
  * [Limitations](tutorial/application-packaging.md#limitations-of-the-node-api)
  * [Adding Unpacked Files to asar Archives](tutorial/application-packaging.md#adding-unpacked-files-to-asar-archives)
* [In Detail: Using Pepper Flash Plugin](tutorial/using-pepper-flash-plugin.md)
* [In Detail: Using Widevine CDM Plugin](tutorial/using-widevine-cdm-plugin.md)
* [오프 스크린 렌더링](tutorial/offscreen-rendering.md)

* * *

* [용어집](glossary.md)

## API 참조 문서

* [개요](api/synopsis.md)
* [프로세스 개체](api/process.md)
* [크롬 명령 줄 스위치 지원](api/chrome-command-line-switches.md)
* [환경 변수](api/environment-variables.md)

### 사용자 지정 DOM 요소:

* [`File` 객체](api/file-object.md)
* [`<webview>` 태그](api/webview-tag.md)
* [`window.open` 함수](api/window-open.md)

### 주요 프로세스 모듈:

* [app](api/app.md)
* [autoUpdater](api/auto-updater.md)
* [BrowserView](api/browser-view.md)
* [BrowserWindow](api/browser-window.md)
* [contentTracing](api/content-tracing.md)
* [dialog](api/dialog.md)
* [globalShortcut](api/global-shortcut.md)
* [inAppPurchase](api/in-app-purchase.md)
* [ipcMain](api/ipc-main.md)
* [Menu](api/menu.md)
* [MenuItem](api/menu-item.md)
* [net](api/net.md)
* [powerMonitor](api/power-monitor.md)
* [powerSaveBlocker](api/power-save-blocker.md)
* [protocol](api/protocol.md)
* [session](api/session.md)
* [systemPreferences](api/system-preferences.md)
* [Tray](api/tray.md)
* [webContents](api/web-contents.md)

### 렌더러 프로세스 (웹 페이지) 에 대한 모듈:

* [desktopCapturer](api/desktop-capturer.md)
* [ipcRenderer](api/ipc-renderer.md)
* [remote](api/remote.md)
* [webFrame](api/web-frame.md)

### 두 프로세스에 대한 모듈:

* [clipboard](api/clipboard.md)
* [crashReporter](api/crash-reporter.md)
* [nativeImage](api/native-image.md)
* [screen](api/screen.md)
* [shell](api/shell.md)

## 개발

* [코딩 스타일](development/coding-style.md)
* [C++ 코드에서 clang 형식 사용하기](development/clang-format.md)
* [Testing](development/testing.md)
* [소스 코드 디렉토리 구조](development/source-code-directory-structure.md)
* [NW.js (node-webkit 이었던) 와 기술 차이](development/atom-shell-vs-node-webkit.md)
* [빌드 시스템 개요](development/build-system-overview.md)
* [빌드 명령 (macOS)](development/build-instructions-osx.md)
* [빌드 명령 (윈도)](development/build-instructions-windows.md)
* [빌드 명령 (Linux)](development/build-instructions-linux.md)
* [디버그 명령 (macOS)](development/debugging-instructions-macos.md)
* [디버그 명령 (Windows)](development/debug-instructions-windows.md)
* [디버거에서 기호 서버 설정 하기](development/setting-up-symbol-server.md)
* [문서 스타일 안내](styleguide.md)
* [Contributing to Electron](../CONTRIBUTING.md)
* [Issues](development/issues.md)
* [Pull Requests](development/pull-requests.md)
* [Upgrading Chromium](development/upgrading-chromium.md)
* [크롬 개발](development/chromium-development.md)
* [V8 개발](development/v8-development.md)