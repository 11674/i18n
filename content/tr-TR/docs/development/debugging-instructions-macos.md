# macOs'da Hata Ayıklama

Eğer Electron'da JavaScriptten kaynaklanmadığını düşündüğünüz Electronun neden olduğu hatalarla karşılaşırsanız, hata ayıklama biraz can sıkıcı olabilir, özellikle yerel / C ++ için kullanılmayan geliştiriciler için hata ayıklama biraz zor olabilir. Bununla birlikte, Electron'un kaynak kodundaki kesme noktaları ile adım adım hata ayıklamayı etkinleştirmek için lldb ve Electron kaynak kodunu kullanmak oldukça kolaydır.

## Gereksinimler

* **A debug build of Electron**: The easiest way is usually building it yourself, using the tools and prerequisites listed in the [build instructions for macOS](build-instructions-osx.md). Electron hata ayıklamada iken doğrudan indirebilirsiniz ve kolayca ekleyebilirsiniz, büyük çoğunluğunun uygun hale getirildiğini göreceksiniz, hata ayıklama işlemi aslında daha zor: Hata ayıklayıcı size tüm içeriğini gösteremeyecek değişkenleri ve yürütme yolu satırlayıcısı, kuyruk aramaları ve diğerderleyici uygun hale getirilmeleri nedeniyle tuhaf görünebilir.

* **Xcode**: Xcode'a ek olarak, ayrıca Xcode'un komut satırı araçlarını da yükler. Mac OS X'de Xcode'ın varsayılan hata ayıklayıcısı olan LLDB'yi içerirler. C, Objective-C ve C++ masaüstünde ve iOS aygıtlarında ve simülatöründe hata ayıklamayı destekler.

## Electron'da ekleme yapma ve hata ayıklama

To start a debugging session, open up Terminal and start `lldb`, passing a debug build of Electron as a parameter.

```sh
$ lldb ./out/D/Electron.app
(lldb) target create "./out/D/Electron.app"
Current executable set to './out/D/Electron.app' (x86_64).
```

### Kesme noktalarını ayarlama

LLDB güçlü bir araçtır ve kod denetimi için birden fazla stratejiyi desteklemektedir. For this basic introduction, let's assume that you're calling a command from JavaScript that isn't behaving correctly - so you'd like to break on that command's C++ counterpart inside the Electron source.

İlgili kod dosyaları, `./ atom / ` 'da olduğu gibi Brightray'de de bulunabilir `./ brightray / tarayıcı ` ve `./ brightray / common `. Eğer kararlı iseniz, Açıkçası ` chromium_src ` 'da bulunan Chromium'u doğrudan hata ayıklayabilirsiniz.

Let's assume that you want to debug `app.setName()`, which is defined in `browser.cc` as `Browser::SetName()`. Set the breakpoint using the `breakpoint` command, specifying file and line to break on:

```sh
(lldb) breakpoint set --file browser.cc --line 117
Breakpoint 1: where = Electron Framework`atom::Browser::SetName(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&) + 20 at browser.cc:118, address = 0x000000000015fdb4
```

Then, start Electron:

```sh
(lldb) Çalıştır
```

The app will immediately be paused, since Electron sets the app's name on launch:

```sh
(lldb) run
Process 25244 launched: '/Users/fr/Code/electron/out/D/Electron.app/Contents/MacOS/Electron' (x86_64)
Process 25244 stopped
* thread #1: tid = 0x839a4c, 0x0000000100162db4 Electron Framework`atom::Browser::SetName(this=0x0000000108b14f20, name="Electron") + 20 at browser.cc:118, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
    frame #0: 0x0000000100162db4 Electron Framework`atom::Browser::SetName(this=0x0000000108b14f20, name="Electron") + 20 at browser.cc:118
   115  }
   116
   117  void Browser::SetName(const std::string& name) {
-> 118    name_override_ = name;
   119  }
   120
   121  int Browser::GetBadgeCount() {
(lldb)
```

To show the arguments and local variables for the current frame, run `frame variable` (or `fr v`), which will show you that the app is currently setting the name to "Electron".

```sh
(lldb) frame variable
(atom::Browser *) this = 0x0000000108b14f20
(const string &) name = "Electron": {
    [...]
}
```

To do a source level single step in the currently selected thread, execute `step` (or `s`). This would take you into `name_override_.empty()`. To proceed and do a step over, run `next` (or `n`).

```sh
(lldb) step
Process 25244 stopped
* thread #1: tid = 0x839a4c, 0x0000000100162dcc Electron Framework`atom::Browser::SetName(this=0x0000000108b14f20, name="Electron") + 44 at browser.cc:119, queue = 'com.apple.main-thread', stop reason = step in
    frame #0: 0x0000000100162dcc Electron Framework`atom::Browser::SetName(this=0x0000000108b14f20, name="Electron") + 44 at browser.cc:119
   116
   117  void Browser::SetName(const std::string& name) {
   118    name_override_ = name;
-> 119  }
   120
   121  int Browser::GetBadgeCount() {
   122    return badge_count_;
```

To finish debugging at this point, run `process continue`. You can also continue until a certain line is hit in this thread (`thread until 100`). This command will run the thread in the current frame till it reaches line 100 in this frame or stops if it leaves the current frame.

Now, if you open up Electron's developer tools and call `setName`, you will once again hit the breakpoint.

### Daha fazla bilgi

LLDB is a powerful tool with a great documentation. To learn more about it, consider Apple's debugging documentation, for instance the [LLDB Command Structure Reference](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/gdb_to_lldb_transition_guide/document/lldb-basics.html#//apple_ref/doc/uid/TP40012917-CH2-SW2) or the introduction to [Using LLDB as a Standalone Debugger](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/gdb_to_lldb_transition_guide/document/lldb-terminal-workflow-tutorial.html).

You can also check out LLDB's fantastic [manual and tutorial](http://lldb.llvm.org/tutorial.html), which will explain more complex debugging scenarios.