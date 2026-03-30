---
title: "Generator: Feature"
sidebar_label: "Generator: Feature"
---

# Generator: Feature

## Gambaran Umum

**Feature** adalah generator yang digunakan untuk membuat struktur fitur baru dengan arsitektur berbasis fitur (feature-based architecture). Generator ini menghasilkan struktur folder lengkap untuk sebuah fitur, termasuk API layer, components, hooks, layouts, pages, slices, stores, types, dan routing.

Generator ini cocok digunakan ketika Anda ingin memulai fitur baru dari awal tanpa integrasi TanStack Query. Fitur yang dihasilkan menggunakan Redux untuk state management.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"feature"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi fitur yang akan dibuat.

## Tahap-Tahap Pertanyaan

### Tahap 1: Konfigurasi Dasar

```
? Apa nama fitur baru ini? (misal: products, auth, user-profile)
```

Masukkan nama fitur dalam format:

- **lowercase dengan hyphen**: `user-profile`, `student-management`
- **lowercase tanpa spasi**: `products`, `students`
- **format subfolder**: `masters/student`, `admin/user-management`

**Contoh:**
```
products
```

atau untuk fitur dengan subfolder:

```
masters/products
```

Jika menggunakan format subfolder akan menghasilkan:

- **Folder utama**: `src/features/masters/products/`
- **Nama fitur**: `products`
- **Parent folder**: `masters`

Nama fitur akan dikonversi ke berbagai format:

- `pascalCase`: `Products`
- `camelCase`: `products`
- `dashCase`: `products`

```
? Sebutkan data (pisahkan dengan koma)? (cth: id, name, email, role)
```

Daftar atribut/field yang akan digunakan dalam fitur. Pisahkan dengan koma tanpa spasi tambahan.

**Contoh:**
```
id, name, email, role, status, createdAt
```

Data ini akan digunakan untuk membuat schema di file `types/schema.ts`.

## Output File yang Dihasilkan

### Struktur Folder

```
src/features/[nama-fitur]/
├── api/
│   └── get[NamaFitur].ts          # API calls
├── assets/
│   └── .gitkeep                   # Folder untuk aset lokal fitur
├── components/
│   └── [nama-fitur]-list.tsx      # Component list data
├── hooks/
│   └── use[NamaFitur].ts          # Custom hook
├── layouts/
│   └── [nama-fitur]-layout.tsx    # Layout fitur
├── pages/
│   └── [nama-fitur].tsx           # Main page
├── slices/
│   └── slice[NamaFitur].tsx       # Redux slice
├── stores/
│   └── store[NamaFitur].tsx       # Redux store config
├── types/
│   └── schema.ts                  # Zod schemas
├── index.ts                       # Barrel file
└── route.ts                       # Route definition
```

### Deskripsi File-File Utama

#### 1. **api/get[NamaFitur].ts**

File API yang berisi fungsi untuk mengambil data dari backend.

```typescript
import { api } from "@/config/axios";

export const get[NamaFitur]Data = async (): Promise<[nama]DataSchema[]> => {
  const response = await api.get("/[nama]");
  return response.data;
};

export const get[NamaFitur]Detail = async (id: string): Promise<[nama]DataSchema> => {
  const response = await api.get(`/[nama]/${id}`);
  return response.data;
};

export const create[NamaFitur] = async (payload: [nama]FormSchema): Promise<[nama]DataSchema> => {
  const response = await api.post("/[nama]", payload);
  return response.data;
};

export const update[NamaFitur] = async (
  id: string,
  payload: [nama]FormSchema
): Promise<[nama]DataSchema> => {
  const response = await api.put(`/[nama]/${id}`, payload);
  return response.data;
};

export const delete[NamaFitur] = async (id: string): Promise<void> => {
  const response = await api.delete(`/[nama]/${id}`);
  return response.data;
};
```

#### 2. **types/schema.ts**

Validasi data menggunakan Zod schema.

```typescript
import { z } from "zod";

// Schema untuk data dari API
export const [nama]DataSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
  role: z.string(),
  status: z.string(),
  createdAt: z.string(),
  updatedAt: z.string(),
});

export type [nama]DataSchema = z.infer<typeof [nama]DataSchema>;

// Schema untuk form (create/update)
export const [nama]FormSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email format"),
  role: z.string(),
  status: z.string(),
});

export type [nama]FormSchema = z.infer<typeof [nama]FormSchema>;

// Schema untuk select dropdown
export const [nama]SelectSchema = z.object({
  id: z.string(),
  text: z.string(),
});

export type [nama]SelectSchema = z.infer<typeof [nama]SelectSchema>;
```

#### 3. **hooks/use[NamaFitur].ts**

Custom hook untuk mengelola state dan API calls dengan Redux.

```typescript
import { useAppDispatch, useAppSelector } from "@/hooks";
import { useCallback } from "react";
import {
  fetch[NamaFitur]List,
  fetch[NamaFitur]Detail,
  create[NamaFitur],
  update[NamaFitur],
  delete[NamaFitur],
  reset[NamaFitur]State,
} from "../slices/slice[NamaFitur]";

export const use[NamaFitur] = () => {
  const dispatch = useAppDispatch();
  const {
    data,
    detail,
    isLoading,
    isSubmitting,
    error,
  } = useAppSelector((state) => state.[nama]);

  const getList = useCallback(() => {
    dispatch(fetch[NamaFitur]List());
  }, [dispatch]);

  const getDetail = useCallback((id: string) => {
    dispatch(fetch[NamaFitur]Detail(id));
  }, [dispatch]);

  const create = useCallback((payload: [nama]FormSchema) => {
    return dispatch(create[NamaFitur](payload)).unwrap();
  }, [dispatch]);

  const update = useCallback((id: string, payload: [nama]FormSchema) => {
    return dispatch(update[NamaFitur]({ id, data: payload })).unwrap();
  }, [dispatch]);

  const remove = useCallback((id: string) => {
    return dispatch(delete[NamaFitur](id)).unwrap();
  }, [dispatch]);

  const reset = useCallback(() => {
    dispatch(reset[NamaFitur]State());
  }, [dispatch]);

  return {
    data,
    detail,
    isLoading,
    isSubmitting,
    error,
    getList,
    getDetail,
    create,
    update,
    remove,
    reset,
  };
};
```

#### 4. **slices/slice[NamaFitur].tsx**

Redux slice untuk mengelola state fitur.

```typescript
import { createSlice, createAsyncThunk, PayloadAction } from "@reduxjs/toolkit";
import {
  get[NamaFitur]Data,
  get[NamaFitur]Detail,
  create[NamaFitur],
  update[NamaFitur],
  delete[NamaFitur],
} from "../api/get[NamaFitur]";
import { [nama]DataSchema, [nama]FormSchema } from "../types/schema";

interface [NamaFitur]State {
  data: [nama]DataSchema[];
  detail: [nama]DataSchema | null;
  isLoading: boolean;
  isSubmitting: boolean;
  error: string | null;
}

const initialState: [NamaFitur]State = {
  data: [],
  detail: null,
  isLoading: false,
  isSubmitting: false,
  error: null,
};

// Async thunks
export const fetch[NamaFitur]List = createAsyncThunk(
  "[nama]/fetchList",
  async (_, { rejectWithValue }) => {
    try {
      const response = await get[NamaFitur]Data();
      return response;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || "Failed to fetch data");
    }
  }
);

export const fetch[NamaFitur]Detail = createAsyncThunk(
  "[nama]/fetchDetail",
  async (id: string, { rejectWithValue }) => {
    try {
      const response = await get[NamaFitur]Detail(id);
      return response;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || "Failed to fetch detail");
    }
  }
);

export const create[NamaFitur] = createAsyncThunk(
  "[nama]/create",
  async (payload: [nama]FormSchema, { rejectWithValue }) => {
    try {
      const response = await create[NamaFitur](payload);
      return response;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || "Failed to create data");
    }
  }
);

export const update[NamaFitur] = createAsyncThunk(
  "[nama]/update",
  async ({ id, data }: { id: string; data: [nama]FormSchema }, { rejectWithValue }) => {
    try {
      const response = await update[NamaFitur](id, data);
      return response;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || "Failed to update data");
    }
  }
);

export const delete[NamaFitur] = createAsyncThunk(
  "[nama]/delete",
  async (id: string, { rejectWithValue }) => {
    try {
      await delete[NamaFitur](id);
      return id;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || "Failed to delete data");
    }
  }
);

const [nama]Slice = createSlice({
  name: "[nama]",
  initialState,
  reducers: {
    reset[NamaFitur]State: () => initialState,
  },
  extraReducers: (builder) => {
    builder
      // Fetch list
      .addCase(fetch[NamaFitur]List.pending, (state) => {
        state.isLoading = true;
        state.error = null;
      })
      .addCase(fetch[NamaFitur]List.fulfilled, (state, action) => {
        state.isLoading = false;
        state.data = action.payload;
      })
      .addCase(fetch[NamaFitur]List.rejected, (state, action) => {
        state.isLoading = false;
        state.error = action.payload as string;
      })
      // Fetch detail
      .addCase(fetch[NamaFitur]Detail.pending, (state) => {
        state.isLoading = true;
        state.error = null;
      })
      .addCase(fetch[NamaFitur]Detail.fulfilled, (state, action) => {
        state.isLoading = false;
        state.detail = action.payload;
      })
      .addCase(fetch[NamaFitur]Detail.rejected, (state, action) => {
        state.isLoading = false;
        state.error = action.payload as string;
      })
      // Create
      .addCase(create[NamaFitur].pending, (state) => {
        state.isSubmitting = true;
        state.error = null;
      })
      .addCase(create[NamaFitur].fulfilled, (state, action) => {
        state.isSubmitting = false;
        state.data.unshift(action.payload);
      })
      .addCase(create[NamaFitur].rejected, (state, action) => {
        state.isSubmitting = false;
        state.error = action.payload as string;
      })
      // Update
      .addCase(update[NamaFitur].pending, (state) => {
        state.isSubmitting = true;
        state.error = null;
      })
      .addCase(update[NamaFitur].fulfilled, (state, action) => {
        state.isSubmitting = false;
        const index = state.data.findIndex((item) => item.id === action.payload.id);
        if (index !== -1) {
          state.data[index] = action.payload;
        }
        state.detail = action.payload;
      })
      .addCase(update[NamaFitur].rejected, (state, action) => {
        state.isSubmitting = false;
        state.error = action.payload as string;
      })
      // Delete
      .addCase(delete[NamaFitur].pending, (state) => {
        state.isSubmitting = true;
        state.error = null;
      })
      .addCase(delete[NamaFitur].fulfilled, (state, action) => {
        state.isSubmitting = false;
        state.data = state.data.filter((item) => item.id !== action.payload);
        if (state.detail?.id === action.payload) {
          state.detail = null;
        }
      })
      .addCase(delete[NamaFitur].rejected, (state, action) => {
        state.isSubmitting = false;
        state.error = action.payload as string;
      });
  },
});

export const { reset[NamaFitur]State } = [nama]Slice.actions;
export default [nama]Slice.reducer;
```

#### 5. **components/[nama-fitur]-list.tsx**

Komponen untuk menampilkan daftar data.

```typescript
import { use[NamaFitur] } from "../hooks/use[NamaFitur]";
import { [nama]DataSchema } from "../types/schema";

interface [NamaFitur]ListProps {
  onEdit?: (item: [nama]DataSchema) => void;
  onDelete?: (id: string) => void;
}

export const [NamaFitur]List = ({ onEdit, onDelete }: [NamaFitur]ListProps) => {
  const { data, isLoading, error, getList } = use[NamaFitur]();

  if (isLoading) {
    return <div className="flex items-center justify-center h-64">Loading...</div>;
  }

  if (error) {
    return (
      <div className="flex items-center justify-center h-64">
        <p className="text-destructive">Error: {error}</p>
      </div>
    );
  }

  if (!data || data.length === 0) {
    return (
      <div className="flex items-center justify-center h-64">
        <p className="text-muted-foreground">No data available</p>
      </div>
    );
  }

  return (
    <div className="space-y-4">
      {data.map((item) => (
        <div
          key={item.id}
          className="bg-white rounded-lg border p-4 flex justify-between items-center"
        >
          <div>
            <h3 className="font-medium">{item.name}</h3>
            <p className="text-sm text-muted-foreground">{item.email}</p>
          </div>
          <div className="flex gap-2">
            <button
              onClick={() => onEdit?.(item)}
              className="px-3 py-1 text-sm bg-blue-100 text-blue-700 rounded hover:bg-blue-200"
            >
              Edit
            </button>
            <button
              onClick={() => onDelete?.(item.id)}
              className="px-3 py-1 text-sm bg-red-100 text-red-700 rounded hover:bg-red-200"
            >
              Delete
            </button>
          </div>
        </div>
      ))}
    </div>
  );
};
```

#### 6. **pages/[nama-fitur].tsx**

Halaman utama fitur.

```typescript
import { useState } from "react";
import { use[NamaFitur] } from "../hooks/use[NamaFitur]";
import { [NamaFitur]List } from "../components/[nama-fitur]-list";
import { [nama]DataSchema } from "../types/schema";

export const [NamaFitur]Page = () => {
  const [showForm, setShowForm] = useState(false);
  const [editingItem, setEditingItem] = useState<[nama]DataSchema | null>(null);
  const { remove } = use[NamaFitur]();

  const handleEdit = (item: [nama]DataSchema) => {
    setEditingItem(item);
    setShowForm(true);
  };

  const handleDelete = async (id: string) => {
    if (confirm("Are you sure you want to delete this item?")) {
      await remove(id);
    }
  };

  const handleSuccess = () => {
    setShowForm(false);
    setEditingItem(null);
  };

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">{capitalizedNama}</h1>
        <button
          onClick={() => setShowForm(true)}
          className="bg-primary text-white px-4 py-2 rounded-md hover:bg-primary/90"
        >
          Add New
        </button>
      </div>

      <[NamaFitur]List onEdit={handleEdit} onDelete={handleDelete} />

      {/* Form modal would go here */}
      {showForm && (
        <div className="fixed inset-0 bg-black/50 flex items-center justify-center z-50">
          <div className="bg-white rounded-lg p-6 w-full max-w-md">
            <h2 className="text-xl font-bold mb-4">
              {editingItem ? "Edit" : "Create"} {capitalizedNama}
            </h2>
            {/* Form content */}
            <div className="flex justify-end gap-2 mt-4">
              <button
                onClick={() => setShowForm(false)}
                className="px-4 py-2 border rounded hover:bg-gray-50"
              >
                Cancel
              </button>
              <button className="px-4 py-2 bg-primary text-white rounded hover:bg-primary/90">
                Save
              </button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};
```

#### 7. **route.ts**

Definisi routing untuk fitur.

```typescript
import { lazy } from "react";
import { [NamaFitur]Layout } from "./layouts/[nama-fitur]-layout";

const [NamaFitur]Page = lazy(() => import("./pages/[nama-fitur]"));

export const [nama]Base = "/[nama]";

export const [nama]Route = [
  {
    path: [nama]Base,
    Component: [NamaFitur]Layout,
    children: [
      {
        index: true,
        Component: [NamaFitur]Page,
      },
    ],
  },
];
```

#### 8. **index.ts (Barrel File)**

Mengekspor semua exports dari fitur untuk kemudahan import.

```typescript
export * from "./types/schema";
export * from "./hooks/use[NamaFitur]";
export * from "./slices/slice[NamaFitur]";
export * from "./api/get[NamaFitur]";
export * from "./components/[nama-fitur]-list";
export * from "./layouts/[nama-fitur]-layout";
export * from "./route";
```

## Contoh Lengkap: Membuat Feature "Products"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `feature`

### Step 2: Jawab Pertanyaan

```
? Apa nama fitur baru ini? (misal: products, auth, user-profile)
→ products

? Sebutkan data (pisahkan dengan koma)? (cth: id, name, email, role)
→ id, name, price, description, category, stock, status, createdAt
```

### Step 3: File yang Dihasilkan

```
src/features/products/
├── api/
│   └── getProducts.ts
├── assets/
│   └── .gitkeep
├── components/
│   └── products-list.tsx
├── hooks/
│   └── useProducts.ts
├── layouts/
│   └── products-layout.tsx
├── pages/
│   └── products.tsx
├── slices/
│   └── sliceProducts.tsx
├── stores/
│   └── storeProducts.tsx
├── types/
│   └── schema.ts
├── index.ts
└── route.ts
```

### Step 4: Integrasi ke Redux Store

Tambahkan reducer ke root store:

```typescript
// src/stores/index.ts
import { combineReducers } from "@reduxjs/toolkit";
import productsReducer from "../features/products/slices/sliceProducts";

export const rootReducer = combineReducers({
  products: productsReducer,
  // ... other reducers
});
```

### Step 5: Integrasi ke Main Routes

Tambahkan route ke `src/route.ts`:

```typescript
import { productsRoute } from "./features/products";

const router = createBrowserRouter([
  ...authRoute,
  ...[
    {
      Component: MainLayout,
      children: [
        ...homeRoute,
        ...productsRoute, // ← Tambahkan ini
        // ... routes lainnya
      ],
    },
  ],
]);
```

## Kustomisasi

### 1. Menambahkan Form Component

Setelah generate, Anda dapat menambahkan komponen form dengan generator `feature:form-hook`:

```bash
npm run generate
→ Pilih: feature:form-hook
→ Nama Form: product
→ Fields: name, price, description, category, stock, status
```

### 2. Menambahkan Datatable

Setelah generate, Anda dapat menambahkan datatable dengan generator `feature:datatable`:

```bash
npm run generate
→ Pilih: feature:datatable
→ Nama: product
→ Columns: name, price, category, stock, status
```

### 3. Menambahkan Async Select

Setelah generate, Anda dapat menambahkan async select dengan generator `feature:async-combobox`:

```bash
npm run generate
→ Pilih: feature:async-combobox
→ Nama Entity: Category
```

## Best Practices

### 1. Naming Convention

- **Folder**: lowercase dengan hyphen (`user-profile`, `student-management`)
- **Component**: PascalCase (`UserProfile`, `StudentForm`)
- **Function**: camelCase (`getUsers`, `fetchStudents`)
- **Type/Schema**: PascalCase (`UserSchema`, `StudentFormSchema`)

### 2. Redux State Management

```typescript
// ✅ Baik: Pisahkan state berdasarkan fitur
const productsSlice = createSlice({
  name: 'products',
  initialState,
  reducers: {},
});

// ❌ Hindari: State global yang terlalu besar
```

### 3. API Calls

```typescript
// ✅ Baik: Gunakan async/await dengan error handling
export const getProducts = async () => {
  try {
    const response = await api.get('/products');
    return response.data;
  } catch (error) {
    console.error('Failed to fetch products:', error);
    throw error;
  }
};

// ❌ Hindari: Tanpa error handling
```

### 4. Component Structure

```typescript
// ✅ Baik: Pisahkan container dan presentational components
export const ProductList = ({ data, onEdit, onDelete }) => {
  // Presentational component
};

export const ProductContainer = () => {
  const { data, remove } = useProducts();
  // Container component with logic
};
```

## Troubleshooting

### Redux Store Tidak Terdaftar

Pastikan reducer sudah ditambahkan ke root store:

```typescript
// src/stores/index.ts
import productsReducer from "../features/products/slices/sliceProducts";

export const rootReducer = combineReducers({
  products: productsReducer,
});
```

### Route Tidak Terdaftar

Pastikan route sudah ditambahkan ke router utama:

```typescript
// src/route.ts
import { productsRoute } from "./features/products";

const router = createBrowserRouter([
  {
    Component: MainLayout,
    children: [
      ...productsRoute,
    ],
  },
]);
```

### API Endpoint Salah

Pastikan endpoint API sesuai dengan backend:

```typescript
// api/getProducts.ts
export const getProducts = async () => {
  // Sesuaikan dengan endpoint yang benar
  const response = await api.get('/products');
  return response.data;
};
```

## Command Reference

```bash
# Generate basic feature
npm run generate
→ Pilih: feature

# Generate dengan subfolder
→ Nama fitur: masters/products

# Generate dengan data fields
→ Data fields: id, name, email, role, status
```

## Lihat Juga

- [Generator: Feature Datatable](./generator-datatable.md)
- [Generator: Feature Form Hook](./generator-form-hook.md)
- [Generator: Feature Page](./generator-page.md)
- [Generator: Feature Async Combobox](./generator-async-combobox.md)
- [Redux Toolkit Documentation](https://redux-toolkit.js.org/)
```