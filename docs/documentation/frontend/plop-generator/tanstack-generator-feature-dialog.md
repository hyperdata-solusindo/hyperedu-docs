---
title: "Feature: Form Dialog"
sidebar_label: "TanStack + Feature: Form Dialog"
---

# Generator: TanStack + Feature: Form Dialog

## Gambaran Umum

**TanStack + Feature: Form Dialog** adalah generator yang digunakan untuk membuat komponen form dialog dengan integrasi **React Hook Form** dan **TanStack Query**. Generator ini menghasilkan form yang siap pakai dalam bentuk modal/dialog untuk operasi create dan edit data.

Generator ini sangat berguna ketika Anda membutuhkan form yang muncul dalam modal tanpa harus membuat halaman terpisah, seperti untuk form tambah/edit data yang ringan.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"TanStack + feature:form-dialog"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi form dialog yang akan dibuat.

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
│   └── [nama-fitur]-form-dialog.tsx   # Komponen form dialog
└── types/
    └── schema.ts                      # Diperbarui dengan form schema
```

### Deskripsi File-File Utama

#### 1. **hooks/use[NamaFitur]Form.ts**

Hook untuk mengelola form dengan React Hook Form dan integrasi TanStack Query.

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
  onClose?: () => void;
}

export const use[NamaFitur]Form = ({
  initialData,
  id,
  onSuccess,
  onClose,
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
      onClose?.();
      form.reset();
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
      onClose?.();
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
    reset: form.reset,
  };
};
```

#### 2. **components/[nama-fitur]-form-dialog.tsx**

Komponen form dialog yang muncul dalam modal.

```typescript
import { useEffect } from "react";
import { FormProvider } from "react-hook-form";
import { Dialog, DialogContent, DialogHeader, DialogTitle, DialogDescription } from "@/components/ui/dialog";
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
import { use[NamaFitur]Form } from "../hooks/use[NamaFitur]Form";
import { [nama]FormSchema } from "../types/schema";

interface [NamaFitur]FormDialogProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  initialData?: Partial<[nama]FormSchema>;
  id?: string;
  onSuccess?: () => void;
  title?: string;
  description?: string;
}

export const [NamaFitur]FormDialog = ({
  open,
  onOpenChange,
  initialData,
  id,
  onSuccess,
  title = id ? `Edit ${capitalizedNama}` : `Create ${capitalizedNama}`,
  description = id
    ? `Update the ${nama} information below.`
    : `Fill in the form below to create a new ${nama}.`,
}: [NamaFitur]FormDialogProps) => {
  const { form, onSubmit, isSubmitting, reset } = use[NamaFitur]Form({
    initialData,
    id,
    onSuccess,
    onClose: () => onOpenChange(false),
  });

  // Reset form when dialog closes or initialData changes
  useEffect(() => {
    if (!open) {
      reset();
    }
  }, [open, reset]);

  useEffect(() => {
    if (initialData) {
      reset(initialData);
    }
  }, [initialData, reset]);

  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent className="sm:max-w-[500px]">
        <DialogHeader>
          <DialogTitle>{title}</DialogTitle>
          <DialogDescription>{description}</DialogDescription>
        </DialogHeader>

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
                onClick={() => onOpenChange(false)}
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
      </DialogContent>
    </Dialog>
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

## File yang Diperlukan (Harus Ada Sebelumnya)

Generator ini mengasumsikan bahwa file-file berikut sudah ada dalam struktur fitur Anda:

### 1. **types/query-keys.ts**

```typescript
export const [nama]QueryKeys = {
  all: ['[nama]'] as const,
  lists: () => [...[nama]QueryKeys.all, 'list'] as const,
  detail: (id: string) => [...[nama]QueryKeys.all, 'detail', id] as const,
};
```

### 2. **api/get[NamaFitur].ts**

```typescript
import { api } from "@/config/axios";
import { [nama]FormSchema } from "../types/schema";

export const create = async (payload: [nama]FormSchema) => {
  const response = await api.post("/[nama]", payload);
  return response.data;
};

export const update = async (id: string, payload: [nama]FormSchema) => {
  const response = await api.put(`/[nama]/${id}`, payload);
  return response.data;
};
```

## Contoh Lengkap: Membuat Form Dialog untuk "Product"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `TanStack + feature:form-dialog`

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
│   └── product-form-dialog.tsx
└── types/
    └── schema.ts (diperbarui dengan ProductFormSchema)
```

### Step 4: Menggunakan Komponen Form Dialog

**Dalam Halaman List:**

```typescript
// pages/product.tsx
import { useState } from "react";
import { Button } from "@/components/ui/button";
import { ProductFormDialog } from "../components/product-form-dialog";
import { ProductTable } from "../components/product-table";

export const ProductPage = () => {
  const [dialogOpen, setDialogOpen] = useState(false);
  const [editingProduct, setEditingProduct] = useState<any>(null);

  const handleEdit = (product: any) => {
    setEditingProduct(product);
    setDialogOpen(true);
  };

  const handleCreate = () => {
    setEditingProduct(null);
    setDialogOpen(true);
  };

  const handleSuccess = () => {
    // Refresh data jika perlu
  };

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">Products</h1>
        <Button onClick={handleCreate}>Add Product</Button>
      </div>

      <ProductTable
        data={products}
        onEdit={handleEdit}
      />

      <ProductFormDialog
        open={dialogOpen}
        onOpenChange={setDialogOpen}
        initialData={editingProduct}
        id={editingProduct?.id}
        onSuccess={handleSuccess}
      />
    </div>
  );
};
```

**Dengan Tombol Edit di Table:**

```typescript
// components/product-table.tsx
import { Button } from "@/components/ui/button";
import { Pencil, Trash2 } from "lucide-react";

export const ProductTable = ({ data, onEdit }: any) => {
  const columns = [
    // ... other columns
    {
      id: "actions",
      cell: ({ row }) => (
        <div className="flex gap-2">
          <Button
            variant="ghost"
            size="icon"
            onClick={() => onEdit(row.original)}
          >
            <Pencil className="h-4 w-4" />
          </Button>
          <Button variant="ghost" size="icon">
            <Trash2 className="h-4 w-4 text-destructive" />
          </Button>
        </div>
      ),
    },
  ];

  // ... rest of table
};
```

## Kustomisasi Form Dialog

### 1. Menambahkan Validasi Kustom

```typescript
// types/schema.ts
export const productFormSchema = z.object({
  name: z.string().min(1, "Name is required"),
  price: z.number().positive("Price must be positive"),
  sku: z.string().regex(/^[A-Z0-9-]+$/, "Invalid SKU format"),
  email: z.string().email("Invalid email format"),
});
```

### 2. Menambahkan Async Select untuk Field Relasi

```typescript
// components/product-form-dialog.tsx
import { useCategorySelect } from "@/features/category/hooks/useCategorySelect";
import { AsyncSelect } from "@/components/custom/async-select";

export const ProductFormDialog = ({ ...props }) => {
  const { items: categories, isLoading } = useCategorySelect();

  return (
    <FormField
      control={form.control}
      name="categoryId"
      render={({ field }) => (
        <FormItem>
          <FormLabel>Category</FormLabel>
          <FormControl>
            <AsyncSelect
              items={categories}
              value={field.value}
              onChange={field.onChange}
              isLoading={isLoading}
              placeholder="Select category"
              renderItem={(category) => (
                <div>
                  <span>{category.text}</span>
                  <span className="text-xs text-muted-foreground ml-2">
                    {category.code}
                  </span>
                </div>
              )}
            />
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
};
```

### 3. Menambahkan Custom Footer

```typescript
// components/product-form-dialog.tsx
export const ProductFormDialog = ({ ...props }) => {
  return (
    <Dialog>
      <DialogContent>
        <DialogHeader>...</DialogHeader>

        <FormProvider {...form}>
          <form onSubmit={onSubmit}>
            {/* Form fields */}

            <div className="flex justify-between items-center pt-4">
              <div className="flex gap-2">
                <Button
                  type="button"
                  variant="outline"
                  size="sm"
                  onClick={() => form.reset()}
                >
                  Reset
                </Button>
              </div>
              <div className="flex gap-2">
                <Button
                  type="button"
                  variant="outline"
                  onClick={() => onOpenChange(false)}
                >
                  Cancel
                </Button>
                <Button type="submit" disabled={isSubmitting}>
                  {isSubmitting ? "Saving..." : id ? "Update" : "Create"}
                </Button>
              </div>
            </div>
          </form>
        </FormProvider>
      </DialogContent>
    </Dialog>
  );
};
```

### 4. Menambahkan Konfirmasi Sebelum Close

```typescript
// components/product-form-dialog.tsx
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

export const ProductFormDialog = ({ ...props }) => {
  const [showConfirm, setShowConfirm] = useState(false);
  const { formState } = form;
  const isDirty = formState.isDirty;

  const handleClose = () => {
    if (isDirty) {
      setShowConfirm(true);
    } else {
      onOpenChange(false);
    }
  };

  return (
    <>
      <Dialog open={open} onOpenChange={handleClose}>
        {/* Dialog content */}
      </Dialog>

      <AlertDialog open={showConfirm} onOpenChange={setShowConfirm}>
        <AlertDialogContent>
          <AlertDialogHeader>
            <AlertDialogTitle>Unsaved Changes</AlertDialogTitle>
            <AlertDialogDescription>
              You have unsaved changes. Are you sure you want to close?
            </AlertDialogDescription>
          </AlertDialogHeader>
          <AlertDialogFooter>
            <AlertDialogCancel>Continue Editing</AlertDialogCancel>
            <AlertDialogAction onClick={() => onOpenChange(false)}>
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

### 1. Reset Form Setelah Submit

```typescript
// ✅ Baik: Reset form setelah sukses submit
const createMutation = useMutation({
  mutationFn: create,
  onSuccess: () => {
    toast.success("Data berhasil disimpan");
    onSuccess?.();
    onClose?.();
    form.reset(); // Reset form
  },
});

// ❌ Hindari: Tidak mereset form
```

### 2. Disable Button Saat Submit

```typescript
// ✅ Baik: Disable button saat submitting
<Button type="submit" disabled={isSubmitting}>
  {isSubmitting ? "Saving..." : "Save"}
</Button>

// ❌ Hindari: User bisa klik multiple times
```

### 3. Handle Error dengan Baik

```typescript
// ✅ Baik: Tampilkan error spesifik
onError: (error: any) => {
  if (error.response?.data?.errors) {
    // Handle validation errors dari backend
    Object.entries(error.response.data.errors).forEach(([key, value]) => {
      form.setError(key as any, { message: value as string });
    });
  } else {
    toast.error(error.message || "Gagal menyimpan data");
  }
}

// ❌ Hindari: Hanya console.log error
```

### 4. Loading State untuk Async Select

```typescript
// ✅ Baik: Tampilkan loading state
<AsyncSelect
  items={categories}
  isLoading={isLoadingCategories}
  placeholder="Select category"
/>

// ❌ Hindari: Tidak ada loading indicator
```

## Troubleshooting

### Dialog Tidak Tertutup Setelah Submit

Pastikan `onClose` dipanggil setelah submit sukses:

```typescript
const createMutation = useMutation({
  mutationFn: create,
  onSuccess: () => {
    // ...
    onClose?.(); // ← Jangan lupa
  },
});
```

### Form Tidak Ter-reset

Pastikan `reset` dipanggil saat dialog ditutup:

```typescript
useEffect(() => {
  if (!open) {
    reset(); // Reset form saat dialog ditutup
  }
}, [open, reset]);
```

### Data Tidak Update Setelah Submit

Pastikan query keys di-invalidate:

```typescript
onSuccess: () => {
  queryClient.invalidateQueries({ queryKey: productKeys.lists() });
}
```

## Command Reference

```bash
# Generate form dialog
npm run generate
→ Pilih: TanStack + feature:form-dialog
→ Nama Form: product
→ Fields: name, price, description, category, is_active
```

## Lihat Juga

- [Generator: TanStack + Feature](./tanstack-generator-feature.md)
- [Generator: TanStack + Feature: Form Hook](.tanstack-/generator-form-hook.md)
- [Generator: TanStack + Feature: Async Select](./tanstack-generator-async-select.md)
- [React Hook Form Documentation](https://react-hook-form.com/)
```