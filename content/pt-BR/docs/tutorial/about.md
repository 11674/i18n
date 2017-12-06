# Sobre Electron

[Electron](https://electronjs.org) is an open source library developed by GitHub for building cross-platform desktop applications with HTML, CSS, and JavaScript. Electron foi feito com uma combinação entre [Chromium](https://www.chromium.org/Home) e [Node.js](https://nodejs.org) em uma única runtime e os applicativos podem ser empacotados para Mac, Windows e Linux.

Electron começou em 2013, como uma framewok que seria utilizado no [Atom](https://atom.io), editor de texto hackable do GitHub, que seria construído. Os dois foram disponibilizados na primavera de 2014, com seu código fonte aberto.

It has since become a popular tool used by open source developers, startups, and established companies. [See who is building on Electron](https://electronjs.org/apps).

Read on to learn more about the contributors and releases of Electron or get started building with Electron in the [Quick Start Guide](quick-start.md).

## Equipes e Contribuidores

Electron é mantido por uma equipe do GitHub, bem como um grupo de [contribuidores ativos](https://github.com/electron/electron/graphs/contributors) da comunidade. Alguns dos contribuidores são indivíduos, e trabalham nas maiores empresas, que estão fazendo o uso do Electron. Estamos felizes em adicionar contribuidores frequentes ao projeto como mantedores. Leia mais sobre isso em como [contribuir para o Electron](https://github.com/electron/electron/blob/master/CONTRIBUTING.md).

## Versões

Frequentemente é liberado nova [versões do Electron](https://github.com/electron/electron/releases). Nós liberamos quando existem importantes correções de bugs, novas APIs ou são versões de atualização do Chromium ou Node.js.

### Atualizando as Dependências

Electron's version of Chromium is usually updated within one or two weeks after a new stable Chromium version is released, depending on the effort involved in the upgrade.

When a new version of Node.js is released, Electron usually waits about a month before upgrading in order to bring in a more stable version.

In Electron, Node.js and Chromium share a single V8 instance—usually the version that Chromium is using. Most of the time this *just works* but sometimes it means patching Node.js.

### Controle de Versão

As of version 2.0 Electron [follows `semver`](http://semver.org). For most applications, and using any recent version of npm, running `$ npm install electron` will do the right thing.

The version update process is detailed explicitly in our [Versioning Doc](versioning.md).

### LTS

Long term support of older versions of Electron does not currently exist. If your current version of Electron works for you, you can stay on it for as long as you'd like. If you want to make use of new features as they come in you should upgrade to a newer version.

A major update came with version `v1.0.0`. If you're not yet using this version, you should [read more about the `v1.0.0` changes](https://electronjs.org/blog/electron-1-0).

## Core Philosophy

In order to keep Electron small (file size) and sustainable (the spread of dependencies and APIs) the project limits the scope of the core project.

For instance, Electron uses just the rendering library from Chromium rather than all of Chromium. This makes it easier to upgrade Chromium but also means some browser features found in Google Chrome do not exist in Electron.

New features added to Electron should primarily be native APIs. If a feature can be its own Node.js module, it probably should be. See the [Electron tools built by the community](https://electronjs.org/community).

## History

Below are milestones in Electron's history.

| :calendar:         | :tada:                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------- |
| **Abril de 2013**  | [Atom Shell é iniciado](https://github.com/electron/electron/commit/6ef8875b1e93787fa9759f602e7880f28e8e6b45).      |
| **Maio de 2014**   | [Atom Shell tem código fonte aberto](http://blog.atom.io/2014/05/06/atom-is-now-open-source.html).                  |
| **Abril 2015**     | [Atom Shell é renomeado para Electron](https://github.com/electron/electron/pull/1389).                             |
| **Maio de 2016**   | [Electron releases `v1.0.0`](https://electronjs.org/blog/electron-1-0).                                             |
| **Maio de 2016**   | [Electron apps compatible with Mac App Store](https://electronjs.org/docs/tutorial/mac-app-store-submission-guide). |
| **Agosto de 2016** | [Windows Store support for Electron apps](https://electronjs.org/docs/tutorial/windows-store-guide).                |