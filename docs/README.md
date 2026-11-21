# Kalender Kerja BHP Medan — versi publik (GitHub Pages)

Halaman mandiri yang bisa dibuka siapa saja lewat tautan biasa, tanpa masuk akun.

- `index.html` — seluruh aplikasi
- `img/pengayoman.png` — lambang Pengayoman, diambil dari kop surat resmi BHP Medan
- `vendor/mqtt.min.js` — klien MQTT untuk sinkronisasi ruang tim
- `vendor/anthropic.min.js` — SDK Anthropic resmi, dipaket untuk peramban

Pustaka ikut disimpan di repo supaya halaman tidak bergantung pada CDN mana pun.

## Tampilan

Satu dunia visual: **terang saja**, tanpa mode gelap. Warna diambil dari lambang
Pengayoman pada kop surat (navy `#021932`, emas `#E6E005`) lalu dilunakkan untuk
layar: kop navy dengan garis emas, kartu putih, dan latar abu kebiruan.
Huruf Plus Jakarta Sans untuk antarmuka, Lora untuk nama instansi.

Kendali yang jarang dipakai — ruang tim, nama, co-pilot, ekspor — dipindahkan ke
satu menu titik tiga di kop, sehingga bilah alat hanya menyisakan navigasi
tanggal, pemilih tampilan, dan tombol tambah acara. Tidak ada fitur yang hilang.

## Menyalakan GitHub Pages

Setelan repositori → **Pages** → *Source*: **Deploy from a branch** →
pilih cabang tempat berkas ini berada, folder **/docs** → **Save**. Alamatnya:

```
https://shelanatasha-bhpmedan.github.io/Try-Out-UKOM-JFKK-Level-Madya-By-Shela-BHPMedan/
```

## Agenda bawaan

Aplikasi memuat agenda resmi yang belum ada di perangkat:

- **Jumat, 13 November 2026** — Rapat Monitoring Lanjutan Pengampuan a.n. Sdri.
  Ariel Ruth Agustin Aritonang, beserta pengingat konfirmasi, persiapan ruang
  Zoom, dan penyusunan notula. Sumber: surat undangan BHP Medan 9 November 2026
  (nomor 3688–3691).
- **Rabu, 18 November 2026** — Monitoring Pengampuan atas Terampu Sorialam
  Sitorus di Ruang Rapat Kantor BHP Medan. Sumber: Laporan Monitoring Pengampuan
  18 November 2026 (Revisi 2). Laporan tidak mencantumkan jam, jadi jam di
  kalender adalah perkiraan.

Acara yang sudah pernah dimuat tidak digandakan, dan acara yang dihapus dari
aplikasi tidak dihidupkan lagi pada kunjungan berikutnya.

Isinya memuat nama para pihak dan data rapat. Halaman ini publik, jadi perlakukan
repositori ini sebagai ruang publik juga.

## Cara kerja ruang tim

Acara dan obrolan selalu tersimpan di perangkat. Bila sebuah **kode ruang** diisi,
isi kalender juga disiarkan lewat relay MQTT publik:

1. Kunci AES-GCM 256-bit diturunkan dari kode ruang dengan PBKDF2
   (150.000 iterasi, SHA-256).
2. Nama topik adalah ringkasan SHA-256 dari kode ruang, bukan kodenya.
3. Setiap paket disegel di perangkat sebelum dikirim, jadi relay hanya melihat
   sandi acak.

Penggabungan antar perangkat memakai aturan *tulisan terbaru menang* per acara
(`updatedAt`), dengan nisan penghapusan. Keadaan terakhir dikirim sebagai pesan
*retained*, sehingga yang bergabung belakangan langsung menerima kalender lengkap.

Siapa pun yang memegang kode ruang atau tautan undangannya bisa membaca dan
mengubah kalender — perlakukan seperti kata sandi bersama.

## Co-pilot AI

Memakai kunci API Anthropic milik pemakai sendiri (menu → **Co-pilot AI**). Kunci
hanya disimpan di peramban itu dan dikirim langsung ke Anthropic, tidak pernah
lewat relay tim.

## Status pengujian

Sinkronisasi diuji dengan broker MQTT lokal dan dua peramban: kehadiran, acara
baru, obrolan, dan penghapusan menyeberang antar sesi. Tampilan bulan, minggu,
agenda, editor acara, dan layar 400px diperiksa langsung. Relay publik (EMQX,
Mosquitto) belum bisa diuji dari lingkungan pengembangan karena jaringan keluarnya
dibatasi.
