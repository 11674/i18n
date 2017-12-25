# Upgrade Crashpad

1. Dapatkan versi crashpad yang akan kita gunakan.
    
    - `libcc/src/third_party/crashpad/README.chromium` akan memiliki garis `Revisi:` dengan checksum
    - Kita perlu memeriksa cabang yang sesuai.
    - Clone Google's crashpad (https://chromium.googlesource.com/crashpad/crashpad)
    - `git klon https://chromium.googlesource.com/crashpad/crashpad`
    - Periksa cabang dengan checksum revisi: 
        - `git checkout <revision checksum>`
    - Add electron's crashpad fork as a remote
    - `git remote add electron https://github.com/electron/crashpad`
    - Lihat cabang baru untuk pembaruan
    - `git checkout -b elektron-crashpad-vA.B.C.D`
    - `A.B.C.D` adalah versi kromium yang ditemukan di `libcc/VERSION` dan akan menjadi seperti `62.0.3202.94`

2. Buatlah daftar checklist dari Electron patch yang perlu diaplikasikan dengan `git log --oneline`
    
    - Or view http://github.com/electron/crashpad/commits/previous-branch-name

3. Untuk setiap patch:
    
    - In `electron-crashpad-vA.B.C.D`, cherry-pick the patch's checksum 
    - `git cherry-pick <checksum>`
    - Selesaikan konflik
    - Make sure it builds then add, commit, and push work to electron's crashpad fork
    - `git dorong elektron elektron-crashpad-vA.B.C.D`

4. Update Electron untuk membangun tabrakan baru:
    
    - `cd vendor/crashpad`
    - `git fetch`
    - `git checkout electron-crashpad-v62.0.3202.94`
5. Regenerate Ninja files against both targets 
    - From Electron root's root, run `script/update.py`
    - `script/build.py -c D --target=crashpad_client`
    - `script/build.py -c D --target=crashpad_handler`
    - Both should build with no errors
6. Push changes to submodule reference 
    - (From electron root) `git add vendor/crashpad`
    - `git push origin upgrade-to-chromium-62`