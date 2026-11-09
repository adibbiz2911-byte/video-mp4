# CLAUDE.md — Projek Video Animasi Kanak-kanak (Slow Pace, Low Stimulus)

## Ringkasan Projek
Saluran YouTube untuk kanak-kanak. Setiap video ialah animasi tenang bertema
(kereta, haiwan, alam, dll.) dengan audio latar zikir, selawat atau bacaan surah.
Durasi 5 minit ke atas. Matlamat utama: menenangkan, bukan merangsang.

Alat utama: **Remotion** (React → MP4). Render dan gabungan audio guna **FFmpeg**.

---

## 1. Peraturan Visual (WAJIB)

### Warna
- Guna palet di bawah sahaja. Jangan tambah warna lain tanpa izin.
  - Latar: `#F5F1E8` (krim), `#E8EFE9` (hijau pudar), `#E6ECF2` (biru pudar)
  - Utama: `#A8C3B0` (sage), `#B8CCE0` (biru lembut), `#E8C9A0` (pic pudar)
  - Aksen: `#D9B8B0` (merah jambu kusam), `#C9B99A` (coklat lembut)
  - Garis/teks: `#5E6B66` (kelabu kehijauan) — jangan guna hitam pekat
- Tiada warna neon, tiada kontras tinggi, tiada gradien terang.

### Gerakan
- Kelajuan maksimum objek: **80 px/saat** (pada resolusi 1920×1080).
- Semua gerakan guna easing lembut (`easeInOutSine` atau setara). Tiada gerakan linear yang tersentak.
- Maksimum **2 objek bergerak serentak** dalam satu masa.
- Gerakan "idle" (bernafas, bergoyang) mesti perlahan: kitaran 4–6 saat.
- Zoom kamera maksimum 5%, berlaku sekurang-kurangnya dalam 8 saat.
- DILARANG: kelipan (flashing), getaran kamera, putaran laju, objek muncul tiba-tiba, kesan zarah yang banyak.

### Adegan & Peralihan
- Setiap adegan: **20–40 saat**.
- Peralihan: **fade sahaja**, tempoh 1.5–2 saat. Tiada wipe, slide atau cut.
- Setiap adegan ada satu tumpuan visual sahaja (contoh: satu kereta melalui jalan).
- Elakkan teks di skrin. Jika perlu, maksimum 3 perkataan, saiz besar, fade masuk perlahan.

### Gaya Ilustrasi
- Flat vector, bentuk bulat dan ringkas, tanpa bayang tajam.
- Watak dan objek dilukis sebagai SVG dalam `assets/svg/` atau terus dalam komponen React.
- Jangan guna atau tiru watak berhak cipta (kartun terkenal, jenama, maskot).
- Ekspresi wajah (jika ada) tenang dan neutral — tiada ekspresi melampau atau lucah jenaka slapstik.

---

## 2. Peraturan Audio (WAJIB)

- Fail audio dalam `assets/audio/` ialah **sumber utama**. Durasi video MESTI mengikut durasi audio, bukan sebaliknya.
- **Jangan sekali-kali memotong audio di tengah ayat, zikir atau selawat.** Jika audio perlu dipendekkan, tanya dahulu.
- Tiada muzik, tiada kesan bunyi (SFX), tiada suara lain yang dicampur.
- Normalisasi kelantangan ke **-16 LUFS**. Fade in 2 saat di awal dan fade out 3 saat di akhir sahaja.
- Setiap fail audio mesti ada fail metadata sepadan `<nama>.json` mengandungi: `tajuk`, `qari_atau_perakam`, `status_lesen`. Jika `status_lesen` tiada atau bukan `"diluluskan"`, **jangan render** — beri amaran.

---

## 3. Adab Kandungan
- Visual mesti tenang dan tidak bercanggah dengan bacaan (tiada aksi jenaka, kejar-mengejar atau babak lucu semasa bacaan surah).
- Jangan letak teks ayat Al-Quran di skrin melainkan diminta dan disemak.
- TODO (pemilik projek): tambah garis panduan daripada ustaz/rujukan agama di sini.

---

## 4. Spesifikasi Teknikal
- Resolusi: 1920×1080, 30 fps
- Video: H.264, CRF 20
- Audio: AAC 192 kbps, 48 kHz
- Format output: `.mp4`
- Nama fail output: `output/<tema>-<nombor>-<tarikh>.mp4` (contoh: `kereta-01-2026-11-10.mp4`)

---

## 5. Struktur Folder
```
/
├── CLAUDE.md
├── assets/
│   ├── audio/        # fail audio + metadata .json
│   └── svg/          # ilustrasi
├── themes/           # satu fail .json bagi setiap tema
├── src/
│   ├── components/   # komponen boleh guna semula (Kereta, Awan, Pokok, dll.)
│   ├── scenes/       # adegan
│   └── compositions/ # gabungan adegan menjadi satu video
├── preview/          # render resolusi rendah untuk semakan
└── output/           # video akhir
```

## 6. Format Fail Tema (`themes/<tema>.json`)
```json
{
  "tema": "kereta",
  "latar": "#E8EFE9",
  "audio": "assets/audio/selawat-01.mp3",
  "adegan": [
    { "id": 1, "huraian": "Kereta biru bergerak perlahan di jalan kampung", "durasi_saat": 30 },
    { "id": 2, "huraian": "Kereta berhenti di bawah pokok, awan bergerak", "durasi_saat": 35 }
  ]
}
```
Jika jumlah durasi adegan tidak sama dengan durasi audio, ulang atau panjangkan adegan secara automatik — jangan potong audio.

---

## 7. Aliran Kerja
1. Sentiasa buat **preview** dahulu (720p, 15 fps) ke folder `preview/` sebelum render penuh.
2. Bina komponen supaya boleh diguna semula antara tema.
3. Tema baharu = fail JSON baharu + komponen baharu jika perlu. Jangan ubah komponen sedia ada tanpa memaklumkan.
4. Tanya dahulu sebelum memasang pakej baharu.

## 8. Senarai Semak Sebelum Selesai Tugas
- [ ] Hanya warna dari palet digunakan
- [ ] Tiada gerakan melebihi 80 px/saat, tiada kelipan
- [ ] Semua peralihan ialah fade 1.5–2 saat
- [ ] Audio penuh, tidak terpotong, -16 LUFS, tiada muzik/SFX
- [ ] Status lesen audio "diluluskan"
- [ ] Output 1920×1080, 30 fps, .mp4, nama fail ikut format
