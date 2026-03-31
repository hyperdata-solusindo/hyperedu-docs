---
title: "Combobox"
sidebar_label: "Combobox"
---

# Combobox

**Lokasi:** `components/ui/combobox.tsx`  
**Dokumentasi Resmi:** [Shadcn Combobox](https://ui.shadcn.com/docs/components/combobox)

## Deskripsi

Komponen combobox dengan fitur pencarian, multiple selection, dan chips, dibangun di atas Base UI Combobox. Combobox digunakan untuk memilih satu atau lebih opsi dari daftar yang besar dengan dukungan pencarian real-time.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Combobox` | Container utama combobox |
| `ComboboxInput` | Input dengan trigger dan clear button terintegrasi |
| `ComboboxContent` | Dropdown content |
| `ComboboxList` | List item container dengan scroll |
| `ComboboxItem` | Item individual yang dapat dipilih |
| `ComboboxGroup` | Group items |
| `ComboboxLabel` | Label untuk group |
| `ComboboxEmpty` | Pesan saat tidak ada item |
| `ComboboxSeparator` | Separator antar items |
| `ComboboxChips` | Container untuk multiple selection chips |
| `ComboboxChip` | Chip untuk item terpilih |
| `ComboboxChipsInput` | Input dalam chips mode |
| `ComboboxTrigger` | Trigger button (untuk single selection) |
| `ComboboxValue` | Menampilkan nilai yang dipilih |
| `ComboboxObserver` | Observer untuk infinite scroll |

## Props

### ComboboxInput
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `showTrigger` | `boolean` | `true` | Menampilkan trigger dropdown |
| `showClear` | `boolean` | `false` | Menampilkan tombol clear |
| `disabled` | `boolean` | `false` | Menonaktifkan combobox |
| `placeholder` | `string` | - | Placeholder input |

### ComboboxItem
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `value` | `any` | - | Nilai item (untuk selection) |
| `disabled` | `boolean` | `false` | Menonaktifkan item |

## Contoh Penggunaan

### Basic Combobox (Single Selection)
```tsx
import {
  Combobox,
  ComboboxContent,
  ComboboxInput,
  ComboboxItem,
  ComboboxList,
} from "@/components/ui/combobox";

const frameworks = [
  { value: "react", label: "React" },
  { value: "vue", label: "Vue" },
  { value: "angular", label: "Angular" },
  { value: "svelte", label: "Svelte" },
];

<Combobox>
  <ComboboxInput placeholder="Search framework..." />
  <ComboboxContent>
    <ComboboxList>
      {frameworks.map((framework) => (
        <ComboboxItem key={framework.value} value={framework}>
          {framework.label}
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Controlled Combobox
```tsx
function ControlledCombobox() {
  const [value, setValue] = useState(null);

  return (
    <Combobox value={value} onValueChange={setValue}>
      <ComboboxInput placeholder="Select item..." />
      <ComboboxContent>
        <ComboboxList>
          {items.map((item) => (
            <ComboboxItem key={item.id} value={item}>
              {item.name}
            </ComboboxItem>
          ))}
        </ComboboxList>
      </ComboboxContent>
    </Combobox>
  );
}
```

### Multiple Selection Combobox (Dengan Chips)
```tsx
<Combobox multiple>
  <ComboboxChips placeholder="Select items...">
    <ComboboxChipsInput />
  </ComboboxChips>
  <ComboboxContent>
    <ComboboxList>
      {items.map((item) => (
        <ComboboxItem key={item.id} value={item}>
          {item.name}
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Combobox dengan Group
```tsx
<Combobox>
  <ComboboxInput placeholder="Search..." />
  <ComboboxContent>
    <ComboboxList>
      <ComboboxGroup>
        <ComboboxLabel>Frameworks</ComboboxLabel>
        <ComboboxItem value="react">React</ComboboxItem>
        <ComboboxItem value="vue">Vue</ComboboxItem>
        <ComboboxItem value="angular">Angular</ComboboxItem>
      </ComboboxGroup>
      <ComboboxGroup>
        <ComboboxLabel>Libraries</ComboboxLabel>
        <ComboboxItem value="lodash">Lodash</ComboboxItem>
        <ComboboxItem value="moment">Moment</ComboboxItem>
        <ComboboxItem value="axios">Axios</ComboboxItem>
      </ComboboxGroup>
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Combobox dengan Infinite Scroll (Observer)
```tsx
import { useIntersectionObserver } from "@/hooks/useIntersectionObserver";

function InfiniteCombobox() {
  const [items, setItems] = useState([]);
  const [page, setPage] = useState(1);
  const [hasMore, setHasMore] = useState(true);
  const [isLoading, setIsLoading] = useState(false);
  const [search, setSearch] = useState("");
  const [open, setOpen] = useState(false);

  const loadMoreRef = useRef(null);
  const isIntersecting = useIntersectionObserver(loadMoreRef);

  const loadItems = async () => {
    if (isLoading || !hasMore) return;
    setIsLoading(true);
    const newItems = await fetchItems(page, search);
    setItems((prev) => [...prev, ...newItems]);
    setHasMore(newItems.length === 20);
    setPage((prev) => prev + 1);
    setIsLoading(false);
  };

  useEffect(() => {
    if (isIntersecting && open) {
      loadItems();
    }
  }, [isIntersecting, open]);

  return (
    <Combobox open={open} onOpenChange={setOpen}>
      <ComboboxInput
        placeholder="Search items..."
        onChange={(e) => setSearch(e.target.value)}
      />
      <ComboboxContent>
        <ComboboxList>
          {items.map((item) => (
            <ComboboxItem key={item.id} value={item}>
              {item.name}
            </ComboboxItem>
          ))}
          <ComboboxObserver ref={loadMoreRef} isLoading={isLoading} />
        </ComboboxList>
      </ComboboxContent>
    </Combobox>
  );
}
```

### Combobox dengan Custom Render Item
```tsx
<Combobox>
  <ComboboxInput placeholder="Select user..." />
  <ComboboxContent>
    <ComboboxList>
      {users.map((user) => (
        <ComboboxItem key={user.id} value={user}>
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
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Combobox dengan Clear Button
```tsx
<Combobox>
  <ComboboxInput placeholder="Search..." showClear />
  <ComboboxContent>
    <ComboboxList>
      {items.map((item) => (
        <ComboboxItem key={item.id} value={item}>
          {item.name}
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Combobox dengan Disabled State
```tsx
<Combobox disabled>
  <ComboboxInput placeholder="Disabled combobox" />
  <ComboboxContent>
    <ComboboxList>
      <ComboboxItem value="1">Option 1</ComboboxItem>
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Combobox dengan Custom Trigger (Single Selection)
```tsx
import {
  Combobox,
  ComboboxContent,
  ComboboxList,
  ComboboxItem,
  ComboboxTrigger,
  ComboboxValue,
} from "@/components/ui/combobox";

<Combobox>
  <ComboboxTrigger
    render={
      <Button variant="outline" className="w-48 justify-between">
        <ComboboxValue placeholder="Select option" />
        <ChevronDownIcon className="ml-2 h-4 w-4" />
      </Button>
    }
  />
  <ComboboxContent>
    <ComboboxList>
      {items.map((item) => (
        <ComboboxItem key={item.id} value={item}>
          {item.name}
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Combobox dalam Form (React Hook Form)
```tsx
import { useForm, Controller } from "react-hook-form";

function ComboboxForm() {
  const { control, handleSubmit } = useForm();

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="category"
        control={control}
        rules={{ required: "Category is required" }}
        render={({ field, fieldState }) => (
          <div className="space-y-1.5">
            <Label>Category</Label>
            <Combobox
              value={field.value}
              onValueChange={field.onChange}
            >
              <ComboboxInput placeholder="Select category..." />
              <ComboboxContent>
                <ComboboxList>
                  {categories.map((cat) => (
                    <ComboboxItem key={cat.id} value={cat}>
                      {cat.name}
                    </ComboboxItem>
                  ))}
                </ComboboxList>
              </ComboboxContent>
            </Combobox>
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

### Combobox Multiple dengan Pre-selected Values
```tsx
const defaultValues = [item1, item2];

<Combobox multiple defaultValue={defaultValues}>
  <ComboboxChips placeholder="Select items...">
    <ComboboxChipsInput />
  </ComboboxChips>
  <ComboboxContent>
    <ComboboxList>
      {items.map((item) => (
        <ComboboxItem key={item.id} value={item}>
          {item.name}
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

## Kustomisasi

### Custom Styling
```tsx
<ComboboxInput className="bg-muted border-none" />
<ComboboxContent className="border-2 border-primary" />
<ComboboxItem className="data-highlighted:bg-primary/20" />
```

### Custom Chips
```tsx
<ComboboxChips className="bg-muted rounded-lg">
  <ComboboxChipsInput className="text-sm" />
</ComboboxChips>

<ComboboxChip className="bg-primary text-primary-foreground">
  <span>{chip.label}</span>
</ComboboxChip>
```

## Best Practices

### 1. Gunakan untuk Opsi yang Banyak
Combobox cocok untuk daftar opsi yang besar karena memiliki fitur pencarian:
```tsx
// ✅ Baik: 50+ opsi
<Combobox>...</Combobox>

// ❌ Hindari: Opsi sedikit (<5)
<Select>...</Select> // Gunakan Select
```

### 2. Debounce Pencarian untuk API Call
```tsx
const [search, setSearch] = useState("");
const debouncedSearch = useDebounce(search, 300);

useEffect(() => {
  if (debouncedSearch) {
    fetchOptions(debouncedSearch);
  }
}, [debouncedSearch]);
```

### 3. Multiple Selection dengan Chips
Gunakan chips mode untuk multiple selection agar UX lebih baik:
```tsx
<Combobox multiple>
  <ComboboxChips>...</ComboboxChips>
</Combobox>
```

## Troubleshooting

### Dropdown Tidak Muncul
- Pastikan `ComboboxContent` di-render dengan benar
- Periksa apakah ada CSS yang menimpa visibility

### Item Tidak Terpilih
- Pastikan `value` prop sesuai dengan data structure
- Untuk multiple selection, pastikan nilai adalah array

### Pencarian Tidak Berfungsi
- Combobox secara default melakukan filter client-side
- Untuk server-side, gunakan `onInputChange` dan update items secara manual

## Referensi

- [Shadcn Combobox Documentation](https://ui.shadcn.com/docs/components/combobox)
- [Base UI Combobox](https://base-ui.com/react/components/combobox)
- [WAI-ARIA Combobox Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/)
