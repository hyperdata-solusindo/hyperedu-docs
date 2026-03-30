---
title: "Generator: TanStack + Feature: Form Hook"
sidebar_label: "Generator: TanStack + Feature: Form Hook"
---

# Generator: TanStack + Feature: Form Hook

## Gambaran Umum

**TanStack + Feature: Form Hook** adalah generator yang digunakan untuk membuat custom hook dan komponen form dengan integrasi **React Hook Form** dan **TanStack Query**. Generator ini menghasilkan form yang siap pakai dengan validasi menggunakan Zod schema.

Generator ini cocok digunakan ketika Anda sudah memiliki struktur fitur dan hanya perlu menambahkan form untuk create/edit data, tanpa harus membuat seluruh struktur fitur dari awal.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"TanStack + feature:form-hook"** dari menu.

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

Hook utama untuk mengelola form menggunakan React Hook Form.

```typescript
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { useMutation, useQueryClient } from "@tanstack/react-query";
import { toast } from "sonner";
import { [nama]FormSchema, type [nama]FormSchema } from "../types/schema";
import { create, update } from "../api/get[NamaFitur]";
import { [nama]QueryKeys } from "../types/query-keys";

interface Use[NamaFitur]FormProps {
  initialData?: Partial<[nama]FormSchema>;
  id?: string;
  onSuccess?: () => void;
}

export const use[NamaFitur]Form = ({
  initialData,
  id,
  onSuccess,
}: Use[NamaFitur]FormProps = {}) => {
  const queryClient = useQueryClient();

  const form = useForm<[nama]FormSchema>({
    resolver: zodResolver([nama]FormSchema),
    defaultValues: initialData,
    mode: "onChange",
  });

  const createMutation = useMutation({
    mutationFn: create,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.lists() });
      toast.success("Data berhasil disimpan");
      onSuccess?.();
    },
    onError: (error: Error) => {
      toast.error(error.message || "Gagal menyimpan data");
    },
  });

  const updateMutation = useMutation({
    mutationFn: ({ id, data }: { id: string; data: [nama]FormSchema }) =>
      update(id, data),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.lists() });
      if (id) {
        queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.detail(id) });
      }
      toast.success("Data berhasil diperbarui");
      onSuccess?.();
    },
    onError: (error: Error) => {
      toast.error(error.message || "Gagal memperbarui data");
    },
  });

  const onSubmit = async (data: [nama]FormSchema) => {
    if (id) {
      await updateMutation.mutateAsync({ id, data });
    } else {
      await createMutation.mutateAsync(data);
    }
  };

  return {
    form,
    onSubmit: form.handleSubmit(onSubmit),
    errors: form.formState.errors,
    isSubmitting: createMutation.isPending || updateMutation.isPending,
  };
};
```

#### 2. **components/[nama-fitur]-form.tsx**

Komponen form yang sudah terintegrasi dengan React Hook Form.

```typescript
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
  const { form, onSubmit, isSubmitting } = use[NamaFitur]Form({
    initialData,
    id,
    onSuccess,
  });

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

#### 4. **api/get[NamaFitur].ts (Update)**

File API akan diperbarui dengan fungsi create dan update.

```typescript
import { api } from "@/config/axios";
import { [nama]FormSchema } from "../types/schema";

// ... existing functions ...

export const create = async (payload: [nama]FormSchema) => {
  const response = await api.post("/[nama]", payload);
  return response.data;
};

export const update = async (id: string, payload: [nama]FormSchema) => {
  const response = await api.put(`/[nama]/${id}`, payload);
  return response.data;
};
```

#### 5. **types/query-keys.ts (Update)**

File query keys akan diperbarui dengan keys yang diperlukan.

```typescript
export const [nama]QueryKeys = {
  all: ['[nama]'] as const,
  lists: () => [...[nama]QueryKeys.all, 'list'] as const,
  detail: (id: string) => [...[nama]QueryKeys.all, 'detail', id] as const,
};
```

## Contoh Lengkap: Membuat Form untuk "Product"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `TanStack + feature:form-hook`

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
├── api/
│   └── getProduct.ts (diperbarui dengan create & update)
├── hooks/
│   └── useProductForm.ts
├── components/
│   └── product-form.tsx
└── types/
    ├── schema.ts (diperbarui dengan ProductFormSchema)
    └── query-keys.ts (diperbarui)
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
      <h1 className="text-2xl font-bold mb-6">Create Product</h1>
      <ProductForm
        onSuccess={handleSuccess}
        onCancel={handleCancel}
      />
    </div>
  );
};
```

**Edit Page:**

```typescript
// pages/product-edit.tsx
import { ProductForm } from "../components/product-form";
import { useParams, useNavigate } from "react-router";
import { useQuery } from "@tanstack/react-query";
import { getDetail } from "../api/getProduct";
import { ProductQueryKeys } from "../types/query-keys";

export const ProductEdit = () => {
  const { id } = useParams();
  const navigate = useNavigate();

  const { data, isLoading } = useQuery({
    queryKey: ProductQueryKeys.detail(id!),
    queryFn: () => getDetail(id!),
    enabled: !!id,
  });

  const handleSuccess = () => {
    navigate("/products");
  };

  const handleCancel = () => {
    navigate("/products");
  };

  if (isLoading) {
    return <div>Loading...</div>;
  }

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-6">Edit Product</h1>
      <ProductForm
        initialData={data}
        id={id}
        onSuccess={handleSuccess}
        onCancel={handleCancel}
      />
    </div>
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
  price: z.number().positive("Price must be positive"),
});

// ❌ Hindari: Validasi manual di komponen
```

### 2. Error Handling

```typescript
// ✅ Baik: Handle error dengan toast
onError: (error: Error) => {
  toast.error(error.message);
  console.error(error);
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

### 4. Query Invalidation

```typescript
// ✅ Baik: Invalidate queries yang relevan
onSuccess: () => {
  queryClient.invalidateQueries({ queryKey: productKeys.lists() });
  if (id) {
    queryClient.invalidateQueries({ queryKey: productKeys.detail(id) });
  }
}

// ❌ Hindari: Tidak meng-invalidate cache
```

## Troubleshooting

### File tidak ter-generate

Pastikan direktori target valid dan memiliki izin tulis:

```bash
# Cek folder exist
ls -la src/features/

# Buat folder jika belum ada
mkdir -p src/features/product
```

### Type error pada schema

Pastikan field yang dimasukkan sesuai dengan yang ada di data API. Jika ada mismatch, edit manual di `types/schema.ts`.

### Select dropdown tidak bekerja

Pastikan Anda telah mengimport komponen Select yang benar:

```typescript
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";
```

## Command Reference

```bash
# Generate form hook untuk feature yang sudah ada
npm run generate
→ Pilih: TanStack + feature:form-hook

# Atau jika ingin generate dari awal dengan struktur lengkap
npm run generate
→ Pilih: TanStack + feature
```

## Lihat Juga

- [Generator: TanStack + Feature](./tanstack-generator-feature.md)
- [Generator: TanStack + Feature: Datatable](./tanstack-generator-datatable.md)
- [Generator: TanStack + Feature: Page](./tanstack-generator-page.md)
- [React Hook Form Documentation](https://react-hook-form.com/)
- [Zod Documentation](https://zod.dev/)
```