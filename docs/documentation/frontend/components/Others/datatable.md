---
title: "DataTable"
sidebar_label: "DataTable"
---

# DataTable

**Lokasi:** `components/custom/datatable.tsx`

## Deskripsi

Komponen tabel data yang powerful dengan fitur sorting, filtering, pagination, column visibility, dan row selection. DataTable dibangun di atas TanStack Table (React Table) dan menyediakan antarmuka yang konsisten untuk menampilkan dan mengelola data tabular.

## Fitur

- Sorting manual dengan query params
- Filtering global
- Column visibility toggle
- Pagination manual dengan custom page sizes
- Loading overlay dengan spinner
- Row selection dengan checkbox
- Responsive design
- Custom title dan actions
- Refresh data functionality

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `columns` | `ColumnDef<TData, TValue>[]` | Ya | - | Definisi kolom tabel |
| `data` | `TData[]` | Ya | - | Data yang ditampilkan |
| `pagination` | `PaginationState` | Ya | - | State pagination (pageIndex, pageSize) |
| `defaultSorting` | `SortingState` | Tidak | `[]` | Default sorting |
| `rowCount` | `number` | Ya | - | Jumlah total baris |
| `totalItems` | `number` | Tidak | - | Jumlah total item (untuk info) |
| `isLoading` | `boolean` | Tidak | `false` | State loading |
| `lengths` | `number[]` | Tidak | `[10, 25, 50, 100]` | Opsi rows per page |
| `title` | `React.ReactNode` | Tidak | - | Judul tabel |
| `actions` | `React.ReactNode[]` | Tidak | - | Tombol aksi tambahan |
| `onRefresh` | `() => Promise<void> \| void` | Tidak | - | Handler refresh data |
| `hideColumnsButton` | `boolean` | Tidak | `true` | Sembunyikan tombol columns |
| `hideSearch` | `boolean` | Tidak | `true` | Sembunyikan pencarian |
| `hideActions` | `boolean` | Tidak | `false` | Sembunyikan area actions |
| `meta` | `any` | Tidak | - | Metadata tambahan untuk table |

## Type Definitions

```typescript
declare module "@tanstack/react-table" {
  interface TableMeta<TData extends RowData> {
    refreshData: () => Promise<void> | void;
  }
}
```

## Contoh Penggunaan

### Basic Usage
```tsx
import { DataTable } from "@/components/custom/datatable";
import { ColumnDef } from "@tanstack/react-table";

interface User {
  id: string;
  name: string;
  email: string;
  role: string;
}

const columns: ColumnDef<User>[] = [
  {
    accessorKey: "name",
    header: "Name",
  },
  {
    accessorKey: "email",
    header: "Email",
  },
  {
    accessorKey: "role",
    header: "Role",
  },
];

function UsersTable() {
  const [pagination, setPagination] = useState({
    pageIndex: 1,
    pageSize: 10,
  });

  const { data, total, isLoading } = useUsers(pagination);

  return (
    <DataTable
      columns={columns}
      data={data}
      pagination={pagination}
      rowCount={total}
      totalItems={total}
      isLoading={isLoading}
    />
  );
}
```

### Dengan Sorting
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  defaultSorting={[{ id: "name", desc: false }]}
  rowCount={total}
/>
```

### Dengan Search
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
  hideSearch={false}
/>
```

### Dengan Actions
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
  actions={[
    <Button key="export" variant="outline" size="sm">
      <DownloadIcon className="mr-2 h-4 w-4" />
      Export
    </Button>,
    <Button key="add" size="sm">
      <PlusIcon className="mr-2 h-4 w-4" />
      Add User
    </Button>,
  ]}
/>
```

### Dengan Title
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
  title={
    <div className="flex items-center gap-2">
      <UsersIcon className="h-5 w-5" />
      <span className="font-semibold">User Management</span>
    </div>
  }
/>
```

### Dengan Refresh
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
  onRefresh={refetch}
/>
```

### Dengan Column Visibility
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
  hideColumnsButton={false} // Menampilkan tombol columns
/>
```

### Dengan Custom Page Sizes
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
  lengths={[5, 10, 20, 50, 100]}
/>
```

### Dengan Row Selection
```tsx
const columns: ColumnDef<User>[] = [
  {
    id: "select",
    header: ({ table }) => (
      <Checkbox
        checked={table.getIsAllPageRowsSelected()}
        onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
      />
    ),
    cell: ({ row }) => (
      <Checkbox
        checked={row.getIsSelected()}
        onCheckedChange={(value) => row.toggleSelected(!!value)}
      />
    ),
  },
  // ... other columns
];

<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
/>
```

## Kustomisasi Kolom

### Header dengan Sorting
```tsx
{
  accessorKey: "name",
  header: ({ column }) => (
    <Button
      variant="ghost"
      onClick={() => column.toggleSorting(column.getIsSorted() === "asc")}
    >
      Name
      <ArrowUpDown className="ml-2 h-4 w-4" />
    </Button>
  ),
}
```

### Cell dengan Badge
```tsx
{
  accessorKey: "status",
  header: "Status",
  cell: ({ row }) => {
    const status = row.getValue("status") as string;
    return (
      <Badge variant={status === "active" ? "success" : "warning"}>
        {status}
      </Badge>
    );
  },
}
```

### Cell dengan Actions
```tsx
{
  id: "actions",
  cell: ({ row }) => {
    const user = row.original;
    return (
      <div className="flex gap-2">
        <Button variant="ghost" size="icon-xs" onClick={() => onEdit(user)}>
          <EditIcon className="h-3 w-3" />
        </Button>
        <Button variant="ghost" size="icon-xs" onClick={() => onDelete(user)}>
          <TrashIcon className="h-3 w-3" />
        </Button>
      </div>
    );
  },
}
```

### Cell dengan Format Tanggal
```tsx
{
  accessorKey: "createdAt",
  header: "Created At",
  cell: ({ row }) => {
    const date = row.getValue("createdAt") as string;
    return new Date(date).toLocaleDateString("id-ID");
  },
}
```

## DataTableTitle Component

### Props
| Prop | Tipe | Deskripsi |
|------|------|-----------|
| `title` | `string` | Judul tabel |
| `Icon` | `LucideIcon` | Ikon untuk judul |

### Contoh Penggunaan
```tsx
import { DataTableTitle } from "@/components/custom/datatable";
import { UsersIcon } from "lucide-react";

<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={total}
  title={<DataTableTitle title="Users" Icon={UsersIcon} />}
/>
```

## Best Practices

### 1. Manual Pagination
Gunakan manual pagination untuk data dari API:
```tsx
const table = useReactTable({
  data,
  columns,
  manualPagination: true,
  pageCount: Math.ceil(rowCount / pagination.pageSize),
  state: { pagination },
  // ...
});
```

### 2. Query Params Sync
Sinkronkan state tabel dengan URL query params:
```tsx
const params = new URLSearchParams(searchParams);
const page = Number(params.get("page")) || 1;
const limit = Number(params.get("limit")) || 10;
const search = params.get("search") || "";
```

### 3. Loading Overlay
Gunakan loading overlay untuk feedback visual:
```tsx
{isLoading && (
  <div className="absolute inset-0 z-10 flex items-center justify-center bg-white/50 backdrop-blur-[2px]">
    <Loader2 className="h-8 w-8 animate-spin text-primary" />
  </div>
)}
```

### 4. Empty State
Tampilkan pesan yang jelas saat tidak ada data:
```tsx
{table.getRowModel().rows?.length ? (
  // render rows
) : (
  <TableRow>
    <TableCell colSpan={columns.length} className="h-32 text-center text-muted-foreground">
      No data available.
    </TableCell>
  </TableRow>
)}
```

## Troubleshooting

### Sorting Tidak Bekerja
- Pastikan `manualSorting` di-set ke `true`
- Periksa apakah sort state di-sinkronkan dengan URL

### Pagination Tidak Bekerja
- Pastikan `manualPagination` di-set ke `true`
- Periksa `rowCount` di-set dengan total data dari API

### Column Visibility Tidak Bekerja
- Pastikan `enableHiding` tidak di-set ke `false` pada kolom
- Periksa `hideColumnsButton` tidak `true`

## Referensi

- [TanStack Table Documentation](https://tanstack.com/table/latest)
- [Shadcn Table Documentation](https://ui.shadcn.com/docs/components/table)
- [React Table Guide](https://tanstack.com/table/v8/docs/guide/introduction)
