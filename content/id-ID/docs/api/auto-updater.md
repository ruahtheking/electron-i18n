# autoUpdater

> Aktifkan aplikasi untuk memperbarui dirinya secara otomatis.

Proses: [Utama](../glossary.md#main-process)

Modul `autoUpdater` menyediakan sebuah antarmuka untuk [kerangka Squirrel](https://github.com/Squirrel).

Anda dapat dengan cepat meluncurkan server pelepasan multi platform untuk mendistribusikannya aplikasi dengan menggunakan salah satu proyek ini:

* [kacang](https://github.com/GitbookIO/nuts): * Server pelepasan cerdas untuk aplikasi Anda, menggunakan GitHub sebagai backend. Pembaruan otomatis dengan Squirrel (Mac & Windows)*
* [elektron-release-server](https://github.com/ArekSredzki/electron-release-server): * Fitur lengkap, host rilis self-host untuk aplikasi elektron, kompatibel dengan auto-updater*
* [squirrel-updates-server](https://github.com/Aluxian/squirrel-updates-server): *Server node.js sederhana untuk Squirrel.Mac dan Squirrel.Windows yang menggunakan rilis GitHub*
* [squirrel-release-server](https://github.com/Arcath/squirrel-release-server): *Aplikasi PHP sederhana untuk Squirrel.Windows yang membaca update dari sebuah folder. Mendukung pembaruan delta.*

## Pemberitahuan platform

Meskipun `autoUpdater` menyediakan API seragam untuk berbagai platform, ada Masih ada beberapa perbedaan halus pada setiap platform.

### macOS

Di macOS, modul `autoUpdater` dibangun di atas [Squirrel.Mac](https://github.com/Squirrel/Squirrel.Mac), artinya Anda tidak memerlukan pengaturan khusus untuk membuatnya bekerja. Untuk server-side persyaratan, Anda dapat membaca [Server Support](https://github.com/Squirrel/Squirrel.Mac#server-support). Perhatikan bahwa [App Keamanan Transportasi](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW35) (ATS) berlaku untuk semua permintaan yang dilakukan sebagai bagian dari proses update. Aplikasi yang perlu di disable ATS bisa menambahkan `NSAllowsArbitraryLoads` kunci ke plist aplikasi mereka.

**Catatan:** Aplikasi Anda harus ditandatangani untuk update otomatis pada macOS. Ini adalah persyaratan `Squirrel.Mac`.

### Windows

Pada Windows, Anda harus menginstal aplikasi Anda ke mesin pengguna sebelum Anda bisa gunakan `autoUpdater`, jadi sebaiknya gunakan [paket pemecah elektron / winstaller](https://github.com/electron/windows-installer),  atau [grunt-electron-installer](https://github.com/electron/grunt-electron-installer) untuk menghasilkan pemasang Windows.</p> 

Bila menggunakan [electron-winstaller](https://github.com/electron/windows-installer) atau [electron-forge](https://github.com/electron-userland/electron-forge) pastikan Anda tidak mencoba memperbarui aplikasi Anda [saat pertama kali berjalan](https://github.com/electron/windows-installer#handling-squirrel-events) (Juga lihat [masalah ini untuk info lebih lanjut](https://github.com/electron/electron/issues/7155)). Sebaiknya gunakan [elektron-squirrel-startup](https://github.com/mongodb-js/electron-squirrel-startup) untuk mendapatkan pintasan desktop untuk aplikasi Anda.

Installer yang dihasilkan dengan Squirrel akan membuat shortcut icon dengan [ID Model Aplikasi Pengguna](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378459(v=vs.85).aspx) dalam format `com.squirrel.PACKAGE_ID.YOUR_EXE_WITHOUT_DOT_EXE`, contohnya adalah `com.squirrel.slack.Slack` dan `com.squirrel.code.Code`. Anda harus menggunakan ID yang sama untuk aplikasi Anda dengan API `app.setAppUserModelId`, jika tidak Windows akan melakukannya tidak bisa pin aplikasi Anda dengan benar di task bar.

Tidak seperti Squirrel.Mac, Windows dapat menginangi update pada S3 atau host file statis lainnya. Anda bisa membaca dokumen [Squirrel.Windows](https://github.com/Squirrel/Squirrel.Windows) untuk mendapatkan rincian lebih lanjut tentang bagaimana Squirrel.Windows bekerja.

### Linux

Tidak ada dukungan built-in untuk auto-updater di Linux, jadi dianjurkan untuk melakukannya gunakan manajer paket distribusi untuk memperbarui aplikasi Anda.

## Acara

Objek `autoUpdater` memancarkan peristiwa berikut:

### Acara: 'kesalahan'

Pengembalian:

* Kesalahan `kesalahan`

Emitted saat ada error saat mengupdate.

### Acara: 'check-for-update'

Emitted saat memeriksa apakah update telah dimulai.

### Acara: 'update-available'

dibunyikan saat ada update yang tersedia. Pembaruan diunduh secara otomatis.

### Acara: 'update-tidak-tersedia'

Emitted saat tidak ada update yang tersedia.

### Acara: 'update-download'

Pengembalian:

* `acara` Acara
* `releaseNotes` String
* `releaseName` String
* `releaseDate` Tanggal
* `updateURL` String

Emitted saat update telah didownload.

Di Windows saja `releaseName` tersedia.

## Metode

Objek `autoUpdater` memiliki metode berikut:

### `autoUpdater.setFeedURL(url[, requestHeaders])`

* `url` String
* `requestHeader` Objek *macOS* (opsional) - header permintaan HTTP.

Menetapkan `url` dan menginisialisasi updater otomatis.

### `autoUpdater.getFeedURL()`

Mengembalikan `String` - URL feed pembaruan saat ini.

### `autoUpdater.checkForUpdates()`

Meminta server apakah ada update. Anda harus menghubungi `setFeedURL` sebelumnya menggunakan API ini.

### `autoUpdater.quitAndInstall()`

Aktifkan ulang aplikasi dan instal pembaruan setelah diunduh. Saya t seharusnya hanya dipanggil setelah `update-download` telah dipancarkan.

**Catatan:** `autoUpdater.quitAndInstall()` akan menutup semua jendela aplikasi pertama dan hanya memancarkan `sebelum-berhenti` pada `aplikasi` setelah itu. Ini berbeda dari urutan kejadian berhenti normal.