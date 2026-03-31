---
title: "Table"
sidebar_label: "Table"
---

# Table

**Lokasi:** `components/ui/table.tsx`  
**Dokumentasi Resmi:** [Shadcn Table](https://ui.shadcn.com/docs/components/table)

## Deskripsi

Komponen table dengan styling konsisten untuk menampilkan data tabular. Table digunakan untuk menyajikan data dalam format baris dan kolom yang terstruktur, mendukung berbagai fitur seperti sorting, filtering, dan selection ketika dikombinasikan dengan TanStack Table.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Table` | Container utama table dengan overflow-x-auto wrapper |
| `TableHeader` | Header section table |
| `TableBody` | Body section table berisi data baris |
| `TableFooter` | Footer section table |
| `TableRow` | Baris dalam table |
| `TableHead` | Header cell (kolom) |
| `TableCell` | Data cell |
| `TableCaption` | Caption untuk table (deskripsi) |

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `className` | `string` | - | Custom CSS classes untuk masing-masing komponen |

## Contoh Penggunaan

### Basic Table
```tsx
import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableFooter,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";

const invoices = [
  { id: "INV001", amount: "$250.00", status: "Paid", email: "john@example.com" },
  { id: "INV002", amount: "$150.00", status: "Pending", email: "jane@example.com" },
  { id: "INV003", amount: "$350.00", status: "Paid", email: "bob@example.com" },
];

<Table>
  <TableCaption>A list of your recent invoices.</TableCaption>
  <TableHeader>
    <TableRow>
      <TableHead className="w-[100px]">Invoice</TableHead>
      <TableHead>Status</TableHead>
      <TableHead>Method</TableHead>
      <TableHead className="text-right">Amount</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {invoices.map((invoice) => (
      <TableRow key={invoice.id}>
        <TableCell className="font-medium">{invoice.id}</TableCell>
        <TableCell>{invoice.status}</TableCell>
        <TableCell>{invoice.email}</TableCell>
        <TableCell className="text-right">{invoice.amount}</TableCell>
      </TableRow>
    ))}
  </TableBody>
  <TableFooter>
    <TableRow>
      <TableCell colSpan={3}>Total</TableCell>
      <TableCell className="text-right">$2,500.00</TableCell>
    </TableRow>
  </TableFooter>
</Table>
```

### Table dengan Sorting
```tsx
import { useState } from "react";
import { ArrowUpDown } from "lucide-react";

function SortableTable() {
  const [sortField, setSortField] = useState("name");
  const [sortDirection, setSortDirection] = useState<"asc" | "desc">("asc");

  const handleSort = (field: string) => {
    if (sortField === field) {
      setSortDirection(sortDirection === "asc" ? "desc" : "asc");
    } else {
      setSortField(field);
      setSortDirection("asc");
    }
  };

  const sortedData = [...data].sort((a, b) => {
    if (sortDirection === "asc") {
      return a[sortField] > b[sortField] ? 1 : -1;
    }
    return a[sortField] < b[sortField] ? 1 : -1;
  });

  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead className="cursor-pointer" onClick={() => handleSort("name")}>
            <div className="flex items-center gap-1">
              Name
              <ArrowUpDown className="h-4 w-4" />
            </div>
          </TableHead>
          <TableHead className="cursor-pointer" onClick={() => handleSort("email")}>
            <div className="flex items-center gap-1">
              Email
              <ArrowUpDown className="h-4 w-4" />
            </div>
          </TableHead>
          <TableHead>Status</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {sortedData.map((item) => (
          <TableRow key={item.id}>
            <TableCell>{item.name}</TableCell>
            <TableCell>{item.email}</TableCell>
            <TableCell>
              <Badge variant={item.status === "active" ? "success" : "warning"}>
                {item.status}
              </Badge>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

### Table dengan Selection (Checkbox)
```tsx
import { Checkbox } from "@/components/ui/checkbox";

function SelectableTable() {
  const [selectedRows, setSelectedRows] = useState<string[]>([]);

  const toggleRow = (id: string) => {
    setSelectedRows(prev =>
      prev.includes(id) ? prev.filter(rowId => rowId !== id) : [...prev, id]
    );
  };

  const toggleAll = () => {
    if (selectedRows.length === data.length) {
      setSelectedRows([]);
    } else {
      setSelectedRows(data.map(item => item.id));
    }
  };

  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead className="w-[50px]">
            <Checkbox
              checked={selectedRows.length === data.length}
              onCheckedChange={toggleAll}
            />
          </TableHead>
          <TableHead>Name</TableHead>
          <TableHead>Email</TableHead>
          <TableHead>Status</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {data.map((item) => (
          <TableRow
            key={item.id}
            className={selectedRows.includes(item.id) ? "bg-muted/50" : ""}
          >
            <TableCell>
              <Checkbox
                checked={selectedRows.includes(item.id)}
                onCheckedChange={() => toggleRow(item.id)}
              />
            </TableCell>
            <TableCell>{item.name}</TableCell>
            <TableCell>{item.email}</TableCell>
            <TableCell>
              <Badge variant={item.status === "active" ? "success" : "warning"}>
                {item.status}
              </Badge>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

### Table dengan Action Buttons
```tsx
function TableWithActions() {
  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead>Name</TableHead>
          <TableHead>Email</TableHead>
          <TableHead>Role</TableHead>
          <TableHead className="text-right">Actions</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {users.map((user) => (
          <TableRow key={user.id}>
            <TableCell className="font-medium">{user.name}</TableCell>
            <TableCell>{user.email}</TableCell>
            <TableCell>
              <Badge variant="outline">{user.role}</Badge>
            </TableCell>
            <TableCell className="text-right">
              <div className="flex justify-end gap-2">
                <Tooltip>
                  <TooltipTrigger asChild>
                    <Button variant="ghost" size="icon-xs">
                      <EyeIcon className="h-3 w-3" />
                    </Button>
                  </TooltipTrigger>
                  <TooltipContent>View</TooltipContent>
                </Tooltip>
                <Tooltip>
                  <TooltipTrigger asChild>
                    <Button variant="ghost" size="icon-xs">
                      <EditIcon className="h-3 w-3" />
                    </Button>
                  </TooltipTrigger>
                  <TooltipContent>Edit</TooltipContent>
                </Tooltip>
                <Tooltip>
                  <TooltipTrigger asChild>
                    <Button variant="ghost" size="icon-xs">
                      <TrashIcon className="h-3 w-3" />
                    </Button>
                  </TooltipTrigger>
                  <TooltipContent>Delete</TooltipContent>
                </Tooltip>
              </div>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

### Table dengan Avatar
```tsx
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

function TableWithAvatar() {
  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead>User</TableHead>
          <TableHead>Email</TableHead>
          <TableHead>Status</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {users.map((user) => (
          <TableRow key={user.id}>
            <TableCell>
              <div className="flex items-center gap-3">
                <Avatar className="h-8 w-8">
                  <AvatarImage src={user.avatar} />
                  <AvatarFallback>{user.name.charAt(0)}</AvatarFallback>
                </Avatar>
                <span className="font-medium">{user.name}</span>
              </div>
            </TableCell>
            <TableCell>{user.email}</TableCell>
            <TableCell>
              <Badge variant={user.status === "active" ? "success" : "warning"}>
                {user.status}
              </Badge>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

### Table dengan Pagination
```tsx
import {
  Pagination,
  PaginationContent,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious,
} from "@/components/ui/pagination";

function TableWithPagination() {
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 5;
  const totalPages = Math.ceil(data.length / itemsPerPage);
  const paginatedData = data.slice(
    (currentPage - 1) * itemsPerPage,
    currentPage * itemsPerPage
  );

  return (
    <div className="space-y-4">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>Name</TableHead>
            <TableHead>Email</TableHead>
            <TableHead>Status</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {paginatedData.map((item) => (
            <TableRow key={item.id}>
              <TableCell>{item.name}</TableCell>
              <TableCell>{item.email}</TableCell>
              <TableCell>
                <Badge variant={item.status === "active" ? "success" : "warning"}>
                  {item.status}
                </Badge>
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>

      <Pagination>
        <PaginationContent>
          <PaginationItem>
            <PaginationPrevious
              onClick={() => setCurrentPage(prev => Math.max(1, prev - 1))}
              className={currentPage === 1 ? "pointer-events-none opacity-50" : "cursor-pointer"}
            />
          </PaginationItem>
          {Array.from({ length: totalPages }, (_, i) => i + 1).map((page) => (
            <PaginationItem key={page}>
              <PaginationLink
                onClick={() => setCurrentPage(page)}
                isActive={currentPage === page}
              >
                {page}
              </PaginationLink>
            </PaginationItem>
          ))}
          <PaginationItem>
            <PaginationNext
              onClick={() => setCurrentPage(prev => Math.min(totalPages, prev + 1))}
              className={currentPage === totalPages ? "pointer-events-none opacity-50" : "cursor-pointer"}
            />
          </PaginationItem>
        </PaginationContent>
      </Pagination>
    </div>
  );
}
```

### Table dengan Sticky Header
```tsx
<Table>
  <TableHeader className="sticky top-0 bg-background">
    <TableRow>
      <TableHead>Name</TableHead>
      <TableHead>Email</TableHead>
      <TableHead>Status</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {largeData.map((item) => (
      <TableRow key={item.id}>
        <TableCell>{item.name}</TableCell>
        <TableCell>{item.email}</TableCell>
        <TableCell>
          <Badge variant={item.status === "active" ? "success" : "warning"}>
            {item.status}
          </Badge>
        </TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### Table dengan Custom Cell Styling
```tsx
<Table>
  <TableBody>
    {data.map((item) => (
      <TableRow key={item.id}>
        <TableCell className="font-mono text-xs">{item.id}</TableCell>
        <TableCell className="text-primary font-semibold">{item.name}</TableCell>
        <TableCell className="text-right">
          <span className={item.amount > 0 ? "text-green-600" : "text-red-600"}>
            {item.amount}
          </span>
        </TableCell>
        <TableCell className="max-w-[200px] truncate">
          {item.description}
        </TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### Table dengan Borderless Style
```tsx
<Table className="border-separate border-spacing-y-2">
  <TableHeader>
    <TableRow className="border-none">
      <TableHead>Name</TableHead>
      <TableHead>Email</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {data.map((item) => (
      <TableRow key={item.id} className="border-none shadow-sm hover:shadow-md transition-shadow">
        <TableCell className="bg-muted/30 rounded-l-lg">{item.name}</TableCell>
        <TableCell className="bg-muted/30 rounded-r-lg">{item.email}</TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

## Kustomisasi

### Custom Column Width
```tsx
<TableHead className="w-[100px]">ID</TableHead>
<TableHead className="min-w-[200px]">Name</TableHead>
<TableHead className="w-[150px]">Status</TableHead>
```

### Custom Row Styling
```tsx
<TableRow className="hover:bg-blue-50 data-[state=selected]:bg-blue-100">
  {/* row content */}
</TableRow>

<TableRow className="border-l-4 border-l-primary">
  {/* highlighted row */}
</TableRow>
```

## Best Practices

### 1. Responsive Table
Table secara default memiliki overflow-x-auto untuk scroll horizontal:
```tsx
// Table sudah dibungkus dengan div overflow-x-auto
<Table>
  {/* content */}
</Table>
```

### 2. Empty State
Tampilkan pesan ketika tidak ada data:
```tsx
<TableBody>
  {data.length === 0 ? (
    <TableRow>
      <TableCell colSpan={columns.length} className="h-32 text-center text-muted-foreground">
        No data available.
      </TableCell>
    </TableRow>
  ) : (
    data.map((item) => <TableRow key={item.id}>...</TableRow>)
  )}
</TableBody>
```

### 3. Loading State
```tsx
{isLoading ? (
  <TableBody>
    <TableRow>
      <TableCell colSpan={columns.length} className="h-32 text-center">
        <Loader2Icon className="h-6 w-6 animate-spin mx-auto" />
      </TableCell>
    </TableRow>
  </TableBody>
) : (
  // Actual data
)}
```

### 4. Accessibility
- Gunakan `TableCaption` untuk deskripsi table
- Pastikan header memiliki konteks yang jelas

## Troubleshooting

### Kolom Tidak Rata
- Pastikan menggunakan `text-left`, `text-center`, atau `text-right` pada TableHead dan TableCell
- Gunakan `w-[...]` untuk lebar kolom yang konsisten

### Scroll Horizontal Tidak Muncul
- Table sudah dibungkus dengan overflow-x-auto, pastikan tidak ada CSS yang menimpanya

## Referensi

- [Shadcn Table Documentation](https://ui.shadcn.com/docs/components/table)
- [TanStack Table Documentation](https://tanstack.com/table/latest)
- [MDN Table Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table)
