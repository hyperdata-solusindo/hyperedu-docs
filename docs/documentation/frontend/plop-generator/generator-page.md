---
title: "Generator: Feature Page"
sidebar_label: "Generator: Feature Page"
---

# Generator: Feature Page

## Gambaran Umum

**Feature Page** adalah generator yang digunakan untuk membuat halaman (page) baru dalam struktur fitur yang sudah ada. Generator ini menghasilkan halaman dengan tiga pilihan jenis: Blank (kosong), Create (tambah data), dan Edit (ubah data). Halaman yang dihasilkan sudah terintegrasi dengan Redux dan menggunakan komponen-komponen yang sudah ada dalam fitur.

Generator ini cocok digunakan ketika Anda sudah memiliki struktur fitur dan hanya perlu menambahkan halaman baru.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"feature:page"** dari menu.

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

Pilih apakah halaman blank akan menyertakan komponen datatable. Jika **Yes**, generator akan mengasumsikan Anda sudah memiliki komponen datatable di `components/[nama-fitur]-table.tsx`.

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
│   └── [nama-fitur]-table.tsx          # Komponen datatable (diasumsikan sudah ada)
└── pages/
    └── [nama-fitur].tsx
```

#### Create Page

```
src/features/[nama-fitur]/
├── components/
│   └── [nama-fitur]-form.tsx           # Komponen form (diasumsikan sudah ada)
└── pages/
    └── [nama-fitur]-create.tsx
```

#### Edit Page

```
src/features/[nama-fitur]/
├── components/
│   └── [nama-fitur]-form.tsx           # Komponen form (diasumsikan sudah ada)
└── pages/
    └── [nama-fitur]-edit.tsx
```

### Deskripsi File-File Utama

#### 1. **pages/[nama-fitur].tsx (Blank Page - Tanpa Datatable)**

Halaman kosong yang siap dikustomisasi.

```typescript
import { useEffect } from "react";
import { use[NamaFitur] } from "../hooks/use[NamaFitur]";

export const [NamaFitur]Page = () => {
  const { data, isLoading, error, getList } = use[NamaFitur]();

  useEffect(() => {
    getList();
  }, [getList]);

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
        <p className="text-destructive">Error: {error}</p>
      </div>
    );
  }

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">{capitalizedNama}</h1>
        <button
          onClick={() => console.log("Add new")}
          className="bg-primary text-white px-4 py-2 rounded-md hover:bg-primary/90"
        >
          Add New
        </button>
      </div>

      <div className="bg-white rounded-lg border p-6">
        <p className="text-muted-foreground">
          Content for {capitalizedNama} page goes here.
        </p>
        <div className="mt-4">
          <pre className="bg-gray-50 p-4 rounded-lg text-sm">
            {JSON.stringify(data, null, 2)}
          </pre>
        </div>
      </div>
    </div>
  );
};
```

#### 2. **pages/[nama-fitur].tsx (Blank Page - Dengan Datatable)**

Halaman dengan integrasi datatable.

```typescript
import { useEffect, useState } from "react";
import { useNavigate } from "react-router";
import { toast } from "sonner";
import { [NamaFitur]Table } from "../components/[nama-fitur]-table";
import { use[NamaFitur] } from "../hooks/use[NamaFitur]";

export const [NamaFitur]Page = () => {
  const navigate = useNavigate();
  const { data, isLoading, error, getList, remove } = use[NamaFitur]();

  useEffect(() => {
    getList();
  }, [getList]);

  const handleEdit = (item: any) => {
    navigate(`/[nama]/edit/${item.id}`);
  };

  const handleDelete = async (item: any) => {
    if (confirm(`Are you sure you want to delete ${item.name}?`)) {
      try {
        await remove(item.id);
        toast.success("Data berhasil dihapus");
      } catch (error) {
        toast.error("Gagal menghapus data");
      }
    }
  };

  const handleView = (item: any) => {
    navigate(`/[nama]/view/${item.id}`);
  };

  const handleCreate = () => {
    navigate(`/[nama]/create`);
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
        <p className="text-destructive">Error: {error}</p>
      </div>
    );
  }

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">{capitalizedNama} List</h1>
        <button
          onClick={handleCreate}
          className="bg-primary text-white px-4 py-2 rounded-md hover:bg-primary/90"
        >
          + Add New
        </button>
      </div>

      <[NamaFitur]Table
        data={data || []}
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
import { useEffect } from "react";
import { useParams, useNavigate } from "react-router";
import { [NamaFitur]Form } from "../components/[nama-fitur]-form";
import { use[NamaFitur] } from "../hooks/use[NamaFitur]";

export const [NamaFitur]Edit = () => {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  const { detail, isLoading, error, getDetail } = use[NamaFitur]();

  useEffect(() => {
    if (id) {
      getDetail(id);
    }
  }, [id, getDetail]);

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
        <p className="text-destructive">Error: {error}</p>
      </div>
    );
  }

  return (
    <div className="p-6">
      <div className="mb-6">
        <h1 className="text-2xl font-bold">Edit {capitalizedNama}</h1>
        <p className="text-muted-foreground mt-1">
          Update the {nama} information below.
        </p>
      </div>

      <div className="bg-white rounded-lg border p-6">
        <[NamaFitur]Form
          initialData={detail}
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

### Step 1: Pastikan Komponen Pendukung Sudah Ada

Sebelum menjalankan generator, pastikan Anda sudah memiliki:

**Untuk Create/Edit Page:**
- `components/product-form.tsx` (dari generator `feature:form-hook`)

**Untuk Blank Page dengan Datatable:**
- `components/product-table.tsx` (dari generator `feature:datatable`)

### Step 2: Jalankan Generator untuk Blank Page

```bash
npm run generate
```

Pilih: `feature:page`

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

**File yang Dihasilkan:**
```
src/features/product/
└── pages/
    └── product.tsx
```

### Step 3: Jalankan Generator untuk Create Page

```bash
npm run generate
```

Pilih: `feature:page`

```
? Nama Entity? (cth: product, user)
→ product

? Jenis Halaman?
→ Create

? Target Directory? (default: "src/features/product")
→ src/features/product
```

**File yang Dihasilkan:**
```
src/features/product/
└── pages/
    └── product-create.tsx
```

### Step 4: Jalankan Generator untuk Edit Page

```bash
npm run generate
```

Pilih: `feature:page`

```
? Nama Entity? (cth: product, user)
→ product

? Jenis Halaman?
→ Edit

? Target Directory? (default: "src/features/product")
→ src/features/product
```

**File yang Dihasilkan:**
```
src/features/product/
└── pages/
    └── product-edit.tsx
```

### Step 5: Integrasi ke Routes

Pastikan route sudah terdaftar di `route.ts`:

```typescript
// src/features/product/route.ts
import { lazy } from "react";
import { ProductLayout } from "./layouts/product-layout";

const ProductPage = lazy(() => import("./pages/product"));
const ProductCreate = lazy(() => import("./pages/product-create"));
const ProductEdit = lazy(() => import("./pages/product-edit"));

export const productBase = "/products";

export const productRoute = [
  {
    path: productBase,
    Component: ProductLayout,
    children: [
      {
        index: true,
        Component: ProductPage,
      },
      {
        path: "create",
        Component: ProductCreate,
      },
      {
        path: "edit/:id",
        Component: ProductEdit,
      },
    ],
  },
];
```

## Kustomisasi Halaman

### 1. Menambahkan Breadcrumb

```typescript
// pages/product-create.tsx
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

      <div className="mb-6">
        <h1 className="text-2xl font-bold">Create Product</h1>
        <p className="text-muted-foreground mt-1">
          Fill in the form below to create a new product.
        </p>
      </div>

      {/* Rest of the page */}
    </div>
  );
};
```

### 2. Menambahkan Header Actions

```typescript
// pages/product.tsx (Blank Page dengan Datatable)
import { Button } from "@/components/ui/button";
import { Download, Upload, Filter } from "lucide-react";

export const ProductPage = () => {
  // ... existing code

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
          <Button onClick={handleCreate}>
            <Upload className="h-4 w-4 mr-2" />
            Add Product
          </Button>
        </div>
      </div>

      <ProductTable
        data={data || []}
        onEdit={handleEdit}
        onDelete={handleDelete}
        onView={handleView}
      />
    </div>
  );
};
```

### 3. Menambahkan Tab Navigation

```typescript
// pages/product-edit.tsx
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";

export const ProductEdit = () => {
  const { id } = useParams();
  // ... existing code

  return (
    <div className="p-6">
      <div className="mb-6">
        <h1 className="text-2xl font-bold">Edit Product</h1>
        <p className="text-muted-foreground mt-1">
          Update the product information below.
        </p>
      </div>

      <Tabs defaultValue="general" className="w-full">
        <TabsList className="mb-4">
          <TabsTrigger value="general">General Information</TabsTrigger>
          <TabsTrigger value="inventory">Inventory</TabsTrigger>
          <TabsTrigger value="images">Images</TabsTrigger>
          <TabsTrigger value="seo">SEO</TabsTrigger>
        </TabsList>

        <TabsContent value="general">
          <div className="bg-white rounded-lg border p-6">
            <ProductForm
              initialData={detail}
              id={id}
              onSuccess={handleSuccess}
              onCancel={handleCancel}
            />
          </div>
        </TabsContent>

        <TabsContent value="inventory">
          <div className="bg-white rounded-lg border p-6">
            {/* Inventory form content */}
          </div>
        </TabsContent>

        <TabsContent value="images">
          <div className="bg-white rounded-lg border p-6">
            {/* Images upload content */}
          </div>
        </TabsContent>

        <TabsContent value="seo">
          <div className="bg-white rounded-lg border p-6">
            {/* SEO form content */}
          </div>
        </TabsContent>
      </Tabs>
    </div>
  );
};
```

### 4. Menambahkan Konfirmasi Sebelum Navigasi (Create/Edit)

```typescript
// pages/product-create.tsx
import { useState } from "react";
import { useNavigate } from "react-router";
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
} from "@/components/ui/alert-dialog";

export const ProductCreate = () => {
  const navigate = useNavigate();
  const [showConfirm, setShowConfirm] = useState(false);
  const [hasChanges, setHasChanges] = useState(false);

  const handleCancel = () => {
    if (hasChanges) {
      setShowConfirm(true);
    } else {
      navigate("/products");
    }
  };

  const handleSuccess = () => {
    navigate("/products");
  };

  return (
    <>
      <div className="p-6">
        {/* Page content */}
        <ProductForm
          onSuccess={handleSuccess}
          onCancel={handleCancel}
          onDirtyChange={setHasChanges}
        />
      </div>

      <AlertDialog open={showConfirm} onOpenChange={setShowConfirm}>
        <AlertDialogContent>
          <AlertDialogHeader>
            <AlertDialogTitle>Unsaved Changes</AlertDialogTitle>
            <AlertDialogDescription>
              You have unsaved changes. Are you sure you want to leave?
            </AlertDialogDescription>
          </AlertDialogHeader>
          <AlertDialogFooter>
            <AlertDialogCancel>Continue Editing</AlertDialogCancel>
            <AlertDialogAction onClick={() => navigate("/products")}>
              Discard Changes
            </AlertDialogAction>
          </AlertDialogFooter>
        </AlertDialogContent>
      </AlertDialog>
    </>
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
        <p className="text-sm text-muted-foreground">{error}</p>
        <Button variant="outline" onClick={getList} className="mt-4">
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

Pastikan route sudah ditambahkan ke file routing utama:

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
→ Pilih: feature:page
→ Jenis: Blank
→ Nama: product

# Generate create page
npm run generate
→ Pilih: feature:page
→ Jenis: Create
→ Nama: product

# Generate edit page
npm run generate
→ Pilih: feature:page
→ Jenis: Edit
→ Nama: product
```

## Lihat Juga

- [Generator: Feature](./generator-feature.md)
- [Generator: Feature Form Hook](./generator-form-hook.md)
- [Generator: Feature Datatable](./generator-datatable.md)
- [React Router Documentation](https://reactrouter.com/)
```