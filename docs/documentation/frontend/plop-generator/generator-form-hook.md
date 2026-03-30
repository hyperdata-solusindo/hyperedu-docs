---
title: "Generator: Feature Form Hook"
sidebar_label: "Generator: Feature Form Hook"
---

# Generator: Feature Form Hook

## Gambaran Umum

**Feature Form Hook** adalah generator yang digunakan untuk membuat custom hook dan komponen form dengan **React Hook Form** dan validasi menggunakan **Zod**. Generator ini menghasilkan form yang siap pakai dengan struktur yang konsisten, mendukung berbagai tipe input seperti text, textarea, select, dan checkbox.

Generator ini cocok digunakan ketika Anda sudah memiliki struktur fitur dan hanya perlu menambahkan form untuk create/edit data, tanpa menggunakan TanStack Query (menggunakan Redux untuk state management).

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"feature:form-hook"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi form yang akan dibuat.

## Tahap-Tahap Pertanyaan

### Tahap 1: Konfigurasi Dasar

```
? Nama Form? (cth: login, user, product)
```

Masukkan nama form dalam format:

- **lowercase**: `product`, `user`, `category`
- **lowercase dengan hyphen**: `user-profile`, `product-category`

**Contoh:**
```
product
```

```
? Target Directory? (default: "src/features/product")
```

Direktori target tempat file akan dibuat. Default akan mengarah ke folder fitur sesuai nama yang dimasukkan.

**Contoh:**
```
src/features/product
```

### Tahap 2: Konfigurasi Field

```
? Sebutkan field input (pisahkan dengan koma)? (cth: email, password, full name)
```

Masukkan daftar field yang akan digunakan dalam form. Pisahkan dengan koma tanpa spasi tambahan.

**Contoh:**
```
name, price, description, category, is_active
```

### Tahap 3: Konfigurasi Tipe Field

Untuk setiap field yang dimasukkan, akan ditanyakan tipe inputnya:

```
--- Konfigurasi Tipe Field ---

? Tipe input untuk field 'name'?
❯ Input Text
  Textarea
  Select Dropdown
  Checkbox/Switch
```

**Jenis-jenis Input:**

| Tipe | Deskripsi | Kasus Penggunaan |
|------|-----------|------------------|
| **Input Text** | Input field untuk teks biasa | nama, email, nomor, price |
| **Textarea** | Area teks multiline | deskripsi, bio, catatan |
| **Select Dropdown** | Dropdown untuk memilih opsi | category, role, status |
| **Checkbox/Switch** | Toggle boolean | is_active, is_verified |

**Contoh Flow:**

```
? Sebutkan field input (pisahkan dengan koma)?
→ name, price, description, category, is_active

--- Konfigurasi Tipe Field ---

? Tipe input untuk field 'name'?
→ Input Text

? Tipe input untuk field 'price'?
→ Input Text

? Tipe input untuk field 'description'?
→ Textarea

? Tipe input untuk field 'category'?
→ Select Dropdown

? Tipe input untuk field 'is_active'?
→ Checkbox/Switch
```

## Output File yang Dihasilkan

### Struktur Folder

```
src/features/[nama-fitur]/
├── hooks/
│   └── use[NamaFitur]Form.ts          # Custom hook untuk form
├── components/
│   └── [nama-fitur]-form.tsx          # Komponen form UI
└── types/
    └── schema.ts                      # Diperbarui dengan form schema
```

### Deskripsi File-File Utama

#### 1. **hooks/use[NamaFitur]Form.ts**

Hook untuk mengelola form dengan React Hook Form dan integrasi Redux.

```typescript
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { useAppDispatch } from "@/hooks";
import { useCallback } from "react";
import { toast } from "sonner";
import { [nama]FormSchema, type [nama]FormSchema } from "../types/schema";
import { create[NamaFitur], update[NamaFitur] } from "../slices/slice[NamaFitur]";

interface Use[NamaFitur]FormProps {
  initialData?: Partial<[nama]FormSchema>;
  id?: string;
  onSuccess?: () => void;
  onClose?: () => void;
}

export const use[NamaFitur]Form = ({
  initialData,
  id,
  onSuccess,
  onClose,
}: Use[NamaFitur]FormProps = {}) => {
  const dispatch = useAppDispatch();

  const form = useForm<[nama]FormSchema>({
    resolver: zodResolver([nama]FormSchema),
    defaultValues: initialData,
    mode: "onChange",
  });

  const [isSubmitting, setIsSubmitting] = React.useState(false);

  const onSubmit = useCallback(async (data: [nama]FormSchema) => {
    setIsSubmitting(true);
    try {
      if (id) {
        await dispatch(update[NamaFitur]({ id, data })).unwrap();
        toast.success("Data berhasil diperbarui");
      } else {
        await dispatch(create[NamaFitur](data)).unwrap();
        toast.success("Data berhasil disimpan");
      }
      onSuccess?.();
      onClose?.();
      form.reset();
    } catch (error: any) {
      toast.error(error.message || "Gagal menyimpan data");
    } finally {
      setIsSubmitting(false);
    }
  }, [dispatch, id, onSuccess, onClose, form]);

  return {
    form,
    onSubmit: form.handleSubmit(onSubmit),
    errors: form.formState.errors,
    isSubmitting,
    reset: form.reset,
  };
};
```

#### 2. **components/[nama-fitur]-form.tsx**

Komponen form yang sudah terintegrasi dengan React Hook Form.

```typescript
import { useEffect } from "react";
import { FormProvider } from "react-hook-form";
import { use[NamaFitur]Form } from "../hooks/use[NamaFitur]Form";
import { Button } from "@/components/ui/button";
import {
  FormField,
  FormItem,
  FormLabel,
  FormControl,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";
import { Checkbox } from "@/components/ui/checkbox";
import { [nama]FormSchema } from "../types/schema";

interface [NamaFitur]FormProps {
  initialData?: Partial<[nama]FormSchema>;
  id?: string;
  onSuccess?: () => void;
  onCancel?: () => void;
}

export const [NamaFitur]Form = ({
  initialData,
  id,
  onSuccess,
  onCancel,
}: [NamaFitur]FormProps) => {
  const { form, onSubmit, isSubmitting, reset } = use[NamaFitur]Form({
    initialData,
    id,
    onSuccess,
    onClose: onCancel,
  });

  // Reset form when initialData changes
  useEffect(() => {
    if (initialData) {
      reset(initialData);
    }
  }, [initialData, reset]);

  return (
    <FormProvider {...form}>
      <form onSubmit={onSubmit} className="space-y-4">
        {/* Field name - Input Text */}
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Name</FormLabel>
              <FormControl>
                <Input {...field} placeholder="Enter name" />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Field price - Input Text */}
        <FormField
          control={form.control}
          name="price"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Price</FormLabel>
              <FormControl>
                <Input {...field} type="number" placeholder="Enter price" />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Field description - Textarea */}
        <FormField
          control={form.control}
          name="description"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Description</FormLabel>
              <FormControl>
                <Textarea
                  {...field}
                  placeholder="Enter description"
                  rows={3}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Field category - Select Dropdown */}
        <FormField
          control={form.control}
          name="category"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Category</FormLabel>
              <Select
                onValueChange={field.onChange}
                defaultValue={field.value}
              >
                <FormControl>
                  <SelectTrigger>
                    <SelectValue placeholder="Select category" />
                  </SelectTrigger>
                </FormControl>
                <SelectContent>
                  <SelectItem value="electronics">Electronics</SelectItem>
                  <SelectItem value="clothing">Clothing</SelectItem>
                  <SelectItem value="books">Books</SelectItem>
                </SelectContent>
              </Select>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Field is_active - Checkbox/Switch */}
        <FormField
          control={form.control}
          name="is_active"
          render={({ field }) => (
            <FormItem className="flex flex-row items-start space-x-3 space-y-0">
              <FormControl>
                <Checkbox
                  checked={field.value}
                  onCheckedChange={field.onChange}
                />
              </FormControl>
              <div className="space-y-1 leading-none">
                <FormLabel>Active Status</FormLabel>
              </div>
              <FormMessage />
            </FormItem>
          )}
        />

        <div className="flex justify-end gap-2 pt-4">
          <Button
            type="button"
            variant="outline"
            onClick={onCancel}
            disabled={isSubmitting}
          >
            Cancel
          </Button>
          <Button type="submit" disabled={isSubmitting}>
            {isSubmitting ? "Saving..." : id ? "Update" : "Create"}
          </Button>
        </div>
      </form>
    </FormProvider>
  );
};
```

#### 3. **types/schema.ts (Update)**

File schema akan diperbarui dengan form schema yang sesuai.

```typescript
import { z } from "zod";

// ... existing schemas ...

export const [nama]FormSchema = z.object({
  name: z.string().min(1, "Name is required"),
  price: z.string().min(1, "Price is required"),
  description: z.string().optional(),
  category: z.string().optional(),
  is_active: z.boolean().default(true),
});

export type [nama]FormSchema = z.infer<typeof [nama]FormSchema>;
```

## Contoh Lengkap: Membuat Form untuk "Product"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `feature:form-hook`

### Step 2: Jawab Pertanyaan

```
? Nama Form? (cth: login, user, product)
→ product

? Target Directory? (default: "src/features/product")
→ src/features/product

? Sebutkan field input (pisahkan dengan koma)?
→ name, price, description, category, is_active

--- Konfigurasi Tipe Field ---

? Tipe input untuk field 'name'?
→ Input Text

? Tipe input untuk field 'price'?
→ Input Text

? Tipe input untuk field 'description'?
→ Textarea

? Tipe input untuk field 'category'?
→ Select Dropdown

? Tipe input untuk field 'is_active'?
→ Checkbox/Switch
```

### Step 3: File yang Dihasilkan

```
src/features/product/
├── hooks/
│   └── useProductForm.ts
├── components/
│   └── product-form.tsx
└── types/
    └── schema.ts (diperbarui dengan ProductFormSchema)
```

### Step 4: Menggunakan Komponen Form

**Create Page:**

```typescript
// pages/product-create.tsx
import { ProductForm } from "../components/product-form";
import { useNavigate } from "react-router";

export const ProductCreate = () => {
  const navigate = useNavigate();

  const handleSuccess = () => {
    navigate("/products");
  };

  const handleCancel = () => {
    navigate("/products");
  };

  return (
    <div className="p-6">
      <div className="mb-6">
        <h1 className="text-2xl font-bold">Create Product</h1>
        <p className="text-muted-foreground mt-1">
          Fill in the form below to create a new product.
        </p>
      </div>

      <div className="bg-white rounded-lg border p-6">
        <ProductForm
          onSuccess={handleSuccess}
          onCancel={handleCancel}
        />
      </div>
    </div>
  );
};
```

**Edit Page:**

```typescript
// pages/product-edit.tsx
import { useEffect, useState } from "react";
import { useParams, useNavigate } from "react-router";
import { ProductForm } from "../components/product-form";
import { useProduct } from "../hooks/useProduct";

export const ProductEdit = () => {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  const { detail, getDetail, isLoading } = useProduct();

  useEffect(() => {
    if (id) {
      getDetail(id);
    }
  }, [id, getDetail]);

  const handleSuccess = () => {
    navigate("/products");
  };

  const handleCancel = () => {
    navigate("/products");
  };

  if (isLoading) {
    return (
      <div className="flex items-center justify-center h-64">
        <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-primary"></div>
      </div>
    );
  }

  return (
    <div className="p-6">
      <div className="mb-6">
        <h1 className="text-2xl font-bold">Edit Product</h1>
        <p className="text-muted-foreground mt-1">
          Update the product information below.
        </p>
      </div>

      <div className="bg-white rounded-lg border p-6">
        <ProductForm
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

**Form Dialog (Modal):**

```typescript
// components/product-form-dialog.tsx
import { Dialog, DialogContent, DialogHeader, DialogTitle } from "@/components/ui/dialog";
import { ProductForm } from "./product-form";

interface ProductFormDialogProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  initialData?: any;
  id?: string;
  onSuccess?: () => void;
}

export const ProductFormDialog = ({
  open,
  onOpenChange,
  initialData,
  id,
  onSuccess,
}: ProductFormDialogProps) => {
  const handleSuccess = () => {
    onSuccess?.();
    onOpenChange(false);
  };

  const handleCancel = () => {
    onOpenChange(false);
  };

  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent className="sm:max-w-[500px]">
        <DialogHeader>
          <DialogTitle>{id ? "Edit Product" : "Create Product"}</DialogTitle>
        </DialogHeader>
        <ProductForm
          initialData={initialData}
          id={id}
          onSuccess={handleSuccess}
          onCancel={handleCancel}
        />
      </DialogContent>
    </Dialog>
  );
};
```

## Kustomisasi Form

### 1. Menambahkan Validasi Kustom

```typescript
// types/schema.ts
export const productFormSchema = z.object({
  name: z.string()
    .min(1, "Name is required")
    .max(100, "Name must be less than 100 characters"),
  price: z.number()
    .positive("Price must be positive")
    .min(1000, "Minimum price is Rp 1,000"),
  email: z.string()
    .email("Invalid email format")
    .optional(),
  sku: z.string()
    .regex(/^[A-Z0-9-]+$/, "Invalid SKU format (only uppercase letters, numbers, and hyphens)"),
});
```

### 2. Menambahkan Field dengan Options Dinamis

```typescript
// components/product-form.tsx
import { useCategorySelect } from "@/features/category/hooks/useCategorySelect";

export const ProductForm = ({ ...props }) => {
  const { categories, isLoading } = useCategorySelect();

  return (
    <FormField
      control={form.control}
      name="categoryId"
      render={({ field }) => (
        <FormItem>
          <FormLabel>Category</FormLabel>
          <Select
            onValueChange={field.onChange}
            defaultValue={field.value}
            disabled={isLoading}
          >
            <FormControl>
              <SelectTrigger>
                <SelectValue placeholder="Select category" />
              </SelectTrigger>
            </FormControl>
            <SelectContent>
              {categories?.map((category) => (
                <SelectItem key={category.id} value={category.id}>
                  {category.name}
                </SelectItem>
              ))}
            </SelectContent>
          </Select>
          <FormMessage />
        </FormItem>
      )}
    />
  );
};
```

### 3. Menambahkan Field Upload File

```typescript
// components/product-form.tsx
import { useState } from "react";

export const ProductForm = ({ ...props }) => {
  const [preview, setPreview] = useState<string | null>(null);

  return (
    <FormField
      control={form.control}
      name="image"
      render={({ field: { onChange, value, ...field } }) => (
        <FormItem>
          <FormLabel>Product Image</FormLabel>
          <FormControl>
            <div className="space-y-2">
              <Input
                type="file"
                accept="image/*"
                onChange={(e) => {
                  const file = e.target.files?.[0];
                  if (file) {
                    onChange(file);
                    setPreview(URL.createObjectURL(file));
                  }
                }}
                {...field}
              />
              {preview && (
                <img
                  src={preview}
                  alt="Preview"
                  className="w-32 h-32 object-cover rounded-lg"
                />
              )}
            </div>
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
};
```

## Best Practices

### 1. Validasi Form

```typescript
// ✅ Baik: Gunakan zod untuk validasi
export const productFormSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email format"),
});

// ❌ Hindari: Validasi manual di komponen
```

### 2. Error Handling

```typescript
// ✅ Baik: Handle error dengan toast
try {
  await dispatch(createProduct(data)).unwrap();
  toast.success("Data berhasil disimpan");
} catch (error: any) {
  toast.error(error.message || "Gagal menyimpan data");
}

// ❌ Hindari: Tidak menampilkan feedback ke user
```

### 3. Loading State

```typescript
// ✅ Baik: Disable button saat submit
<Button type="submit" disabled={isSubmitting}>
  {isSubmitting ? "Saving..." : "Save"}
</Button>

// ❌ Hindari: User bisa klik multiple times
```

### 4. Reset Form

```typescript
// ✅ Baik: Reset form setelah sukses submit
const onSubmit = async (data) => {
  await dispatch(createProduct(data));
  form.reset(); // Reset form
  onSuccess?.();
};

// ❌ Hindari: Form tetap terisi setelah submit
```

### 5. Default Values

```typescript
// ✅ Baik: Set default values yang sesuai
const form = useForm({
  defaultValues: {
    is_active: true, // Default untuk checkbox
    status: "pending", // Default untuk select
  },
});

// ❌ Hindari: Membiarkan undefined
```

## Troubleshooting

### Form Tidak Tersubmit

Pastikan form memiliki `onSubmit` yang benar:

```typescript
// ✅ Benar
<form onSubmit={onSubmit}>

// ❌ Salah
<form onSubmit={handleSubmit}>
```

### Validasi Tidak Berjalan

Pastikan resolver sudah di-set dengan benar:

```typescript
const form = useForm({
  resolver: zodResolver(productFormSchema), // ← Jangan lupa
});
```

### Field Tidak Terdaftar di Form

Pastikan field sudah di-register dengan benar:

```typescript
<FormField
  control={form.control}
  name="email" // ← Harus sama dengan key di schema
  render={({ field }) => (
    <FormItem>
      <FormControl>
        <Input {...field} /> // ← Spread field
      </FormControl>
    </FormItem>
  )}
/>
```

## Command Reference

```bash
# Generate form hook untuk feature yang sudah ada
npm run generate
→ Pilih: feature:form-hook
→ Nama: product
→ Fields: name, price, description, category, is_active
```

## Lihat Juga

- [Generator: Feature](./generator-feature.md)
- [Generator: Feature Datatable](./generator-datatable.md)
- [Generator: Feature Page](./generator-page.md)
- [React Hook Form Documentation](https://react-hook-form.com/)
- [Zod Documentation](https://zod.dev/)
```