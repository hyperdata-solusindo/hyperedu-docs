---
title: "Sortable"
sidebar_label: "Sortable"
---

# Sortable

**Lokasi:** `components/custom/sortable.tsx`

## Deskripsi

Komponen untuk drag-and-drop sorting menggunakan dnd-kit-sortable-tree. Sortable digunakan untuk mengatur ulang urutan item dalam list atau menu dengan antarmuka drag-and-drop yang intuitif.

## Fitur

- Drag and drop sorting dengan animasi halus
- Visual feedback saat dragging
- Ikon untuk setiap item
- Handle custom styling
- Dukungan untuk nested items (tree structure)
- Collapse/expand untuk nested items

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Sortable` | Container utama untuk sortable list |
| `MinimalTreeItemComponent` | Komponen item internal dengan styling khusus |

## Type Definitions

```typescript
interface MinimalTreeItemData {
  name: string;
  icon: string;
}

type TreeItems<T> = Array<TreeItem<T>>;

interface TreeItem<T> {
  id: string;
  children?: TreeItems<T>;
  data?: T;
}
```

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `items` | `TreeItems<MinimalTreeItemData>` | Tidak | `[]` | Data items yang dapat di-sort |
| `onItemsChanged` | `(items: TreeItems<MinimalTreeItemData>) => void` | Tidak | - | Handler perubahan urutan |

## Contoh Penggunaan

### Basic Usage
```tsx
import { Sortable } from "@/components/custom/sortable";

function MenuSortable() {
  const [items, setItems] = useState([
    {
      id: "1",
      data: { name: "Dashboard", icon: "LayoutDashboard" },
    },
    {
      id: "2",
      data: { name: "Users", icon: "Users" },
    },
    {
      id: "3",
      data: { name: "Settings", icon: "Settings" },
    },
  ]);

  return (
    <Sortable
      items={items}
      onItemsChanged={setItems}
    />
  );
}
```

### Dengan Nested Items (Tree Structure)
```tsx
function NestedMenuSortable() {
  const [items, setItems] = useState([
    {
      id: "1",
      data: { name: "Dashboard", icon: "LayoutDashboard" },
    },
    {
      id: "2",
      data: { name: "User Management", icon: "Users" },
      children: [
        {
          id: "2-1",
          data: { name: "All Users", icon: "User" },
        },
        {
          id: "2-2",
          data: { name: "Roles", icon: "Shield" },
          children: [
            {
              id: "2-2-1",
              data: { name: "Permissions", icon: "Key" },
            },
          ],
        },
      ],
    },
    {
      id: "3",
      data: { name: "Settings", icon: "Settings" },
    },
  ]);

  return (
    <Sortable
      items={items}
      onItemsChanged={setItems}
    />
  );
}
```

### Untuk Menu Sidebar
```tsx
function SidebarMenuEditor() {
  const [menuItems, setMenuItems] = useState(initialMenuItems);

  const handleReorder = (newItems) => {
    setMenuItems(newItems);
    // Save to API
    saveMenuOrder(newItems);
  };

  return (
    <div className="space-y-4">
      <div className="flex justify-between items-center">
        <h3 className="text-lg font-semibold">Menu Order</h3>
        <Button size="sm" onClick={() => saveMenuOrder(menuItems)}>
          Save Order
        </Button>
      </div>
      
      <Card>
        <CardContent className="p-4">
          <Sortable
            items={menuItems}
            onItemsChanged={handleReorder}
          />
        </CardContent>
      </Card>
    </div>
  );
}
```

### Dengan Custom Styling
```tsx
function CustomStyledSortable() {
  const [items, setItems] = useState(menuItems);

  return (
    <div className="border rounded-lg p-2 bg-muted/30">
      <Sortable
        items={items}
        onItemsChanged={setItems}
      />
    </div>
  );
}
```

### Dengan Persist ke LocalStorage
```tsx
function PersistentSortable() {
  const [items, setItems] = useState(() => {
    const saved = localStorage.getItem("menu-order");
    return saved ? JSON.parse(saved) : defaultMenuItems;
  });

  const handleReorder = (newItems) => {
    setItems(newItems);
    localStorage.setItem("menu-order", JSON.stringify(newItems));
    toast.success("Menu order saved");
  };

  return (
    <Sortable
      items={items}
      onItemsChanged={handleReorder}
    />
  );
}
```

### Dengan API Save
```tsx
function SortableWithAPI() {
  const [items, setItems] = useState([]);
  const [isSaving, setIsSaving] = useState(false);
  const { data, isLoading } = useQuery({
    queryKey: ["menu"],
    queryFn: fetchMenu,
  });

  useEffect(() => {
    if (data) {
      setItems(data);
    }
  }, [data]);

  const handleReorder = async (newItems) => {
    setItems(newItems);
    setIsSaving(true);
    try {
      await api.put("/menu/order", { items: newItems });
      toast.success("Menu order updated");
    } catch (error) {
      toast.error("Failed to save order");
      // Rollback
      setItems(data);
    } finally {
      setIsSaving(false);
    }
  };

  if (isLoading) {
    return <Skeleton className="h-96 w-full" />;
  }

  return (
    <div>
      {isSaving && (
        <div className="mb-2 text-sm text-muted-foreground flex items-center gap-2">
          <Loader2Icon className="h-4 w-4 animate-spin" />
          Saving...
        </div>
      )}
      <Sortable
        items={items}
        onItemsChanged={handleReorder}
      />
    </div>
  );
}
```

### Dengan Konfirmasi Reset
```tsx
function SortableWithReset() {
  const [items, setItems] = useState(defaultMenuItems);
  const [originalItems, setOriginalItems] = useState(defaultMenuItems);

  const hasChanges = JSON.stringify(items) !== JSON.stringify(originalItems);

  const handleReset = () => {
    setItems(originalItems);
    toast.info("Changes discarded");
  };

  const handleSave = async () => {
    await saveToAPI(items);
    setOriginalItems(items);
    toast.success("Menu order saved");
  };

  return (
    <div className="space-y-4">
      <div className="flex justify-end gap-2">
        <Button
          variant="outline"
          onClick={handleReset}
          disabled={!hasChanges}
        >
          Reset
        </Button>
        <Button
          onClick={handleSave}
          disabled={!hasChanges}
        >
          Save Changes
        </Button>
      </div>
      
      <Sortable
        items={items}
        onItemsChanged={setItems}
      />
    </div>
  );
}
```

## Kustomisasi

### Custom Item Styling
Item styling dapat dimodifikasi melalui CSS classes di `MinimalTreeItemComponent`:

```tsx
const MinimalTreeItemComponent = React.forwardRef<HTMLDivElement, TreeItemComponentProps<MinimalTreeItemData>>(
  (props, ref) => (
    <SimpleTreeItemWrapper
      {...props}
      className={cn(
        '[&_div[class*="tree-item"]]:bg-sidebar-foreground!',
        '[&_div[class*="tree-item"]]:rounded-md!',
        '[&_div[class*="tree-item"]]:p-2!',
        '[&_div[class*="tree-item"]]:mb-.5!',
        '[&_div[class*="tree-item"]]:cursor-pointer!',
        '[&_div[class*="tree-item"]]:transition-opacity!',
        '[&_div[class*="tree-item"]]:duration-300!',
        '[&_div[class*="tree-item"]]:ease-in-out!',
        '[&_div[class*="tree-item"]]:border-input!',
        '[&_button[class*="tree-item-collapse"]]:hidden!',
        '[&_div[class*="handle"]]:hidden!',
        '[&_div[class*="tree-item"]]:hover:opacity-90!',
        props.className
      )}
      ref={ref}
    >
      <div className="w-full flex items-center justify-start gap-2 text-sidebar-accent text-sm">
        {props.item.data?.icon ? (
          <LazyIcon name={props.item.data.icon} className="w-4! h-4! text-card-foreground" />
        ) : (
          <div className="w-4" />
        )}
        <div className="flex-1 text-card-foreground">{props.item.data?.name}</div>
        <ChevronsUpDown className="w-3.5! h-3.5! ml-auto mr-1 text-card-foreground!" />
      </div>
    </SimpleTreeItemWrapper>
  )
);
```

### Custom Drag Handle
Drag handle dapat dikustomisasi dengan memodifikasi komponen.

## Best Practices

### 1. Simpan Order ke Backend
Selalu simpan urutan baru ke backend setelah perubahan:
```tsx
const handleReorder = async (newItems) => {
  setItems(newItems);
  await saveOrderToAPI(newItems);
};
```

### 2. Optimistic Updates
Gunakan optimistic update untuk UX yang lebih baik:
```tsx
const handleReorder = async (newItems) => {
  const oldItems = items;
  setItems(newItems);
  try {
    await saveOrder(newItems);
  } catch {
    setItems(oldItems);
    toast.error("Failed to save order");
  }
};
```

### 3. Loading State
Tampilkan loading state saat menyimpan perubahan.

### 4. Konfirmasi Reset
Sediakan tombol reset untuk membatalkan perubahan yang belum disimpan.

## Troubleshooting

### Drag Tidak Berfungsi
- Pastikan komponen dibungkus dengan provider yang sesuai
- Periksa apakah ada CSS yang mengganggu pointer events

### Item Tidak Bisa Dipindahkan
- Pastikan setiap item memiliki `id` yang unik
- Periksa struktur data sesuai dengan yang diharapkan

### Nested Items Tidak Bisa Di-collapse
- Button collapse disembunyikan secara default, modifikasi CSS untuk menampilkannya

## Referensi

- [dnd-kit Documentation](https://dndkit.com/)
- [dnd-kit-sortable-tree](https://github.com/wholevinski/dnd-kit-sortable-tree)
- [React DnD Best Practices](https://docs.dndkit.com/introduction)
