---
title: "TanStack + Feature"
sidebar_label: "TanStack + Feature"
---

# Generator: TanStack + Feature

## Gambaran Umum

**TanStack + Feature** adalah tools otomatis untuk membuat struktur fitur lengkap yang mengintegrasikan TanStack React Query, Redux, dan berbagai komponen UI yang kompleks. Generator ini menghasilkan arsitektur modular yang siap pakai dengan routing, API layer, state management, dan form handling.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"TanStack + feature"** dari menu.

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

### Tahap 2: Konfigurasi Datatable (Opsional)

```
? Include Datatable? (Y/n)
```

Tentukan apakah fitur memerlukan komponen tabel untuk menampilkan data.

- **Yes** → Lanjut ke pemilihan kolom
- **No** → Lewati ke tahap berikutnya

Jika memilih **Yes**, akan diminta memilih kolom mana saja yang ditampilkan di tabel:

```
? Pilih data untuk kolom tabel:
❯ ◯ id
  ◯ name
  ◯ email
  ◯ role
  ◯ status
  ◯ createdAt
```

**Tips:**

- Gunakan `Space` untuk memilih/membatalkan
- Gunakan `Arrow Keys` untuk navigasi
- Gunakan `Enter` untuk konfirmasi

### Tahap 3: Konfigurasi Form Hook (Inti)

```
? Include Custom Form Hook? (Y/n)
```

Pilih apakah fitur memerlukan form untuk create/edit data.

Jika **Yes**, akan diminta:

1. **Pilih field untuk form:**

```
? Pilih data untuk input form:
❯ ◯ id
  ◯ name
  ◯ email
  ◯ role
  ◯ status
  ◯ createdAt
```

2. **Tentukan tipe input untuk setiap field:**

Untuk setiap field yang dipilih, akan ditanyakan tipe inputnya:

```
? Tipe input untuk field 'name'?
❯ Input Text
  Textarea
  Select Dropdown
  Checkbox/Switch
```

**Jenis-jenis Input:**

| Tipe                | Deskripsi                    | Kasus Penggunaan        |
| ------------------- | ---------------------------- | ----------------------- |
| **Input Text**      | Input field untuk teks biasa | nama, email, nomor      |
| **Textarea**        | Area teks multiline          | deskripsi, bio, catatan |
| **Select Dropdown** | Dropdown untuk memilih opsi  | role, status, kategori  |
| **Checkbox/Switch** | Toggle boolean               | aktif/nonaktif, setujui |

**Contoh Flow:**

```
? Pilih data untuk input form:
  ◉ name
  ◉ email
  ◉ role
  ○ createdAt
  (3 dipilih)

--- Konfigurasi Tipe Field ---

? Tipe input untuk field 'name'?
❯ Input Text

? Tipe input untuk field 'email'?
❯ Input Text

? Tipe input untuk field 'role'?
❯ Select Dropdown
```

### Tahap 4: Konfigurasi Fitur Tambahan

```
? Include Async Select Hook? (Y/n)
```

Untuk dropdown yang data-nya diambil dari API dengan pagination & search.

**Kapan gunakan:**

- Ada field dropdown yang data source-nya dari backend API
- Memerlukan search/autocomplete functionality
- Data terlalu banyak untuk di-hardcode

```
? Include Edit/Create Page? (Y/n)
```

Pilih apakah fitur memerlukan page terpisah untuk create dan edit data.

Jika **Yes**, akan ditanyakan jenis halaman:

```
? Jenis Halaman?
❯ ◉ Create
  ◉ Edit
```

Pilih kedua-duanya untuk membuat page create dan edit, atau salah satu saja sesuai kebutuhan.

## Output File yang Dihasilkan

### Struktur Folder

```
src/features/[nama-fitur]/
├── api/
│   └── get[NamaFitur].ts          # API calls dan data fetching
├── assets/
│   └── .gitkeep                   # Folder untuk aset lokal fitur
├── components/
│   ├── [nama-fitur]-list.tsx      # Component list data
│   ├── [nama-fitur]-table.tsx     # Component datatable (optional)
│   └── [nama-fitur]-form.tsx      # Component form (optional)
├── hooks/
│   ├── use[NamaFitur].ts          # Hook untuk fetch data
│   ├── use[NamaFitur]Select.ts    # Hook untuk async select (optional)
│   └── use[NamaFitur]Form.ts      # Hook untuk form (optional)
├── layouts/
│   └── [nama-fitur]-layout.tsx    # Layout fitur
├── pages/
│   ├── [nama-fitur].tsx           # Main page
│   ├── [nama-fitur]-create.tsx    # Create page (optional)
│   └── [nama-fitur]-edit.tsx      # Edit page (optional)
├── slices/
│   └── slice[NamaFitur].tsx       # Redux slice untuk state fitur
├── stores/
│   └── store[NamaFitur].tsx       # Redux store config
├── types/
│   ├── schema.ts                  # Zod schemas untuk validasi
│   └── query-keys.ts              # TanStack Query keys
├── index.ts                       # Barrel file (export semua)
└── route.ts                       # Route definition fitur
```

### Deskripsi File-File Utama

#### 1. **api/get[NamaFitur].ts**

Berisi semua fungsi API untuk fetch data dari backend.

```typescript
// Contoh: products feature

export const getSelectData = async (pageNum, pageSize, searchTerm)
// → Fetch data untuk async select dengan pagination & search

export const getData = async ()
// → Fetch semua data produk

export const getPaginatedData = async (params)
// → Fetch data dengan pagination

export const getDetail = async (id)
// → Fetch detail produk berdasarkan ID

export const create = async (payload)
// → Create produk baru

export const update = async (id, payload)
// → Update produk

export const delete = async (id)
// → Hapus produk
```

#### 2. **types/schema.ts**

Validasi data menggunakan Zod schema.

```typescript
export const [nama]DataSchema = z.object({
  // Field yang dipilih
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
  // ...
})

export type [nama]DataSchema = z.infer<typeof [nama]DataSchema>

export const [nama]FormSchema = z.object({
  // Field yang akan disubmit form
})

export const [nama]SelectSchema = z.object({
  id: z.string(),
  text: z.string(),
})
```

#### 3. **types/query-keys.ts**

Konfigurasi query keys untuk TanStack Query (React Query).

```typescript
export const [nama]QueryKeys = {
  all: ['[nama]'] as const,
  lists: () => [...[nama]QueryKeys.all, 'list'] as const,
  detail: (id: string) => [...[nama]QueryKeys.all, 'detail', id] as const,
  // ... dll
}
```

#### 4. **hooks/use[NamaFitur].ts**

Hook utama untuk fetch data menggunakan TanStack Query.

```typescript
export const use[NamaFitur] = () => {
  const dispatch = useAppDispatch();

  // Query untuk fetch semua data
  const { data, isLoading, error } = useQuery({
    queryKey: [...keys.lists()],
    queryFn: getData,
  });

  // Mutation untuk create
  const createMutation = useMutation({
    mutationFn: create,
    onSuccess: () => queryClient.invalidateQueries(...),
  });

  // Mutation untuk update
  const updateMutation = useMutation({
    mutationFn: (data) => update(data.id, data),
  });

  // Mutation untuk delete
  const deleteMutation = useMutation({
    mutationFn: delete,
  });

  return {
    data,
    isLoading,
    error,
    createMutation,
    updateMutation,
    deleteMutation,
  }
}
```

#### 5. **hooks/use[NamaFitur]Form.ts**

Hook untuk mengelola form menggunakan React Hook Form.

```typescript
export const use[NamaFitur]Form = (initialData?: FormSchema) => {
  const form = useForm<FormSchema>({
    resolver: zodResolver([nama]FormSchema),
    defaultValues: initialData,
    mode: 'onChange',
  });

  const { handleSubmit, watch, formState: { errors } } = form;

  const onSubmit = async (data: FormSchema) => {
    // Handle submit
  };

  return {
    form,
    onSubmit: handleSubmit(onSubmit),
    errors,
    // ...
  }
}
```

#### 6. **components/[nama-fitur]-form.tsx**

Komponen form yang sudah terintegrasi dengan React Hook Form.

```typescript
export const [NamaFitur]Form = ({ initialData }: Props) => {
  const { form, onSubmit, errors } = use[NamaFitur]Form(initialData);

  return (
    <form onSubmit={onSubmit} className="space-y-4">
      {/* Field-field sesuai konfigurasi */}
      {/* Input text, textarea, select, checkbox */}
      <button type="submit">Simpan</button>
    </form>
  );
}
```

#### 7. **components/[nama-fitur]-table.tsx**

Komponen tabel dengan TanStack Table untuk menampilkan data list.

```typescript
export const [NamaFitur]Table = ({ data }: Props) => {
  const table = useReactTable({
    data,
    columns: [
      // Kolom sesuai pilihan di tahap 2
    ],
  });

  return (
    <DataTable table={table} />
  );
}
```

#### 8. **slices/slice[NamaFitur].tsx**

Redux slice untuk state lokal fitur.

```typescript
const slice = createSlice({
  name: "[nama]",
  initialState,
  reducers: {
    setFilter(state, action) {
      /* ... */
    },
    setSort(state, action) {
      /* ... */
    },
    // ... action lainnya
  },
});
```

#### 9. **route.ts**

Definisi routing untuk fitur.

```typescript
export const [nama]Base = "/[nama]"

export const [nama]Route = [
  {
    Component: [NamaFitur]Layout,
    children: [
      {
        path: [nama]Base,
        Component: [NamaFitur],
      },
      // Create & Edit pages (jika dipilih)
    ]
  }
]
```

#### 10. **index.ts (Barrel File)**

Mengekspor semua exports dari fitur untuk kemudahan import.

```typescript
export * from "./types/schema";
export * from "./types/query-keys";
export * from "./hooks/use[NamaFitur]";
export * from "./hooks/use[NamaFitur]Form"; // (jika ada)
export * from "./hooks/use[NamaFitur]Select"; // (jika ada)
export * from "./slices/slice[NamaFitur]";
export * from "./stores/store[NamaFitur]";
export * from "./route";
```

## Contoh Lengkap: Membuat Feature "Products"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `TanStack + feature`

### Step 2: Jawab Pertanyaan

```
? Apa nama fitur baru ini?
→ products

? Sebutkan data (pisahkan dengan koma)?
→ id, name, price, description, category, stock, status

? Include Datatable?
→ Yes

? Pilih data untuk kolom tabel:
→ name, price, category, stock, status

? Include Custom Form Hook?
→ Yes

? Pilih data untuk input form:
→ name, price, description, category, stock, status

? Tipe input untuk field 'name'?
→ Input Text

? Tipe input untuk field 'price'?
→ Input Text

? Tipe input untuk field 'description'?
→ Textarea

? Tipe input untuk field 'category'?
→ Select Dropdown

? Tipe input untuk field 'stock'?
→ Input Text

? Tipe input untuk field 'status'?
→ Checkbox/Switch

? Include Async Select Hook?
→ Yes (untuk fetch category)

? Include Edit/Create Page?
→ Yes

? Jenis Halaman?
→ Create, Edit
```

### Step 3: File yang Dihasilkan

```
src/features/products/
├── api/
│   └── getProducts.ts
├── components/
│   ├── products-list.tsx
│   ├── products-table.tsx
│   └── products-form.tsx
├── hooks/
│   ├── useProducts.ts
│   ├── useProductsSelect.ts (untuk category)
│   └── useProductsForm.ts
├── layouts/
│   └── products-layout.tsx
├── pages/
│   ├── products.tsx
│   ├── products-create.tsx
│   └── products-edit.tsx
├── slices/
│   └── sliceProducts.tsx
├── stores/
│   └── storeProducts.tsx
├── types/
│   ├── schema.ts
│   └── query-keys.ts
├── index.ts
└── route.ts
```

### Step 4: Integrasi ke Main Routes

Tambahkan ke `src/route.ts`:

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

## Best Practices

### 1. Naming Convention

- **Folder**: lowercase dengan hyphen (`user-profile`, `student-management`)
- **Component**: PascalCase (`UserProfile`, `StudentForm`)
- **Function**: camelCase (`getUsers`, `fetchStudents`)
- **Type/Schema**: PascalCase (`UserSchema`, `StudentFormSchema`)

### 2. Field Configuration

```typescript
// ✅ Baik: Explicit field configuration
{
  type: 'input',
  name: 'email',
  validation: z.string().email()
}

// ❌ Hindari: Mengubah tipe field setelah generate
// Lebih baik re-generate
```

### 3. Form Submission

```typescript
// ✅ Baik: Handle success dan error
const mutation = useMutation({
  mutationFn: createProduct,
  onSuccess: () => {
    queryClient.invalidateQueries(queryKeys);
    toast.success("Berhasil disimpan");
    navigate("/products");
  },
  onError: (error) => {
    toast.error(error.message);
  },
});

// ❌ Hindari: Tidak ada feedback ke user
```

### 4. Query Optimization

```typescript
// ✅ Baik: Gunakan query keys yang proper
const queryKeys = {
  all: ['products'],
  lists: () => [...queryKeys.all, 'list'],
  detail: (id) => [...queryKeys.all, 'detail', id]
}

// ❌ Hindari: Query keys yang tidak konsisten
['products'], ['productList'], ['prod']
```

## Troubleshooting

### File sudah ada, tidak di-generate ulang

Generator menggunakan `skipIfExists: true`, jadi jika file sudah ada tidak akan di-override. Solusi:

```bash
# Hapus folder feature lama
rm -rf src/features/[nama-fitur]

# Jalankan generator lagi
npm run generate
```

### Field tidak sesuai yang diinginkan

Jika field configuration tidak sesuai, bisa manual edit file setelah di-generate di:

- `types/schema.ts` - ubah Zod schema
- `hooks/use[NamaFitur]Form.ts` - ubah React Hook Form config

### Template error

Pastikan:

- Tidak ada spasi lebih di nama field
- Nama fitur tidak menggunakan spasi
- Format nama konsisten (lowercase)

## Command Reference

### Generate dengan Berbagai Options

```bash
# Generate feature lengkap dengan datatable & form
npm run generate
→ Pilih: TanStack + feature

# Generate hanya form-hook ke existing feature
npm run generate
→ Pilih: TanStack + feature:form-hook

# Generate hanya datatable
npm run generate
→ Pilih: TanStack + feature:datatable

# Generate hanya CRUD hooks (mutations)
npm run generate
→ Pilih: TanStack + feature:crud-hook
```

## Lihat Juga

- [Arsitektur Keseluruhan](./architecture.md)
- [TanStack React Query Documentation](https://tanstack.com/query/latest)
- [React Hook Form Documentation](https://react-hook-form.com/)
