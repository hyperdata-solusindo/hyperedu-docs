---
sidebar_position: 2
---

# Getting Started

## Overview

Panduan ini akan membantu Anda mengatur environment development HyperEdu dengan cepat.
## Prasyarat

Sebelum memulai, pastikan Anda telah menginstall yang berikut:

- **PHP 8.4 atau lebih tinggi** dengan Composer
- **Node.js 24+** dengan npm atau yarn
- **Database PostgreSQL**
- **Git** untuk kontrol versi

## Setup Backend (Laravel + Sactum)

1. **Clone repository**  

	```bash
	git clone https://github.com/hyperdata-solusindo/hyperedu-api.git
	cd hyperedu-api
	```

2. **Install dependensi PHP**  

	```bash
	composer install
	```

3. **Konfigurasi environment**  

	```bash
	cp .env.example .env
	php artisan key:generate
	```

	Ubah file `.env` dengan kredensial database Anda:  

	```
	DB_CONNECTION=pgsql
	DB_HOST=127.0.0.1
	DB_PORT=5432
	DB_DATABASE=hyperedu
	DB_USERNAME=your_username
	DB_PASSWORD=your_password
	```

4. **Jalankan migrasi database**  

    ```bash
    php artisan migrate
    ```

5. **Seed database (opsional)**  

	```bash
	php artisan db:seed
	```
	
6. **Jalankan development server**  

	```bash
	php artisan serve
	```

	Backend akan tersedia di `http://localhost:8000`

## Setup Frontend (React + TypeScript)

1. **Clone repository frontend**  

	```bash
	git clone https://github.com/hyperdata-solusindo/hyperedu.git
	cd hyperedu-frontend
	```

2. **Install dependensi Node.js**

	```bash
	npm install
	# atau
	yarn install
	```
	
3. **Konfigurasi environment**

	```bash
	cp .env.example .env
	```

	Konfigurasikan endpoint API dan pengaturan lainnya:

	```
	VITE_API_URL=http://localhost:8000/api
	VITE_PORT=5173
	VITE_BASE_URL=/
	```

	**Catatan**:  
	- `VITE_API_URL`: Endpoint API backend  
	- `VITE_PORT`: Port development server frontend (default: 5173)  
	- `VITE_BASE_URL`: Base path untuk aplikasi (default: /)

4. **Jalankan development server**

	```bash
	npm run dev
	# atau
	yarn run dev
	```

	Frontend akan tersedia di `http://localhost:5173`

5. **Mengubah port frontend (opsional)**

	Jika Anda perlu menjalankan frontend di port berbeda (contoh: jika port 5173 sudah digunakan), Anda dapat mengubah nilai `VITE_PORT` di file `.env` Anda

	```
	VITE_PORT=3000
	```

	Setelah mengubah port, restart development server. Frontend akan tersedia di `http://localhost:3000` (atau port yang Anda tentukan).

6. **Menjalankan frontend dengan subfolder (opsional)**

	Jika Anda ingin menjalankan frontend di subfolder (contoh: `http://localhost:5173/hyperedu`), Anda dapat mengatur `VITE_BASE_URL` di file `.env` untuk memastikan routing React bekerja dengan benar:

	```
	VITE_BASE_URL=/hyperedu/
	```

	Ini berguna ketika:  
	- Deploy ke subdirektori di web server  
	- Menjalankan multiple aplikasi di domain yang sama  
	- Menggunakan reverse proxy dengan routing berbasis path
	
	Setelah mengatur `VITE_BASE_URL`, restart development server. Frontend akan dapat diakses di `http://localhost:5173/hyperedu/`.

## Troubleshooting

### Masalah Umum

**Backend tidak start:**  
- Periksa versi PHP: `php --version`  
- Pastikan semua ekstensi terinstall: `php -m`  
- Verifikasi koneksi database

**Error build frontend:**  
- Hapus node_modules: `rm -rf node_modules && npm install`  
- Periksa versi Node.js: `node --version`

**Masalah koneksi database:**  
- Pastikan database server berjalan  
- Periksa kredensial di file `.env`  
- Jalankan `php artisan config:clear` untuk membersihkan config cache

Untuk bantuan lebih lanjut, periksa panduan [Kontribusi](http://10.21.1.30/hyperedu-docs/public/contributing/) atau buka issue.