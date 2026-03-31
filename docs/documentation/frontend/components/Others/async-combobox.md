---
title: "AsyncCombobox"
sidebar_label: "AsyncCombobox"
---

# AsyncCombobox

**Lokasi:** `components/custom/async-combobox.tsx`

## Deskripsi

Komponen combobox asinkron yang mendukung pencarian, state loading, infinite scrolling, dan multiple selection. AsyncCombobox adalah versi yang lebih ringan dari AsyncSelect, menggunakan Base UI Combobox primitives untuk performa yang lebih baik.

## Fitur

- Memuat data secara asinkron
- Pencarian dengan debounce (ditangani eksternal)
- Infinite scrolling dengan observer
- Dukungan multiple selection
- Custom rendering untuk item dan nilai terpilih
- Tombol clear untuk reset pilihan
- Dukungan modal mode untuk render dalam dialog

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `refSearch` | `RefObject<HTMLInputElement>` | Tidak | - | Referensi ke input pencarian |
| `refLoadMore` | `RefObject<HTMLDivElement>` | Tidak | - | Referensi untuk infinite scroll |
| `open` | `boolean` | Ya | - | Mengontrol visibilitas dropdown |
| `placeholder` | `string` | Tidak | - | Placeholder input |
| `value` | `any` | Tidak | - | Nilai yang dipilih |
| `disabled` | `boolean` | Tidak | `false` | Menonaktifkan komponen |
| `defaultValue` | `any` | Tidak | - | Nilai default |
| `multiple` | `boolean` | Tidak | `false` | Mengaktifkan multiple selection |
| `showClear` | `boolean` | Tidak | `true` | Menampilkan tombol clear |
| `isLoading` | `boolean` | Ya | - | State loading |
| `items` | `TData[]` | Ya | - | Array item yang ditampilkan |
| `modal` | `boolean` | Tidak | `false` | Render dalam konteks modal |
| `onOpenChange` | `(value: boolean) => void` | Ya | - | Handler perubahan open state |
| `onValueChange` | `(value: any) => void` | Tidak | - | Handler perubahan nilai |
| `onSearch` | `(value: any) => void` | Ya | - | Handler pencarian |
| `renderItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item dalam dropdown |
| `renderSelectedItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item yang terpilih |

## Type Definitions

```typescript
interface AsyncComboboxOption {
  id: string;
  text: string;
  // Additional fields can be added
}

type AsyncComboboxProps<TData = AsyncComboboxOption | any> = {
  // ... props
}
```

## Contoh Penggunaan

### Basic Usage
```tsx
import { AsyncCombobox } from "@/components/custom/async-combobox";

const [open, setOpen] = useState(false);
const [selected, setSelected] = useState(null);
const [search, setSearch] = useState("");
const [items, setItems] = useState([]);
const [isLoading, setIsLoading] = useState(false);

<AsyncCombobox
  open={open}
  onOpenChange={setOpen}
  items={items}
  value={selected}
  onValueChange={setSelected}
  onSearch={setSearch}
  isLoading={isLoading}
  placeholder="Pilih kategori..."
/>
```

### Dengan Data dari API
```tsx
function CategoryCombobox() {
  const [open, setOpen] = useState(false);
  const [selected, setSelected] = useState(null);
  const [search, setSearch] = useState("");
  const [page, setPage] = useState(1);
  const [items, setItems] = useState([]);
  const [hasMore, setHasMore] = useState(true);
  const [isLoading, setIsLoading] = useState(false);
  const [isFetchingNext, setIsFetchingNext] = useState(false);
  
  const loadMoreRef = useRef<HTMLDivElement>(null);

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
    if (open) {
      loadData();
    }
  }, [search, open]);

  return (
    <AsyncCombobox
      open={open}
      onOpenChange={setOpen}
      items={items}
      value={selected}
      onValueChange={setSelected}
      onSearch={setSearch}
      isLoading={isLoading}
      refLoadMore={loadMoreRef}
      placeholder="Pilih kategori..."
    />
  );
}
```

### Dengan Custom Render Item
```tsx
<AsyncCombobox
  open={open}
  onOpenChange={setOpen}
  items={users}
  value={selectedUser}
  onValueChange={setSelectedUser}
  onSearch={handleSearch}
  isLoading={isLoading}
  renderItem={(user) => (
    <div className="flex items-center gap-2">
      <Avatar className="h-6 w-6">
        <AvatarImage src={user.avatar} />
        <AvatarFallback>{user.name[0]}</AvatarFallback>
      </Avatar>
      <div className="flex flex-col">
        <span className="font-medium">{user.name}</span>
        <span className="text-xs text-muted-foreground">{user.email}</span>
      </div>
    </div>
  )}
  renderSelectedItem={(user) => (
    <div className="flex items-center gap-2">
      <span>{user.name}</span>
      <Badge variant="outline" className="text-xs">
        {user.email}
      </Badge>
    </div>
  )}
/>
```

### Multiple Selection
```tsx
const [selected, setSelected] = useState([]);

<AsyncCombobox
  multiple
  open={open}
  onOpenChange={setOpen}
  items={tags}
  value={selected}
  onValueChange={setSelected}
  onSearch={handleSearch}
  isLoading={isLoading}
  placeholder="Pilih tag..."
  renderSelectedItem={(tag) => (
    <span className="bg-primary/10 text-primary px-2 py-0.5 rounded-full text-xs">
      {tag.name}
    </span>
  )}
/>
```

### Dengan Clear Button
```tsx
<AsyncCombobox
  open={open}
  onOpenChange={setOpen}
  items={items}
  value={selected}
  onValueChange={setSelected}
  onSearch={handleSearch}
  isLoading={isLoading}
  showClear
  placeholder="Pilih opsi..."
/>
```

### Dalam Modal
```tsx
<Dialog open={dialogOpen} onOpenChange={setDialogOpen}>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Pilih Kategori</DialogTitle>
    </DialogHeader>
    <AsyncCombobox
      modal
      open={open}
      onOpenChange={setOpen}
      items={categories}
      value={selected}
      onValueChange={setSelected}
      onSearch={handleSearch}
      isLoading={isLoading}
      placeholder="Cari kategori..."
    />
  </DialogContent>
</Dialog>
```

### Dengan Ref untuk Search Input
```tsx
const searchInputRef = useRef<HTMLInputElement>(null);

useEffect(() => {
  if (open) {
    setTimeout(() => {
      searchInputRef.current?.focus();
    }, 100);
  }
}, [open]);

<AsyncCombobox
  refSearch={searchInputRef}
  open={open}
  onOpenChange={setOpen}
  items={items}
  value={selected}
  onValueChange={setSelected}
  onSearch={handleSearch}
  isLoading={isLoading}
  placeholder="Cari..."
/>
```

### Dalam Form (React Hook Form)
```tsx
import { useForm, Controller } from "react-hook-form";

function ProductForm() {
  const { control, handleSubmit } = useForm();
  const [open, setOpen] = useState(false);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="categoryId"
        control={control}
        rules={{ required: "Category is required" }}
        render={({ field, fieldState }) => (
          <div className="space-y-1.5">
            <Label>Category</Label>
            <AsyncCombobox
              open={open}
              onOpenChange={setOpen}
              items={categories}
              value={field.value}
              onValueChange={field.onChange}
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

### Dengan Infinite Scroll Observer
```tsx
// Pasang refLoadMore untuk infinite scroll
const loadMoreRef = useRef<HTMLDivElement>(null);

useIntersectionObserver(loadMoreRef, () => {
  if (hasMore && !isFetchingNext) {
    loadMore();
  }
});

<AsyncCombobox
  refLoadMore={loadMoreRef}
  open={open}
  onOpenChange={setOpen}
  items={items}
  value={selected}
  onValueChange={setSelected}
  onSearch={handleSearch}
  isLoading={isLoading}
  placeholder="Pilih item..."
/>
```

## Kustomisasi

### Custom Styling
```tsx
<AsyncCombobox
  className="[&_[role=trigger]]:bg-muted [&_[role=trigger]]:rounded-full"
  // ... props
/>
```

### Custom Dropdown Width
```tsx
<AsyncCombobox
  className="[&_[data-slot=combobox-content]]:w-96"
  // ... props
/>
```

## Best Practices

### 1. Handle Open State
Gunakan state untuk mengontrol open/close:
```tsx
const [open, setOpen] = useState(false);

<AsyncCombobox
  open={open}
  onOpenChange={setOpen}
  // ... props
/>
```

### 2. Reset Data on Close
Reset data saat dropdown ditutup untuk mengurangi memory:
```tsx
const handleOpenChange = (isOpen: boolean) => {
  setOpen(isOpen);
  if (!isOpen) {
    setItems([]);
    setSearch("");
    setPage(1);
  }
};
```

### 3. Debounce Search External
AsyncCombobox tidak memiliki debounce internal, implementasikan di luar:
```tsx
const debouncedSearch = useDebounce(search, 500);

useEffect(() => {
  if (open) {
    fetchData(debouncedSearch);
  }
}, [debouncedSearch, open]);
```

### 4. Focus Management
Set focus pada search input saat dropdown terbuka:
```tsx
useEffect(() => {
  if (open) {
    setTimeout(() => {
      searchInputRef.current?.focus();
    }, 100);
  }
}, [open]);
```

## Troubleshooting

### Dropdown Tidak Muncul
- Pastikan `open` state di-set ke `true`
- Periksa `onOpenChange` handler sudah diimplementasikan

### Data Tidak Muncul
- Periksa format data: setiap item harus memiliki `id` dan `text`
- Pastikan `items` array tidak kosong

### Infinite Scroll Tidak Berfungsi
- Pastikan `refLoadMore` di-pass
- Implementasikan observer untuk mendeteksi saat elemen terlihat

## Referensi

- [Base UI Combobox Documentation](https://base-ui.com/react/components/combobox)
- [Shadcn Combobox Documentation](https://ui.shadcn.com/docs/components/combobox)
- [React Hook Form Controller](https://react-hook-form.com/api/usecontroller/controller)
