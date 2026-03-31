---
title: Async Select
sidebar_label: Async Select
sidebar_position: 3
---
# Pembuatan Custom Hook Select

## Deskripsi

Custom hook select adalah solusi efektif untuk mengelola state dan logika select dropdown dalam aplikasi React, khususnya untuk data yang besar dan memerlukan pencarian dinamis. Hook ini memanfaatkan **@tanstack/react-query** sebagai library utama untuk state management query data, yang menyediakan caching otomatis, error handling, dan optimasi performa. Dengan integrasi debouncing pada pencarian dan infinite scrolling untuk pagination, hook ini memungkinkan pengembang untuk membuat komponen select yang responsif dan efisien tanpa perlu menangani kompleksitas query manual.

Hook ini sangat berguna untuk skenario di mana Anda perlu:

- **Pencarian real-time dengan debouncing**: Mengurangi beban server dengan menunda eksekusi pencarian selama beberapa milidetik, meningkatkan performa aplikasi.
- **Infinite scrolling**: Memuat data tambahan secara otomatis saat pengguna scroll, ideal untuk dataset besar tanpa membebani memori.
- **State management terintegrasi**: Mengelola status dropdown (open/close), nilai terpilih, dan input pencarian dalam satu hook yang konsisten.

Dengan menggunakan custom hook select, developer dapat fokus pada logika bisnis aplikasi sambil memastikan pengalaman pengguna yang mulus, termasuk loading states, error handling, dan optimasi query. Referensi utama: [@tanstack/react-query Documentation](https://tanstack.com/query/latest) untuk pemahaman mendalam tentang query management di React.

## Parameter

Hook menerima objek parameter dengan properti berikut (semua opsional):

|Nama|Tipe|Keterangan|
|---|---|---|
|`defaultValue`|`YourSelectSchema`|Nilai default yang akan di-set pada select|
|`pagination`|`{ length: number }`|Konfigurasi pagination, default length adalah 10|
|`open`|`boolean`|Status awal dropdown (tidak digunakan dalam implementasi saat ini)|

## Return Value

Hook mengembalikan objek dengan properti berikut:

|Nama|Tipe|Keterangan/Kegunaan|
|---|---|---|
|`items`|`YourSelectSchema[]`|Array data yang telah dimuat|
|`search`|`string`|String pencarian saat ini|
|`onSearch`|`(value: string) => void`|Function untuk mengubah nilai pencarian|
|`open`|`boolean`|Status dropdown terbuka/tutup|
|`onOpenChange`|`(value: boolean) => void`|Function untuk mengubah status dropdown|
|`value`|`YourSelectSchema \| any`|Nilai terpilih saat ini|
|`setValue`|`(value: YourSelectSchema \| any) => void`|Function untuk mengubah nilai terpilih|
|`isLoading`|`boolean`|Status loading untuk query awal|
|`isFetching`|`boolean`|Status fetching untuk semua query|
|`isFetchingNextPage`|`boolean`|Status fetching untuk halaman berikutnya|
|`onIntersectingScroll`|`() => void`|Function untuk memuat halaman berikutnya (digunakan untuk infinite scroll)|

## Struktur Custom Hook Select

Berikut adalah panduan langkah demi langkah untuk membuat custom hook select dengan fitur infinite scrolling dan debouncing, berdasarkan pola `useMenuSelect`. Setiap langkah dijelaskan secara detail.

### Langkah 1: Import Dependencies

```typescript
import { useMemo, useState } from "react";
import { useInfiniteQuery } from "@tanstack/react-query";
import { useDebounce } from "@/hooks/useDebounce";
import type { UseFormSelect } from "@/types";

// Import API function, query keys, dan schema sesuai kebutuhan
import { getSelectData } from "../api/getYourData";
import { yourKeys } from "../types/query-keys";
import type { YourSelectSchema } from "../types/schema";
```

**Penjelasan:**

- `useMemo` dan `useState` dari React untuk state management dan optimasi.
- `useInfiniteQuery` dari TanStack Query untuk data fetching dengan pagination.
- `useDebounce` untuk menunda pencarian.
- `UseFormSelect` sebagai type return interface.
- Import spesifik untuk API, keys, dan schema yang akan digunakan.

### Langkah 2: Define Hook Parameters

```typescript
export const useYourSelect = ({
  defaultValue,
  pagination,
}: {
  defaultValue?: YourSelectSchema;
  pagination?: { length: number };
  open?: boolean;
} = {}): UseFormSelect<YourSelectSchema> => {
```

**Penjelasan:**

- Parameter `defaultValue` untuk nilai awal select.
- `pagination` untuk mengatur jumlah item per halaman.
- `open` opsional (tidak selalu digunakan).
- Return type `UseFormSelect<YourSelectSchema>` untuk konsistensi interface.

### Langkah 3: Initialize State Variables

```typescript
const [open, setOpen] = useState(false);
const [value, setValue] = useState<YourSelectSchema | any>(defaultValue);
const [search, setSearch] = useState("");
```

**Penjelasan:**

- `open`: Mengontrol status dropdown terbuka/tutup.
- `value`: Menyimpan nilai terpilih saat ini.
- `search`: Menyimpan input pencarian pengguna.

### Langkah 4: Setup Debounced Search

```typescript
const debouncedSearch = useDebounce(search, 500);
```

**Penjelasan:**

- Menggunakan `useDebounce` untuk menunda eksekusi pencarian selama 500ms.
- Ini mengurangi frekuensi API calls saat pengguna mengetik, meningkatkan performa.

### Langkah 5: Configure Infinite Query

```typescript
const query = useInfiniteQuery({
  queryKey: yourKeys.select(debouncedSearch),
  queryFn: async ({ pageParam = 1 }) => {
    const result = await getSelectData(pageParam, pagination?.length || 10, debouncedSearch);
    return result;
  },
  initialPageParam: 1,
  getNextPageParam: (lastPage, allPages) => {
    return lastPage.length < (pagination?.length || 10) ? undefined : allPages.length + 1;
  },
});
```

**Penjelasan:**

- `queryKey`: Unik untuk setiap pencarian, memungkinkan caching per query.
- `queryFn`: Fungsi untuk fetch data, menggunakan `pageParam` untuk pagination.
- `initialPageParam`: Mulai dari halaman 1.
- `getNextPageParam`: Logika untuk menentukan halaman berikutnya berdasarkan panjang data.

### Langkah 6: Process Data with Memoization

```typescript
const items = useMemo(() => {
  return query.data?.pages.flatMap((page) => page) ?? [];
}, [query.data]);
```

**Penjelasan:**

- Menggabungkan semua halaman data menjadi satu array menggunakan `flatMap`.
- `useMemo` memastikan komputasi hanya terjadi saat `query.data` berubah, mengoptimalkan performa.

### Langkah 7: Return Hook Interface

```typescript
return {
  items,
  search,
  onSearch: setSearch,
  open,
  onOpenChange: setOpen,
  value,
  setValue,
  isLoading: query.isLoading,
  isFetching: query.isFetching,
  isFetchingNextPage: query.isFetchingNextPage,
  onIntersectingScroll: query.fetchNextPage,
};
```

**Penjelasan:**

- Mengembalikan objek dengan semua properti yang diperlukan untuk komponen select.
- Termasuk data (`items`), handlers (`onSearch`, `setValue`), dan status loading (`isLoading`, dll.).

Dengan mengikuti langkah-langkah ini, Anda dapat membuat custom hook select yang konsisten dan dapat digunakan kembali di berbagai komponen.

## Penulisan Custom Hook Select

Berikut adalah contoh lengkap penulisan custom hook select dengan infinite scrolling, debouncing, dan integrasi TanStack Query. Gunakan ini sebagai template untuk membuat hook select serupa:

```typescript
import { useMemo, useState } from "react";
import { useInfiniteQuery } from "@tanstack/react-query";
import { useDebounce } from "@/hooks/useDebounce";
import type { UseFormSelect } from "@/types";

// Ganti dengan API function Anda
import { getSelectData } from "../api/getYourData";
// Ganti dengan query keys Anda
import { yourKeys } from "../types/query-keys";
import type { YourSelectSchema } from "../types/schema";

export const useYourSelect = ({
  defaultValue,
  pagination,
}: {
  defaultValue?: YourSelectSchema;
  pagination?: { length: number };
  open?: boolean;
} = {}): UseFormSelect<YourSelectSchema> => {
  const [open, setOpen] = useState(false);
  const [value, setValue] = useState<YourSelectSchema | any>(defaultValue);
  const [search, setSearch] = useState("");

  const debouncedSearch = useDebounce(search, 500);

  const query = useInfiniteQuery({
    queryKey: yourKeys.select(debouncedSearch), // Ganti dengan query key Anda
    queryFn: async ({ pageParam = 1 }) => {
      // Ganti dengan API call Anda
      const result = await getSelectData(pageParam, pagination?.length || 10, debouncedSearch);
      return result;
    },
    initialPageParam: 1,
    getNextPageParam: (lastPage, allPages) => {
      return lastPage.length < (pagination?.length || 10) ? undefined : allPages.length + 1;
    },
  });

  const items = useMemo(() => {
    return query.data?.pages.flatMap((page) => page) ?? [];
  }, [query.data]);

  return {
    items,
    search,
    onSearch: setSearch,
    open,
    onOpenChange: setOpen,
    value,
    setValue,
    isLoading: query.isLoading,
    isFetching: query.isFetching,
    isFetchingNextPage: query.isFetchingNextPage,
    onIntersectingScroll: query.fetchNextPage,
  };
};
```

**Penjelasan Kode:**

1. **State Management**: Menggunakan `useState` untuk `open`, `value`, dan `search`.
2. **Debouncing**: `useDebounce` untuk menunda pencarian selama 500ms.
3. **Infinite Query**: `useInfiniteQuery` untuk pagination otomatis.
4. **Items Flattening**: `useMemo` untuk menggabungkan halaman data.
5. **Return Object**: Mengembalikan semua properti yang diperlukan untuk komponen select.

Sesuaikan `getSelectData`, `yourKeys`, dan `YourSelectSchema` dengan kebutuhan Anda.

**Catatan**

- Pencarian di-debounce selama 500ms untuk performa optimal
- Pagination default adalah 10 item per halaman
- Hook menggunakan `useInfiniteQuery` untuk mengelola state pagination secara otomatis

## Contoh Penggunaan

### Penggunaan Dasar

```typescript
import { useYourSelect } from '../hooks/useYourSelect';
import type { YourSelectSchema } from '../types/schema';
import { AsyncSelect } from '@/components/custom/async-select';

const MyComponent = () => {
  const {
    items,
    search,
    onSearch,
    open,
    onOpenChange,
    value,
    setValue,
    isLoading,
    isFetching,
    isFetchingNextPage,
    onIntersectingScroll,
  } = useYourSelect({
    defaultValue: { id: 1, name: 'Default Item' },
    pagination: { length: 20 },
  });

  return (
    <div>
      <AsyncSelect<YourSelectSchema>
        placeholder="Pilih item"
        items={items}
        value={value}
        onValueChange={setValue}
        search={search}
        onSearch={onSearch}
        open={open}
        onOpenChange={onOpenChange}
        isLoading={isLoading}
        isFetching={isFetching}
        isFetchingNextPage={isFetchingNextPage}
        onIntersectingScroll={onIntersectingScroll}
        renderItem={(item) => (
          <div key={item.id}>
            {item.name}
          </div>
        )}
        renderSelectedItem={(item) => item.name}
      />
    </div>
  );
};
```

atau bisa lebih singkat dengan cara berikut:

```typescript
import { useYourSelect } from '../hooks/useYourSelect';
import type { YourSelectSchema } from '../types/schema';
import { AsyncSelect } from '@/components/custom/async-select';

const MyComponent = () => {
  const selectProps = useYourSelect({
    defaultValue: { id: 1, text: 'Default Item' },
    pagination: { length: 20 },
  });

  return (
    <div>
      <AsyncSelect<YourSelectSchema>
        placeholder="Pilih item"
        renderItem={(item) => (
          <div key={item.id}>
            {item.text}
          </div>
        )}
        renderSelectedItem={(item) => item.text}
        {...selectProps}
      />
    </div>
  );
};
```

:::info Tips 
Pada penggunaan ini diperlukan untuk melakukan perubahan state secara manual untuk update nilai terpilih dari `AsyncSelect`. Disarankan implementasi dengan Custom Form Hook untuk penanganan perubahan state nilai terpilih 
:::

### Penggunaan dengan Form Hook

Untuk integrasi dengan react-hook-form, gunakan custom form hook yang sudah mengintegrasikan select hook:

```typescript
import { useYourForm } from '../hooks/useYourForm';
import { YourForm } from '../components/your-form';
import type { YourFormSchema, YourDataSchema } from '../types/schema';

const MyFormComponent = () => {
  const form = useYourForm();

  const onSubmit = (data: YourFormSchema) => {
    console.log('Form data:', data);
    // Handle submit logic
  };

  return (
    <YourForm
      form={form}
      onSubmit={onSubmit}
      state="create"
    />
  );
};
```

Dalam form, select menggunakan `registerSelect("field")` yang secara otomatis terhubung dengan select hook.

`registerSelect` fungsi bawaan dari Custom Hook Form yang mengembalikan objek dengan properti yang diperlukan untuk `AsyncSelect`, seperti:

- `value`: nilai terpilih
- `onValueChange`: handler untuk mengubah nilai
- `items`: array data untuk select
- `search`, `onSearch`: untuk pencarian
- `isLoading`, `isFetching`, dll.: status loading
- `onIntersectingScroll`: untuk infinite scroll

Contoh penggunaan manual `registerSelect`:

```typescript
const { registerSelect } = form;

// Di dalam JSX
<AsyncSelect placeholder="Pilih item" {...registerSelect("field")} />
```

Ini memungkinkan integrasi seamless antara form validation dan select dengan infinite scrolling.

## Best Practices

Untuk mendapatkan performa dan pengalaman pengguna terbaik saat menggunakan `useMenuSelect`, ikuti praktik-praktik berikut:

1. Optimalkan Debouncing

- Sesuaikan delay debouncing (default 500ms) berdasarkan kompleksitas API Anda.
- Untuk API cepat, kurangi delay; untuk API lambat, tingkatkan delay.

2. Kelola Query Keys dengan Baik

- Pastikan query keys unik untuk setiap kombinasi parameter.
- Gunakan struktur yang konsisten: `yourKeys.select(searchTerm)`.

3. Handle Error States

- Selalu tampilkan error UI saat query gagal.
- Gunakan `query.error` untuk menampilkan pesan error yang relevan.

4. Optimalkan Pagination

- Sesuaikan `pagination.length` berdasarkan ukuran data dan performa jaringan.
- Hindari pagination terlalu besar yang dapat memperlambat loading awal.

5. Gunakan Memoization dengan Bijak

- Pastikan `useMemo` untuk `items` hanya bergantung pada `query.data`.
- Hindari over-memoization yang tidak perlu.

6. Integrasi dengan Form

- Gunakan `useBindSelect` untuk integrasi seamless dengan react-hook-form.
- Pastikan schema form konsisten dengan select schema.

7. Monitor Performance

- Gunakan React DevTools untuk memantau re-renders.
- Pastikan infinite scroll tidak memicu fetch berlebihan.

## Troubleshooting

Berikut adalah masalah umum dan solusinya saat menggunakan `useMenuSelect`:

**Infinite Scroll Tidak Bekerja**

- **Masalah**: Data tidak dimuat saat scroll ke bawah.
- **Solusi**: Pastikan `onIntersectingScroll` dipanggil pada intersection observer. Periksa `getNextPageParam` logic – pastikan mengembalikan `undefined` saat tidak ada data lagi.

**Debouncing Tidak Efektif**

- **Masalah**: Terlalu banyak API calls meskipun menggunakan debouncing.
- **Solusi**: Periksa implementasi `useDebounce`. Pastikan delay cukup panjang (minimal 300ms). Verifikasi bahwa `debouncedSearch` digunakan di `queryKey`.

**Data Tidak Update Saat Pencarian**

- **Masalah**: Hasil pencarian tidak berubah.
- **Solusi**: Pastikan `queryKey` termasuk `debouncedSearch`. Bersihkan cache query jika diperlukan dengan `queryClient.invalidateQueries()`.

**Loading States Tidak Muncul**

- **Masalah**: UI tidak menampilkan loading meskipun query sedang berjalan.
- **Solusi**: Gunakan `isFetching` untuk semua fetch, `isLoading` untuk initial load, dan `isFetchingNextPage` untuk infinite scroll. Pastikan komponen UI merespons perubahan state ini.

**Memory Leaks atau Re-renders Berlebihan**

- **Masalah**: Komponen re-render terlalu sering.
- **Solusi**: Optimalkan dependencies di `useMemo` dan `useCallback`. Pastikan `items` hanya dihitung ulang saat `query.data` berubah. Gunakan `React.memo` pada komponen yang menggunakan hook.

**Error Handling Tidak Bekerja**

- **Masalah**: Error tidak ditangani dengan baik.
- **Solusi**: Implementasikan `query.error` handling. Gunakan `onError` callback di `useInfiniteQuery` untuk logging atau fallback. Pastikan UI menampilkan error message yang user-friendly.

**Performance Issue pada Large Datasets**

- **Masalah**: Lambat saat menangani banyak data.
- **Solusi**: Kurangi `pagination.length`. Implementasikan virtual scrolling di komponen UI. Optimalkan `renderItem` untuk menghindari re-renders berat.

**Integrasi Form Tidak Sinkron**

- **Masalah**: Nilai form dan select tidak sinkron.
- **Solusi**: Pastikan `registerSelect` digunakan dengan benar. Verifikasi bahwa `setValue` dari hook dipanggil saat form update. Gunakan `useBindSelect` untuk sinkronisasi otomatis.

Jika masalah berlanjut, periksa console untuk error TanStack Query dan React. Pastikan versi dependencies terbaru dan kompatibel.

## Referensi

- **@tanstack/react-query**: [https://tanstack.com/query/latest](https://tanstack.com/query/latest) - Library utama untuk query management dengan infinite scrolling dan caching.
- **useDebounce**: Hook internal untuk debouncing pencarian, mengurangi frekuensi API calls.
- **API function**: Fungsi untuk mengambil data select dari server (sesuaikan dengan endpoint Anda).
- **Query keys**: Definisi keys untuk caching query TanStack Query (sesuaikan dengan struktur Anda).
- **Type definitions**: Schema dan tipe data untuk select (sesuaikan dengan model data Anda).