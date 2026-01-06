# 🏥 JaninSehat (MomCare)

<p align="center">
  <img src="assets/main.png" alt="JaninSehat Logo" width="200"/>
</p>

<p align="center">
  <strong>Sistem Pemantauan Kehamilan Cerdas</strong><br>
  Mobile Application untuk Ibu Hamil dengan Fitur Pencarian Bidan & Deteksi Anomali
</p>

---

## 📋 Daftar Isi

- [Tentang Proyek](#tentang-proyek)
- [Fitur Utama](#fitur-utama)
- [Arsitektur Sistem](#arsitektur-sistem)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Struktur Proyek](#struktur-proyek)
- [Instalasi & Menjalankan](#instalasi--menjalankan)
- [API Documentation](#api-documentation)
- [Fitur Unggulan](#fitur-unggulan)
- [Screenshot Aplikasi](#screenshot-aplikasi)
- [Lisensi](#lisensi)

---

## 🎯 Tentang Proyek

**JaninSehat** adalah aplikasi mobile berbasis Flutter yang dirancang untuk membantu ibu hamil dalam:
- ✅ Memantau perkembangan kehamilan secara real-time
- ✅ Menemukan bidan terdekat dengan rute tercepat
- ✅ Mengatur jadwal check-up danUSG
- ✅ Mendeteksi dini anomali kehamilan menggunakan Machine Learning
- ✅ Memberikan rekomendasi kesehatan berbasis data

Sistem ini dibangun dengan arsitektur **client-server** menggunakan **Flutter** untuk frontend dan **FastAPI** untuk backend.

---

## ✨ Fitur Utama

| Modul | Deskripsi |
|-------|-----------|
| **🔐 Autentikasi** | Register dan Login pengguna dengan database SQLite |
| **🏠 Homepage** | Dashboard pemantauan kehamilan dengan deteksi anomali ML |
| **🗺️ Peta Bidan** | Find nearest midwives dengan rute tercepat (Dijkstra) |
| **📅 Kalender** | Jadwal check-up, USG Trimester 1 & 3 |
| **👤 Profil** | Riwayat perkembangan janin & update data kehamilan |
| **📊 Grafik** | Visualisasi pertumbuhan janin mingguan |
| **🔔 Notifikasi** | Pengingat jadwal pemeriksaan |

---

## 🏗️ Arsitektur Sistem

```
┌─────────────────────────────────────────────────────────────┐
│                      FLUTTER APP (Mobile)                   │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌─────────────┐  │
│  │  Homepage │ │   Maps    │ │  Calendar │ │   Profile   │  │
│  └─────┬─────┘ └─────┬─────┘ └─────┬─────┘ └──────┬──────┘  │
│        │             │             │              │         │
│        └─────────────┴─────────────┴──────────────┘         │
│                           │                                  │
│                      REST API                                │
└───────────────────────────┼──────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                      FASTAPI BACKEND                         │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────┐  │
│  │   Auth   │ │   Home   │ │  Bidan   │ │ Calendar/Prof. │  │
│  └────┬─────┘ └────┬─────┘ └────┬─────┘ └───────┬────────┘  │
│       │            │            │               │           │
│       └────────────┴────────────┴───────────────┘           │
│                           │                                  │
│                    ┌──────┴──────┐                          │
│                    │   SQLite    │                          │
│                    │   Database  │                          │
│                    └─────────────┘                          │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  ML Model   │    │ GeoJSON     │    │   Excel     │
│ (Isolation  │    │ Road Network│    │ (Pregnancy  │
│  Forest)    │    │  Dijkstra   │    │    Data)    │
└─────────────┘    └─────────────┘    └─────────────┘
```

---

## 🛠️ Teknologi yang Digunakan

### Frontend (Flutter)
| Package | Fungsi |
|---------|--------|
| `flutter_map` | Peta interaktif |
| `latlong2` | Koordinat geografis |
| `http` | Komunikasi REST API |
| `fl_chart` | Grafik perkembangan janin |
| `table_calendar` | Kalender jadwal |
| `sqflite` | Database lokal |
| `animations` | Transisi halaman |
| `flutter_local_notifications` | Notifikasi pengingat |
| `url_launcher` | Buka link eksternal |

### Backend (FastAPI)
| Library | Fungsi |
|---------|--------|
| `fastapi` | Web framework |
| `uvicorn` | ASGI server |
| `pandas` | Manipulasi data |
| `scikit-learn` | ML Isolation Forest |
| `geojson` | Parsing data GeoJSON |
| `sqlite3` | Database |

### Data & Algoritma
- **Road Network**: GeoJSON (dari QGIS)
- **Routing Algorithm**: Dijkstra's Algorithm
- **ML Algorithm**: Isolation Forest
- **Reference Data**: prediksiml.xlsx

---

## 📁 Struktur Proyek

```
janinsehat/
├── aplikasi/                    # Flutter Mobile App
│   ├── lib/
│   │   ├── main.dart           # Entry point
│   │   ├── login_page.dart     # Halaman login
│   │   ├── register_page.dart  # Halaman registrasi
│   │   ├── home_page.dart      # Dashboard utama
│   │   ├── maps_page.dart      # Peta bidan
│   │   ├── calendar_page.dart  # Kalender jadwal
│   │   ├── profile_page.dart   # Profil pengguna
│   │   ├── api_service.dart    # Service API calls
│   │   ├── db_helper.dart      # Database helper
│   │   └── config/
│   │       └── api.dart        # API configuration
│   ├── assets/                 # Images & assets
│   ├── pubspec.yaml            # Flutter dependencies
│   └── android/                # Android build files
│
├── backend/                    # FastAPI Backend
│   ├── app/
│   │   ├── main.py            # Entry point + CORS config
│   │   ├── db.py              # Database initialization
│   │   ├── models.py          # Data models
│   │   ├── ml_utils.py        # ML anomaly detection
│   │   ├── map_utils.py       # Dijkstra + road network
│   │   └── routes/
│   │       ├── auth.py        # /auth endpoints
│   │       ├── home.py        # /home endpoints
│   │       ├── bidan.py       # /bidan endpoints
│   │       ├── calendar.py    # /calendar endpoints
│   │       └── profile.py     # /profile endpoints
│   ├── data/
│   │   ├── bidan_points.csv   # Data bidan
│   │   ├── prediksiml.xlsx    # ML reference data
│   │   └── Jaringan_jalanan_indonesia.geojson  # Road network
│   └── requirements.txt       # Python dependencies
│
├── assets/                     # App screenshots & assets
│   ├── main.png
│   ├── maps.png
│   └── homepage.png
│
└── README.md                   # Dokumentasi ini
```

---

## 🚀 Instalasi & Menjalankan

### Prerequisites
- **Python 3.8+**
- **Flutter SDK 3.0+**
- **Git**

### 1. Clone Repository
```bash
git clone <repository-url>
cd JanetSehat
```

### 2. Setup Backend (FastAPI)

```bash
cd backend/app

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
# atau: venv\Scripts\activate  # Windows

# Install dependencies
pip install -r requirements.txt

# Run server
python main.py
# Server akan berjalan di http://localhost:8000
```

**Backend Requirements (`requirements.txt`):**
```
fastapi
uvicorn
pandas
scikit-learn
geojson
openpyxl
python-multipart
```

### 3. Setup Frontend (Flutter)

```bash
cd aplikasi

# Install dependencies
flutter pub get

# Run aplikasi
flutter run
```

### 4. Akses Aplikasi
- **Backend API Docs**: http://localhost:8000/docs
- **Mobile App**: Jalankan di emulator/device

---

## 📡 API Documentation

### Base URL
```
http://localhost:8000
```

### Endpoints

#### 🔐 Autentikasi
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| POST | `/auth/register` | Registrasi user baru |
| POST | `/auth/login` | Login user |

#### 🏠 Home
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/home/{user_id}` | Data dashboard & deteksi anomali |

#### 🗺️ Bidan & Maps
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/bidan/bidan_list` | Daftar semua bidan |
| GET | `/bidan/route` | Hitung rute ke bidan |

#### 📅 Calendar
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/calendar/schedule` | Jadwal kehamilan dari HPHT |

#### 👤 Profile
| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| GET | `/profile/{user_id}` | Ambil profil user |
| PUT | `/profile/update/{user_id}` | Update profil |
| POST | `/profile/{user_id}/update_hpht` | Update HPHT |
| POST | `/profile/{user_id}/growth` | Tambah data perkembangan |

---

## 🧠 Fitur Unggulan

### 1. 🔍 Deteksi Anomali Kehamilan (Machine Learning)
Sistem menggunakan **Isolation Forest** untuk mendeteksi anomali berdasarkan:
- Minggu kehamilan
- Berat fetus
- Panjang fetus  
- Detak jantung fetus

### 2. 🛣️ Perhitungan Rute Tercepat (Dijkstra)
- Menggunakan data jaringan jalan **GeoJSON** dari QGIS
- Algoritma Dijkstra untuk mencari rute terdekat
- Fallback ke garis lurus jika jaringan jalan tidak tersedia
- Coverage area: Jakarta (106.6° - 107.0° BT, -6.4° - -6.05° LS)

### 3. 📊 Rekomendasi Berdasarkan Minggu Kehamilan
- Saran kesehatan mingguan
- Artikel terkait perkembangan janin
- Batas normal berat/panjang/HR per minggu

### 4. 📅 Smart Calendar
- Auto-generate jadwal check-up bulanan
- Pengingat USG Trimester 1 & 3
- Perkiraan Due Date (280 hari dari HPHT)

---

## 📸 Screenshot Aplikasi

| Splash Screen | Home Page | Maps Page |
|---------------|-----------|-----------|
| ![Splash](assets/main.png) | ![Home](assets/homepage.png) | ![Maps](assets/maps.png) |

---

## 📦 Data Sources

| File | Deskripsi |
|------|-----------|
| `prediksiml.xlsx` | Data normatif kehamilan (berat, panjang, HR per minggu) |
| `bidan_points.csv` | Database bidan (nama, koordinat, rating, kontak) |
| `Jaringan_jalanan_indonesia.geojson` | Model jaringan jalan Jakarta untuk routing |

---

## 🔧 Troubleshooting

### Backend tidak bisa start?
```bash
# Pastikan tidak ada process lain di port 8000
lsof -i :8000

# Install dependencies lengkap
pip install fastapi uvicorn pandas scikit-learn geojson openpyxl
```

### Maps tidak menampilkan rute?
- Pastikan file `Jaringan_jalanan_indonesia.geojson` ada di `backend/data/`
- Koordinat harus dalam area Jakarta

### ML tidak berfungsi?
- Pastikan `prediksiml.xlsx` ada di `backend/data/`
- Minimal 5 data minggu untuk deteksi anomali

---

## 📄 Lisensi

Proyek ini dibuat untuk tujuan edukasi dan penelitian.

---

## 👨‍💻 Kontributor

Dikembangkan sebagai proyek tugas akhir/kapstone.

---

<p align="center">
  Dibuat dengan ❤️ untuk ibu hamil Indonesia
</p>

