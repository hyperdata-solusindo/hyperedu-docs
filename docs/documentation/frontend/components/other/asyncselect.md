---
title: "AsyncSelect"
sidebar_label: "AsyncSelect"
---

# AsyncSelect

**Lokasi:** `components/custom/async-select.tsx`

## Deskripsi

Komponen select asinkron komprehensif dengan pencarian, state loading, pagination, dan multiple selection. AsyncSelect dirancang untuk menangani data dari API dengan jumlah besar, mendukung infinite scroll, debounced search, dan berbagai opsi kustomisasi.

## Fitur

- Pencarian dengan debounce bawaan
- Infinite scrolling dengan intersection observer
- State loading untuk initial load dan pagination
- Multiple selection support
- Multiple size variants (xs, sm, default)
- Custom rendering untuk item dan nilai terpilih
- Styling kustom menggunakan class-variance-authority
- Tombol clear untuk reset pilihan
- Support untuk disabled state

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `multiple` | `boolean` | Tidak | `false` | Mengaktifkan multiple selection |
| `disabled` | `boolean` | Tidak | `false` | Menonaktifkan komponen |
| `open` | `boolean` | Tidak | `false` | Mengontrol visibilitas dropdown (controlled) |
| `showClear` | `boolean` | Tidak | `false` | Menampilkan tombol clear |
| `items` | `TData[]` | Tidak | `[]` | Array item yang ditampilkan |
| `value` | `any` | Tidak | - | Nilai yang dipilih (controlled) |
| `defaultValue` | `any` | Tidak | - | Nilai default (uncontrolled) |
| `isFetching` | `boolean` | Tidak | `false` | State fetching (initial load) |
| `isLoading` | `boolean` | Tidak | `false` | State loading |
| `isFetchingNextPage` | `boolean` | Tidak | `false` | State fetching halaman berikutnya |
| `placeholder` | `string` | Tidak | `"Choose select"` | Placeholder input |
| `variant` | `"default"` | Tidak | `"default"` | Varian visual |
| `size` | `"default" \| "sm" \| "xs"` | Tidak | `"default"` | Ukuran komponen |
| `onIntersectingScroll` | `() => void` | Tidak | - | Handler infinite scroll (dipanggil saat scroll mencapai bottom) |
| `onRefetch` | `() => void` | Tidak | - | Handler refetch data |
| `onSearch` | `(value: string) => void` | Tidak | - | Handler pencarian |
| `onChange` | `(value: any) => void` | Tidak | - | Handler perubahan nilai |
| `onOpenChange` | `(value: boolean) => void` | Tidak | - | Handler perubahan open state |
| `renderItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item dalam dropdown |
| `renderSelectedItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item yang terpilih |

## Type Definitions

```typescript
interface AsyncSelectOption {
  id: any;
  text: string;
  // Additional fields can be added
}

type AsyncSelectProps<TData = AsyncSelectOption | AsyncSelectOption[]> = {
  // ... props
}
```

## Ukuran

| Ukuran | Height | Font Size | Penggunaan |
|--------|--------|-----------|------------|
| **xs** | `h-7` | `12px` | Select kecil di dalam tabel atau toolbar |
| **sm** | `h-8` | `12px` | Select compact |
| **default** | `h-9` | `12px` | Select standar |

## Contoh Penggunaan

### Basic Usage
```tsx
import { AsyncSelect } from "@/components/custom/async-select";

const [selected, setSelected] = useState(null);
const [search, setSearch] = useState("");
const [items, setItems] = useState([]);
const [isLoading, setIsLoading] = useState(false);

<AsyncSelect
  items={items}
  value={selected}
  onChange={setSelected}
  onSearch={setSearch}
  isLoading={isLoading}
  placeholder="Pilih kategori..."
/>
```

### Dengan Data dari API
```tsx
function CategorySelect() {
  const [selected, setSelected] = useState(null);
  const [search, setSearch] = useState("");
  const [page, setPage] = useState(1);
  const [items, setItems] = useState([]);
  const [hasMore, setHasMore] = useState(true);
  const [isLoading, setIsLoading] = useState(false);
  const [isFetchingNext, setIsFetchingNext] = useState(false);

  const fetchData = async (searchTerm: string, pageNum: number) => {
    const response = await api.get("/categories", {
      params: { search: searchTerm, page: pageNum, limit: 20 }
    });
    return response.data;
  };

  const loadData = async () => {
    setIsLoading(true);
    const data = await fetchData(search, 1);
    setItems(data.items);
    setHasMore(data.hasMore);
    setPage(2);
    setIsLoading(false);
  };

  const loadMore = async () => {
    if (isFetchingNext || !hasMore) return;
    setIsFetchingNext(true);
    const data = await fetchData(search, page);
    setItems(prev => [...prev, ...data.items]);
    setHasMore(data.hasMore);
    setPage(prev => prev + 1);
    setIsFetchingNext(false);
  };

  useEffect(() => {
    loadData();
  }, [search]);

  return (
    <AsyncSelect
      items={items}
      value={selected}
      onChange={setSelected}
      onSearch={setSearch}
      onIntersectingScroll={loadMore}
      isLoading={isLoading}
      isFetchingNextPage={isFetchingNext}
      placeholder="Pilih kategori..."
    />
  );
}
```

### Dengan Custom Render Item
```tsx
<AsyncSelect
  items={categories}
  value={selectedCategory}
  onChange={setSelectedCategory}
  renderItem={(category) => (
    <div className="flex flex-col">
      <span className="font-medium">{category.name}</span>
      <span className="text-xs text-muted-foreground">
        Code: {category.code}
      </span>
    </div>
  )}
  renderSelectedItem={(category) => (
    <div className="flex items-center gap-2">
      <span>{category.name}</span>
      <Badge variant="outline">{category.code}</Badge>
    </div>
  )}
/>
```

### Multiple Selection
```tsx
const [selected, setSelected] = useState([]);

<AsyncSelect
  multiple
  items={users}
  value={selected}
  onChange={setSelected}
  placeholder="Pilih pengguna..."
  renderSelectedItem={(user) => (
    <Badge variant="secondary" className="mr-1">
      {user.name}
    </Badge>
  )}
/>
```

### Dengan Clear Button
```tsx
<AsyncSelect
  items={items}
  value={selected}
  onChange={setSelected}
  showClear
  placeholder="Pilih opsi..."
/>
```

### Dengan Ukuran Berbeda
```tsx
<div className="space-y-4">
  <AsyncSelect size="xs" placeholder="Extra Small" />
  <AsyncSelect size="sm" placeholder="Small" />
  <AsyncSelect size="default" placeholder="Default" />
</div>
```

### Dalam Form (React Hook Form)
```tsx
import { useForm, Controller } from "react-hook-form";

function ProductForm() {
  const { control, handleSubmit } = useForm();

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="categoryId"
        control={control}
        rules={{ required: "Category is required" }}
        render={({ field, fieldState }) => (
          <div className="space-y-1.5">
            <Label>Category</Label>
            <AsyncSelect
              items={categories}
              value={field.value}
              onChange={field.onChange}
              onSearch={handleSearch}
              isLoading={isLoadingCategories}
              placeholder="Pilih kategori..."
            />
            {fieldState.error && (
              <p className="text-xs text-red-500">{fieldState.error.message}</p>
            )}
          </div>
        )}
      />
      <Button type="submit">Submit</Button>
    </form>
  );
}
```

### Dengan Debounced Search (Built-in)
```tsx
// AsyncSelect sudah memiliki debounce internal untuk onSearch
// Cukup pass handler yang akan dipanggil setelah user selesai mengetik
<AsyncSelect
  items={items}
  onSearch={(searchTerm) => {
    console.log("Search term after debounce:", searchTerm);
    // Fetch data with searchTerm
  }}
/>
```

## Kustomisasi

### Custom Styling dengan Variant
```tsx
<AsyncSelect
  variant="default"
  className="[&_[role=trigger]]:bg-muted"
/>
```

### Custom Width
```tsx
<AsyncSelect className="w-64" />
```

### Custom Dropdown Position
```tsx
<AsyncSelect
  // Dropdown akan muncul di atas trigger
  className="[&_[data-side=bottom]]:data-[state=open]:animate-in"
/>
```

## Best Practices

### 1. Implementasi Infinite Scroll
Gunakan `onIntersectingScroll` untuk load more data:
```tsx
const loadMore = async () => {
  if (isFetchingNext || !hasMore) return;
  setIsFetchingNext(true);
  const newData = await fetchData(page);
  setItems(prev => [...prev, ...newData]);
  setHasMore(newData.length === pageSize);
  setPage(prev => prev + 1);
  setIsFetchingNext(false);
};
```

### 2. Debounced Search
AsyncSelect sudah memiliki debounce internal, cukup pass handler yang akan dipanggil setelah user selesai mengetik:
```tsx
const handleSearch = useCallback((searchTerm: string) => {
  // Fetch data with searchTerm
  fetchOptions(searchTerm);
}, []);
```

### 3. Loading States
Gunakan ketiga loading states untuk UX yang lebih baik:
- `isLoading`: untuk initial load
- `isFetching`: untuk search/re-fetch
- `isFetchingNextPage`: untuk infinite scroll

### 4. Error Handling
Tampilkan error state jika fetch gagal:
```tsx
if (error) {
  return (
    <div className="text-destructive text-sm p-2 border rounded-md">
      Failed to load data. 
      <button onClick={refetch} className="underline ml-2">Retry</button>
    </div>
  );
}
```

## Troubleshooting

### Data Tidak Muncul
- Periksa format data: setiap item harus memiliki `id` dan `text` property
- Pastikan `items` array tidak kosong dan sudah di-set

### Infinite Scroll Tidak Berfungsi
- Pastikan `onIntersectingScroll` di-pass
- Periksa `hasMore` state untuk menghentikan fetch lebih lanjut

### Search Tidak Memicu Fetch
- Pastikan `onSearch` handler diimplementasikan
- Periksa bahwa search term di-pass ke fungsi fetch

## Referensi

- [Shadcn Select Documentation](https://ui.shadcn.com/docs/components/select)
- [Base UI Combobox](https://base-ui.com/react/components/combobox)
- [React Hook Form Integration](https://react-hook-form.com/api/usecontroller/controller)
