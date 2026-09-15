# Anjungan Selaras — Kalender Tim ber-AI

Aplikasi kalender kerja kelompok satu berkas (`index.html`) yang dirancang untuk
dibuka banyak orang sekaligus. Dipublikasikan sebagai Artifact Claude sehingga
mendapat kemampuan runtime: basis data bersama, ruang kehadiran langsung,
co-pilot AI, dan unduhan berkas.

## Fitur

| Kelompok | Rincian |
|---|---|
| Tampilan | Bulan, Minggu (garis waktu jam beserta penanda waktu sekarang), Agenda 30 hari |
| Kolaborasi | Sinkronisasi acara realtime, daftar anggota yang sedang membuka, kursor rekan di atas kalender, sinyal "◎" ke seluruh peserta, obrolan tim yang tersimpan |
| Co-pilot AI | Tulis acara dari kalimat biasa, cari slot kosong bebas bentrok, ringkas periode, susun agenda rapat lalu simpan ke catatan acara, tanya jawab bebas soal jadwal |
| Analitik | Deteksi bentrok (irisan waktu + orang yang sama), meter beban jam per anggota, ringkasan acara/jam/bentrok per periode |
| Lain-lain | Saring per kategori, anggota, dan kata kunci; ekspor `.ics`; tema terang/gelap; pintasan papan tik `←` `→` `t` `n` |

## Model data

- `events/<id>` — `{title, date, start, end, cat, attendees[], notes, priority, owner, updatedAt}`
- `team/roster` — `{members: {nama: {warna, terakhir}}}`
- `channel/main` — `{messages: [{id, who, warna, text, at}]}` (60 pesan terakhir)

Kategori acara: `rapat`, `lapangan`, `tenggat`, `fokus`. Palet kategori sudah
diuji keterbacaannya untuk mode terang maupun gelap, termasuk untuk pembaca
dengan buta warna.

## Menjalankan

Berkas ini ditulis sebagai isi halaman Artifact (tanpa bungkus `<html>`), namun
tetap terbuka langsung di peramban. Tanpa runtime Claude, aplikasi berjalan
dalam **mode lokal**: kalender, penyaringan, editor acara, dan ekspor tetap
berfungsi dengan jadwal contoh, sedangkan sinkronisasi tim dan co-pilot AI
nonaktif.
