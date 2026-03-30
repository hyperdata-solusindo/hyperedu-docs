---
title: "Generator: Feature Async Combobox"
sidebar_label: "Generator: Feature Async Combobox"
---

# Generator: Feature Async Combobox

## Gambaran Umum

**Feature Async Combobox** adalah generator yang digunakan untuk membuat custom hook dan komponen untuk async select/combobox. Generator ini menghasilkan hook yang mengambil data dari API secara asinkron dengan dukungan pencarian (search), pagination, dan infinite scroll, serta komponen select yang siap pakai.

Generator ini cocok digunakan ketika Anda memiliki dropdown yang data source-nya berasal dari backend API, seperti pemilihan kategori, user, atau data master lainnya, dan menggunakan Redux untuk state management.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"feature:async-combobox"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi async combobox yang akan dibuat.

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
├── hooks/
│   └── use[NamaFitur]Select.ts          # Custom hook untuk async select
└── components/
    └── [nama-fitur]-select.tsx          # Komponen select UI
```

### Deskripsi File-File Utama

#### 1. **hooks/use[NamaFitur]Select.ts**

Hook untuk mengambil data dari API dengan dukungan pagination, search, dan infinite scroll menggunakan Redux.

```typescript
import { useEffect, useState, useCallback, useRef } from "react";
import { useAppDispatch, useAppSelector } from "@/hooks";
import { useDebounce } from "@/hooks/useDebounce";
import {
  fetch[NamaFitur]Select,
  reset[NamaFitur]Select,
  set[NamaFitur]Search,
} from "../slices/slice[NamaFitur]";

interface Use[NamaFitur]SelectProps {
  initialSearch?: string;
  pageSize?: number;
  enabled?: boolean;
}

export const use[NamaFitur]Select = ({
  initialSearch = "",
  pageSize = 10,
  enabled = true,
}: Use[NamaFitur]SelectProps = {}) => {
  const dispatch = useAppDispatch();
  const [search, setSearch] = useState(initialSearch);
  const [page, setPage] = useState(1);
  const [hasMore, setHasMore] = useState(true);
  const [isLoadingMore, setIsLoadingMore] = useState(false);
  
  const debouncedSearch = useDebounce(search, 500);
  
  const {
    items: selectItems,
    isLoading: isLoadingSelect,
    error: selectError,
    total,
  } = useAppSelector((state) => state.[nama].select);

  // Load initial data
  useEffect(() => {
    if (enabled) {
      dispatch(set[NamaFitur]Search(debouncedSearch));
      dispatch(fetch[NamaFitur]Select({ page: 1, limit: pageSize, search: debouncedSearch }));
      setPage(1);
      setHasMore(true);
    }
  }, [debouncedSearch, enabled, pageSize, dispatch]);

  // Handle load more (infinite scroll)
  const loadMore = useCallback(async () => {
    if (!hasMore || isLoadingMore || isLoadingSelect) return;
    
    setIsLoadingMore(true);
    const nextPage = page + 1;
    
    try {
      const result = await dispatch(fetch[NamaFitur]Select({
        page: nextPage,
        limit: pageSize,
        search: debouncedSearch,
      })).unwrap();
      
      setPage(nextPage);
      setHasMore(result.data.length === pageSize);
    } catch (error) {
      console.error("Failed to load more:", error);
    } finally {
      setIsLoadingMore(false);
    }
  }, [page, hasMore, isLoadingMore, isLoadingSelect, debouncedSearch, pageSize, dispatch]);

  // Reset select data
  const reset = useCallback(() => {
    dispatch(reset[NamaFitur]Select());
    setSearch(initialSearch);
    setPage(1);
    setHasMore(true);
    setIsLoadingMore(false);
  }, [dispatch, initialSearch]);

  return {
    items: selectItems,
    isLoading: isLoadingSelect,
    isLoadingMore,
    hasMore,
    error: selectError,
    total,
    search,
    setSearch,
    loadMore,
    reset,
  };
};
```

#### 2. **components/[nama-fitur]-select.tsx**

Komponen select yang siap pakai dengan async loading dan infinite scroll.

```typescript
import { useEffect, useRef } from "react";
import { use[NamaFitur]Select } from "../hooks/use[NamaFitur]Select";
import { AsyncSelect } from "@/components/custom/async-select";
import { [nama]SelectSchema, type [nama]SelectSchema } from "../types/schema";

interface [NamaFitur]SelectProps {
  value?: any;
  onChange?: (value: any) => void;
  placeholder?: string;
  disabled?: boolean;
  showClear?: boolean;
  multiple?: boolean;
  onSearch?: (search: string) => void;
  renderItem?: (item: [nama]SelectSchema) => React.ReactNode;
  renderSelectedItem?: (item: [nama]SelectSchema) => React.ReactNode;
}

export const [NamaFitur]Select = ({
  value,
  onChange,
  placeholder = "Select {nama}...",
  disabled = false,
  showClear = true,
  multiple = false,
  onSearch,
  renderItem,
  renderSelectedItem,
}: [NamaFitur]SelectProps) => {
  const {
    items,
    isLoading,
    isLoadingMore,
    hasMore,
    error,
    search,
    setSearch,
    loadMore,
    reset,
  } = use[NamaFitur]Select();

  // Reset search when component unmounts
  useEffect(() => {
    return () => {
      reset();
    };
  }, [reset]);

  // Handle search
  const handleSearch = (value: string) => {
    setSearch(value);
    onSearch?.(value);
  };

  // Default render item
  const defaultRenderItem = (item: [nama]SelectSchema) => (
    <div className="flex flex-col">
      <span className="font-medium">{item.text}</span>
      {item.description && (
        <span className="text-xs text-muted-foreground">{item.description}</span>
      )}
    </div>
  );

  // Default render selected item
  const defaultRenderSelectedItem = (item: [nama]SelectSchema) => (
    <div className="flex items-center gap-2">
      <span>{item.text}</span>
      {item.code && (
        <span className="text-xs text-muted-foreground">({item.code})</span>
      )}
    </div>
  );

  if (error) {
    return (
      <div className="text-sm text-destructive p-2 border rounded-md">
        Error: {error}
        <button
          onClick={() => reset()}
          className="ml-2 text-primary underline"
        >
          Retry
        </button>
      </div>
    );
  }

  return (
    <AsyncSelect
      value={value}
      onChange={onChange}
      items={items}
      isLoading={isLoading}
      isFetchingNextPage={isLoadingMore}
      onIntersectingScroll={loadMore}
      onSearch={handleSearch}
      placeholder={placeholder}
      disabled={disabled}
      showClear={showClear}
      multiple={multiple}
      renderItem={renderItem || defaultRenderItem}
      renderSelectedItem={renderSelectedItem || defaultRenderSelectedItem}
    />
  );
};
```

## File yang Diperlukan (Harus Ada Sebelumnya)

Generator ini mengasumsikan bahwa file-file berikut sudah ada dalam struktur fitur Anda:

### 1. **types/schema.ts**

File ini harus sudah berisi schema untuk select data.

```typescript
import { z } from "zod";

// Schema untuk data select
export const [nama]SelectSchema = z.object({
  id: z.string(),
  text: z.string(),
  code: z.string().optional(),
  description: z.string().optional(),
  // Tambahkan field lain jika diperlukan
});

export type [nama]SelectSchema = z.infer<typeof [nama]SelectSchema>;
```

### 2. **slices/slice[NamaFitur].tsx**

File slice harus memiliki state untuk select data dan actions yang diperlukan.

```typescript
import { createSlice, createAsyncThunk, PayloadAction } from "@reduxjs/toolkit";
import { getSelectData } from "../api/get[NamaFitur]";
import { [nama]SelectSchema } from "../types/schema";

interface [NamaFitur]SelectState {
  items: [nama]SelectSchema[];
  isLoading: boolean;
  error: string | null;
  total: number;
  search: string;
}

const initialSelectState: [NamaFitur]SelectState = {
  items: [],
  isLoading: false,
  error: null,
  total: 0,
  search: "",
};

// Async thunk untuk fetch select data
export const fetch[NamaFitur]Select = createAsyncThunk(
  "[nama]/fetchSelect",
  async (params: { page: number; limit: number; search: string }) => {
    const response = await getSelectData(params.page, params.limit, params.search);
    return response;
  }
);

// Slice dengan select state
const [nama]Slice = createSlice({
  name: "[nama]",
  initialState: {
    // ... existing state
    select: initialSelectState,
  },
  reducers: {
    // ... existing reducers
    set[NamaFitur]Search: (state, action: PayloadAction<string>) => {
      state.select.search = action.payload;
    },
    reset[NamaFitur]Select: (state) => {
      state.select = initialSelectState;
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetch[NamaFitur]Select.pending, (state) => {
        state.select.isLoading = true;
        state.select.error = null;
      })
      .addCase(fetch[NamaFitur]Select.fulfilled, (state, action) => {
        state.select.isLoading = false;
        if (action.meta.arg.page === 1) {
          state.select.items = action.payload.data;
        } else {
          state.select.items = [...state.select.items, ...action.payload.data];
        }
        state.select.total = action.payload.total;
      })
      .addCase(fetch[NamaFitur]Select.rejected, (state, action) => {
        state.select.isLoading = false;
        state.select.error = action.error.message || "Failed to fetch data";
      });
  },
});

export const { set[NamaFitur]Search, reset[NamaFitur]Select } = [nama]Slice.actions;
```

### 3. **api/get[NamaFitur].ts**

File API harus memiliki fungsi untuk mengambil data select.

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
  const response = await api.get(`/[nama]/select`, {
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

**src/features/category/types/schema.ts**
```typescript
import { z } from "zod";

export const CategorySelectSchema = z.object({
  id: z.string(),
  text: z.string(),
  code: z.string().optional(),
  description: z.string().optional(),
});

export type CategorySelectSchema = z.infer<typeof CategorySelectSchema>;
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
  const response = await api.get(`/categories/select`, {
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

Pilih: `feature:async-combobox`

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
├── hooks/
│   └── useCategorySelect.ts
└── components/
    └── category-select.tsx
```

### Step 5: Menggunakan Komponen Select

**Dalam Form:**

```typescript
// components/product-form.tsx
import { CategorySelect } from "@/features/category/components/category-select";
import { useForm } from "react-hook-form";

export const ProductForm = () => {
  const { setValue, watch } = useForm();
  const categoryId = watch("categoryId");

  return (
    <div className="space-y-4">
      <div>
        <label className="text-sm font-medium">Category</label>
        <CategorySelect
          value={categoryId}
          onChange={(value) => setValue("categoryId", value)}
          placeholder="Select product category"
          showClear
        />
      </div>
    </div>
  );
};
```

**Dalam Filter Component:**

```typescript
// components/product-filter.tsx
import { CategorySelect } from "@/features/category/components/category-select";
import { useState } from "react";

export const ProductFilter = ({ onFilterChange }: any) => {
  const [categoryId, setCategoryId] = useState<string>();

  const handleCategoryChange = (value: string) => {
    setCategoryId(value);
    onFilterChange({ categoryId: value });
  };

  return (
    <div className="flex gap-4">
      <CategorySelect
        value={categoryId}
        onChange={handleCategoryChange}
        placeholder="Filter by category"
        showClear
      />
    </div>
  );
};
```

### Step 6: Kustomisasi Render Item

```typescript
// components/product-form.tsx
import { CategorySelect } from "@/features/category/components/category-select";

export const ProductForm = () => {
  return (
    <CategorySelect
      value={categoryId}
      onChange={setValue}
      renderItem={(category) => (
        <div className="flex justify-between items-center w-full">
          <div>
            <span className="font-medium">{category.text}</span>
            {category.description && (
              <p className="text-xs text-muted-foreground">{category.description}</p>
            )}
          </div>
          {category.code && (
            <span className="text-xs bg-gray-100 px-2 py-1 rounded">
              {category.code}
            </span>
          )}
        </div>
      )}
      renderSelectedItem={(category) => (
        <div className="flex items-center gap-2">
          <span>{category.text}</span>
          {category.code && (
            <span className="text-xs text-muted-foreground">
              ({category.code})
            </span>
          )}
        </div>
      )}
    />
  );
};
```

## Kustomisasi Hook

### 1. Menambahkan Filter Tambahan

```typescript
// hooks/useCategorySelect.ts
interface UseCategorySelectProps {
  initialSearch?: string;
  pageSize?: number;
  enabled?: boolean;
  status?: "active" | "inactive" | "all"; // Filter tambahan
  type?: string; // Filter tambahan
}

export const useCategorySelect = ({
  initialSearch = "",
  pageSize = 10,
  enabled = true,
  status = "all",
  type,
}: UseCategorySelectProps = {}) => {
  // ... existing code

  useEffect(() => {
    if (enabled) {
      dispatch(setCategorySearch(debouncedSearch));
      dispatch(fetchCategorySelect({
        page: 1,
        limit: pageSize,
        search: debouncedSearch,
        status, // Pass filter to API
        type,   // Pass filter to API
      }));
      setPage(1);
      setHasMore(true);
    }
  }, [debouncedSearch, enabled, pageSize, status, type, dispatch]);

  // ... rest of hook
};
```

### 2. Menambahkan Sorting

```typescript
// hooks/useCategorySelect.ts
interface UseCategorySelectProps {
  initialSearch?: string;
  pageSize?: number;
  enabled?: boolean;
  sortBy?: "name" | "code" | "createdAt";
  sortOrder?: "asc" | "desc";
}

export const useCategorySelect = ({
  initialSearch = "",
  pageSize = 10,
  enabled = true,
  sortBy = "name",
  sortOrder = "asc",
}: UseCategorySelectProps = {}) => {
  // ... existing code

  useEffect(() => {
    if (enabled) {
      dispatch(setCategorySearch(debouncedSearch));
      dispatch(fetchCategorySelect({
        page: 1,
        limit: pageSize,
        search: debouncedSearch,
        sortBy,
        sortOrder,
      }));
      setPage(1);
      setHasMore(true);
    }
  }, [debouncedSearch, enabled, pageSize, sortBy, sortOrder, dispatch]);

  // ... rest of hook
};
```

### 3. Menambahkan Cache

```typescript
// hooks/useCategorySelect.ts
import { useAppDispatch, useAppSelector } from "@/hooks";

export const useCategorySelect = (props: UseCategorySelectProps = {}) => {
  const dispatch = useAppDispatch();
  const cacheKey = `${debouncedSearch}_${pageSize}_${status}_${sortBy}_${sortOrder}`;
  
  // Gunakan selector yang sudah di-memoize
  const cachedData = useAppSelector((state) => 
    state.category.selectCache[cacheKey]
  );

  // Jika data sudah ada di cache, gunakan langsung
  useEffect(() => {
    if (cachedData) {
      setItems(cachedData.items);
      setHasMore(cachedData.hasMore);
      return;
    }
    
    // Otherwise fetch from API
    dispatch(fetchCategorySelect({ ...params }));
  }, [cacheKey, cachedData]);

  // ... rest of hook
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
if (isLoading) return <SelectSkeleton />;
if (error) return <ErrorMessage error={error} onRetry={reset} />;

// ❌ Hindari: Tidak handle error
```

### 4. Reset State Saat Unmount

```typescript
// ✅ Baik: Reset state saat komponen unmount
useEffect(() => {
  return () => {
    reset();
  };
}, [reset]);

// ❌ Hindari: State tetap tersimpan
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

Pastikan `hasMore`, `loadMore`, dan `isLoadingMore` sudah di-pass ke AsyncSelect:

```typescript
<AsyncSelect
  items={items}
  onIntersectingScroll={loadMore}  // ← Jangan lupa
  isFetchingNextPage={isLoadingMore} // ← Jangan lupa
/>
```

### Search Tidak Memicu Fetch Ulang

Pastikan search term digunakan dalam effect dependency:

```typescript
useEffect(() => {
  if (enabled) {
    dispatch(fetchCategorySelect({ search: debouncedSearch }));
  }
}, [debouncedSearch, enabled]); // ← search harus ada di dependency
```

### Redux State Tidak Terupdate

Pastikan reducer sudah ditambahkan ke root store:

```typescript
// src/stores/index.ts
import categoryReducer from "../features/category/slices/sliceCategory";

export const rootReducer = combineReducers({
  category: categoryReducer,
  // ... other reducers
});
```

## Command Reference

```bash
# Generate async select hook dan component
npm run generate
→ Pilih: feature:async-combobox
→ Nama Entity: Category
→ Directory: src/features/category
```

## Lihat Juga

- [Generator: Feature](./generator-feature.md)
- [Generator: Feature Form Hook](./generator-form-hook.md)
- [Generator: Feature Page](./generator-page.md)
- [AsyncSelect Component](../custom-components.md#asyncselect)
- [useDebounce Hook](../hooks.md#usedebounce)
```