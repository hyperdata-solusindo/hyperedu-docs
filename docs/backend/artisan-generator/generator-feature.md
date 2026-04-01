---
title: "Generator: Swagger"
sidebar_label: "Generator: Swagger"
---

# Generator: Swagger

## Gambaran Umum

**Swagger** adalah alat terotomatisasi yang dibuat untuk men-generate dokumentasi API yang terstruktur dan rapi dengan menggunakan nama model sebagai nama foldernya.

## Cara Menggunakan

### 1. Jalankan Command

```bash
php artisan make:swagger NamaModel
```

Masukkan nama model dalam format dibawah ini:
- **PascalCase**: `AcademicYear`, `SubjectCurriculum`
- **PascalCase tanpa spasi**: `Province`, `Religion`
- **Format subfolder**: `Core/District`, `Masters/Division`

**Contoh:**

```bash
php artisan make:swagger District
```

atau untuk API dengan subfolder/scheme:

```bash
php artisan make:swagger Core/District
```

Dengan menggunakan format subfolder akan menghasilkan:

- **Folder utama**: `swagger/Core/District/`
- **Nama class**: `District`
- **Parent folder**: `Core`

## Output File yang akan Dihasilkan
### Struktur Folder

```
swagger/[NamaClass]/
├── [NamaClass].php                # Dasar swagger untuk API calling
├── [NamaClass]Collection.php      # Array data untuk list
├── [NamaClass]Request.php         # Parameter untuk create
└── [NamaClass]Resource.php        # Resource untuk fetch data
```

### Deskripsi File-File Utama

#### 1. **[NamaClass].php**

Berisi semua fungsi API untuk melakukan aksi RESTful API ke Backend

```php
// Contoh: Core/District swagger
class District
{
    #[OA\Get(
        path: "/api/district",
        summary: "Datatables District",
        description: "Ambil daftar district (Datatable)",
        tags: ["District"]
    )]

    #[OA\Parameter(ref: PaginationPageParam::class)]
    #[OA\Parameter(ref: PaginationLengthParam::class)]
    #[OA\Parameter(ref: PaginationSearchParam::class)]
    #[OA\Parameter(ref: PaginationSortParam::class)]

    #[OA\Response(
        response: 200,
        description: "Successfully",
        content: new OA\JsonContent(
            allOf: [
                new OA\Schema(properties: [
                    new OA\Property(property: "data", type: "array", items: new OA\Items(ref: DistrictResource::class))
                ]),
                new OA\Schema(ref: PaginationResponse::class),
            ]
        )
    )]
    #[OA\Response(response: 401, ref: UnauthenticatedResponse::class)]
    #[OA\Response(response: 500, ref: InternalServerErrorResponse::class)]
    public function index() {}
    // -> Fetch data dengan pagination

    #[OA\Get(
        path: "/api/district/{id}",
        summary: "Find District",
        description: "Proses ambil data district",
        tags: ["District"]
    )]
    #[OA\Parameter(
        name: "id",
        in: "path",
        required: true,
        description: "ID unik dari district yang ingin dicari",
        schema: new OA\Schema(
            type: "string",
            example: "eyJpdiI6..."
        )
    )]
    #[OA\Response(
        response: 200,
        description: "Successfully",
        content: new OA\JsonContent(
            allOf: [
                new OA\Schema(ref: DistrictResource::class),
                new OA\Schema(ref: ResponseSuccess::class),
            ]
        ),
    )]
    #[OA\Response(response: 401, ref: UnauthenticatedResponse::class)]
    #[OA\Response(response: 404, ref: NotFoundResponse::class)]
    #[OA\Response(response: 500, ref: InternalServerErrorResponse::class)]
    public function show() {}
    // -> Fetch district dengan ID

    #[OA\Post(
        path: "/api/district",
        summary: "Create District",
        description: "Tambah district baru",
        tags: ["District"]
    )]
    #[OA\RequestBody(
        required: true,
        content: new OA\MediaType(
            mediaType: "multipart/form-data",
            schema: new OA\Schema(ref: DistrictRequest::class)
        )
    )]
    #[OA\Response(
        response: 201,
        description: "Successfully",
        content: new OA\JsonContent(
            allOf: [
                new OA\Schema(properties: [
                    new OA\Property(property: "data", ref: DistrictResource::class)
                ]),
                new OA\Schema(ref: ResponseSuccess::class),
            ]
        ),
    )]
    #[OA\Response(response: 422, ref: ValidationErrorResponse::class)]
    #[OA\Response(response: 401, ref: UnauthenticatedResponse::class)]
    #[OA\Response(response: 404, ref: NotFoundResponse::class)]
    #[OA\Response(response: 500, ref: InternalServerErrorResponse::class)]
    public function store() {}
    // -> Create district baru

    #[OA\Put(
        path: "/api/district/{id}",
        summary: "Update District",
        description: "Proses update data district",
        tags: ["District"]
    )]
    #[OA\Parameter(
        name: "id",
        in: "path",
        required: true,
        description: "ID unik dari district yang ingin dicari",
        schema: new OA\Schema(
            type: "string",
            example: "eyJpdiI6..."
        )
    )]
    #[OA\RequestBody(
        required: true,
        content: new OA\MediaType(
            mediaType: "multipart/form-data",
            schema: new OA\Schema(ref: DistrictRequest::class)
        )
    )]
    #[OA\Response(
        response: 200,
        description: "Successfully",
        content: new OA\JsonContent(
            allOf: [
                new OA\Schema(properties: [
                    new OA\Property(property: "data", ref: DistrictResource::class)
                ]),
                new OA\Schema(ref: ResponseSuccess::class),
            ]
        )
    )]
    #[OA\Response(response: 422, ref: ValidationErrorResponse::class)]
    #[OA\Response(response: 401, ref: UnauthenticatedResponse::class)]
    #[OA\Response(response: 404, ref: NotFoundResponse::class)]
    #[OA\Response(response: 500, ref: InternalServerErrorResponse::class)]
    public function update() {}
    // -> Update district

    #[OA\Delete(
        path: "/api/district/{id}",
        summary: "Delete District",
        description: "Proses delete data district",
        tags: ["District"]
    )]
    #[OA\Parameter(
        name: "id",
        in: "path",
        required: true,
        description: "ID unik dari district yang ingin dicari",
        schema: new OA\Schema(
            type: "string",
            example: "eyJpdiI6..."
        )
    )]
    #[OA\Response(
        response: 200,
        description: "Successfully",
        content: new OA\JsonContent(ref: SuccessResponse::class)
    )]
    #[OA\Response(response: 401, ref: UnauthenticatedResponse::class)]
    #[OA\Response(response: 404, ref: NotFoundResponse::class)]
    #[OA\Response(response: 500, ref: InternalServerErrorResponse::class)]
    public function destroy() {}
    // -> Hapus district
}
```

#### 2. **[NamaClass]Collection.php**

Bungkusan array data untuk list

```php
#[OA\Schema(
    schema: "DistrictCollection",
    title: "District Collection",
    description: "Bungkusan array data untuk list District",
    properties: [
        new OA\Property(
            property: "data",
            type: "array",
            items: new OA\Items(ref: DistrictResource::class)
        )
    ]
)]
class DistrictCollection {}
// -> Membungkus array data untuk list District
```

#### 3. **[NamaClass]Request.php**

Parameter untuk create

```php
#[OA\Schema(
    schema: "DistrictRequest",
    title: "District Request",
    description: "Parameter yang dibutuhkan untuk membuat district baru",
    required: ["city_id", "district_name"],
    properties: [
        new OA\Property(property: "city_id", type: "string", example: "eyJpdiI6..."),
        new OA\Property(property: "district_code", type: "string", maxLength: 10, example: "DST-01-001"),
        new OA\Property(property: "district_name", type: "string", maxLength: 100, example: "Lowokwaru"),
    ]
)]
class DistrictRequest {}
// -> Parameter yang dibutuhkan untuk create District
```

#### 4. **[NamaClass]Resource.php**

Fetch data dari API dengan resource

```php
#[OA\Schema(
    schema: "DistrictResource",
    title: "District Resource",
    type: "object",
    properties: [
        new OA\Property(property: "district_id", type: "string", example: "eyJpdiI6..."),
        new OA\Property(property: "city_id", type: "string", example: "eyJpdiI6..."),
        new OA\Property(property: "district_code", type: "string", maxLength: 10, example: "DST-01-001"),
        new OA\Property(property: "district_name", type: "string", maxLength: 100, example: "Lowokwaru"),
    ]
)]
class DistrictResource {}
// -> Fetch data district dengan DistrictResource
```

## Contoh Lengkap: Membuat Swagger "Core/City"

### Step 1: Jalankan Generator

```bash
php artisan make:swagger Core/City
```

### Step 2: File yang Dihasilkan

```
swagger/Core/City
├── City.php
├── CityCollection.php
├── CityRequest.php
└── CityResource.php
```

### Step 3: Cocokkan request dengan yang di DataTransferObjects/Core/CityData.php

```php
#[OA\Schema(
    schema: "CityRequest",
    title: "City Request",
    description: "Parameter yang dibutuhkan untuk membuat city baru",
    required: ["province_id", "city_name", "city_type"], // <- Isi kolom yang wajib diisi
    properties: [
        // Sesuaikan kolom-kolom dibawah dengan DTO
        new OA\Property(property: "province_id", type: "string", example: "eyJpdiI6..."),
        new OA\Property(property: "city_code", type: "string", maxLength: 10, example: "CTY-01-001"),
        new OA\Property(property: "city_name", type: "string", maxLength: 100, example: "Malang"),
        new OA\Property(property: "city_type", type: "string", maxLength: 20, example: "Smart City"),
    ]
)]
class CityRequest {}
```

### Step 4: Cocokkan resource dengan Resources/Core/DistrictResource.php

```php
#[OA\Schema(
    schema: "CityResource",
    title: "City Resource",
    type: "object",
    properties: [
        // Cocokkan dengan kolom-kolom dibawah dengan CityResource.php
        new OA\Property(property: "city_id", type: "string", example: "eyJpdiI6..."),
        new OA\Property(property: "province_id", type: "string", example: "eyJpdiI6..."),
        new OA\Property(property: "city_code", type: "string", maxLength: 10, example: "CTY-01-001"),
        new OA\Property(property: "city_name", type: "string", maxLength: 100, example: "Malang"),
        new OA\Property(property: "city_type", type: "string", maxLength: 20, example: "Smart City"),
    ]
)]
class CityResource {}
```

## Best Practices

### 1. Naming Convention

- **Folder**: Titlecase (`Core`, `Masters`)
- **Files**: PascalCase (`AcademicYear`, `SubjectCurriculum`)

### Template error

Pastikan:

- Tidak ada spasi lebih di nama field
- Nama class tidak menggunakan spasi
- Format nama konsisten (PascalCase)

## Lihat Juga

- [Arsitektur Keseluruhan](../architecture.md)