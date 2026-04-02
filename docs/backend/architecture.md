---
sidebar_position: 1
---
# Architecture

## Overview

Backend HyperEdu adalah REST API modern yang menyediakan layanan data dan logika bisnis untuk platform pendidikan. Dibangun di atas **Laravel 12**, API ini mengikuti praktik terbaik PHP modern dan menggunakan arsitektur berlapis yang terstruktur.

### Prinsip Utama

- **Modularitas**: Setiap domain bisnis (Auth, Core, Academic, Master) berdiri sendiri dengan controller, service, DTO, dan resource masing-masing.
- **Pemisahan Tanggung Jawab**: Penerimaan request, validasi, logika bisnis, dan transformasi respons dipisahkan dengan jelas ke dalam layer yang berbeda.
- **Dapat Digunakan Ulang**: Trait, helper, dan abstraksi dipusatkan agar dapat dipakai lintas domain.
- **Skalabilitas**: Penambahan fitur baru tidak memengaruhi domain yang sudah ada, cukup ikuti pola yang sudah berjalan.

## Struktur Aplikasi

Aplikasi HyperEdu Backend menggunakan pendekatan **Domain-Based Layered Architecture**. Setiap domain bisnis diorganisir secara vertikal melewati semua layer, dari controller hingga model.

```
app/
├── Console/
│   └── Commands/           # Artisan commands kustom (generator scaffold)
│       └── Swagger/        # Generator dokumentasi Swagger otomatis
├── DataTransferObjects/    # DTO — transformasi & sanitasi input per domain
│   ├── Academic/
│   ├── Auth/
│   ├── Core/
│   └── Master/
├── Exceptions/             # Exception handler kustom (AppException)
│   └── Auth/
├── Helpers/                # Utilitas SHARED yang dipakai semua domain
│   ├── Models/             # Trait model (HasCommonFunction, BelongsToSchool)
│   ├── Pagination/         # PaginationRequest & HasPaginationReact
│   └── Response/           # Trait ApiResponse (format JSON standar)
├── Http/
│   ├── Controllers/        # Menerima request, memanggil Service
│   │   ├── Academic/
│   │   ├── Core/
│   │   └── Master/
│   ├── Middleware/         # ProductionSanctumAuth, DecodeXsrfToken
│   ├── Requests/           # Form Request Validation per domain
│   │   ├── Academic/
│   │   ├── Auth/
│   │   ├── Core/
│   │   └── Master/
│   └── Resources/          # Transformasi output JSON per domain
│       ├── Academic/
│       ├── Auth/
│       ├── Core/
│       └── Master/
├── Models/                 # Eloquent ORM per domain
│   ├── Academic/
│   ├── Auth/
│   ├── Core/
│   └── Master/
└── Services/               # Business logic per domain
    ├── Academic/
    ├── Auth/
    ├── Core/
    └── Master/
```

:::note "Prinsip Utama"
Setiap folder domain (Academic, Core, Master) harus bersifat mandiri di setiap layer. Jika domain `Academic` membutuhkan data dari `Master`, akses dilakukan melalui Service yang bersangkutan, bukan langsung ke model domain lain.
:::

Setiap domain mengikuti struktur layer yang konsisten:

```
[Domain]/
├── Controllers/    # Terima HTTP request, delegasikan ke Service
├── Requests/       # Validasi input (Form Request)
├── DataTransferObjects/  # Sanitasi & strukturisasi data input
├── Services/       # Business logic & operasi data
├── Models/         # Eloquent ORM & relasi database
└── Resources/      # Transformasi output sebelum dikirim ke client
```

## Teknologi Utama

### Framework Backend
- **Laravel 12**: Framework PHP modern dengan ekosistem lengkap
- **PHP 8.2+**: Memanfaatkan fitur modern seperti readonly properties, named arguments, dan match expression
- **Laravel Octane**: Boost performa dengan server persistent (FrankenPHP/Swoole)

### Autentikasi
- **Laravel Sanctum 4.0**: Token-based authentication untuk SPA dan mobile
- **Middleware Kustom** (`ProductionSanctumAuth`): Layer autentikasi dengan bypass otomatis di environment lokal/staging

### Dokumentasi API
- **L5-Swagger (OpenAPI 3.0)**: Dokumentasi API interaktif yang di-generate dari anotasi kode
- **Swagger Namespace Terpisah** (`swagger/`): File anotasi swagger dipisah dari kode utama agar tidak mencemari domain logic

### Developer Tools
- **Laravel Telescope**: Debugging dan monitoring request di development
- **Laravel IDE Helper**: Autocomplete dan type hint untuk Eloquent
- **Laravel Pint**: Code style formatter (PSR-12)
- **Artisan Custom Commands**: Generator scaffold otomatis untuk Controller, Service, DTO, dan Swagger

## Pola Arsitektur

### Arsitektur Berlapis (Layered Architecture)

Request HTTP mengalir secara linear melalui layer yang sudah ditentukan:

```
Request
  → Middleware         (autentikasi, CORS, XSRF)
  → Form Request       (validasi input)
  → Controller         (koordinasi alur)
  → DTO                (sanitasi & transformasi input)
  → Service            (business logic)
  → Model/Eloquent     (akses database)
  → Resource           (transformasi output)
  → JSON Response
```

Tidak ada layer yang melewati layer lain secara langsung. Controller tidak boleh akses Model langsung jika ada Service terkait.

### Data Transfer Object (DTO)

Input dari request tidak langsung diteruskan ke Service sebagai raw array. Setiap operasi tulis menggunakan DTO untuk menjamin struktur data yang konsisten:

```php
// Di Controller
$data = StudentData::fromRequest($request);
$this->studentServices->create($data->toArray());

// Di DTO — bersifat readonly, immutable
class StudentData {
    public function __construct(
        public readonly string $fullName,
        public readonly string $nis,
        // ...
    ) {}

    public static function fromRequest(StudentRequest $request): self { ... }
    public function toArray(): array { ... }
}
```

### Trait HasCommonFunction

Service tidak perlu mengimplementasikan CRUD dasar secara berulang. Trait `HasCommonFunction` menyediakan operasi standar yang siap pakai:

```php
class StudentServices {
    use HasCommonFunction;

    protected $modelClass = Student::class;

    // find(), create(), update(), delete(), getPaginatedData()
    // sudah tersedia dari trait — tinggal override jika perlu
}
```

### Multi-Tenant dengan BelongsToSchool

Isolasi data antar sekolah ditangani secara transparan melalui trait `BelongsToSchool` di level model. Tidak perlu menambahkan filter `school_id` secara manual di setiap query:

```php
class Student extends Model {
    use BelongsToSchool;
    // Global scope school_id otomatis diterapkan di semua query
    // school_id otomatis diisi saat create
}
```

### Resource untuk Transformasi Output

Output API tidak langsung mengekspos atribut model mentah. Setiap resource memiliki tiga format output yang konsisten:

```php
class StudentResource extends JsonResource {
    public function toArray(Request $request): array { ... }  // Format standar
    public function toSchema(): array { ... }                 // Format schema frontend
    public function mapOptions(): array { ... }               // Format select/dropdown
}
```

### ApiResponse Trait

Semua controller menggunakan trait `ApiResponse` untuk memastikan format JSON response yang konsisten di seluruh API:

```php
// Struktur response sukses
{ "status": "success", "message": "...", "data": { ... } }

// Struktur response error
{ "status": "error", "message": "...", "data": null }
```

### Enkripsi ID

Seluruh primary key yang dikirim ke client dienkripsi menggunakan `encrypt()` bawaan Laravel. Sisi client tidak pernah menerima ID numerik asli. Dekripsi dilakukan melalui helper `safeDecrypt()` sebelum diproses di server.

## Alur Data

1. **Request Masuk** → Middleware memverifikasi token Sanctum
2. **Validasi** → Form Request menolak input tidak valid sebelum menyentuh controller
3. **Transformasi Input** → DTO mengubah raw request menjadi objek terstruktur
4. **Business Logic** → Service memproses data, memanggil model
5. **Akses Database** → Eloquent query dengan global scope (school_id, soft delete)
6. **Transformasi Output** → Resource mengubah model menjadi format JSON untuk frontend
7. **Response Dikirim** → Format JSON standar melalui ApiResponse trait

## Generasi Kode dengan Artisan

Proyek menyediakan Artisan command kustom untuk scaffolding kode yang konsisten. Generator yang tersedia:

- `make:crud-controller {name}`: Generate Controller lengkap dengan Service, Resource, dan Request
- `make:service {name}`: Generate Service class
- `make:dto {name}`: Generate Data Transfer Object
- `make:collection {name}`: Generate Resource Collection
- `make:swagger {name}`: Generate anotasi Swagger lengkap
- `make:swagger-collection {name}`: Generate anotasi Swagger untuk collection
- `make:swagger-request {name}`: Generate anotasi Swagger untuk request
- `make:swagger-resource {name}`: Generate anotasi Swagger untuk resource

Untuk generate kode baru:

```bash
php artisan make:crud-controller Master/StudentController
```

## Menambahkan Domain/Fitur Baru

1. Generate Controller dan skeleton terkait:

```bash
php artisan make:crud-controller Master/NamaController
```

2. Buat DTO di `app/DataTransferObjects/[Domain]/`:

```bash
php artisan make:dto [Domain]/NamaData
```

3. Buat Service di `app/Services/[Domain]/` dan extend `HasCommonFunction`

4. Buat Model di `app/Models/[Domain]/` dan tambahkan trait `BelongsToSchool` jika diperlukan

5. Generate Swagger untuk dokumentasi otomatis:

```bash
php artisan make:swagger [Domain]/Nama
```

6. Daftarkan route di `routes/api.php` dalam group `auth:sanctum`

## Domain API

### Auth
Menangani siklus hidup autentikasi pengguna. Endpoint publik (tidak memerlukan token):

| Method | Endpoint | Deskripsi |
|---|---|---|
| POST | `/signin` | Login dan mendapatkan token |
| POST | `/register` | Registrasi akun baru |
| POST | `/forgot-password` | Kirim email reset password |
| POST | `/reset-password/{id}` | Reset password dengan token |

Endpoint terautentikasi:

| Method | Endpoint | Deskripsi |
|---|---|---|
| GET | `/signout` | Logout dan hapus token |
| GET | `/session` | Data user yang sedang login |
| GET | `/access` | Daftar menu yang dapat diakses |

### Core
Mengelola data master infrastruktur sistem: wilayah (Provinsi → Kota → Kecamatan → Kelurahan), Sekolah, Role, Menu, Feature, dan Access Menu (kontrol akses berbasis role).

### Academic
Mengelola data akademik: Tahun Ajaran, Kelas, Kurikulum, Semester, Mata Pelajaran Kurikulum, dan Periode pembelajaran.

### Master
Mengelola data master operasional: Siswa (lengkap dengan upload dokumen), Mata Pelajaran, Divisi, Jurusan, Agama, Gedung, Lantai, dan Ruangan.

## Konfigurasi Environment

```bash
# Framework
APP_ENV=local|staging|production
APP_DEBUG=true|false

# Database (default SQLite, siap migrasi ke MySQL)
DB_CONNECTION=sqlite|mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=hyperedu

# CORS — daftarkan domain frontend
ALLOWED_ORIGINS=http://localhost:3000,https://app.hyperedu.id

# Mail
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
```

## Pertimbangan Performa

- **Laravel Octane**: Server persistent menghilangkan overhead bootstrap Laravel di setiap request
- **Eager Loading**: Relasi Eloquent dimuat eksplisit di Service untuk menghindari N+1 query
- **Pagination**: Semua endpoint list menggunakan `PaginationRequest` dengan parameter `length`, `page`, `search`, dan `sort`
- **Query Optimization**: Global scope `BelongsToSchool` memastikan query selalu terfilter tanpa overhead tambahan

## Strategi Testing

_(Catatan: Detail implementasi testing akan dikembangkan sesuai kebutuhan proyek)_

- Unit test untuk Service dan helper functions
- Feature test untuk endpoint API (menggunakan database SQLite in-memory)
- Validasi request menggunakan Form Request testing
- Jalankan test suite:

```bash
php artisan test
# atau
composer test
```

## Deployment

Aplikasi dapat di-deploy ke server PHP standar atau menggunakan Docker. Setup minimal:

```bash
# Install dependencies
composer install --optimize-autoloader --no-dev

# Setup environment
cp .env.example .env
php artisan key:generate

# Database
php artisan migrate --force

# Cache production
php artisan config:cache
php artisan route:cache
php artisan view:cache

# Jalankan server (standard)
composer run server

# Jalankan dengan Octane (performa tinggi)
composer run server:octane
```


