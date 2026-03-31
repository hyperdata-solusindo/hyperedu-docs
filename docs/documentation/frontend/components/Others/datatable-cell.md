---
title: "DataTableCell"
sidebar_label: "DataTableCell"
---

# DataTableCell

**Lokasi:** `components/custom/datatables-cell.tsx`

## Deskripsi

Komponen wrapper untuk cell tabel dengan styling konsisten. DataTableCell menyediakan styling default untuk semua cell dalam tabel, memastikan konsistensi visual di seluruh aplikasi.

## Fitur

- Styling konsisten untuk semua cell tabel
- Font size 14px
- Font weight medium (500)
- Text alignment kiri (text-start)
- Warna teks slate-700
- Dukungan custom styling melalui className
- Menggunakan tailwind-merge untuk penggabungan class

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes tambahan |
| `children` | `React.ReactNode` | Tidak | - | Konten cell (teks, komponen, dll) |

## Styling Default

| Property | Value | Deskripsi |
|----------|-------|-----------|
| Font Size | `14px` | Ukuran teks standar untuk cell |
| Font Weight | `500` (medium) | Ketebalan teks standar |
| Text Color | `slate-700` | Warna teks abu-abu gelap |
| Text Align | `start` | Rata kiri |

## Contoh Penggunaan

### Basic Usage
```tsx
import { DataTableCell } from "@/components/custom/datatables-cell";

<Table>
  <TableBody>
    <TableRow>
      <TableCell>
        <DataTableCell>John Doe</DataTableCell>
      </TableCell>
      <TableCell>
        <DataTableCell>john@example.com</DataTableCell>
      </TableCell>
    </TableRow>
  </TableBody>
</Table>
```

### Dalam Definisi Kolom
```tsx
const columns: ColumnDef<User>[] = [
  {
    accessorKey: "name",
    header: "Name",
    cell: ({ row }) => (
      <DataTableCell>
        {row.getValue("name")}
      </DataTableCell>
    ),
  },
  {
    accessorKey: "email",
    header: "Email",
    cell: ({ row }) => (
      <DataTableCell>
        {row.getValue("email")}
      </DataTableCell>
    ),
  },
];
```

### Dengan Custom Alignment
```tsx
{
  accessorKey: "price",
  header: "Price",
  cell: ({ row }) => (
    <DataTableCell className="text-right">
      ${row.getValue("price")}
    </DataTableCell>
  ),
}
```

### Dengan Custom Warna
```tsx
{
  accessorKey: "status",
  header: "Status",
  cell: ({ row }) => {
    const status = row.getValue("status");
    return (
      <DataTableCell className={status === "active" ? "text-green-600" : "text-red-600"}>
        {status}
      </DataTableCell>
    );
  },
}
```

### Dengan Badge
```tsx
{
  accessorKey: "role",
  header: "Role",
  cell: ({ row }) => (
    <DataTableCell>
      <Badge variant="outline">
        {row.getValue("role")}
      </Badge>
    </DataTableCell>
  ),
}
```

### Dengan Format Tanggal
```tsx
{
  accessorKey: "createdAt",
  header: "Created At",
  cell: ({ row }) => {
    const date = row.getValue("createdAt") as string;
    return (
      <DataTableCell>
        {new Date(date).toLocaleDateString("id-ID")}
      </DataTableCell>
    );
  },
}
```

### Dengan Ikon
```tsx
{
  id: "actions",
  cell: ({ row }) => (
    <DataTableCell className="flex gap-2">
      <Button variant="ghost" size="icon">
        <EditIcon className="h-4 w-4" />
      </Button>
      <Button variant="ghost" size="icon">
        <TrashIcon className="h-4 w-4" />
      </Button>
    </DataTableCell>
  ),
}
```

### Dengan Custom Font Weight
```tsx
{
  accessorKey: "total",
  header: "Total",
  cell: ({ row }) => (
    <DataTableCell className="font-bold">
      Rp {row.getValue("total").toLocaleString()}
    </DataTableCell>
  ),
}
```

### Dengan Truncated Text
```tsx
{
  accessorKey: "description",
  header: "Description",
  cell: ({ row }) => (
    <DataTableCell className="max-w-xs truncate">
      {row.getValue("description")}
    </DataTableCell>
  ),
}
```

## Kustomisasi

### Custom Font Size
```tsx
<DataTableCell className="text-xs">
  Small text
</DataTableCell>

<DataTableCell className="text-base">
  Larger text
</DataTableCell>
```

### Custom Text Color
```tsx
<DataTableCell className="text-primary">
  Primary color
</DataTableCell>

<DataTableCell className="text-destructive">
  Destructive color
</DataTableCell>
```

### Custom Background
```tsx
<DataTableCell className="bg-muted/30">
  Highlighted cell
</DataTableCell>
```

### Custom Padding
```tsx
<DataTableCell className="py-3">
  Extra padding
</DataTableCell>
```

## Best Practices

### 1. Gunakan untuk Semua Cell Tabel
Gunakan DataTableCell untuk semua cell agar styling konsisten:
```tsx
// ✅ Baik: Menggunakan DataTableCell
<DataTableCell>Content</DataTableCell>

// ❌ Hindari: Styling manual
<div className="text-[14px] text-slate-700 font-medium">Content</div>
```

### 2. Kombinasikan dengan Tailwind Classes
Gunakan className untuk kustomisasi tambahan:
```tsx
<DataTableCell className="text-right font-bold">
  {value}
</DataTableCell>
```

### 3. Handle Long Text
Gunakan truncate untuk teks panjang:
```tsx
<DataTableCell className="max-w-[200px] truncate">
  {longText}
</DataTableCell>
```

### 4. Konsisten dengan Alignment
Gunakan alignment yang konsisten dengan header:
- Text untuk kolom teks: kiri (`text-start`)
- Number untuk kolom angka: kanan (`text-right`)
- Actions: tengah (`text-center`)

## Troubleshooting

### Teks Terpotong
- Gunakan `truncate` atau `whitespace-normal` sesuai kebutuhan
- Atur lebar maksimum dengan `max-w-[...]`

### Styling Tidak Sesuai
- Periksa urutan className (tailwind-merge akan menggabungkan dengan benar)
- Pastikan tidak ada CSS yang menimpa

## Referensi

- [Tailwind CSS Typography](https://tailwindcss.com/docs/font-size)
- [Tailwind Merge](https://github.com/dcastil/tailwind-merge)
- [Tailwind Text Color](https://tailwindcss.com/docs/text-color)
