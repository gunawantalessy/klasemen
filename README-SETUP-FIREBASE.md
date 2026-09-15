# Setup Firebase agar Data Sinkron ke Semua Device

Setelah website di-publish ke GitHub Pages, data **harus** disimpan di cloud supaya semua orang melihat skor yang sama.

## Langkah 1 – Buat Project Firebase (Gratis)

1. Buka https://console.firebase.google.com
2. Klik **Add project** / **Buat project**
3. Isi nama project (contoh: `hsn-cup-2026`)
4. Matikan Google Analytics jika tidak perlu → **Create project**

## Langkah 2 – Buat Realtime Database

1. Di sidebar kiri → **Build** → **Realtime Database**
2. Klik **Create Database**
3. Pilih lokasi **asia-southeast1** (Singapore) atau yang terdekat
4. Pilih **Start in test mode** (untuk sementara)
5. Klik **Enable**

## Langkah 3 – Ambil Config Web

1. Di Project Overview → klik ikon **</>** (Add app → Web)
2. Beri nama app (contoh: `klasemen`)
3. Centang **Also set up Firebase Hosting** (opsional)
4. Klik **Register app**
5. **Copy** object `firebaseConfig` yang muncul

Contoh:
```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "hsn-cup-2026.firebaseapp.com",
  databaseURL: "https://hsn-cup-2026-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "hsn-cup-2026",
  storageBucket: "hsn-cup-2026.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

## Langkah 4 – Paste Config ke File

Buka **admin.html** dan **user.html**, cari bagian:

```js
const firebaseConfig = {
    apiKey: "GANTI_DENGAN_API_KEY",
    ...
};
```

Ganti seluruh object dengan config yang Anda copy dari Firebase.
**Pastikan kedua file (admin & user) memakai config yang SAMA.**

## Langkah 5 – Atur Rules (Penting untuk Keamanan)

Di Firebase Console → Realtime Database → tab **Rules**, ganti menjadi:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

(Ini memungkinkan siapa saja baca & tulis. Untuk produksi lebih baik di-lock, tapi untuk event internal sudah cukup.)

Klik **Publish**.

## Langkah 6 – Deploy ke GitHub

1. Push folder `WEBSITE KLASEMEN` ke repository GitHub
2. Aktifkan **GitHub Pages** (Settings → Pages → Source: main / docs atau root)
3. Buka `admin.html` untuk input skor
4. Buka `user.html` di device lain → skor akan **otomatis muncul** tanpa refresh

## Cara Kerja

- Admin input skor → data ditulis ke Firebase
- Semua device yang membuka `user.html` / `admin.html` **mendengarkan** perubahan real-time
- Tidak perlu refresh halaman

## Troubleshooting

- Data tidak muncul? → Buka Console browser (F12) → lihat error Firebase (biasanya config salah atau rules masih locked)
- Masih pakai data lama? → Clear localStorage sekali: di Console ketik  
  `localStorage.removeItem('badmintonTournamentCleanV5')` lalu refresh
