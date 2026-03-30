---
sidebar_position: 1
---

# Custom Functions

## Overview

Dokumentasi ini menyediakan panduan lengkap untuk custom class, trait, dan helper function yang digunakan pada backend HyperEdu (Laravel). Komponen-komponen ini dirancang untuk dapat digunakan kembali (reusable), mudah dipelihara (maintainable), dan mengikuti konvensi serta praktik terbaik proyek.

## Daftar Custom Function

1. [ApiResponse](#apiresponse)
2. [BelongsToSchool](#belongstoschool)
3. [HasCommonFunction](#hascommonfunction)
4. [HasPaginationReact](#haspaginationreact)
5. [PaginationRequest](#paginationrequest)
6. [Function Helpers](#function-helpers)

---

## ApiResponse

**Lokasi:** `App\Helpers\Response\ApiResponse`  
**Tipe:** Trait  
**Digunakan di:** Controller

### Deskripsi
Trait untuk memformat response JSON API secara konsisten di seluruh aplikasi. Menyediakan tiga metode standar yang merepresentasikan state sukses, data mentah, dan error.

### Metode

| Metode | Return | Deskripsi |
|--------|--------|-----------|
| `successResponse($data, $message, $code)` | `JsonResponse` | Response sukses dengan wrapper `status`, `message`, dan `data` |
| `dataResponse($data, $code)` | `JsonResponse` | Response data mentah tanpa wrapper tambahan |
| `errorResponse($message, $code)` | `JsonResponse` | Response error dengan wrapper `status`, `message`, dan `data: null` |

### Parameter `successResponse`

| Parameter | Tipe | Default | Deskripsi |
|-----------|------|---------|-----------|
| `$data` | `mixed` | — | Data yang dikembalikan ke client |
| `$message` | `string\|null` | `null` | Pesan keterangan |
| `$code` | `int` | `200` | HTTP status code |

### Parameter `errorResponse`

| Parameter | Tipe | Default | Deskripsi |
|-----------|------|---------|-----------|
| `$message` | `string` | — | Pesan error |
| `$code` | `int` | — | HTTP status code (mis. `422`, `404`, `500`) |

### Fitur
- Format response JSON yang seragam di semua endpoint
- Pemisahan antara response sukses (`successResponse`) dan data mentah (`dataResponse`)
- Struktur error yang konsisten

### Contoh Penggunaan
```php
use App\Helpers\Response\ApiResponse;

class ProductController extends Controller
{
    use ApiResponse;

    public function index()
    {
        $products = $this->productService->all();
        return $this->successResponse($products, 'Data berhasil diambil');
    }

    public function store(Request $request)
    {
        // Digunakan untuk Resource Collection hasil getPaginatedData()
        $data = $this->productService->getPaginatedData(PaginationRequest::make($request));
        return $this->dataResponse($data);
    }

    public function destroy($id)
    {
        return $this->errorResponse('Data tidak ditemukan', 404);
    }
}
```

:::tip Kapan pakai `dataResponse` vs `successResponse`?
Gunakan `dataResponse` ketika data sudah berbentuk Laravel Resource Collection (hasil `getPaginatedData`) karena koleksi tersebut sudah membawa meta paginasi sendiri. Gunakan `successResponse` untuk response CRUD biasa.
:::

---

## BelongsToSchool

**Lokasi:** `App\Helpers\Models\BelongsToSchool`  
**Tipe:** Trait  
**Digunakan di:** Model Eloquent

### Deskripsi
Trait untuk mengimplementasikan **multi-tenancy berbasis sekolah**. Saat di-attach ke sebuah Model, trait ini secara otomatis memfilter semua query dan mengisi `school_id` pada setiap pembuatan record baru.

### Fitur
- Otomatis menambahkan `WHERE school_id = ?` ke semua query (SELECT, UPDATE, DELETE) melalui Global Scope
- Otomatis mengisi kolom `school_id` saat membuat record baru via `creating` event
- Tidak memerlukan konfigurasi tambahan setelah di-attach ke model

### Contoh Penggunaan
```php
use App\Helpers\Models\BelongsToSchool;

class Student extends Model
{
    use BelongsToSchool;

    // Pastikan tabel memiliki kolom school_id
}
```

```php
// Otomatis hanya mengembalikan data milik sekolah aktif
Student::all();

// school_id otomatis terisi dari currentSchoolId()
Student::create(['name' => 'Budi', 'class' => '10A']);

// Query tambahan, school_id tetap difilter di background
Student::where('class', '10A')->get();
```

### Menonaktifkan Global Scope

Untuk keperluan khusus seperti query lintas sekolah oleh Super Admin:

```php
// Abaikan scope untuk satu query
Student::withoutGlobalScope('school_filter')->get();

// Abaikan semua global scope
Student::withoutGlobalScopes()->get();
```

:::caution
Fungsi `currentSchoolId()` harus mengembalikan nilai yang valid agar scope berjalan. Jika nilai kosong (`empty`), global scope tidak akan diterapkan.
:::

---

## HasCommonFunction

**Lokasi:** `App\Helpers\Models\HasCommonFunction`  
**Tipe:** Trait  
**Digunakan di:** Service class

### Deskripsi
Trait yang menyediakan operasi CRUD standar dan utilitas query siap pakai. Kelas Service yang menggunakan trait ini cukup mendefinisikan properti `$modelClass`.

### Prasyarat

```php
protected string $modelClass = YourModel::class;
```

### Metode

| Metode | Return | Deskripsi |
|--------|--------|-----------|
| `findQuery()` | `Builder` | Base query untuk operasi find. Override untuk tambahkan eager loading |
| `find($id)` | `Model` | Cari satu record by primary key. Throws `ModelNotFoundException` |
| `create(array $data)` | `Model` | Buat record baru, otomatis isi `created_by` dan `created_date` |
| `insert(array $data)` | `bool` | Mass insert tanpa Eloquent events |
| `update($id, array $data)` | `Model` | Update record, otomatis isi `updated_by` dan `updated_date` |
| `delete($id)` | `bool` | Hapus record by primary key |
| `all()` | `Collection` | Ambil semua record |
| `findBy(array $criteria)` | `Collection` | Cari banyak record berdasarkan kondisi `where` |
| `firstBy(array $criteria)` | `Model\|null` | Ambil satu record pertama yang cocok |
| `exists(array $criteria)` | `bool` | Cek apakah ada record yang memenuhi kriteria |
| `count(array $criteria)` | `int` | Hitung jumlah record |
| `firstOrCreate(array $attributes, array $values)` | `Model` | Cari record atau buat baru jika tidak ada |
| `upsert($data, $keys, $fillable)` | `bool` | Mass insert/update dalam satu transaksi |
| `getSelectData(PaginationRequest $pr)` | `Collection` | Ambil data untuk keperluan dropdown/select |

### Fitur
- Otomatis mengisi audit columns (`created_by`, `created_date`, `updated_by`, `updated_date`)
- `upsert` berjalan dalam database transaction
- Semua query `find*` menggunakan `findQuery()` — cukup override satu metode untuk eager loading global

### Contoh Penggunaan
```php
use App\Helpers\Models\HasCommonFunction;

class ProductService
{
    use HasCommonFunction;

    protected string $modelClass = Product::class;

    // Override untuk menambahkan eager loading
    public function findQuery()
    {
        return $this->modelClass::query()->with(['category', 'supplier']);
    }
}
```

```php
$service = new ProductService();

$service->find(1);
$service->create(['name' => 'Produk A', 'price' => 50000]);
$service->update(1, ['price' => 55000]);
$service->delete(1);
$service->findBy(['status' => 'active']);
$service->exists(['sku' => 'SKU-001']);
$service->upsert($rows, ['sku'], ['name', 'price']);
```

:::note
Kolom `created_by`, `created_date`, `updated_by`, dan `updated_date` harus tersedia di tabel agar tidak terjadi error saat `create` atau `update`.
:::

---

## HasPaginationReact

**Lokasi:** `App\Helpers\Pagination\HasPaginationReact`  
**Tipe:** Trait  
**Digunakan di:** Service class

### Deskripsi
Trait yang menyediakan fungsi paginasi yang dioptimalkan untuk dikonsumsi oleh frontend React (TanStack Query). Mendukung pencarian full-text, pengurutan dinamis multi-kolom, dan infinite scroll via intersection observer.

### Prasyarat

```php
protected string $modelClass = YourModel::class;
protected string $resourceClass = YourResource::class;
protected array $columnSearchable = ['column1', 'column2'];
protected array $sortingColumns = ['id' => 'id', 'name' => 'name'];
protected array $defaultSortingColumn = ['id' => 'created_at', 'desc' => true];
```

### Metode

| Metode | Return | Deskripsi |
|--------|--------|-----------|
| `getPaginatedData(PaginationRequest $pr)` | `ResourceCollection` | Eksekusi query dengan search, sort, dan paginasi |
| `paginationQuery(Request $request)` | `Builder` | Base query untuk paginasi. Override untuk eager loading |
| `filterColumn($key, $callback)` | `void` | Daftarkan custom filter dengan callback |
| `sortingColumn($key, $callback)` | `void` | Daftarkan custom sort dengan callback |

### Fitur
- Pencarian full-text case-insensitive (`ilike`) pada kolom yang didefinisikan
- Pengurutan dinamis multi-kolom berdasarkan parameter dari frontend
- Dukungan custom filter dan sort via callback untuk relasi atau logika kompleks
- Response otomatis menyertakan meta `sorts` untuk sinkronisasi state di frontend

### Contoh Penggunaan
```php
use App\Helpers\Pagination\HasPaginationReact;
use App\Helpers\Pagination\PaginationRequest;

class ProductService
{
    use HasPaginationReact;

    protected string $modelClass = Product::class;
    protected string $resourceClass = ProductResource::class;
    protected array $columnSearchable = ['name', 'sku'];
    protected array $sortingColumns = [
        'name'  => 'name',
        'price' => 'price',
        'date'  => 'created_at',
    ];
    protected array $defaultSortingColumn = ['id' => 'created_at', 'desc' => true];

    public function paginationQuery(Request $request)
    {
        return $this->modelClass::query()->with(['category', 'supplier']);
    }
}
```

```php
// Di Controller
public function index(Request $request, ProductService $service)
{
    $data = $service->getPaginatedData(PaginationRequest::make($request));
    return $this->dataResponse($data);
}
```

### Custom Filter & Sort

Gunakan callback untuk pencarian atau pengurutan yang melibatkan relasi:

```php
// Custom filter dengan relasi
$service->filterColumn('category_name', function ($query, $search, $method) {
    $query->$method(function ($q) use ($search) {
        $q->whereHas('category', fn($q) => $q->whereRaw("name ilike '%{$search}%'"));
    });
});

// Custom sort dengan join
$service->sortingColumn('category_name', function ($query, $dir) {
    $query->join('categories', 'products.category_id', '=', 'categories.id')
          ->orderBy('categories.name', $dir);
});
```

### Format Response
```json
{
  "data": [ "..." ],
  "links": { "first": "...", "last": "...", "prev": null, "next": "..." },
  "meta": {
    "current_page": 1,
    "total": 50,
    "per_page": 10,
    "sorts": [{ "id": "name", "desc": false, "dir": "asc" }]
  }
}
```

:::note
Pencarian menggunakan operator `ilike` yang spesifik untuk **PostgreSQL**. Jika menggunakan database lain, override metode `getPaginatedData` dan sesuaikan operatornya.
:::

---

## PaginationRequest

**Lokasi:** `App\Helpers\Pagination\PaginationRequest`  
**Tipe:** Class

### Deskripsi
Class untuk mem-parsing parameter paginasi dari HTTP Request menjadi objek terstruktur yang siap digunakan oleh `HasPaginationReact` dan `HasCommonFunction`.

### Parameter Request yang Dibaca

| Parameter | Tipe | Default | Deskripsi |
|-----------|------|---------|-----------|
| `search` | `string` | `""` | Kata kunci pencarian |
| `length` | `int` | `10` | Jumlah item per halaman |
| `page` | `int` | — | Nomor halaman saat ini |
| `sort` | `string` | — | Format: `field:asc,field2:desc` |

### Getter

| Metode | Return | Deskripsi |
|--------|--------|-----------|
| `getSearch()` | `string` | Kata kunci (lowercase, trimmed) |
| `getLength()` | `int` | Jumlah item per halaman |
| `getPage()` | `int` | Nomor halaman aktif |
| `getOffset()` | `int` | Offset untuk query limit/offset |
| `getSorts()` | `array` | Array objek sort `{id, desc, dir}` |
| `getRequest()` | `Request` | Instance request Laravel asli |

### Fitur
- Parsing `sort` dari format string `field:direction` menjadi array objek
- `getSearch()` otomatis lowercase dan trim
- `getOffset()` dihitung dari `(page * length) - length`

### Contoh Penggunaan

```php
$pr = PaginationRequest::make($request);

$pr->getSearch();  // "buku"
$pr->getLength();  // 15
$pr->getPage();    // 2
$pr->getOffset();  // 15
$pr->getSorts();   // [{ id: "name", desc: false, dir: "asc" }]
```

### Contoh Request dari Frontend

```
GET /api/products?search=buku&length=15&page=2&sort=name:asc,price:desc
```

:::note
`getOffset()` menghitung `(page * length) - length`. Pastikan `page` dimulai dari `1` (bukan `0`) dari sisi frontend.
:::

---

## Function Helpers

**Lokasi:** `app/helpers/function_helper.php`  
**Tipe:** Global PHP Functions  
**Dimuat otomatis:** via `composer.json` autoload files

### Deskripsi
Kumpulan fungsi global yang tersedia di seluruh aplikasi tanpa perlu import atau instantiasi.

### Daftar Fungsi

| Fungsi | Return | Deskripsi |
|--------|--------|-----------|
| `currentSchoolId()` | `int` | Mengembalikan ID sekolah yang sedang aktif |
| `safeDecrypt($value, $default)` | `mixed` | Mendekripsi nilai dengan fallback jika gagal |
| `create_schema($name)` | `bool` | Membuat PostgreSQL schema baru jika belum ada |

---

### `currentSchoolId()`

Mengembalikan ID sekolah aktif. Digunakan oleh `BelongsToSchool` untuk menerapkan multi-tenancy.

```php
$schoolId = currentSchoolId(); // int
```

:::caution Catatan Pengembangan
Implementasi `currentSchoolId()` saat ini bersifat sementara (hardcoded `return 1`). Sebelum production, fungsi ini harus diperbarui agar membaca ID sekolah dari autentikasi user yang sedang login (JWT token atau session).
:::

---

### `safeDecrypt($value, $default)`

Mendekripsi nilai terenkripsi Laravel secara aman. Jika dekripsi gagal, mengembalikan `$default` tanpa melempar exception.

| Parameter | Tipe | Default | Deskripsi |
|-----------|------|---------|-----------|
| `$value` | `mixed` | — | Nilai terenkripsi |
| `$default` | `mixed` | `null` | Nilai fallback jika dekripsi gagal |

```php
$plain = safeDecrypt($encryptedValue);

// Dengan fallback
$plain = safeDecrypt($encryptedToken, 'anonymous');

// Aman digunakan meski nilai null atau corrupt
$userId = safeDecrypt($request->token, 0);
```

---

### `create_schema($name)`

Membuat PostgreSQL schema baru jika belum ada. Digunakan untuk manajemen database multi-tenant berbasis schema.

```php
create_schema('school_1'); // CREATE SCHEMA IF NOT EXISTS school_1
```

---

## Praktik Terbaik Penggunaan

### 1. Struktur Service
- Definisikan `$modelClass` dan properti terkait di awal class
- Override `findQuery()` atau `paginationQuery()` untuk menambahkan eager loading default
- Pisahkan logika bisnis khusus ke metode tersendiri di dalam Service

### 2. Response API
- Selalu gunakan `ApiResponse` trait di Controller, jangan format JSON secara manual
- Gunakan `dataResponse` untuk Resource Collection, `successResponse` untuk CRUD biasa

### 3. Multi-Tenancy
- Pastikan semua model yang datanya bersifat per-sekolah menggunakan `BelongsToSchool`
- Gunakan `withoutGlobalScope` hanya untuk query administratif yang memang membutuhkan data lintas sekolah

### 4. Paginasi
- Selalu gunakan `PaginationRequest::make($request)` untuk parsing, jangan baca parameter request secara manual
- Definisikan `$defaultSortingColumn` agar tabel tidak tampil tanpa urutan ketika frontend tidak mengirim parameter sort

---

## Menambahkan Custom Function Baru

1. Untuk **Trait Model/Service**: buat file baru di `app/Helpers/Models/` atau `app/Helpers/Pagination/`
2. Untuk **Global Helper**: tambahkan function baru di `app/helpers/function_helper.php` dengan guard `function_exists`
3. Pastikan namespace sesuai dengan struktur folder
4. Update dokumentasi ini dengan informasi function baru

### Template Trait

```php
<?php

namespace App\Helpers\Models;

trait HasNewFeature
{
    /**
     * Deskripsi singkat metode.
     *
     * @param mixed $param Deskripsi parameter
     * @return mixed
     */
    public function newMethod($param)
    {
        // implementasi
    }
}
```

### Template Global Helper

```php
if (!function_exists('newHelperFunction')) {
    function newHelperFunction(mixed $param, mixed $default = null): mixed
    {
        // implementasi
    }
}
```
