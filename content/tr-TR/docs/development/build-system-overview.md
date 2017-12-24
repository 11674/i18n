# Sistem Genel Bakışı Oluşturma

Electron proje üretimi için [gyp](https://gyp.gsrc.io/) ve inşa etmek için [ninja](https://ninja-build.org/) kullanır. Proje yapılandırmaları `.gyp` ve `.gypi` dosyaları içinde bulunabilir.

## Gyp Dosyaları

Aşağıdaki `gyp` dosyaları Electron oluşturmanın ana kurallarını içerir:

* `electron.gyp` Electron 'un kendisinin nasıl inşa edildiğini tanımlar.
* `common.gypi` Node yapılandırmalarının oluşumunu Chromium ile birlikte oluşmasını ayarlar.
* `brightray/brightray.gyp` `brightray` 'nin nasıl oluştuğunu ve Chromium ile bağlanmak için varsayılan yapılandırmaları nasıl içerdiğini tanımlar.
* `brightray/brightray.gypi` inşa etmekle ilgili genel yapı konfigürasyonlarını içerir.

## Öğe Oluşturma

Chromium oldukça büyük bir proje olduğu için son bağlama aşaması ilerlemeyi zorlaştıran bir kaç dakika alabilir. In order to solve this, Chromium introduced the "component build", which builds each component as a separate shared library, making linking very quick but sacrificing file size and performance.

In Electron we took a very similar approach: for `Debug` builds, the binary will be linked to a shared library version of Chromium's components to achieve fast linking time; for `Release` builds, the binary will be linked to the static library versions, so we can have the best possible binary size and performance.

## Minimal Bootstrapping

All of Chromium's prebuilt binaries (`libchromiumcontent`) are downloaded when running the bootstrap script. By default both static libraries and shared libraries will be downloaded and the final size should be between 800MB and 2GB depending on the platform.

By default, `libchromiumcontent` is downloaded from Amazon Web Services. If the `LIBCHROMIUMCONTENT_MIRROR` environment variable is set, the bootstrap script will download from it. [`libchromiumcontent-qiniu-mirror`](https://github.com/hokein/libchromiumcontent-qiniu-mirror) is a mirror for `libchromiumcontent`. If you have trouble in accessing AWS, you can switch the download address to it via `export LIBCHROMIUMCONTENT_MIRROR=http://7xk3d2.dl1.z0.glb.clouddn.com/`

If you only want to build Electron quickly for testing or development, you can download just the shared library versions by passing the `--dev` parameter:

```sh
$ ./script/bootstrap.py --dev
$ ./script/build.py -c D
```

## Two-Phase Project Generation

Electron links with different sets of libraries in `Release` and `Debug` builds. `gyp`, however, doesn't support configuring different link settings for different configurations.

To work around this Electron uses a `gyp` variable `libchromiumcontent_component` to control which link settings to use and only generates one target when running `gyp`.

## Target Names

Unlike most projects that use `Release` and `Debug` as target names, Electron uses `R` and `D` instead. This is because `gyp` randomly crashes if there is only one `Release` or `Debug` build configuration defined, and Electron only has to generate one target at a time as stated above.

This only affects developers, if you are just building Electron for rebranding you are not affected.

## Testler

Test your changes conform to the project coding style using:

```sh
$ npm run lint
```

Test functionality using:

```sh
$ npm test
```

Whenever you make changes to Electron source code, you'll need to re-run the build before the tests:

```sh
$ npm run build && npm test
```

You can make the test suite run faster by isolating the specific test or block you're currently working on using Mocha's [exclusive tests](https://mochajs.org/#exclusive-tests) feature. Just append `.only` to any `describe` or `it` function call:

```js
describe.only('some feature', function () {
  // ... only tests in this block will be run
})
```

Alternatively, you can use mocha's `grep` option to only run tests matching the given regular expression pattern:

```sh
$ npm test -- --grep child_process
```

Tests that include native modules (e.g. `runas`) can't be executed with the debug build (see [#2558](https://github.com/electron/electron/issues/2558) for details), but they will work with the release build.

To run the tests with the release build use:

```sh
$ npm test -- -R
```