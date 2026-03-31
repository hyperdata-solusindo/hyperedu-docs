---
title: "Pagination"
sidebar_label: "Pagination"
---

# Pagination

**Lokasi:** `components/ui/pagination.tsx`  
**Dokumentasi Resmi:** [Shadcn Pagination](https://ui.shadcn.com/docs/components/pagination)

## Deskripsi

Komponen pagination untuk navigasi halaman. Pagination digunakan untuk membagi data yang banyak menjadi beberapa halaman, memungkinkan pengguna untuk menavigasi antar halaman dengan mudah.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Pagination` | Container utama pagination |
| `PaginationContent` | Container untuk daftar item pagination |
| `PaginationItem` | Item individual dalam pagination |
| `PaginationLink` | Link untuk halaman tertentu |
| `PaginationPrevious` | Tombol untuk halaman sebelumnya |
| `PaginationNext` | Tombol untuk halaman berikutnya |
| `PaginationEllipsis` | Indikator halaman yang di-singkat |

## Props

### PaginationLink
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `isActive` | `boolean` | `false` | Menandai halaman aktif |
| `size` | `"default" \| "sm" \| "lg" \| "icon"` | `"icon"` | Ukuran link |

## Contoh Penggunaan

### Basic Pagination
```tsx
import {
  Pagination,
  PaginationContent,
  PaginationEllipsis,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious,
} from "@/components/ui/pagination";

<Pagination>
  <PaginationContent>
    <PaginationItem>
      <PaginationPrevious href="#" />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">1</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#" isActive>2</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">3</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationEllipsis />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">10</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationNext href="#" />
    </PaginationItem>
  </PaginationContent>
</Pagination>
```

### Controlled Pagination dengan State
```tsx
function ControlledPagination() {
  const [currentPage, setCurrentPage] = useState(1);
  const totalPages = 10;

  const handlePageChange = (page: number) => {
    setCurrentPage(page);
    // Fetch data for new page
  };

  return (
    <Pagination>
      <PaginationContent>
        <PaginationItem>
          <PaginationPrevious
            onClick={() => handlePageChange(currentPage - 1)}
            className={currentPage === 1 ? "pointer-events-none opacity-50" : "cursor-pointer"}
          />
        </PaginationItem>
        
        {Array.from({ length: totalPages }, (_, i) => i + 1).map((page) => (
          <PaginationItem key={page}>
            <PaginationLink
              onClick={() => handlePageChange(page)}
              isActive={currentPage === page}
            >
              {page}
            </PaginationLink>
          </PaginationItem>
        ))}
        
        <PaginationItem>
          <PaginationNext
            onClick={() => handlePageChange(currentPage + 1)}
            className={currentPage === totalPages ? "pointer-events-none opacity-50" : "cursor-pointer"}
          />
        </PaginationItem>
      </PaginationContent>
    </Pagination>
  );
}
```

### Pagination dengan Dynamic Page Range
```tsx
function DynamicPagination() {
  const [currentPage, setCurrentPage] = useState(1);
  const totalPages = 20;

  const getPageNumbers = () => {
    const delta = 2;
    const range = [];
    const rangeWithDots = [];
    let l;

    for (let i = 1; i <= totalPages; i++) {
      if (i === 1 || i === totalPages || (i >= currentPage - delta && i <= currentPage + delta)) {
        range.push(i);
      }
    }

    range.forEach((i) => {
      if (l) {
        if (i - l === 2) {
          rangeWithDots.push(l + 1);
        } else if (i - l !== 1) {
          rangeWithDots.push("...");
        }
      }
      rangeWithDots.push(i);
      l = i;
    });

    return rangeWithDots;
  };

  return (
    <Pagination>
      <PaginationContent>
        <PaginationItem>
          <PaginationPrevious
            onClick={() => setCurrentPage((p) => Math.max(1, p - 1))}
            className={currentPage === 1 ? "pointer-events-none opacity-50" : "cursor-pointer"}
          />
        </PaginationItem>

        {getPageNumbers().map((page, index) => (
          <PaginationItem key={index}>
            {page === "..." ? (
              <PaginationEllipsis />
            ) : (
              <PaginationLink
                onClick={() => setCurrentPage(page as number)}
                isActive={currentPage === page}
              >
                {page}
              </PaginationLink>
            )}
          </PaginationItem>
        ))}

        <PaginationItem>
          <PaginationNext
            onClick={() => setCurrentPage((p) => Math.min(totalPages, p + 1))}
            className={currentPage === totalPages ? "pointer-events-none opacity-50" : "cursor-pointer"}
          />
        </PaginationItem>
      </PaginationContent>
    </Pagination>
  );
}
```

### Pagination dengan Info Halaman
```tsx
function PaginationWithInfo() {
  const [currentPage, setCurrentPage] = useState(1);
  const totalPages = 15;
  const itemsPerPage = 10;
  const totalItems = 145;
  const startItem = (currentPage - 1) * itemsPerPage + 1;
  const endItem = Math.min(currentPage * itemsPerPage, totalItems);

  return (
    <div className="space-y-4">
      <div className="text-sm text-muted-foreground text-center">
        Showing {startItem} to {endItem} of {totalItems} results
      </div>
      
      <Pagination>
        <PaginationContent>
          <PaginationItem>
            <PaginationPrevious
              onClick={() => setCurrentPage((p) => Math.max(1, p - 1))}
              className={currentPage === 1 ? "pointer-events-none opacity-50" : "cursor-pointer"}
            />
          </PaginationItem>
          
          {Array.from({ length: Math.min(5, totalPages) }, (_, i) => {
            let pageNum;
            if (currentPage <= 3) {
              pageNum = i + 1;
            } else if (currentPage >= totalPages - 2) {
              pageNum = totalPages - 4 + i;
            } else {
              pageNum = currentPage - 2 + i;
            }
            if (pageNum > 0 && pageNum <= totalPages) {
              return (
                <PaginationItem key={pageNum}>
                  <PaginationLink
                    onClick={() => setCurrentPage(pageNum)}
                    isActive={currentPage === pageNum}
                  >
                    {pageNum}
                  </PaginationLink>
                </PaginationItem>
              );
            }
            return null;
          })}
          
          <PaginationItem>
            <PaginationNext
              onClick={() => setCurrentPage((p) => Math.min(totalPages, p + 1))}
              className={currentPage === totalPages ? "pointer-events-none opacity-50" : "cursor-pointer"}
            />
          </PaginationItem>
        </PaginationContent>
      </Pagination>
    </div>
  );
}
```

### Pagination dengan Rows Per Page Select
```tsx
function PaginationWithRowsPerPage() {
  const [currentPage, setCurrentPage] = useState(1);
  const [rowsPerPage, setRowsPerPage] = useState(10);
  const totalItems = 145;
  const totalPages = Math.ceil(totalItems / rowsPerPage);

  return (
    <div className="space-y-4">
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-2">
          <span className="text-sm text-muted-foreground">Rows per page:</span>
          <Select value={String(rowsPerPage)} onValueChange={(v) => {
            setRowsPerPage(Number(v));
            setCurrentPage(1);
          }}>
            <SelectTrigger className="w-20 h-8">
              <SelectValue />
            </SelectTrigger>
            <SelectContent>
              <SelectItem value="10">10</SelectItem>
              <SelectItem value="25">25</SelectItem>
              <SelectItem value="50">50</SelectItem>
              <SelectItem value="100">100</SelectItem>
            </SelectContent>
          </Select>
        </div>
        
        <div className="text-sm text-muted-foreground">
          {((currentPage - 1) * rowsPerPage) + 1} - {Math.min(currentPage * rowsPerPage, totalItems)} of {totalItems}
        </div>
      </div>
      
      <Pagination>
        <PaginationContent>
          <PaginationItem>
            <PaginationPrevious
              onClick={() => setCurrentPage((p) => Math.max(1, p - 1))}
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
              onClick={() => setCurrentPage((p) => Math.min(totalPages, p + 1))}
              className={currentPage === totalPages ? "pointer-events-none opacity-50" : "cursor-pointer"}
            />
          </PaginationItem>
        </PaginationContent>
      </Pagination>
    </div>
  );
}
```

### Pagination dengan Jump to Page
```tsx
function PaginationWithJump() {
  const [currentPage, setCurrentPage] = useState(1);
  const [jumpPage, setJumpPage] = useState("");
  const totalPages = 25;

  const handleJump = () => {
    const page = parseInt(jumpPage);
    if (page >= 1 && page <= totalPages) {
      setCurrentPage(page);
      setJumpPage("");
    }
  };

  return (
    <div className="space-y-4">
      <Pagination>
        <PaginationContent>
          <PaginationItem>
            <PaginationPrevious
              onClick={() => setCurrentPage((p) => Math.max(1, p - 1))}
              className={currentPage === 1 ? "pointer-events-none opacity-50" : "cursor-pointer"}
            />
          </PaginationItem>
          
          {Array.from({ length: Math.min(5, totalPages) }, (_, i) => {
            let pageNum = currentPage - 2 + i;
            if (pageNum < 1) pageNum = i + 1;
            if (pageNum > totalPages) pageNum = totalPages - 4 + i;
            if (pageNum > 0 && pageNum <= totalPages) {
              return (
                <PaginationItem key={pageNum}>
                  <PaginationLink
                    onClick={() => setCurrentPage(pageNum)}
                    isActive={currentPage === pageNum}
                  >
                    {pageNum}
                  </PaginationLink>
                </PaginationItem>
              );
            }
            return null;
          })}
          
          <PaginationItem>
            <PaginationNext
              onClick={() => setCurrentPage((p) => Math.min(totalPages, p + 1))}
              className={currentPage === totalPages ? "pointer-events-none opacity-50" : "cursor-pointer"}
            />
          </PaginationItem>
        </PaginationContent>
      </Pagination>
      
      <div className="flex items-center justify-center gap-2">
        <span className="text-sm text-muted-foreground">Jump to:</span>
        <Input
          type="number"
          className="w-20 h-8"
          value={jumpPage}
          onChange={(e) => setJumpPage(e.target.value)}
          onKeyDown={(e) => e.key === "Enter" && handleJump()}
          min={1}
          max={totalPages}
        />
        <Button size="sm" onClick={handleJump}>Go</Button>
      </div>
    </div>
  );
}
```

## Kustomisasi

### Custom Styling
```tsx
<PaginationLink className="bg-primary text-primary-foreground hover:bg-primary/90">
  1
</PaginationLink>
```

### Custom Size
```tsx
<PaginationLink size="sm">1</PaginationLink>
<PaginationLink size="lg">1</PaginationLink>
```

### Custom Ikon
```tsx
<PaginationPrevious>
  <ChevronLeftIcon className="h-4 w-4" />
  <span>Previous</span>
</PaginationPrevious>
```

## Best Practices

### 1. Tampilkan Info Halaman
Selalu tampilkan informasi halaman dan total data untuk konteks yang jelas.

### 2. Gunakan Ellipsis untuk Halaman Banyak
Gunakan `PaginationEllipsis` untuk menyembunyikan halaman yang tidak perlu.

### 3. Disable Tombol pada Edge
Disable tombol Previous di halaman pertama dan Next di halaman terakhir.

### 4. Responsive Design
Untuk mobile, pertimbangkan untuk menyembunyikan beberapa halaman atau menggunakan dropdown.

## Troubleshooting

### Pagination Tidak Responsif
- Pastikan menggunakan overflow-x-auto untuk container jika perlu
- Untuk mobile, gunakan scroll horizontal atau dropdown

### Halaman Aktif Tidak Tampil
- Pastikan `isActive` prop di-set dengan benar
- Periksa logika perbandingan halaman

## Referensi

- [Shadcn Pagination Documentation](https://ui.shadcn.com/docs/components/pagination)
- [TanStack Table Pagination](https://tanstack.com/table/v8/docs/guide/pagination)
- [MDN Pagination Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/pagination/)
