---
title: "TanStack + Feature: Page"
sidebar_label: "TanStack + Feature: Page"
---

# Generator: TanStack + Feature: Page

## Gambaran Umum

**TanStack + Feature: Page** adalah generator yang digunakan untuk membuat halaman (page) baru dengan integrasi **TanStack Query**. Generator ini menghasilkan halaman yang siap pakai dengan struktur yang konsisten, mendukung tiga jenis halaman: Blank (kosong), Create (tambah data), dan Edit (ubah data).

Generator ini cocok digunakan ketika Anda sudah memiliki struktur fitur dan hanya perlu menambahkan halaman baru.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"TanStack + feature:page"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi halaman yang akan dibuat.

## Tahap-Tahap Pertanyaan

### Tahap 1: Konfigurasi Dasar

```
? Nama Entity? (cth: product, user)
```

Masukkan nama entity dalam format:

- **lowercase**: `product`, `user`, `category`
- **lowercase dengan hyphen**: `user-profile`, `product-category`

**Contoh:**
```
product
```

```
? Jenis Halaman?
❯ Blank
  Create
  Edit
```

Pilih jenis halaman yang akan dibuat:

| Jenis | Deskripsi | Penggunaan |
|-------|-----------|------------|
| **Blank** | Halaman kosong tanpa form | Dashboard, list view, landing page |
| **Create** | Halaman dengan form untuk menambah data | Form tambah data baru |
| **Edit** | Halaman dengan form untuk mengubah data | Form edit data yang sudah ada |

```
? Target Directory? (default: "src/features/product")
```

Direktori target tempat file akan dibuat. Default akan mengarah ke folder fitur sesuai nama yang dimasukkan.

**Contoh:**
```
src/features/product
```

### Tahap 2: Konfigurasi Tambahan (Khusus Blank Page)

Jika memilih **Blank**, akan ditanyakan:

```
? Apakah ingin menyertakan konfigurasi Datatable? (Y/n)
```

Pilih apakah halaman blank akan menyertakan komponen datatable.

## Output File yang Dihasilkan

### Struktur Folder

#### Blank Page (tanpa datatable)

```
src/features/[nama-fitur]/
└── pages/
    └── [nama-fitur].tsx
```

#### Blank Page (dengan datatable)

```
src/features/[nama-fitur]/
├── components/
│   └── [nama-fitur]-table.tsx          # Komponen datatable (jika sudah ada)
└── pages/
    └── [nama-fitur].tsx
```

#### Create Page

```
src/features/[nama-fitur]/
├── components/
│   └── [nama-fitur]-form.tsx           # Komponen form (jika sudah ada)
└── pages/
    └── [nama-fitur]-create.tsx
```

#### Edit Page

```
src/features/[nama-fitur]/
├── components/
│   └── [nama-fitur]-form.tsx           # Komponen form (jika sudah ada)
└── pages/
    └── [nama-fitur]-edit.tsx
```

### Deskripsi File-File Utama

#### 1. **pages/[nama-fitur].tsx (Blank Page - Tanpa Datatable)**

Halaman kosong yang siap dikustomisasi.

```typescript
import { useQuery } from "@tanstack/react-query";
import { [nama]QueryKeys } from "../types/query-keys";
import { getData } from "../api/get[NamaFitur]";

export const [NamaFitur]Page = () => {
  const { data, isLoading, error } = useQuery({
    queryKey: [nama]QueryKeys.lists(),
    queryFn: getData,
  });

  if (isLoading) {
    return (
      <div className="flex items-center justify-center h-64">
        <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-primary"></div>
      </div>
    );
  }

  if (error) {
    return (
      <div className="flex items-center justify-center h-64">
        <p className="text-destructive">Error loading data: {error.message}</p>
      </div>
    );
  }

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">{capitalizedNama} Management</h1>
      </div>

      <div className="bg-white rounded-lg border p-6">
        <p className="text-muted-foreground">
          Content for {capitalizedNama} page goes here.
        </p>
      </div>
    </div>
  );
};
```

#### 2. **pages/[nama-fitur].tsx (Blank Page - Dengan Datatable)**

Halaman dengan integrasi datatable.

```typescript
import { useQuery } from "@tanstack/react-query";
import { useNavigate } from "react-router";
import { toast } from "sonner";
import { [NamaFitur]Table } from "../components/[nama-fitur]-table";
import { [nama]QueryKeys } from "../types/query-keys";
import { getPaginatedData, deleteItem } from "../api/get[NamaFitur]";

export const [NamaFitur]Page = () => {
  const navigate = useNavigate();
  const [page, setPage] = React.useState(1);
  const [pageSize, setPageSize] = React.useState(10);

  const { data, isLoading, refetch } = useQuery({
    queryKey: [nama]QueryKeys.lists(),
    queryFn: getPaginatedData,
  });

  const handleEdit = (item: any) => {
    navigate(`/[nama]/edit/${item.id}`);
  };

  const handleDelete = async (item: any) => {
    try {
      await deleteItem(item.id);
      toast.success("Data berhasil dihapus");
      refetch();
    } catch (error) {
      toast.error("Gagal menghapus data");
    }
  };

  const handleView = (item: any) => {
    navigate(`/[nama]/view/${item.id}`);
  };

  const handleCreate = () => {
    navigate(`/[nama]/create`);
  };

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">{capitalizedNama} List</h1>
        <button
          onClick={handleCreate}
          className="bg-primary text-white px-4 py-2 rounded-md hover:bg-primary/90 transition-colors"
        >
          + Add New
        </button>
      </div>

      <[NamaFitur]Table
        data={data?.data || []}
        isLoading={isLoading}
        onEdit={handleEdit}
        onDelete={handleDelete}
        onView={handleView}
      />
    </div>
  );
};
```

#### 3. **pages/[nama-fitur]-create.tsx (Create Page)**

Halaman untuk menambah data baru.

```typescript
import { useNavigate } from "react-router";
import { [NamaFitur]Form } from "../components/[nama-fitur]-form";

export const [NamaFitur]Create = () => {
  const navigate = useNavigate();

  const handleSuccess = () => {
    navigate("/[nama]");
  };

  const handleCancel = () => {
    navigate("/[nama]");
  };

  return (
    <div className="p-6">
      <div className="mb-6">
        <h1 className="text-2xl font-bold">Create {capitalizedNama}</h1>
        <p className="text-muted-foreground mt-1">
          Fill in the form below to create a new {nama}.
        </p>
      </div>

      <div className="bg-white rounded-lg border p-6">
        <[NamaFitur]Form
          onSuccess={handleSuccess}
          onCancel={handleCancel}
        />
      </div>
    </div>
  );
};
```

#### 4. **pages/[nama-fitur]-edit.tsx (Edit Page)**

Halaman untuk mengubah data yang sudah ada.

```typescript
import { useParams, useNavigate } from "react-router";
import { useQuery } from "@tanstack/react-query";
import { [NamaFitur]Form } from "../components/[nama-fitur]-form";
import { [nama]QueryKeys } from "../types/query-keys";
import { getDetail } from "../api/get[NamaFitur]";

export const [NamaFitur]Edit = () => {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();

  const { data, isLoading, error } = useQuery({
    queryKey: [nama]QueryKeys.detail(id!),
    queryFn: () => getDetail(id!),
    enabled: !!id,
  });

  const handleSuccess = () => {
    navigate("/[nama]");
  };

  const handleCancel = () => {
    navigate("/[nama]");
  };

  if (isLoading) {
    return (
      <div className="flex items-center justify-center h-64">
        <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-primary"></div>
      </div>
    );
  }

  if (error) {
    return (
      <div className="flex items-center justify-center h-64">
        <p className="text-destructive">Error loading data: {error.message}</p>
      </div>
    );
  }

  return (
    <div className="p-6">
      <div className="mb-6">
        <h1 className="text-2xl font-bold">Edit {capitalizedNama}</h1>
        <p className="text-muted-foreground mt-1">
          Update the information below.
        </p>
      </div>

      <div className="bg-white rounded-lg border p-6">
        <[NamaFitur]Form
          initialData={data}
          id={id}
          onSuccess={handleSuccess}
          onCancel={handleCancel}
        />
      </div>
    </div>
  );
};
```

## Contoh Lengkap: Membuat Page untuk "Product"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `TanStack + feature:page`

### Step 2: Jawab Pertanyaan untuk Blank Page

```
? Nama Entity? (cth: product, user)
→ product

? Jenis Halaman?
→ Blank

? Target Directory? (default: "src/features/product")
→ src/features/product

? Apakah ingin menyertakan konfigurasi Datatable?
→ Yes
```

### Step 3: File yang Dihasilkan

```
src/features/product/
├── components/
│   └── product-table.tsx (jika sudah ada, akan digunakan)
└── pages/
    └── product.tsx
```

### Step 4: Jawab Pertanyaan untuk Create Page

```
? Nama Entity? (cth: product, user)
→ product

? Jenis Halaman?
→ Create

? Target Directory? (default: "src/features/product")
→ src/features/product
```

### Step 5: File yang Dihasilkan

```
src/features/product/
├── components/
│   └── product-form.tsx (jika sudah ada, akan digunakan)
└── pages/
    └── product-create.tsx
```

### Step 6: Jawab Pertanyaan untuk Edit Page

```
? Nama Entity? (cth: product, user)
→ product

? Jenis Halaman?
→ Edit

? Target Directory? (default: "src/features/product")
→ src/features/product
```

### Step 7: File yang Dihasilkan

```
src/features/product/
├── components/
│   └── product-form.tsx (jika sudah ada, akan digunakan)
└── pages/
    └── product-edit.tsx
```

## Kustomisasi Halaman

### Menambahkan Breadcrumb

```typescript
import { Breadcrumb, BreadcrumbItem, BreadcrumbLink, BreadcrumbList, BreadcrumbPage, BreadcrumbSeparator } from "@/components/ui/breadcrumb";

export const ProductCreate = () => {
  return (
    <div className="p-6">
      <Breadcrumb className="mb-4">
        <BreadcrumbList>
          <BreadcrumbItem>
            <BreadcrumbLink href="/">Home</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbLink href="/products">Products</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbPage>Create</BreadcrumbPage>
          </BreadcrumbItem>
        </BreadcrumbList>
      </Breadcrumb>

      <h1 className="text-2xl font-bold mb-6">Create Product</h1>
      {/* Rest of the page */}
    </div>
  );
};
```

### Menambahkan Header Actions

```typescript
import { Button } from "@/components/ui/button";
import { Download, Upload, Filter } from "lucide-react";

export const ProductPage = () => {
  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">Products</h1>
        <div className="flex gap-2">
          <Button variant="outline" size="sm">
            <Filter className="h-4 w-4 mr-2" />
            Filter
          </Button>
          <Button variant="outline" size="sm">
            <Download className="h-4 w-4 mr-2" />
            Export
          </Button>
          <Button>
            <Upload className="h-4 w-4 mr-2" />
            Import
          </Button>
        </div>
      </div>
      {/* Rest of the page */}
    </div>
  );
};
```

### Menambahkan Tab Navigation

```typescript
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";

export const ProductEdit = () => {
  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-6">Edit Product</h1>

      <Tabs defaultValue="general" className="w-full">
        <TabsList className="mb-4">
          <TabsTrigger value="general">General Information</TabsTrigger>
          <TabsTrigger value="inventory">Inventory</TabsTrigger>
          <TabsTrigger value="images">Images</TabsTrigger>
          <TabsTrigger value="seo">SEO</TabsTrigger>
        </TabsList>

        <TabsContent value="general">
          <ProductForm
            initialData={data}
            id={id}
            onSuccess={handleSuccess}
            onCancel={handleCancel}
          />
        </TabsContent>

        <TabsContent value="inventory">
          {/* Inventory content */}
        </TabsContent>

        <TabsContent value="images">
          {/* Images content */}
        </TabsContent>

        <TabsContent value="seo">
          {/* SEO content */}
        </TabsContent>
      </Tabs>
    </div>
  );
};
```

## Best Practices

### 1. Loading State

```typescript
// ✅ Baik: Tampilkan skeleton atau spinner saat loading
if (isLoading) {
  return (
    <div className="flex items-center justify-center h-64">
      <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-primary"></div>
    </div>
  );
}

// ❌ Hindari: Tidak menampilkan loading state
```

### 2. Error Handling

```typescript
// ✅ Baik: Tampilkan pesan error yang jelas
if (error) {
  return (
    <div className="flex items-center justify-center h-64">
      <div className="text-center">
        <p className="text-destructive mb-2">Failed to load data</p>
        <p className="text-sm text-muted-foreground">{error.message}</p>
        <Button variant="outline" onClick={() => refetch()} className="mt-4">
          Try Again
        </Button>
      </div>
    </div>
  );
}

// ❌ Hindari: Hanya console.log error
```

### 3. Navigation After Submit

```typescript
// ✅ Baik: Navigasi setelah sukses
const handleSuccess = () => {
  toast.success("Data saved successfully");
  navigate("/products");
};

// ❌ Hindari: Tidak ada feedback navigasi
```

### 4. Page Title

```typescript
// ✅ Baik: Gunakan useEffect untuk update document title
useEffect(() => {
  document.title = `Create Product - HyperEdu`;
  return () => {
    document.title = "HyperEdu";
  };
}, []);

// ❌ Hindari: Membiarkan title default
```

## Troubleshooting

### Halaman Tidak Terdaftar di Routes

Pastikan Anda telah menambahkan route ke file routing utama:

```typescript
// src/route.ts
import { productRoute } from "./features/product";

const router = createBrowserRouter([
  {
    Component: MainLayout,
    children: [
      ...productRoute, // ← Tambahkan ini
    ],
  },
]);
```

### Data Tidak Muncul di Edit Page

Pastikan parameter ID diambil dengan benar:

```typescript
// ✅ Benar
const { id } = useParams<{ id: string }>();

// ❌ Salah
const { id } = useParams();
```

### Form Tidak Terintegrasi

Pastikan komponen form sudah di-import dan props-nya sesuai:

```typescript
// Pastikan komponen form menerima props yang diperlukan
interface ProductFormProps {
  initialData?: ProductFormSchema;
  id?: string;
  onSuccess?: () => void;
  onCancel?: () => void;
}
```

## Command Reference

```bash
# Generate blank page
npm run generate
→ Pilih: TanStack + feature:page
→ Jenis: Blank

# Generate create page
npm run generate
→ Pilih: TanStack + feature:page
→ Jenis: Create

# Generate edit page
npm run generate
→ Pilih: TanStack + feature:page
→ Jenis: Edit
```

## Lihat Juga

- [Generator: TanStack + Feature](./tanstack-generator-feature.md)
- [Generator: TanStack + Feature: Form Hook](./tanstack-generator-form-hook.md)
- [Generator: TanStack + Feature: Datatable](./tanstack-generator-datatable.md)
- [React Router Documentation](https://reactrouter.com/)
```