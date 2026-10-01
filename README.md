# Drezz LAN Chat

Aplikasi chat lokal tanpa Termux. Satu HP Android menjadi host; peserta lain membuka browser di Wi-Fi atau LAN yang sama. Tidak perlu server internet untuk berkirim pesan.

## Ambil APK

1. Buka tab **Actions** di repository ini.
2. Pilih run **Buat APK Drezz Chat** yang berstatus hijau.
3. Di **Artifacts**, unduh **Drezz-LAN-Chat-APK** lalu ekstrak ZIP.
4. Instal `Drezz-LAN-Chat-debug.apk` di Android 8.0 atau lebih baru. Jika diminta Android, izinkan pemasangan dari aplikasi pengunduh yang dipakai.

APK ini versi debug untuk pemakaian dan pengujian langsung. Setiap runner GitHub dapat memakai sertifikat debug berbeda; pembaruan dari APK build lain mungkin mengharuskan uninstall versi sebelumnya.

## Mulai chat

1. Sambungkan HP host dan peserta ke jaringan lokal yang sama.
2. Buka aplikasi host, pilih alamat jaringan, lalu **Mulai host**.
3. Bagikan alamat `http://drezzchat-xxxxxx.local:8080` yang ditampilkan aplikasi beserta PIN ruang.
4. Peserta membuka alamat itu di browser dan mengisi nama serta PIN.
5. Jika domain `.local` tidak terbuka, gunakan alamat IP yang ditampilkan aplikasi, misalnya `http://192.168.1.10:8080`.

Nama domain dibuat otomatis untuk setiap host. Resolusi `.local` bergantung pada router dan perangkat. Matikan fitur guest/client isolation di jaringan yang Anda kelola bila perangkat tidak bisa saling terhubung. Untuk jaringan hotspot, kompatibilitas bergantung pada HP host. Aplikasi menampilkan alamat LAN yang tersedia.

Ruang mendukung teks, emoji, daftar peserta, dan koneksi ulang otomatis. Batas: 24 sesi dan 300 pesan terakhir. Riwayat hanya berada di memori dan hilang saat host dihentikan. Ini satu ruang bersama; belum ada chat pribadi, lampiran, telepon, atau enkripsi end-to-end. Gunakan jaringan yang dipercaya karena koneksi menggunakan HTTP.

## Build ulang dengan mudah

Di tab **Actions**, pilih **Buat APK Drezz Chat → Run workflow → Run workflow**. Tunggu sampai hijau, lalu unduh artifact APK. Build otomatis juga berjalan saat kode sumber atau workflow diubah.

## Kode sumber lengkap / Android Studio

Unduh artifact **Drezz-LAN-Chat-Source** dari run yang berhasil. Ekstrak ZIP artifact lalu ZIP proyek di dalamnya. Buka folder `DrezzLANChat` di Android Studio, gunakan JDK 17, biarkan Gradle Sync selesai, lalu pilih menu build APK. Proyek menggunakan Gradle 8.9, Android Gradle Plugin 8.7.3, compileSdk 35, dan minSdk 26. Tutorial lebih lengkap ada di `PANDUAN.html` dan `README.md` dalam proyek.

Karena transfer ZIP dari sesi browser terhambat, sumber asli disimpan sebagai teks di `materialize.py`. Script ini menghasilkan 31 berkas proyek tanpa mengubah isinya. Jalankan `python3 materialize.py` untuk membukanya. Workflow menambahkan Gradle Wrapper JAR resmi saat build. Bila membangun dari checkout sendiri, siapkan Gradle 8.9 dan Android SDK lalu jalankan:

```sh
python3 materialize.py
gradle -p DrezzLANChat wrapper --gradle-version 8.9
gradle -p DrezzLANChat :app:assembleDebug
```

Untuk mengubah aplikasi, gunakan proyek yang sudah diekstrak. Jika tetap memakai format repository ini, ubah isi berkas terkait dalam `FILES` di `materialize.py`. Jangan commit keystore atau password.

## Validasi

Workflow menguji server HTTP, menjalankan `assembleDebug`, menjalankan Android Lint, dan menyertakan checksum SHA-256 APK. Antarmuka web juga telah diuji dengan dua sesi Chromium pada build awal. Pengujian native di HP nyata dan resolusi mDNS antarperangkat tetap perlu dilakukan pada jaringan pengguna.

Lisensi kode asli: MIT. Rincian dependensi dan lisensinya ada di `THIRD_PARTY.md` pada proyek lengkap.
