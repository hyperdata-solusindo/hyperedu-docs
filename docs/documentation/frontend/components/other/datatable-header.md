---
title: "DataTableHeader"
sidebar_label: "DataTableHeader"
---

# DataTableHeader

**Lokasi:** `components/custom/datatable-header.tsx`

## Deskripsi

Komponen header kolom tabel dengan dukungan sorting. DataTableHeader menyediakan antarmuka yang konsisten untuk menampilkan header kolom yang dapat di-sort, dengan indikator visual berupa ikon panah atas/bawah yang menunjukkan arah sorting.

## Fitur

- Indikator visual untuk kolom yang dapat di-sort (sortable)
- Ikon panah atas/bawah untuk menunjukkan arah sorting
- Opacity dinamis untuk menunjukkan sorting aktif
- Cursor pointer pada kolom yang dapat di-sort
- Handler klik untuk toggle sorting (ascending/descending)
- Mendukung multiple column sorting

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `info` | `HeaderContext<any, any>` | Ya | - | Header context dari TanStack Table |
| `children` | `React.ReactNode` | Tidak | - | Konten header (biasanya nama kolom) |

## Cara Kerja

### Deteksi Sortable Column
Komponen memeriksa apakah kolom dapat di-sort dengan:
```typescript
const isSortable = info.table
  .getAllLeafColumns()
  .filter((column) => column.id === info.column.id && column.getCanSort()).length > 0;
```

### Deteksi Sorting Aktif
Mendeteksi apakah kolom sedang di-sort:
```typescript
const sorting = info.table.getState().sorting;
const indexSorted = sorting.findIndex((sort) => sort.id === info.column.id);
const isSorted = indexSorted != -1;
```

### Opacity Ikon
- **Up arrow**: Opacity 0.1 jika tidak di-sort atau sorting descending
- **Down arrow**: Opacity 0.1 jika tidak di-sort atau sorting ascending
- Opacity 1 untuk arah sorting yang aktif

### Handler Click
```typescript
onClick={() => isSortable && info.column.toggleSorting(info.column.getIsSorted() === "asc")}
```

## Contoh Penggunaan

### Basic Usage
```tsx
import { DataTableHeader } from "@/components/custom/datatable-header";

const columns: ColumnDef<User>[] = [
  {
    accessorKey: "name",
    header: ({ column }) => (
      <DataTableHeader info={{ column, table }}>
        Name
      </DataTableHeader>
    ),
  },
  {
    accessorKey: "email",
    header: ({ column }) => (
      <DataTableHeader info={{ column, table }}>
        Email
      </DataTableHeader>
    ),
  },
];
```

### Dengan Custom Children
```tsx
{
  accessorKey: "status",
  header: ({ column, table }) => (
    <DataTableHeader info={{ column, table }}>
      <div className="flex items-center gap-1">
        <StatusIcon className="h-4 w-4" />
        <span>Status</span>
      </div>
    </DataTableHeader>
  ),
}
```

### Dalam DataTable
```tsx
import { DataTable } from "@/components/custom/datatable";
import { DataTableHeader } from "@/components/custom/datatable-header";

const columns: ColumnDef<User>[] = [
  {
    id: "select",
    header: ({ table }) => (
      <Checkbox
        checked={table.getIsAllPageRowsSelected()}
        onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
      />
    ),
  },
  {
    accessorKey: "name",
    header: ({ column, table }) => (
      <DataTableHeader info={{ column, table }}>
        Name
      </DataTableHeader>
    ),
  },
  {
    accessorKey: "email",
    header: ({ column, table }) => (
      <DataTableHeader info={{ column, table }}>
        Email
      </DataTableHeader>
    ),
  },
  {
    accessorKey: "createdAt",
    header: ({ column, table }) => (
      <DataTableHeader info={{ column, table }}>
        Created At
      </DataTableHeader>
    ),
  },
  {
    id: "actions",
    cell: ({ row }) => (
      <div className="flex gap-2">
        <Button variant="ghost" size="icon">Edit</Button>
        <Button variant="ghost" size="icon">Delete</Button>
      </div>
    ),
  },
];

function UsersTable() {
  const [sorting, setSorting] = useState<SortingState>([]);
  
  const table = useReactTable({
    data,
    columns,
    state: { sorting },
    onSortingChange: setSorting,
    // ... other options
  });

  return (
    <Table>
      <TableHeader>
        {table.getHeaderGroups().map((headerGroup) => (
          <TableRow key={headerGroup.id}>
            {headerGroup.headers.map((header) => (
              <TableHead key={header.id}>
                {header.isPlaceholder
                  ? null
                  : flexRender(
                      header.column.columnDef.header,
                      header.getContext()
                    )}
              </TableHead>
            ))}
          </TableRow>
        ))}
      </TableHeader>
      {/* TableBody */}
    </Table>
  );
}
```

### Dengan Custom Styling
```tsx
{
  accessorKey: "price",
  header: ({ column, table }) => (
    <DataTableHeader info={{ column, table }}>
      <span className="text-right block">Price</span>
    </DataTableHeader>
  ),
  cell: ({ row }) => (
    <div className="text-right">
      ${row.getValue("price")}
    </div>
  ),
}
```

### Untuk Kolom yang Tidak Dapat Di-sort
```tsx
{
  accessorKey: "avatar",
  header: "Avatar",
  enableSorting: false, // DataTableHeader akan mendeteksi ini
  cell: ({ row }) => (
    <Avatar>
      <AvatarImage src={row.getValue("avatar")} />
    </Avatar>
  ),
}
```

## Kustomisasi

### Custom Icon Colors
```tsx
// Modifikasi ikon untuk warna yang berbeda
<MoveUp
  className="absolute top-1/2 -translate-y-1/2 -right-3 size-3.5"
  opacity={opacityUp}
  fill="blue"
  color="blue"
/>
```

### Custom Icon Size
```tsx
<MoveUp className="absolute top-1/2 -translate-y-1/2 -right-3 size-4" />
<MoveDown className="absolute top-1/2 -translate-y-1/2 -right-5 size-4" />
```

### Custom Positioning
```tsx
// Posisi ikon lebih dekat ke teks
<MoveUp className="absolute top-1/2 -translate-y-1/2 -right-2 size-3" />
<MoveDown className="absolute top-1/2 -translate-y-1/2 -right-4 size-3" />
```

## Best Practices

### 1. Gunakan untuk Semua Kolom yang Dapat Di-sort
```tsx
// ✅ Baik: Gunakan DataTableHeader untuk kolom sortable
{
  accessorKey: "name",
  header: ({ column, table }) => (
    <DataTableHeader info={{ column, table }}>
      Name
    </DataTableHeader>
  ),
}

// ❌ Hindari: Manual implementation
{
  accessorKey: "name",
  header: ({ column }) => (
    <div onClick={() => column.toggleSorting()}>
      Name
      {column.getIsSorted() === "asc" ? " ↑" : " ↓"}
    </div>
  ),
}
```

### 2. Konsisten dengan Alignment
Pastikan alignment teks dan ikon konsisten:
```tsx
// Center alignment (default)
<DataTableHeader info={info}>Name</DataTableHeader>

// Left alignment
<DataTableHeader info={info}>
  <div className="text-left">Name</div>
</DataTableHeader>
```

### 3. Handle Multi-column Sorting
DataTableHeader mendukung multi-column sorting secara native dari TanStack Table.

## Troubleshooting

### Ikon Tidak Muncul
- Periksa apakah kolom memiliki `enableSorting: true` (default)
- Pastikan `info` context di-pass dengan benar

### Sorting Tidak Berfungsi
- Pastikan `manualSorting` tidak di-set ke `true` jika tidak diperlukan
- Periksa handler `onSortingChange` di table

### Ikon Terpotong
- Pastikan `TableHead` memiliki padding yang cukup
- Sesuaikan posisi ikon dengan mengubah nilai `-right`

## Referensi

- [TanStack Table Sorting Guide](https://tanstack.com/table/v8/docs/guide/sorting)
- [Lucide Icons - MoveUp](https://lucide.dev/icons/move-up)
- [Lucide Icons - MoveDown](https://lucide.dev/icons/move-down)
