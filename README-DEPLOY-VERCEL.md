# Deploy ke Vercel — Nusa Data Console v7.6

Paket ini **statis**: hanya satu berkas HTML + satu berkas konfigurasi. Tidak ada server,
tidak ada endpoint, tidak ada database. Semua data disimpan di browser pengunjung.

## Isi paket

| Berkas | Guna |
|---|---|
| `index.html` | Aplikasi lengkap (302.652 byte, SHA-256 `67853542…a64137`) |
| `vercel.json` | Menambahkan header keamanan + `Cache-Control: no-store` |

`index.html` harus bernama `index.html` supaya langsung terbuka di alamat akar
(`https://nama-proyek.vercel.app/`).

---

## Cara 1 — Vercel CLI (paling cepat, ±2 menit)

Butuh Node.js di komputer (bukan di HP).

```bash
cd deploy-vercel
npx vercel          # login lewat browser, jawab pertanyaan sekali
npx vercel --prod   # menerbitkan ke alamat produksi
```

Saat ditanya:

| Pertanyaan | Jawaban |
|---|---|
| Set up and deploy? | `Y` |
| Which scope? | akunmu |
| Link to existing project? | `N` |
| Project name | mis. `nusa-console` |
| Directory | `.` (tekan Enter) |
| Override settings? | `N` |

Selesai → terminal menampilkan URL `https://nusa-console-xxxx.vercel.app`.

## Cara 2 — Lewat GitHub (tanpa terminal)

1. Buat repo baru, unggah `index.html` dan `vercel.json` ke akar repo.
2. Buka [vercel.com/new](https://vercel.com/new) → **Import Git Repository** → pilih repo.
3. Framework Preset: **Other**. Biarkan Build Command & Output kosong.
4. **Deploy**.

## Cara 3 — dari HP

Vercel tidak punya unggah-drag di web untuk proyek baru tanpa Git. Kalau hanya ada HP:
unggah dua berkas ke repo GitHub lewat aplikasi GitHub, lalu lakukan Cara 2 dari browser HP.

---

## Setelah deploy

1. Buka URL-nya di Chrome (HTTPS otomatis dari Vercel).
2. **Daftar akun** → salin token 12 karakter yang tampil 60 detik → verifikasi.
3. Masuk **Pengaturan** → biarkan **Base URL kosong**. Tidak ada endpoint yang perlu diisi.
4. Tambah satu catatan + satu foto untuk memastikan kamera/galeri bekerja.

## Domain sendiri (opsional)

Vercel → Project → **Settings → Domains → Add**, lalu arahkan DNS domainmu
(`A`/`CNAME` sesuai petunjuk Vercel). Tunggu status **Valid** — sertifikat HTTPS dibuat otomatis.

## Hal yang harus dijaga

1. **Alamat = rumah data.** Data disimpan per-asal (origin). Pindah dari
   `file://` ke `https://…` berarti mulai dari kosong; pindah domain juga.
   Pilih satu alamat, lalu pakai terus.
2. **Jangan pasang di dua alamat berbeda** sambil berharap datanya sama.
3. **`Cache-Control: no-store`** sudah diatur supaya pembaruan langsung terlihat.
   Kalau suatu saat mengganti `index.html`, cukup unggah ulang + `npx vercel --prod`.
4. **Kamera** butuh HTTPS — Vercel sudah memberi. Kalau browser menolak, izinkan
   kamera untuk situs itu di pengaturan situs Chrome.
5. **Jangan menaruh token di tempat umum.** Token hanya tampil sekali di layar verifikasi.
6. **Kata sandi akun adalah kunci isi data.** Lupa sandi = data tidak bisa dibuka.

## Verifikasi setelah unggah (opsional, 1 menit)

```bash
# bandingkan dengan berkas lokal
shasum -a 256 index.html
# harus 678535422f7f2484ae832304eaa36f4dc615c57eaba0067f0986b54249a64137
```

Di browser: buka URL → DevTools → Console. `document.title` harus memuat `v7.6`,
dan `document.getElementById('buildBanner').textContent` memuat `v7.6`.
Itu menandakan berkas yang terunggah memang versi ini.

## Kalau bermasalah

| Gejala | Sebab umum | Tindakan |
|---|---|---|
| Halaman 404 | berkas tidak bernama `index.html` | ganti nama, deploy ulang |
| Layar kosong | berkas terpotong saat unggah | cek ukuran 302.652 byte |
| “Peti data tidak dapat dibuka (butuh WebCrypto)” | dibuka lewat `http://` biasa | pakai HTTPS atau `localhost` |
| Kamera tidak muncul | izin situs ditolak | Chrome → ikon gembok → Camera → Allow |
| Data hilang setelah ganti alamat | origin berbeda | kembali ke alamat lama |
