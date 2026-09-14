# Troubleshooting - NIM Tidak Terdaftar

## Masalah: "NIM 24105111017 tidak terdaftar"

### Penyebab Kemungkinan:
1. ❌ NIM belum ditambahkan di admin.html
2. ❌ Data tidak tersimpan dengan benar ke localStorage
3. ❌ Koneksi ke Google Sheets bermasalah
4. ❌ Cache browser yang lama masih tersimpan

---

## Solusi Step-by-Step

### Step 1: Verifikasi NIM di Admin Panel

**1.1. Buka admin.html**
```
http://localhost/absensi2/admin.html
```

**1.2. Login dengan:**
- Username: `admin`
- Password: `admin123`

**1.3. Buka Tab "Mahasiswa"**

**1.4. Lihat bagian "SEMUA MAHASISWA"**
- Cari NIM `24105111017` di list
- ✅ Jika ada = lanjut ke Step 2
- ❌ Jika tidak ada = tambahkan mahasiswa (lihat Step 2b)

---

### Step 2: Tambahkan atau Verifikasi NIM di Admin

**2a. Jika NIM sudah ada, gunakan Developer Console:**

1. Buka admin.html (sudah login)
2. Tekan **F12** untuk buka Developer Tools
3. Buka tab **Console**
4. Ketik perintah ini:
   ```javascript
   window.DEBUG.showMahasiswa()
   ```
5. Lihat output, pastikan `24105111017` muncul di list
6. Jika muncul, data sudah tersimpan ✓

**Contoh output yang benar:**
```
Data Mahasiswa (admin):
1. 24105111017 - Budi Santoso (2A)
2. 24105111018 - Ani Wijaya (2A)
...
```

**2b. Jika NIM belum ada, tambahkan sekarang:**

1. Di tab Mahasiswa → bagian TAMBAH MAHASISWA
2. Isi form:
   - **NIM**: `24105111017`
   - **Nama**: nama lengkap mahasiswa
   - **Kelas**: kelas (misal: 2A)
   - **Jurusan**: pilih dari dropdown
3. Klik **"Tambah ke Sheets"**
4. Tunggu sampai muncul toast "Data ditambahkan"
5. Buka Console (F12) dan cek:
   ```javascript
   window.DEBUG.showMahasiswa()
   ```
   - Pastikan data baru muncul di list

---

### Step 3: Verifikasi localStorage Tersinkronisasi

**Di Console Browser admin.html, jalankan:**
```javascript
window.DEBUG.storage()
```

**Output yang benar:**
```
LocalStorage: [
  {nim: "24105111017", nama: "Budi Santoso", kelas: "2A", jurusan: "..."},
  ...
]
```

**Jika kosong/undefined:**
- Refresh halaman (Ctrl+F5)
- Tambahkan mahasiswa ulang
- Lihat console.log saat mahasiswa ditambahkan

---

### Step 4: Buka login.html dan Verifikasi

**4.1. Buka login.html**
```
http://localhost/absensi2/login.html
```

**4.2. Tekan F12 dan buka Console**

**4.3. Lihat auto-loading message:**
Seharusnya muncul:
```
✓ Mahasiswa (localStorage): 2 records
  1. 24105111017 - Budi Santoso
  2. 24105111018 - Ani Wijaya
```

**Jika tidak muncul message, refresh halaman (Ctrl+F5)**

**4.4. Verifikasi di Console:**
```javascript
window.DEBUG.showMahasiswa()
```

**4.5. Test NIM spesifik:**
```javascript
window.DEBUG.checkNIM('24105111017')
```

**Output yang benar:**
```
✓ DITEMUKAN: {nim:"24105111017", nama:"Budi Santoso", ...}
```

---

### Step 5: Coba Registrasi

**5.1. Tekan Tab DAFTAR di login.html**

**5.2. Isi form:**
- **Nama**: Budi Santoso (atau sesuai yang di admin)
- **NIM**: 24105111017
- **Kelas**: 2A (sesuai yang di admin)
- **Jurusan**: pilih jurusan yang sama
- **Password**: minimal 6 karakter

**5.3. Klik "Buat Akun"**

**5.4. Buka Console untuk lihat debugging:**
```
✓ NIM ditemukan: 24105111017 {...}
```

✅ Jika muncul = **Registrasi berhasil!**
❌ Jika masih error = lanjut ke Step 6

---

### Step 6: Nuclear Option - Clear & Reload

Jika masih bermasalah, lakukan reset penuh:

**6.1. Di admin.html Console:**
```javascript
// Backup dulu data (copy output ini)
window.DEBUG.exportJSON()

// Kemudian reset localStorage
localStorage.removeItem('mahasiswaData')
location.reload()
```

**6.2. Di admin.html yang sudah reload:**
- Tambahkan mahasiswa ulang
- Lihat console.log: `✓ Mahasiswa disimpan ke localStorage`

**6.3. Di login.html Console:**
```javascript
window.DEBUG.reset()
location.reload()
```

**6.4. Coba registrasi lagi**

---

## Debug Checklist

Sebelum keluh-kesah, pastikan:

- [ ] NIM sudah ditambahkan di admin.html tab Mahasiswa
- [ ] Toast menunjukkan "Data ditambahkan"
- [ ] `window.DEBUG.showMahasiswa()` di admin.html show data terbaru
- [ ] `window.DEBUG.storage()` di admin.html show data di localStorage
- [ ] Refresh login.html dengan Ctrl+F5
- [ ] `window.DEBUG.showMahasiswa()` di login.html show data yang sama
- [ ] `window.DEBUG.checkNIM('24105111017')` return "DITEMUKAN"
- [ ] Registrasi dengan data yang sama dengan di admin

---

## Pesan Error & Solusi

### ❌ "NIM tidak terdaftar di data mahasiswa"
**Solusi:** 
- NIM belum ditambahkan di admin.html
- Atau cache browser lama, coba Ctrl+F5 refresh
- Jalankan `window.DEBUG.checkNIM('24105111017')` di login.html untuk verifikasi

### ❌ "Data Mahasiswa (localStorage): 0 records"
**Solusi:**
- Admin belum menambahkan data mahasiswa
- Atau Google Apps Script macro tidak merespon
- Tambahkan data di admin.html → test dengan `window.DEBUG.showMahasiswa()`

### ❌ "NIM sudah terdaftar"
**Solusi:**
- Akun dengan NIM ini sudah dibuat sebelumnya
- Coba login dengan password yang sudah dibuat
- Atau gunakan NIM lain

---

## Google Sheets Integration

**URL yang benar:**
```
https://docs.google.com/spreadsheets/d/1oSruZCsg6TQqdfh2J3cFthv3-tFHwPEmfkMHg_BtTec/edit?usp=sharing
```

**Google Apps Script Macro:**
```
https://script.google.com/macros/s/AKfycbxNoxMP4BpRbytR5u4xTp2SuQAHZINXSWRvxgzBvHgTArh-hIMcUaJaItzo9bMAmDyGhw/exec
```

**Jika Google Sheets tidak merespon:**
- System akan otomatis fallback ke localStorage
- Data tetap berfungsi meski offline
- Cek console untuk warning: `⚠ Google Sheets timeout`

---

## Tips Debugging Lanjutan

### Lihat semua data yang tersimpan:
```javascript
// Admin panel
console.log('Admin State:', State.mahasiswaData)
console.log('LocalStorage:', JSON.parse(localStorage.getItem('mahasiswaData')))

// Login page
console.log('Login mahasiswaData:', mahasiswaData)
```

### Cek koneksi Google Sheets:
```javascript
fetch('https://script.google.com/macros/s/AKfycbxNoxMP4BpRbytR5u4xTp2SuQAHZINXSWRvxgzBvHgTArh-hIMcUaJaItzo9bMAmDyGhw/exec?action=get_all')
  .then(r => r.json())
  .then(d => console.log('Google Sheets response:', d))
  .catch(e => console.error('Koneksi error:', e))
```

### Monitor localStorage perubahan:
```javascript
// Setiap kali localStorage berubah, akan log
const original = localStorage.setItem
localStorage.setItem = function(key, value) {
  console.log('🔄 localStorage updated:', key, JSON.parse(value))
  original.apply(this, arguments)
}
```

---

## Kontak Support

Jika masih error setelah ikuti semua step:
1. Screenshot error message
2. Copy output dari `window.DEBUG.exportJSON()`
3. Copy output dari `window.DEBUG.storage()`
4. Screenshot tab Mahasiswa di admin.html
5. Kirim ke admin dengan informasi tersebut
