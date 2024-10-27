
## 📂 **1. Nama Modul: ChatTasker-Backend**

- **Fungsi Utama**: Mengelola tugas pengguna melalui API, menyimpan data di Firebase, dan menyediakan pengingat otomatis berbasis tanggal.
- **Teknologi**:
  - *Server Framework*: Node.js, Express.js
  - *Database dan Penyimpanan*: Firebase
  - *Keamanan*: JSON Web Token (JWT) untuk autentikasi
  - *Pengingat*: Firebase Cloud Functions atau Node-Cron

---

## 📘 **2. Deskripsi Modul**

Modul ini menyediakan API yang memungkinkan klien (frontend atau chatbot) untuk berinteraksi dengan aplikasi. API menyediakan endpoint CRUD (Create, Read, Update, Delete) untuk tugas, serta fitur autentikasi menggunakan JWT. Firebase digunakan untuk penyimpanan data, termasuk penyimpanan tugas, pengguna, dan pengaturan pengingat.

---

## 🚧 **3. Cara Kerja Modul**

### Alur Kerja

1. **Autentikasi**:
   - Saat pengguna melakukan login, backend akan memverifikasi identitas dan mengeluarkan token JWT yang digunakan untuk akses aman ke API.
2. **CRUD Tugas**:
   - API menyediakan endpoint untuk membuat, membaca, memperbarui, dan menghapus tugas di Firebase.
3. **Pengingat Otomatis**:
   - Firebase Cloud Functions atau Node-cron akan memeriksa tugas dengan `dueDate` yang mendekati dan mengirim notifikasi pengingat kepada pengguna.

### Diagram Alur

1. **Request Autentikasi** ➔ **Generate JWT** ➔ **Store Token**
2. **API CRUD Tugas** ➔ **Store/Update/Delete Task di Firebase** ➔ **Response ke Klien**
3. **Pengingat Otomatis** ➔ **Firebase Cloud Function** ➔ **Kirim Notifikasi ke Klien**

---

## 🛠️ **4. Tugas dan Fungsi Utama dalam Pengembangan**

### A. **Setup Proyek**

- **Tugas**: Membuat struktur proyek dan menginstal dependensi yang diperlukan.
- **Langkah**:
  1. Inisialisasi proyek Node.js:
     ```bash
     npm init -y
     npm install express firebase-admin jsonwebtoken dotenv
     ```
  2. Konfigurasi `.env` untuk Firebase dan JWT.

### B. **Konfigurasi Firebase dan JWT**

- **Tugas**: Integrasi Firebase untuk penyimpanan tugas dan JWT untuk autentikasi.
- **Langkah**:
  1. Setup akun Firebase, buat project, dan unduh file konfigurasi Firebase (`firebase-adminsdk`).
  2. Setup JWT secret di `.env` file untuk keamanan.
  3. Buat file konfigurasi `firebaseConfig.js` untuk menginisialisasi Firebase SDK.

### C. **Implementasi Autentikasi JWT**

- **Tugas**: Membuat middleware untuk autentikasi menggunakan JWT.
- **Langkah**:
  1. Buat middleware `authMiddleware.js` untuk memeriksa token JWT di setiap request.
  2. Buat endpoint login yang menghasilkan JWT setelah pengguna berhasil autentikasi.

### D. **API CRUD untuk Tugas**

- **Tugas**: Membuat endpoint untuk pengelolaan tugas.
- **Langkah**:

  1. **Create Task**: Endpoint untuk menambahkan tugas baru.
  2. **Read Task**: Endpoint untuk mendapatkan detail tugas berdasarkan ID.
  3. **Update Task**: Endpoint untuk memperbarui tugas yang ada.
  4. **Delete Task**: Endpoint untuk menghapus tugas.

  - *Contoh Endpoint*:
    ```javascript
    app.post('/tasks', authMiddleware, createTask)
    app.get('/tasks/:id', authMiddleware, getTaskById)
    app.put('/tasks/:id', authMiddleware, updateTask)
    app.delete('/tasks/:id', authMiddleware, deleteTask)
    ```

### E. **Pengingat Berdasarkan `dueDate`**

- **Tugas**: Menjalankan pengingat otomatis saat `dueDate` mendekati.
- **Langkah**:

  1. Buat Firebase Cloud Function atau gunakan *Node-Cron* untuk penjadwalan pengingat.
  2. Kirim notifikasi jika `dueDate` mendekati.

  - *Contoh Firebase Cloud Function*:
    ```javascript
    const functions = require('firebase-functions');
    exports.sendReminder = functions.pubsub.schedule('every 24 hours').onRun(async (context) => {
        // Logic to check tasks and send reminders
    });
    ```

---

## 🔗 **5. Struktur Proyek**

```
ChatTasker-Backend/
├── src/
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── taskController.js
│   ├── middleware/
│   │   ├── authMiddleware.js
│   ├── config/
│   │   ├── firebaseConfig.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── taskRoutes.js
│   ├── index.js
├── .env
├── package.json
└── README.md
```

---

## 🔑 **6. Konfigurasi dan Variabel Lingkungan**

### `.env`

```
FIREBASE_PROJECT_ID=<YOUR_PROJECT_ID>
FIREBASE_CLIENT_EMAIL=<YOUR_CLIENT_EMAIL>
FIREBASE_PRIVATE_KEY=<YOUR_PRIVATE_KEY>
JWT_SECRET=<YOUR_SECRET_KEY>
```

### `firebaseConfig.js`

```javascript
const admin = require('firebase-admin');
const serviceAccount = require('path/to/firebase-adminsdk.json');

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount)
});

const db = admin.firestore();
module.exports = { db };
```

---

## 📌 **7. API Endpoint dan Dokumentasi**

| HTTP Method | Endpoint       | Description              | Authorization |
| ----------- | -------------- | ------------------------ | ------------- |
| POST        | `/login`     | Login pengguna           | Tidak perlu   |
| POST        | `/tasks`     | Membuat tugas baru       | JWT           |
| GET         | `/tasks/:id` | Mendapatkan detail tugas | JWT           |
| PUT         | `/tasks/:id` | Memperbarui tugas        | JWT           |
| DELETE      | `/tasks/:id` | Menghapus tugas          | JWT           |

---

## 🔒 **8. Keamanan dan Autentikasi**

- Gunakan **JWT** untuk semua endpoint yang memerlukan autentikasi.
- Simpan token JWT di klien (misalnya, localStorage di frontend).
- Middleware `authMiddleware.js` memverifikasi JWT pada setiap request yang terautentikasi.

---

## 📝 **9. Panduan Pengembangan**

1. **Clone Repositori**:

   ```bash
   git clone https://github.com/yourusername/ChatTasker-Backend.git
   cd ChatTasker-Backend
   ```
2. **Instalasi Dependensi**:

   ```bash
   npm install
   ```
3. **Jalankan Aplikasi**:

   ```bash
   npm start
   ```
4. **Testing API**: Gunakan Postman untuk menguji endpoint CRUD dan autentikasi.

---

## ✅ **10. Testing dan Pengujian**

- **Unit Testing**: Pastikan untuk menulis test pada tiap controller menggunakan Jest atau Mocha.
- **Integration Testing**: Uji autentikasi dan API CRUD dengan data di Firebase.
- **E2E Testing**: Lakukan uji end-to-end untuk memastikan pengingat berfungsi dengan benar.
