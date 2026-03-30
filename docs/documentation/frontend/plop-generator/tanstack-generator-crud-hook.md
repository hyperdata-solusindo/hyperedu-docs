---
title: "Generator: TanStack + Feature: CRUD Hook"
sidebar_label: "Generator: TanStack + Feature: CRUD Hook"
---

# Generator: TanStack + Feature: CRUD Hook

## Gambaran Umum

**TanStack + Feature: CRUD Hook** adalah generator yang digunakan untuk membuat custom hook lengkap untuk operasi CRUD (Create, Read, Update, Delete) dengan integrasi **TanStack Query**. Generator ini menghasilkan hook yang siap pakai untuk mengelola data dari API, termasuk fetching data, pagination, sorting, filtering, dan mutations.

Generator ini sangat berguna ketika Anda membutuhkan hook yang komprehensif untuk mengelola data pada suatu fitur, lengkap dengan caching, background updates, dan error handling.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"TanStack + feature:crud-hook"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi CRUD hook yang akan dibuat.

## Tahap-Tahap Pertanyaan

### Tahap 1: Konfigurasi Dasar

```
? Nama Entity? (cth: Category, Product, User)
```

Masukkan nama entity dalam format **PascalCase**:

- **PascalCase**: `Category`, `Product`, `UserProfile`
- **Single word**: `User`, `Role`, `Department`

**Contoh:**
```
Product
```

```
? Directory? (default: "src/features/Product")
```

Direktori target tempat file akan dibuat. Default akan mengarah ke folder fitur sesuai nama yang dimasukkan.

**Contoh:**
```
src/features/product
```

> **Catatan:** Nama direktori akan dikonversi ke lowercase secara otomatis.

## Output File yang Dihasilkan

### Struktur Folder

```
src/features/[nama-fitur]/
├── api/
│   └── get[NamaFitur].ts          # API functions
├── hooks/
│   └── use[NamaFitur].ts          # CRUD hook utama
└── types/
    └── schema.ts                  # Diperbarui dengan semua schema
```

### Deskripsi File-File Utama

#### 1. **api/get[NamaFitur].ts**

File API yang berisi semua fungsi untuk berinteraksi dengan backend.

```typescript
import { api } from "@/config/axios";
import {
  [nama]DataSchema,
  [nama]DataTableSchema,
  [nama]FormSchema,
  type [nama]DataSchema,
  type [nama]DataTableSchema,
  type [nama]FormSchema,
} from "../types/schema";

// Get data untuk async select (with pagination & search)
export const getSelectData = async (
  pageNum: number,
  pageSize: number,
  searchTerm: string
): Promise<{
  data: Array<{ id: string; text: string }>;
  total: number;
}> => {
  const response = await api.get(`/[nama]/select`, {
    params: { page: pageNum, limit: pageSize, search: searchTerm || undefined },
  });
  return response.data;
};

// Get all data (no pagination)
export const getData = async (): Promise<[nama]DataSchema[]> => {
  const response = await api.get(`/[nama]`);
  return response.data;
};

// Get paginated data (for table)
export const getPaginatedData = async (
  params: {
    page?: number;
    limit?: number;
    search?: string;
    sort?: string;
    order?: "asc" | "desc";
  } = {}
): Promise<{
  data: [nama]DataTableSchema[];
  total: number;
  page: number;
  limit: number;
}> => {
  const response = await api.get(`/[nama]/paginated`, { params });
  return response.data;
};

// Get detail by ID
export const getDetail = async (id: string): Promise<[nama]DataSchema> => {
  const response = await api.get(`/[nama]/${id}`);
  return response.data;
};

// Create new item
export const create = async (payload: [nama]FormSchema): Promise<[nama]DataSchema> => {
  const response = await api.post(`/[nama]`, payload);
  return response.data;
};

// Update existing item
export const update = async (
  id: string,
  payload: [nama]FormSchema
): Promise<[nama]DataSchema> => {
  const response = await api.put(`/[nama]/${id}`, payload);
  return response.data;
};

// Delete item
export const remove = async (id: string): Promise<void> => {
  const response = await api.delete(`/[nama]/${id}`);
  return response.data;
};
```

#### 2. **hooks/use[NamaFitur].ts**

Hook utama yang mengelola semua operasi CRUD dengan TanStack Query.

```typescript
import { useQuery, useMutation, useQueryClient, type UseQueryOptions } from "@tanstack/react-query";
import { toast } from "sonner";
import {
  getData,
  getPaginatedData,
  getDetail,
  create,
  update,
  remove,
} from "../api/get[NamaFitur]";
import { [nama]QueryKeys } from "../types/query-keys";
import type {
  [nama]DataSchema,
  [nama]DataTableSchema,
  [nama]FormSchema,
} from "../types/schema";

interface Use[NamaFitur]Props {
  id?: string;
  pagination?: {
    page: number;
    limit: number;
    search?: string;
    sort?: string;
    order?: "asc" | "desc";
  };
  enabled?: boolean;
}

export const use[NamaFitur] = ({
  id,
  pagination,
  enabled = true,
}: Use[NamaFitur]Props = {}) => {
  const queryClient = useQueryClient();

  // Query: Get all data (no pagination)
  const {
    data: allData,
    isLoading: isLoadingAll,
    error: allError,
    refetch: refetchAll,
  } = useQuery({
    queryKey: [nama]QueryKeys.lists(),
    queryFn: getData,
    enabled: enabled && !pagination,
  });

  // Query: Get paginated data (for table)
  const {
    data: paginatedData,
    isLoading: isLoadingPaginated,
    error: paginatedError,
    refetch: refetchPaginated,
  } = useQuery({
    queryKey: [nama]QueryKeys.paginated(pagination),
    queryFn: () => getPaginatedData(pagination),
    enabled: enabled && !!pagination,
  });

  // Query: Get detail by ID
  const {
    data: detail,
    isLoading: isLoadingDetail,
    error: detailError,
    refetch: refetchDetail,
  } = useQuery({
    queryKey: [nama]QueryKeys.detail(id!),
    queryFn: () => getDetail(id!),
    enabled: enabled && !!id,
  });

  // Mutation: Create
  const createMutation = useMutation({
    mutationFn: create,
    onSuccess: () => {
      // Invalidate all list queries
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.lists() });
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.paginated() });
      toast.success("Data berhasil disimpan");
    },
    onError: (error: Error) => {
      toast.error(error.message || "Gagal menyimpan data");
    },
  });

  // Mutation: Update
  const updateMutation = useMutation({
    mutationFn: ({ id, data }: { id: string; data: [nama]FormSchema }) =>
      update(id, data),
    onSuccess: (_, variables) => {
      // Invalidate list queries and detail query
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.lists() });
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.paginated() });
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.detail(variables.id) });
      toast.success("Data berhasil diperbarui");
    },
    onError: (error: Error) => {
      toast.error(error.message || "Gagal memperbarui data");
    },
  });

  // Mutation: Delete
  const deleteMutation = useMutation({
    mutationFn: remove,
    onSuccess: (_, id) => {
      // Invalidate list queries
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.lists() });
      queryClient.invalidateQueries({ queryKey: [nama]QueryKeys.paginated() });
      // Remove detail query from cache
      queryClient.removeQueries({ queryKey: [nama]QueryKeys.detail(id) });
      toast.success("Data berhasil dihapus");
    },
    onError: (error: Error) => {
      toast.error(error.message || "Gagal menghapus data");
    },
  });

  // Helper function to reset mutations
  const resetMutations = () => {
    createMutation.reset();
    updateMutation.reset();
    deleteMutation.reset();
  };

  return {
    // Data states
    data: pagination ? paginatedData?.data : allData,
    total: pagination ? paginatedData?.total : allData?.length,
    detail,
    
    // Loading states
    isLoading: pagination ? isLoadingPaginated : isLoadingAll,
    isLoadingDetail,
    isCreating: createMutation.isPending,
    isUpdating: updateMutation.isPending,
    isDeleting: deleteMutation.isPending,
    
    // Error states
    error: pagination ? paginatedError : allError,
    detailError,
    createError: createMutation.error,
    updateError: updateMutation.error,
    deleteError: deleteMutation.error,
    
    // Actions
    refetch: pagination ? refetchPaginated : refetchAll,
    refetchDetail,
    create: createMutation.mutate,
    createAsync: createMutation.mutateAsync,
    update: updateMutation.mutate,
    updateAsync: updateMutation.mutateAsync,
    remove: deleteMutation.mutate,
    removeAsync: deleteMutation.mutateAsync,
    resetMutations,
  };
};
```

#### 3. **types/schema.ts (Update)**

File schema akan diperbarui dengan semua schema yang diperlukan.

```typescript
import { z } from "zod";

// Schema untuk data (response dari API)
export const [nama]DataSchema = z.object({
  id: z.string(),
  name: z.string(),
  // ... fields yang dipilih
  createdAt: z.string(),
  updatedAt: z.string(),
});

export type [nama]DataSchema = z.infer<typeof [nama]DataSchema>;

// Schema untuk datatable (subset dari data)
export const [nama]DataTableSchema = [nama]DataSchema.omit({
  // Omit fields yang tidak perlu ditampilkan di tabel
  updatedAt: true,
});

export type [nama]DataTableSchema = z.infer<typeof [nama]DataTableSchema>;

// Schema untuk form (create/update)
export const [nama]FormSchema = z.object({
  name: z.string().min(1, "Name is required"),
  // ... fields yang dipilih
});

export type [nama]FormSchema = z.infer<typeof [nama]FormSchema>;

// Schema untuk select (dropdown)
export const [nama]SelectSchema = z.object({
  id: z.string(),
  text: z.string(),
});

export type [nama]SelectSchema = z.infer<typeof [nama]SelectSchema>;
```

#### 4. **types/query-keys.ts (Update)**

File query keys akan diperbarui dengan keys yang lengkap.

```typescript
export const [nama]QueryKeys = {
  all: ['[nama]'] as const,
  lists: () => [...[nama]QueryKeys.all, 'list'] as const,
  paginated: (params?: {
    page?: number;
    limit?: number;
    search?: string;
    sort?: string;
    order?: "asc" | "desc";
  }) => [...[nama]QueryKeys.lists(), 'paginated', params] as const,
  detail: (id: string) => [...[nama]QueryKeys.all, 'detail', id] as const,
  select: (search: string) => [...[nama]QueryKeys.lists(), 'select', search] as const,
};
```

## Contoh Lengkap: Membuat CRUD Hook untuk "Product"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `TanStack + feature:crud-hook`

### Step 2: Jawab Pertanyaan

```
? Nama Entity? (cth: Category, Product, User)
→ Product

? Directory? (default: "src/features/Product")
→ src/features/product
```

### Step 3: File yang Dihasilkan

```
src/features/product/
├── api/
│   └── getProduct.ts
├── hooks/
│   └── useProduct.ts
└── types/
    └── schema.ts (diperbarui)
    └── query-keys.ts (diperbarui)
```

### Step 4: Menggunakan CRUD Hook

**Dalam Halaman List dengan Pagination:**

```typescript
// pages/product.tsx
import { useState } from "react";
import { useProduct } from "../hooks/useProduct";
import { ProductTable } from "../components/product-table";
import { ProductFormDialog } from "../components/product-form-dialog";
import { Button } from "@/components/ui/button";

export const ProductPage = () => {
  const [page, setPage] = useState(1);
  const [search, setSearch] = useState("");
  const [dialogOpen, setDialogOpen] = useState(false);
  const [editingId, setEditingId] = useState<string | null>(null);

  const {
    data: products,
    total,
    isLoading,
    refetch,
    remove,
    isDeleting,
  } = useProduct({
    pagination: {
      page,
      limit: 10,
      search,
      sort: "name",
      order: "asc",
    },
  });

  const handleDelete = async (id: string) => {
    if (confirm("Are you sure?")) {
      await remove(id);
    }
  };

  const handleEdit = (id: string) => {
    setEditingId(id);
    setDialogOpen(true);
  };

  const handleCreate = () => {
    setEditingId(null);
    setDialogOpen(true);
  };

  const handleSuccess = () => {
    setDialogOpen(false);
    refetch();
  };

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">Products ({total})</h1>
        <Button onClick={handleCreate}>Add Product</Button>
      </div>

      <div className="mb-4">
        <input
          type="text"
          placeholder="Search products..."
          value={search}
          onChange={(e) => setSearch(e.target.value)}
          className="border rounded px-3 py-2 w-full max-w-sm"
        />
      </div>

      <ProductTable
        data={products || []}
        isLoading={isLoading}
        onEdit={handleEdit}
        onDelete={handleDelete}
        isDeleting={isDeleting}
      />

      <div className="flex justify-between items-center mt-4">
        <div className="text-sm text-muted-foreground">
          Showing page {page}
        </div>
        <div className="flex gap-2">
          <Button
            variant="outline"
            onClick={() => setPage(p => Math.max(1, p - 1))}
            disabled={page === 1}
          >
            Previous
          </Button>
          <Button
            variant="outline"
            onClick={() => setPage(p => p + 1)}
            disabled={!products || products.length < 10}
          >
            Next
          </Button>
        </div>
      </div>

      <ProductFormDialog
        open={dialogOpen}
        onOpenChange={setDialogOpen}
        id={editingId || undefined}
        onSuccess={handleSuccess}
      />
    </div>
  );
};
```

**Dalam Komponen Form Dialog:**

```typescript
// components/product-form-dialog.tsx
import { useEffect } from "react";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { Dialog, DialogContent, DialogHeader, DialogTitle } from "@/components/ui/dialog";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { useProduct } from "../hooks/useProduct";
import { productFormSchema, type ProductFormSchema } from "../types/schema";

interface ProductFormDialogProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  id?: string;
  onSuccess?: () => void;
}

export const ProductFormDialog = ({
  open,
  onOpenChange,
  id,
  onSuccess,
}: ProductFormDialogProps) => {
  const { detail, isLoadingDetail, create, update, isCreating, isUpdating } = useProduct({
    id,
    enabled: open && !!id,
  });

  const form = useForm<ProductFormSchema>({
    resolver: zodResolver(productFormSchema),
    defaultValues: {
      name: "",
      description: "",
      price: 0,
    },
  });

  useEffect(() => {
    if (detail) {
      form.reset(detail);
    } else {
      form.reset({
        name: "",
        description: "",
        price: 0,
      });
    }
  }, [detail, form]);

  const onSubmit = async (data: ProductFormSchema) => {
    if (id) {
      await update({ id, data });
    } else {
      await create(data);
    }
    onSuccess?.();
  };

  const isSubmitting = isCreating || isUpdating;

  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>{id ? "Edit Product" : "Create Product"}</DialogTitle>
        </DialogHeader>

        <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
          <div>
            <label className="text-sm font-medium">Name</label>
            <Input
              {...form.register("name")}
              placeholder="Product name"
            />
            {form.formState.errors.name && (
              <p className="text-sm text-destructive mt-1">
                {form.formState.errors.name.message}
              </p>
            )}
          </div>

          <div>
            <label className="text-sm font-medium">Price</label>
            <Input
              type="number"
              {...form.register("price", { valueAsNumber: true })}
              placeholder="Product price"
            />
            {form.formState.errors.price && (
              <p className="text-sm text-destructive mt-1">
                {form.formState.errors.price.message}
              </p>
            )}
          </div>

          <div>
            <label className="text-sm font-medium">Description</label>
            <Textarea
              {...form.register("description")}
              placeholder="Product description"
              rows={3}
            />
          </div>

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
      </DialogContent>
    </Dialog>
  );
};
```

**Dalam Komponen Table:**

```typescript
// components/product-table.tsx
import { Button } from "@/components/ui/button";
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table";
import { Pencil, Trash2, Loader2 } from "lucide-react";

interface ProductTableProps {
  data: any[];
  isLoading: boolean;
  onEdit: (id: string) => void;
  onDelete: (id: string) => void;
  isDeleting: boolean;
}

export const ProductTable = ({
  data,
  isLoading,
  onEdit,
  onDelete,
  isDeleting,
}: ProductTableProps) => {
  if (isLoading) {
    return (
      <div className="flex items-center justify-center h-64">
        <Loader2 className="h-8 w-8 animate-spin text-muted-foreground" />
      </div>
    );
  }

  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead>Name</TableHead>
          <TableHead>Price</TableHead>
          <TableHead>Description</TableHead>
          <TableHead className="w-24">Actions</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {data.map((product) => (
          <TableRow key={product.id}>
            <TableCell className="font-medium">{product.name}</TableCell>
            <TableCell>Rp {product.price.toLocaleString()}</TableCell>
            <TableCell className="max-w-md truncate">{product.description}</TableCell>
            <TableCell>
              <div className="flex gap-2">
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={() => onEdit(product.id)}
                >
                  <Pencil className="h-4 w-4" />
                </Button>
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={() => onDelete(product.id)}
                  disabled={isDeleting}
                >
                  <Trash2 className="h-4 w-4 text-destructive" />
                </Button>
              </div>
            </TableCell>
          </TableRow>
        ))}
        {data.length === 0 && (
          <TableRow>
            <TableCell colSpan={4} className="text-center text-muted-foreground">
              No products found
            </TableCell>
          </TableRow>
        )}
      </TableBody>
    </Table>
  );
};
```

## Kustomisasi Hook

### 1. Menambahkan Filter Lanjutan

```typescript
// hooks/useProduct.ts
interface UseProductProps {
  id?: string;
  pagination?: {
    page: number;
    limit: number;
    search?: string;
    sort?: string;
    order?: "asc" | "desc";
    category?: string; // Filter tambahan
    minPrice?: number;
    maxPrice?: number;
  };
  enabled?: boolean;
}

export const useProduct = ({ id, pagination, enabled = true }: UseProductProps = {}) => {
  // ... rest of hook

  const {
    data: paginatedData,
    isLoading: isLoadingPaginated,
    error: paginatedError,
    refetch: refetchPaginated,
  } = useQuery({
    queryKey: [nama]QueryKeys.paginated(pagination),
    queryFn: () => getPaginatedData(pagination), // API harus mendukung filter tambahan
    enabled: enabled && !!pagination,
  });

  // ... rest of hook
};
```

### 2. Menambahkan Optimistic Updates

```typescript
// hooks/useProduct.ts
const deleteMutation = useMutation({
  mutationFn: remove,
  onMutate: async (id) => {
    // Cancel outgoing refetches
    await queryClient.cancelQueries({ queryKey: productKeys.paginated() });

    // Snapshot the previous value
    const previousData = queryClient.getQueryData(productKeys.paginated());

    // Optimistically update to the new value
    queryClient.setQueryData(productKeys.paginated(), (old: any) => ({
      ...old,
      data: old?.data?.filter((item: any) => item.id !== id),
      total: (old?.total || 0) - 1,
    }));

    // Return a context object with the snapshotted value
    return { previousData };
  },
  onError: (err, id, context) => {
    // Rollback on error
    queryClient.setQueryData(productKeys.paginated(), context?.previousData);
    toast.error("Gagal menghapus data");
  },
  onSuccess: () => {
    toast.success("Data berhasil dihapus");
  },
});
```

### 3. Menambahkan Prefetching

```typescript
// hooks/useProduct.ts
export const useProduct = () => {
  const queryClient = useQueryClient();

  const prefetchDetail = async (id: string) => {
    await queryClient.prefetchQuery({
      queryKey: productKeys.detail(id),
      queryFn: () => getDetail(id),
    });
  };

  return {
    // ... existing returns
    prefetchDetail,
  };
};
```

## Best Practices

### 1. Query Keys yang Proper

```typescript
// ✅ Baik: Query keys mencakup semua dependensi
queryKey: productKeys.paginated({ page, limit, search, sort, order })

// ❌ Hindari: Query keys tidak lengkap
queryKey: ['products', page]
```

### 2. Cache Invalidation yang Tepat

```typescript
// ✅ Baik: Invalidate queries yang relevan
onSuccess: () => {
  queryClient.invalidateQueries({ queryKey: productKeys.lists() });
  queryClient.invalidateQueries({ queryKey: productKeys.paginated() });
  if (id) {
    queryClient.invalidateQueries({ queryKey: productKeys.detail(id) });
  }
}

// ❌ Hindari: Invalidate semua queries
queryClient.invalidateQueries();
```

### 3. Error Handling yang Konsisten

```typescript
// ✅ Baik: Handle error dengan toast
onError: (error: Error) => {
  toast.error(error.message);
  console.error(error);
}

// ❌ Hindari: Hanya console.log
onError: (error) => console.error(error)
```

### 4. Loading States yang Jelas

```typescript
// ✅ Baik: Tampilkan loading state yang jelas
const { isLoading, isCreating, isUpdating, isDeleting } = useProduct();

// ❌ Hindari: Satu loading state untuk semua operasi
const { isLoading } = useProduct();
```

## Troubleshooting

### Data Tidak Muncul

Pastikan API endpoint yang digunakan benar:

```typescript
// Cek URL API
console.log("API URL:", api.defaults.baseURL + "/products");
```

### Query Tidak Re-fetch Setelah Mutation

Pastikan query keys di-invalidate dengan benar:

```typescript
onSuccess: () => {
  queryClient.invalidateQueries({ queryKey: productKeys.lists() });
}
```

### Type Error

Pastikan schema yang digunakan sesuai dengan response API:

```typescript
// Cek response API
console.log("API Response:", response.data);
```

## Command Reference

```bash
# Generate CRUD hook
npm run generate
→ Pilih: TanStack + feature:crud-hook
→ Nama Entity: Product
→ Directory: src/features/product
```

## Lihat Juga

- [Generator: TanStack + Feature](./tanstack-generator-feature.md)
- [Generator: TanStack + Feature: Form Hook](./tanstack-generator-form-hook.md)
- [Generator: TanStack + Feature: Async Select](./tanstack-generator-async-select.md)
- [TanStack Query Documentation](https://tanstack.com/query/latest)
```