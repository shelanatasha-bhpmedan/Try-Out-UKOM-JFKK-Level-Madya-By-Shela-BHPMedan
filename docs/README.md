# Anjungan Selaras — versi publik (GitHub Pages)

Halaman ini adalah versi mandiri dari `kalender-ai/index.html`: bisa dibuka
siapa saja lewat tautan biasa, tanpa masuk akun apa pun.

- `index.html` — seluruh aplikasi
- `vendor/mqtt.min.js` — klien MQTT (untuk sinkronisasi ruang tim)
- `vendor/anthropic.min.js` — SDK Anthropic resmi, dipaket untuk peramban

Keduanya ikut disimpan di repo supaya halaman tidak bergantung pada CDN mana pun.

## Menyalakan GitHub Pages

Setelan repositori → **Pages** → *Source*: **Deploy from a branch** →
pilih cabang tempat berkas ini berada, folder **/docs** → **Save**.
Alamatnya akan menjadi:

```
https://shelanatasha-bhpmedan.github.io/Try-Out-UKOM-JFKK-Level-Madya-By-Shela-BHPMedan/
```

## Cara kerja ruang tim

Acara dan obrolan selalu tersimpan di perangkat (localStorage). Bila sebuah
**kode ruang** diisi, isi kalender juga disiarkan lewat relay MQTT publik:

1. Kunci AES-GCM 256-bit diturunkan dari kode ruang dengan PBKDF2
   (150.000 iterasi, SHA-256).
2. Nama topik adalah ringkasan SHA-256 dari kode ruang, bukan kodenya.
3. Setiap paket disegel di perangkat sebelum dikirim, jadi relay hanya
   melihat sandi acak.

Penggabungan antar perangkat memakai aturan *tulisan terbaru menang* per acara
(`updatedAt`), dengan nisan penghapusan agar acara yang dihapus tidak hidup lagi.
Keadaan terakhir dikirim sebagai pesan *retained*, sehingga yang bergabung
belakangan langsung menerima kalender lengkap.

**Batas yang perlu diketahui:** siapa pun yang memegang kode ruang atau tautan
undangannya bisa membaca dan mengubah kalender. Perlakukan seperti kata sandi
bersama, dan pilih kode yang tidak mudah ditebak.

## Co-pilot AI

Versi publik memakai kunci API Anthropic milik pemakai sendiri (tombol
**Aktifkan co-pilot**). Kunci hanya disimpan di peramban itu dan dikirim
langsung ke Anthropic — tidak pernah lewat relay tim. Untuk co-pilot tanpa
kunci, gunakan versi Artifact di `kalender-ai/index.html`.

## Status pengujian

Sinkronisasi diuji dengan broker MQTT lokal dan dua peramban: kehadiran,
pembuatan acara, obrolan, dan penghapusan semuanya menyeberang antar sesi.
Relay publik (EMQX, Mosquitto) belum bisa diuji dari lingkungan pengembangan
karena jaringan keluarnya dibatasi.
