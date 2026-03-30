---
title: "Feature: Datatable"
sidebar_label: "TanStack + Feature: Datatable"
---

# Generator: TanStack + Feature: Datatable

## Gambaran Umum

**TanStack + Feature: Datatable** adalah generator yang digunakan untuk membuat komponen tabel data lengkap dengan menggunakan **TanStack Table** (React Table). Generator ini menghasilkan komponen tabel yang mendukung sorting, filtering, pagination, column visibility, dan selection.

Generator ini cocok digunakan ketika Anda sudah memiliki struktur fitur dan hanya perlu menambahkan komponen tabel untuk menampilkan data list.

## Cara Menggunakan

### 1. Jalankan Perintah Generator

```bash
npm run generate
```

Kemudian pilih opsi **"TanStack + feature:datatable"** dari menu.

### 2. Ikuti Proses Interaktif

Generator akan mengajukan serangkaian pertanyaan untuk mengkonfigurasi datatable yang akan dibuat.

## Tahap-Tahap Pertanyaan

### Tahap 1: Konfigurasi Dasar

```
? Nama Component/Table? (cth: user)
```

Masukkan nama tabel dalam format:

- **lowercase**: `user`, `product`, `category`
- **lowercase dengan hyphen**: `user-list`, `product-table`

**Contoh:**
```
user
```

```
? Target Directory? (default: "src/features/user")
```

Direktori target tempat file akan dibuat. Default akan mengarah ke folder fitur sesuai nama yang dimasukkan.

**Contoh:**
```
src/features/user
```

### Tahap 2: Konfigurasi Kolom

```
? Sebutkan kolom (pisahkan dengan koma)? (cth: id, name, email, role)
```

Masukkan daftar kolom yang akan ditampilkan dalam tabel. Pisahkan dengan koma tanpa spasi tambahan.

**Contoh:**
```
id, name, email, role, status, createdAt
```

## Output File yang Dihasilkan

### Struktur Folder

```
src/features/[nama-fitur]/
├── components/
│   └── [nama-fitur]-table.tsx          # Komponen datatable
└── types/
    └── schema.ts                       # Diperbarui dengan datatable schema
```

### Deskripsi File-File Utama

#### 1. **components/[nama-fitur]-table.tsx**

Komponen tabel dengan TanStack Table yang sudah dikonfigurasi.

```typescript
"use client";

import * as React from "react";
import {
  ColumnDef,
  ColumnFiltersState,
  SortingState,
  VisibilityState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from "@tanstack/react-table";
import { ArrowUpDown, ChevronDown, MoreHorizontal } from "lucide-react";

import { Button } from "@/components/ui/button";
import { Checkbox } from "@/components/ui/checkbox";
import {
  DropdownMenu,
  DropdownMenuCheckboxItem,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import { Input } from "@/components/ui/input";
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";
import { Badge } from "@/components/ui/badge";

import { [nama]DataTableSchema, type [nama]DataTableSchema } from "../types/schema";

interface [NamaFitur]TableProps {
  data: [nama]DataTableSchema[];
  isLoading?: boolean;
  onEdit?: (item: [nama]DataTableSchema) => void;
  onDelete?: (item: [nama]DataTableSchema) => void;
  onView?: (item: [nama]DataTableSchema) => void;
}

export const [NamaFitur]Table = ({
  data,
  isLoading = false,
  onEdit,
  onDelete,
  onView,
}: [NamaFitur]TableProps) => {
  const [sorting, setSorting] = React.useState<SortingState>([]);
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>([]);
  const [columnVisibility, setColumnVisibility] = React.useState<VisibilityState>({});
  const [rowSelection, setRowSelection] = React.useState({});

  // Definisikan kolom berdasarkan input user
  const columns: ColumnDef<[nama]DataTableSchema>[] = [
    // Kolom selection checkbox
    {
      id: "select",
      header: ({ table }) => (
        <Checkbox
          checked={
            table.getIsAllPageRowsSelected() ||
            (table.getIsSomePageRowsSelected() && "indeterminate")
          }
          onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
          aria-label="Select all"
        />
      ),
      cell: ({ row }) => (
        <Checkbox
          checked={row.getIsSelected()}
          onCheckedChange={(value) => row.toggleSelected(!!value)}
          aria-label="Select row"
        />
      ),
      enableSorting: false,
      enableHiding: false,
    },
    // Kolom ID
    {
      accessorKey: "id",
      header: ({ column }) => {
        return (
          <Button
            variant="ghost"
            onClick={() => column.toggleSorting(column.getIsSorted() === "asc")}
            className="p-0 hover:bg-transparent"
          >
            ID
            <ArrowUpDown className="ml-2 h-4 w-4" />
          </Button>
        );
      },
      cell: ({ row }) => <div className="font-mono text-xs">{row.getValue("id")}</div>,
    },
    // Kolom Name
    {
      accessorKey: "name",
      header: ({ column }) => {
        return (
          <Button
            variant="ghost"
            onClick={() => column.toggleSorting(column.getIsSorted() === "asc")}
            className="p-0 hover:bg-transparent"
          >
            Name
            <ArrowUpDown className="ml-2 h-4 w-4" />
          </Button>
        );
      },
      cell: ({ row }) => <div className="font-medium">{row.getValue("name")}</div>,
    },
    // Kolom Email
    {
      accessorKey: "email",
      header: ({ column }) => {
        return (
          <Button
            variant="ghost"
            onClick={() => column.toggleSorting(column.getIsSorted() === "asc")}
            className="p-0 hover:bg-transparent"
          >
            Email
            <ArrowUpDown className="ml-2 h-4 w-4" />
          </Button>
        );
      },
      cell: ({ row }) => <div className="lowercase">{row.getValue("email")}</div>,
    },
    // Kolom Role
    {
      accessorKey: "role",
      header: "Role",
      cell: ({ row }) => {
        const role = row.getValue("role") as string;
        return (
          <Badge variant="outline" className="capitalize">
            {role}
          </Badge>
        );
      },
    },
    // Kolom Status
    {
      accessorKey: "status",
      header: "Status",
      cell: ({ row }) => {
        const status = row.getValue("status") as string;
        const variant = status === "active" ? "success" : status === "inactive" ? "warning" : "info";
        return (
          <Badge variant={variant as any} className="capitalize">
            {status}
          </Badge>
        );
      },
    },
    // Kolom Created At
    {
      accessorKey: "createdAt",
      header: "Created At",
      cell: ({ row }) => {
        const date = row.getValue("createdAt") as string;
        return <div className="text-xs text-muted-foreground">{new Date(date).toLocaleDateString()}</div>;
      },
    },
    // Kolom Actions
    {
      id: "actions",
      enableHiding: false,
      cell: ({ row }) => {
        const item = row.original;

        return (
          <DropdownMenu>
            <DropdownMenuTrigger asChild>
              <Button variant="ghost" className="h-8 w-8 p-0">
                <span className="sr-only">Open menu</span>
                <MoreHorizontal className="h-4 w-4" />
              </Button>
            </DropdownMenuTrigger>
            <DropdownMenuContent align="end">
              <DropdownMenuLabel>Actions</DropdownMenuLabel>
              <DropdownMenuItem onClick={() => onView?.(item)}>
                View details
              </DropdownMenuItem>
              <DropdownMenuItem onClick={() => onEdit?.(item)}>
                Edit
              </DropdownMenuItem>
              <DropdownMenuSeparator />
              <DropdownMenuItem
                onClick={() => onDelete?.(item)}
                className="text-destructive focus:text-destructive"
              >
                Delete
              </DropdownMenuItem>
            </DropdownMenuContent>
          </DropdownMenu>
        );
      },
    },
  ];

  const table = useReactTable({
    data,
    columns,
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    onColumnVisibilityChange: setColumnVisibility,
    onRowSelectionChange: setRowSelection,
    state: {
      sorting,
      columnFilters,
      columnVisibility,
      rowSelection,
    },
  });

  return (
    <div className="w-full">
      {/* Toolbar */}
      <div className="flex items-center justify-between py-4">
        <div className="flex items-center gap-2">
          <Input
            placeholder="Filter by name..."
            value={(table.getColumn("name")?.getFilterValue() as string) ?? ""}
            onChange={(event) =>
              table.getColumn("name")?.setFilterValue(event.target.value)
            }
            className="max-w-sm h-9"
          />
          {table.getColumn("email") && (
            <Input
              placeholder="Filter by email..."
              value={(table.getColumn("email")?.getFilterValue() as string) ?? ""}
              onChange={(event) =>
                table.getColumn("email")?.setFilterValue(event.target.value)
              }
              className="max-w-sm h-9"
            />
          )}
        </div>

        <div className="flex items-center gap-2">
          {/* Column visibility dropdown */}
          <DropdownMenu>
            <DropdownMenuTrigger asChild>
              <Button variant="outline" size="sm">
                Columns <ChevronDown className="ml-2 h-4 w-4" />
              </Button>
            </DropdownMenuTrigger>
            <DropdownMenuContent align="end">
              {table
                .getAllColumns()
                .filter((column) => column.getCanHide())
                .map((column) => {
                  return (
                    <DropdownMenuCheckboxItem
                      key={column.id}
                      className="capitalize"
                      checked={column.getIsVisible()}
                      onCheckedChange={(value) => column.toggleVisibility(!!value)}
                    >
                      {column.id}
                    </DropdownMenuCheckboxItem>
                  );
                })}
            </DropdownMenuContent>
          </DropdownMenu>

          {/* Selected rows info */}
          {table.getFilteredSelectedRowModel().rows.length > 0 && (
            <div className="text-sm text-muted-foreground">
              {table.getFilteredSelectedRowModel().rows.length} selected
            </div>
          )}
        </div>
      </div>

      {/* Table */}
      <div className="rounded-md border">
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
          <TableBody>
            {isLoading ? (
              <TableRow>
                <TableCell colSpan={columns.length} className="h-24 text-center">
                  <div className="flex items-center justify-center">
                    <div className="animate-spin rounded-full h-6 w-6 border-b-2 border-primary"></div>
                    <span className="ml-2">Loading...</span>
                  </div>
                </TableCell>
              </TableRow>
            ) : table.getRowModel().rows?.length ? (
              table.getRowModel().rows.map((row) => (
                <TableRow
                  key={row.id}
                  data-state={row.getIsSelected() && "selected"}
                >
                  {row.getVisibleCells().map((cell) => (
                    <TableCell key={cell.id}>
                      {flexRender(
                        cell.column.columnDef.cell,
                        cell.getContext()
                      )}
                    </TableCell>
                  ))}
                </TableRow>
              ))
            ) : (
              <TableRow>
                <TableCell
                  colSpan={columns.length}
                  className="h-24 text-center text-muted-foreground"
                >
                  No data available.
                </TableCell>
              </TableRow>
            )}
          </TableBody>
        </Table>
      </div>

      {/* Pagination */}
      <div className="flex items-center justify-between space-x-2 py-4">
        <div className="flex-1 text-sm text-muted-foreground">
          {table.getFilteredSelectedRowModel().rows.length} of{" "}
          {table.getFilteredRowModel().rows.length} row(s) selected.
        </div>
        <div className="space-x-2">
          <Button
            variant="outline"
            size="sm"
            onClick={() => table.previousPage()}
            disabled={!table.getCanPreviousPage()}
          >
            Previous
          </Button>
          <Button
            variant="outline"
            size="sm"
            onClick={() => table.nextPage()}
            disabled={!table.getCanNextPage()}
          >
            Next
          </Button>
        </div>
      </div>

      {/* Page info */}
      <div className="flex items-center justify-end text-xs text-muted-foreground">
        Page {table.getState().pagination.pageIndex + 1} of{" "}
        {table.getPageCount()}
      </div>
    </div>
  );
};
```

#### 2. **types/schema.ts (Update)**

File schema akan diperbarui dengan datatable schema yang sesuai.

```typescript
import { z } from "zod";

// ... existing schemas ...

export const [nama]DataTableSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string().email(),
  role: z.string(),
  status: z.string(),
  createdAt: z.string(),
});

export type [nama]DataTableSchema = z.infer<typeof [nama]DataTableSchema>;
```

## Contoh Lengkap: Membuat Datatable untuk "User"

### Step 1: Jalankan Generator

```bash
npm run generate
```

Pilih: `TanStack + feature:datatable`

### Step 2: Jawab Pertanyaan

```
? Nama Component/Table? (cth: user)
→ user

? Target Directory? (default: "src/features/user")
→ src/features/user

? Sebutkan kolom (pisahkan dengan koma)?
→ id, name, email, role, status, createdAt
```

### Step 3: File yang Dihasilkan

```
src/features/user/
├── components/
│   └── user-table.tsx
└── types/
    └── schema.ts (diperbarui dengan UserDataTableSchema)
```

### Step 4: Menggunakan Komponen Datatable

**Page Component:**

```typescript
// pages/user.tsx
import { useQuery } from "@tanstack/react-query";
import { UserTable } from "../components/user-table";
import { getPaginatedData } from "../api/getUser";
import { UserQueryKeys } from "../types/query-keys";
import { useNavigate } from "react-router";
import { toast } from "sonner";

export const UserPage = () => {
  const navigate = useNavigate();
  const [page, setPage] = React.useState(1);
  const [pageSize, setPageSize] = React.useState(10);

  const { data, isLoading } = useQuery({
    queryKey: UserQueryKeys.lists(),
    queryFn: getPaginatedData,
  });

  const handleEdit = (user: any) => {
    navigate(`/users/edit/${user.id}`);
  };

  const handleDelete = async (user: any) => {
    // Implement delete logic
    toast.success("User deleted successfully");
  };

  const handleView = (user: any) => {
    navigate(`/users/${user.id}`);
  };

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">Users</h1>
        <button
          onClick={() => navigate("/users/create")}
          className="bg-primary text-white px-4 py-2 rounded-md"
        >
          Add User
        </button>
      </div>

      <UserTable
        data={data?.data || []}
        isLoading={isLoading}
        onEdit={handleEdit}
        onDelete={handleDelete}
        onView={handleView}
      />
    </div>
  );
};
```

### Step 5: Integrasi dengan API

**api/getUser.ts:**

```typescript
import { api } from "@/config/axios";
import { UserDataTableSchema } from "../types/schema";

export const getPaginatedData = async (params?: {
  page?: number;
  limit?: number;
  search?: string;
}) => {
  const response = await api.get("/users", { params });
  return response.data;
};

export const getDetail = async (id: string) => {
  const response = await api.get(`/users/${id}`);
  return response.data;
};

export const create = async (payload: any) => {
  const response = await api.post("/users", payload);
  return response.data;
};

export const update = async (id: string, payload: any) => {
  const response = await api.put(`/users/${id}`, payload);
  return response.data;
};

export const delete = async (id: string) => {
  const response = await api.delete(`/users/${id}`);
  return response.data;
};
```

## Fitur-Fitur Datatable

### 1. Sorting
Klik pada header kolom untuk mengurutkan data secara ascending/descending.

### 2. Filtering
Gunakan input filter di toolbar untuk mencari data berdasarkan kolom tertentu.

### 3. Column Visibility
Pilih kolom mana yang ingin ditampilkan atau disembunyikan melalui dropdown.

### 4. Row Selection
Pilih satu atau multiple baris menggunakan checkbox.

### 5. Pagination
Navigasi halaman dengan tombol Previous dan Next.

### 6. Actions Menu
Setiap baris memiliki dropdown menu untuk aksi View, Edit, Delete.

### 7. Loading State
Menampilkan spinner saat data sedang dimuat.

### 8. Empty State
Menampilkan pesan "No data available" ketika tidak ada data.

## Kustomisasi Kolom

### Mengubah Header Kolom

```typescript
{
  accessorKey: "name",
  header: ({ column }) => {
    return (
      <Button
        variant="ghost"
        onClick={() => column.toggleSorting(column.getIsSorted() === "asc")}
      >
        Full Name {/* Ganti teks header */}
        <ArrowUpDown className="ml-2 h-4 w-4" />
      </Button>
    );
  },
}
```

### Menambahkan Badge Warna

```typescript
{
  accessorKey: "status",
  header: "Status",
  cell: ({ row }) => {
    const status = row.getValue("status") as string;
    const variants = {
      active: "bg-green-100 text-green-800",
      inactive: "bg-red-100 text-red-800",
      pending: "bg-yellow-100 text-yellow-800",
    };
    return (
      <span className={`px-2 py-1 rounded-full text-xs font-medium ${variants[status]}`}>
        {status}
      </span>
    );
  },
}
```

### Menambahkan Format Tanggal

```typescript
{
  accessorKey: "createdAt",
  header: "Created At",
  cell: ({ row }) => {
    const date = row.getValue("createdAt") as string;
    return (
      <div className="text-xs">
        {new Date(date).toLocaleDateString("id-ID", {
          day: "numeric",
          month: "long",
          year: "numeric",
        })}
      </div>
    );
  },
}
```

### Menambahkan Kolom dengan Custom Render

```typescript
{
  accessorKey: "avatar",
  header: "Avatar",
  cell: ({ row }) => {
    const avatar = row.getValue("avatar") as string;
    return (
      <img
        src={avatar}
        alt="avatar"
        className="w-8 h-8 rounded-full object-cover"
      />
    );
  },
}
```

## Best Practices

### 1. Type Safety

```typescript
// ✅ Baik: Gunakan type dari schema
import { UserDataTableSchema } from "../types/schema";

const columns: ColumnDef<UserDataTableSchema>[] = [...]

// ❌ Hindari: Menggunakan any
const columns: ColumnDef<any>[] = [...]
```

### 2. Performance Optimization

```typescript
// ✅ Baik: Gunakan useMemo untuk data yang sudah diproses
const processedData = useMemo(() => {
  return data.map(item => ({
    ...item,
    formattedDate: formatDate(item.createdAt)
  }));
}, [data]);

// ❌ Hindari: Memproses data di dalam render
```

### 3. Accessibility

```typescript
// ✅ Baik: Tambahkan aria-label
<Button aria-label="Edit user">Edit</Button>

// ✅ Baik: Gunakan sr-only untuk screen reader
<span className="sr-only">Open menu</span>
```

### 4. Responsive Design

```typescript
// ✅ Baik: Gunakan overflow-x-auto untuk tabel
<div className="overflow-x-auto">
  <Table>...</Table>
</div>
```

## Troubleshooting

### Kolom Tidak Tampil

Pastikan kolom yang didefinisikan sesuai dengan data yang diterima:

```typescript
// Cek struktur data
console.log("Data:", data[0]);

// Pastikan accessorKey sesuai dengan key di data
{
  accessorKey: "email", // Harus sama dengan key di data
}
```

### Sorting Tidak Bekerja

Pastikan kolom memiliki accessorKey yang benar dan tidak disable sorting:

```typescript
{
  accessorKey: "name",
  header: "Name",
  enableSorting: true, // Default true, pastikan tidak false
}
```

### Filter Tidak Bekerja

Pastikan kolom memiliki accessorKey yang benar:

```typescript
<Input
  placeholder="Filter by name..."
  value={(table.getColumn("name")?.getFilterValue() as string) ?? ""}
  onChange={(event) =>
    table.getColumn("name")?.setFilterValue(event.target.value)
  }
/>
```

## Command Reference

```bash
# Generate datatable untuk feature yang sudah ada
npm run generate
→ Pilih: TanStack + feature:datatable

# Atau jika ingin generate dari awal dengan struktur lengkap
npm run generate
→ Pilih: TanStack + feature
```

## Lihat Juga

- [Generator: TanStack + Feature](./tanstack-generator-feature.md)
- [Generator: TanStack + Feature: Form Hook](./tanstack-generator-form-hook.md)
- [Generator: TanStack + Feature: Page](./tanstack-generator-page.md)
- [TanStack Table Documentation](https://tanstack.com/table/latest)
```