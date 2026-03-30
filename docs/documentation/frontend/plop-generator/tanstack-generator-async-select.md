---
title: "Generator: TanStack + Feature: Async Select"
sidebar_label: "Generator: TanStack + Feature: Async Select"
---

# Generator: TanStack + Feature: Async Select

## Gambaran Umum

**TanStack + Feature: Async Select** adalah generator yang digunakan untuk membuat custom hook untuk async select/combobox dengan integrasi **TanStack Query**. Hook ini memungkinkan Anda untuk mengambil data dari API secara asinkron dengan dukungan pencarian (search), pagination, dan infinite scroll.

Generator ini sangat berguna ketika Anda memiliki dropdown yang data source-nya berasal dari backend API, seperti pemilihan kategori, user, atau data master lainnya.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"TanStack + feature:asyncselect"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi async select hook yang akan dibuat.

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
Category
```

```
? Directory? (default: "src/features/Category")
```

Direktori target tempat file akan dibuat. Default akan mengarah ke folder fitur sesuai nama yang dimasukkan.

**Contoh:**
```
src/features/category
```

> **Catatan:** Nama direktori akan dikonversi ke lowercase secara otomatis.

## Output File yang Dihasilkan

### Struktur Folder

```
src/features/[nama-fitur]/
└── hooks/
    └── use[NamaFitur]Select.ts          # Custom hook untuk async select
```

### Deskripsi File

#### **hooks/use[NamaFitur]Select.ts**

Hook untuk mengambil data dari API dengan dukungan pagination, search, dan infinite scroll.

```typescript
import { useInfiniteQuery } from "@tanstack/react-query";
import { [nama]QueryKeys } from "../types/query-keys";
import { getSelectData } from "../api/get[NamaFitur]";
import { [nama]SelectSchema, type [nama]SelectSchema } from "../types/schema";

interface Use[NamaFitur]SelectProps {
  search?: string;
  enabled?: boolean;
  pageSize?: number;
}

export const use[NamaFitur]Select = ({
  search = "",
  enabled = true,
  pageSize = 10,
}: Use[NamaFitur]SelectProps = {}) => {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage,
    isLoading,
    isError,
    error,
    refetch,
  } = useInfiniteQuery({
    queryKey: [nama]QueryKeys.select(search),
    queryFn: async ({ pageParam = 1 }) => {
      const response = await getSelectData(pageParam, pageSize, search);
      return response;
    },
    getNextPageParam: (lastPage, pages) => {
      if (lastPage.data.length < pageSize) return undefined;
      return pages.length + 1;
    },
    initialPageParam: 1,
    enabled: enabled,
  });

  // Flatten all pages data into a single array
  const items = data?.pages.flatMap((page) => page.data) || [];

  return {
    items,
    isLoading,
    isError,
    error,
    hasNextPage,
    isFetchingNextPage,
    fetchNextPage,
    refetch,
  };
};
```

## File yang Diperlukan (Harus Ada Sebelumnya)

Generator ini mengasumsikan bahwa file-file berikut sudah ada dalam struktur fitur Anda:

### 1. **types/schema.ts**

File ini harus sudah berisi schema untuk select data.

```typescript
import { z } from "zod";

// Schema untuk data yang akan ditampilkan di select
export const [nama]SelectSchema = z.object({
  id: z.string(),
  text: z.string(),
  // Tambahkan field lain jika diperlukan
});

export type [nama]SelectSchema = z.infer<typeof [nama]SelectSchema>;
```

### 2. **types/query-keys.ts**

File ini harus sudah berisi query keys configuration.

```typescript
export const [nama]QueryKeys = {
  all: ['[nama]'] as const,
  lists: () => [...[nama]QueryKeys.all, 'list'] as const,
  select: (search: string) => [...[nama]QueryKeys.lists(), 'select', search] as const,
  detail: (id: string) => [...[nama]QueryKeys.all, 'detail', id] as const,
};
```

### 3. **api/get[NamaFitur].ts**

File ini harus sudah berisi fungsi API untuk mengambil data select.

```typescript
import { api } from "@/config/axios";
import { [nama]SelectSchema } from "../types/schema";

export const getSelectData = async (
  page: number,
  limit: number,
  search: string
): Promise<{
  data: [nama]SelectSchema[];
  total: number;
}> => {
  const response = await api.get(`/[nama]`, {
    params: {
      page,
      limit,
      search: search || undefined,
    },
  });
  return response.data;
};
```

## Contoh Lengkap: Membuat Async Select untuk "Category"

### Step 1: Pastikan File yang Diperlukan Sudah Ada

Sebelum menjalankan generator, pastikan Anda sudah memiliki struktur fitur category dengan file-file berikut:

**src/features/category/types/schema.ts**
```typescript
import { z } from "zod";

export const CategorySelectSchema = z.object({
  id: z.string(),
  text: z.string(),
  code: z.string().optional(),
});

export type CategorySelectSchema = z.infer<typeof CategorySelectSchema>;
```

**src/features/category/types/query-keys.ts**
```typescript
export const CategoryQueryKeys = {
  all: ['category'] as const,
  lists: () => [...CategoryQueryKeys.all, 'list'] as const,
  select: (search: string) => [...CategoryQueryKeys.lists(), 'select', search] as const,
  detail: (id: string) => [...CategoryQueryKeys.all, 'detail', id] as const,
};
```

**src/features/category/api/getCategory.ts**
```typescript
import { api } from "@/config/axios";
import { CategorySelectSchema } from "../types/schema";

export const getSelectData = async (
  page: number,
  limit: number,
  search: string
): Promise<{
  data: CategorySelectSchema[];
  total: number;
}> => {
  const response = await api.get(`/categories`, {
    params: {
      page,
      limit,
      search: search || undefined,
    },
  });
  return response.data;
};
```

### Step 2: Jalankan Generator

```bash
npm run generate
```

Pilih: `TanStack + feature:asyncselect`

### Step 3: Jawab Pertanyaan

```
? Nama Entity? (cth: Category, Product, User)
→ Category

? Directory? (default: "src/features/Category")
→ src/features/category
```

### Step 4: File yang Dihasilkan

```
src/features/category/
└── hooks/
    └── useCategorySelect.ts
```

### Step 5: Hasil File

**src/features/category/hooks/useCategorySelect.ts**

```typescript
import { useInfiniteQuery } from "@tanstack/react-query";
import { CategoryQueryKeys } from "../types/query-keys";
import { getSelectData } from "../api/getCategory";
import { CategorySelectSchema, type CategorySelectSchema } from "../types/schema";

interface UseCategorySelectProps {
  search?: string;
  enabled?: boolean;
  pageSize?: number;
}

export const useCategorySelect = ({
  search = "",
  enabled = true,
  pageSize = 10,
}: UseCategorySelectProps = {}) => {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage,
    isLoading,
    isError,
    error,
    refetch,
  } = useInfiniteQuery({
    queryKey: CategoryQueryKeys.select(search),
    queryFn: async ({ pageParam = 1 }) => {
      const response = await getSelectData(pageParam, pageSize, search);
      return response;
    },
    getNextPageParam: (lastPage, pages) => {
      if (lastPage.data.length < pageSize) return undefined;
      return pages.length + 1;
    },
    initialPageParam: 1,
    enabled: enabled,
  });

  // Flatten all pages data into a single array
  const items = data?.pages.flatMap((page) => page.data) || [];

  return {
    items,
    isLoading,
    isError,
    error,
    hasNextPage,
    isFetchingNextPage,
    fetchNextPage,
    refetch,
  };
};
```

## Cara Menggunakan Hook Async Select

### 1. Dalam Komponen Form

```typescript
import { useCategorySelect } from "@/features/category/hooks/useCategorySelect";
import { AsyncSelect } from "@/components/custom/async-select";

export const ProductForm = () => {
  const {
    items: categories,
    isLoading,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage,
  } = useCategorySelect({
    search: "", // Search term bisa di-pass dari input search
    pageSize: 20,
  });

  return (
    <AsyncSelect
      items={categories}
      isLoading={isLoading}
      isFetchingNextPage={isFetchingNextPage}
      onIntersectingScroll={fetchNextPage}
      placeholder="Select category"
      onChange={(value) => console.log(value)}
      renderItem={(category) => (
        <div className="flex flex-col">
          <span className="font-medium">{category.text}</span>
          {category.code && (
            <span className="text-xs text-muted-foreground">
              Code: {category.code}
            </span>
          )}
        </div>
      )}
    />
  );
};
```

### 2. Dengan Pencarian

```typescript
import { useState } from "react";
import { useDebounce } from "@/hooks/useDebounce";
import { useCategorySelect } from "@/features/category/hooks/useCategorySelect";

export const CategoryFilter = () => {
  const [searchTerm, setSearchTerm] = useState("");
  const debouncedSearch = useDebounce(searchTerm, 500);

  const { items, isLoading, fetchNextPage, hasNextPage } = useCategorySelect({
    search: debouncedSearch,
    pageSize: 20,
  });

  return (
    <div>
      <input
        type="text"
        placeholder="Search category..."
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        className="border rounded p-2 mb-4"
      />
      
      <AsyncSelect
        items={items}
        isLoading={isLoading}
        onIntersectingScroll={fetchNextPage}
        placeholder="Select category"
      />
    </div>
  );
};
```

### 3. Dengan Form Hook Form

```typescript
import { useForm } from "react-hook-form";
import { useCategorySelect } from "@/features/category/hooks/useCategorySelect";
import { FormField, FormItem, FormLabel, FormControl, FormMessage } from "@/components/ui/form";
import { AsyncSelect } from "@/components/custom/async-select";

export const ProductForm = () => {
  const form = useForm();
  const { items, isLoading, fetchNextPage, isFetchingNextPage } = useCategorySelect();

  return (
    <form>
      <FormField
        control={form.control}
        name="categoryId"
        render={({ field }) => (
          <FormItem>
            <FormLabel>Category</FormLabel>
            <FormControl>
              <AsyncSelect
                items={items}
                value={field.value}
                onChange={field.onChange}
                isLoading={isLoading}
                isFetchingNextPage={isFetchingNextPage}
                onIntersectingScroll={fetchNextPage}
                placeholder="Select category"
                renderItem={(category) => (
                  <div className="flex justify-between items-center w-full">
                    <span>{category.text}</span>
                    <span className="text-xs text-muted-foreground">
                      {category.code}
                    </span>
                  </div>
                )}
                renderSelectedItem={(category) => (
                  <div className="flex items-center gap-2">
                    <span>{category.text}</span>
                    <span className="text-xs text-muted-foreground">
                      ({category.code})
                    </span>
                  </div>
                )}
              />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </form>
  );
};
```

## Kustomisasi Hook

### Menambahkan Filter Tambahan

```typescript
interface UseCategorySelectProps {
  search?: string;
  enabled?: boolean;
  pageSize?: number;
  status?: "active" | "inactive" | "all"; // Filter tambahan
}

export const useCategorySelect = ({
  search = "",
  enabled = true,
  pageSize = 10,
  status = "all",
}: UseCategorySelectProps = {}) => {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage,
    isLoading,
    isError,
    error,
    refetch,
  } = useInfiniteQuery({
    queryKey: [...CategoryQueryKeys.select(search), status], // Tambahkan status ke query key
    queryFn: async ({ pageParam = 1 }) => {
      const response = await getSelectData(pageParam, pageSize, search, status);
      return response;
    },
    getNextPageParam: (lastPage, pages) => {
      if (lastPage.data.length < pageSize) return undefined;
      return pages.length + 1;
    },
    initialPageParam: 1,
    enabled: enabled,
  });

  // ... rest of the hook
};
```

### Menambahkan Sorting

```typescript
interface UseCategorySelectProps {
  search?: string;
  enabled?: boolean;
  pageSize?: number;
  sortBy?: "name" | "code" | "createdAt";
  sortOrder?: "asc" | "desc";
}

export const useCategorySelect = ({
  search = "",
  enabled = true,
  pageSize = 10,
  sortBy = "name",
  sortOrder = "asc",
}: UseCategorySelectProps = {}) => {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage,
    isLoading,
    isError,
    error,
    refetch,
  } = useInfiniteQuery({
    queryKey: [...CategoryQueryKeys.select(search), sortBy, sortOrder],
    queryFn: async ({ pageParam = 1 }) => {
      const response = await getSelectData(
        pageParam,
        pageSize,
        search,
        sortBy,
        sortOrder
      );
      return response;
    },
    // ... rest of the hook
  });

  return {
    items,
    isLoading,
    isError,
    error,
    hasNextPage,
    isFetchingNextPage,
    fetchNextPage,
    refetch,
  };
};
```

## Best Practices

### 1. Gunakan Debounce untuk Pencarian

```typescript
// ✅ Baik: Debounce search untuk mengurangi request API
const [search, setSearch] = useState("");
const debouncedSearch = useDebounce(search, 500);
const { items } = useCategorySelect({ search: debouncedSearch });

// ❌ Hindari: Langsung fetch setiap kali user mengetik
const { items } = useCategorySelect({ search }); // Akan banyak request
```

### 2. Set Page Size yang Optimal

```typescript
// ✅ Baik: Page size yang sesuai dengan kebutuhan
const { items } = useCategorySelect({ pageSize: 20 }); // 20 items per page

// ❌ Hindari: Page size terlalu besar
const { items } = useCategorySelect({ pageSize: 1000 }); // Performa buruk
```

### 3. Handle Loading dan Error States

```typescript
// ✅ Baik: Tampilkan loading dan error state
const { items, isLoading, isError, error, refetch } = useCategorySelect();

if (isError) {
  return (
    <div className="text-destructive">
      Error: {error.message}
      <button onClick={() => refetch()}>Retry</button>
    </div>
  );
}

// ❌ Hindari: Tidak handle error
```

### 4. Gunakan Query Keys yang Proper

```typescript
// ✅ Baik: Query keys mencakup semua dependensi
queryKey: [...CategoryQueryKeys.select(search), status, sortBy]

// ❌ Hindari: Query keys tidak lengkap
queryKey: ['category', 'select'] // Tidak akan re-fetch saat search berubah
```

## Troubleshooting

### Data Tidak Muncul di Select

Pastikan data yang diterima memiliki format yang sesuai dengan yang diharapkan oleh AsyncSelect component:

```typescript
// Format yang diharapkan
{
  id: "1",
  text: "Category Name"
}

// Cek struktur data dari API
console.log("API Response:", response.data);
```

### Infinite Scroll Tidak Bekerja

Pastikan `hasNextPage` dan `fetchNextPage` sudah di-pass ke AsyncSelect:

```typescript
<AsyncSelect
  items={items}
  onIntersectingScroll={fetchNextPage}  // ← Jangan lupa
  isFetchingNextPage={isFetchingNextPage} // ← Jangan lupa
/>
```

### Search Tidak Memicu Fetch Ulang

Pastikan query key mencakup search parameter:

```typescript
queryKey: [nama]QueryKeys.select(search), // ← search harus ada di query key
```

## Command Reference

```bash
# Generate async select hook
npm run generate
→ Pilih: TanStack + feature:asyncselect
→ Nama Entity: Category
→ Directory: src/features/category
```

## Lihat Juga

- [Generator: TanStack + Feature](./tanstack-generator-feature.md)
- [Generator: TanStack + Feature: Form Hook](./tanstack-generator-form-hook.md)
- [AsyncSelect Component](../Components/custom-component#asyncselect)
- [TanStack Query Infinite Queries](https://tanstack.com/query/latest/docs/framework/react/guides/infinite-queries)
```