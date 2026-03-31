---
title: "DataTablePagination"
sidebar_label: "DataTablePagination"
---

# DataTablePagination

**Lokasi:** `components/custom/datatable-pagination.tsx`

## Deskripsi

Komponen pagination untuk navigasi halaman pada data table. DataTablePagination menyediakan navigasi halaman yang cerdas dengan ellipsis untuk halaman yang banyak, serta tombol Previous dan Next.

## Fitur

- Navigasi halaman dengan tombol Previous dan Next
- Tampilan halaman yang dinamis dengan ellipsis untuk halaman banyak
- Link halaman aktif dengan styling berbeda
- Dukungan untuk total halaman hingga ribuan
- Responsif dan accessible
- Tombol Previous/Next dinonaktifkan pada halaman pertama/terakhir

## Komponen yang Digunakan

| Komponen | Deskripsi |
|----------|-----------|
| `Pagination` | Container utama pagination dari Shadcn |
| `PaginationContent` | Container untuk daftar item pagination |
| `PaginationItem` | Item individual dalam pagination |
| `PaginationLink` | Link untuk halaman tertentu |
| `PaginationPrevious` | Tombol untuk halaman sebelumnya |
| `PaginationNext` | Tombol untuk halaman berikutnya |
| `PaginationEllipsis` | Indikator halaman yang di-singkat |
| `Button` | Tombol untuk halaman aktif (custom) |

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `currentPage` | `number` | Ya | - | Halaman yang sedang aktif |
| `totalPages` | `number` | Ya | - | Jumlah total halaman |
| `onPageChange` | `(pageNumber: number) => void` | Ya | - | Handler ketika halaman berubah |
| `showPreviousNext` | `boolean` | Ya | - | Menampilkan tombol Previous dan Next |

## Internal Components

### PaginationPageLink
Komponen internal untuk menampilkan link halaman.

```typescript
function PaginationPageLink({
  pageNum,
  onPageChange,
  isActive = false,
}: {
  pageNum: number;
  onPageChange: (page: number) => void;
  isActive?: boolean;
})
```

- **isActive = true**: Halaman aktif (ditampilkan sebagai button disabled)
- **isActive = false**: Halaman tidak aktif (ditampilkan sebagai link yang dapat diklik)

### generatePaginationLinks
Fungsi untuk menghasilkan daftar link halaman dengan logika ellipsis.

**Logika:**
- Jika `totalPages <= 6`: tampilkan semua halaman
- Jika `totalPages > 6`:
  - Selalu tampilkan halaman pertama
  - Selalu tampilkan halaman terakhir
  - Tampilkan halaman di sekitar halaman aktif (1-3 halaman)
  - Tambahkan ellipsis jika ada halaman yang disembunyikan

## Contoh Penggunaan

### Basic Usage
```tsx
import DataTablePagination from "@/components/custom/datatable-pagination";

function UsersTable() {
  const [currentPage, setCurrentPage] = useState(1);
  const totalPages = 10;

  return (
    <DataTablePagination
      currentPage={currentPage}
      totalPages={totalPages}
      onPageChange={setCurrentPage}
      showPreviousNext={true}
    />
  );
}
```

### Dalam DataTable
```tsx
function DataTableWithPagination() {
  const [pagination, setPagination] = useState({
    pageIndex: 1,
    pageSize: 10,
  });
  const totalItems = 150;
  const totalPages = Math.ceil(totalItems / pagination.pageSize);

  const handlePageChange = (page: number) => {
    setPagination(prev => ({ ...prev, pageIndex: page }));
    // Fetch data for new page
  };

  return (
    <div>
      <Table>
        {/* Table content */}
      </Table>
      
      <div className="mt-4 flex justify-end">
        <DataTablePagination
          currentPage={pagination.pageIndex}
          totalPages={totalPages}
          onPageChange={handlePageChange}
          showPreviousNext={true}
        />
      </div>
    </div>
  );
}
```

### Tanpa Tombol Previous/Next
```tsx
<DataTablePagination
  currentPage={currentPage}
  totalPages={totalPages}
  onPageChange={setCurrentPage}
  showPreviousNext={false}
/>
```

### Dengan Total Pages Sedikit (≤6)
```tsx
// totalPages = 5, menampilkan semua halaman: 1 2 3 4 5
<DataTablePagination
  currentPage={3}
  totalPages={5}
  onPageChange={setCurrentPage}
  showPreviousNext={true}
/>
```

### Dengan Total Pages Banyak (>6)
```tsx
// totalPages = 20, currentPage = 5
// Output: 1 ... 4 5 6 ... 20

// totalPages = 20, currentPage = 1
// Output: 1 2 3 ... 20

// totalPages = 20, currentPage = 18
// Output: 1 ... 17 18 19 20
```

## Logika Pagination

### Kasus 1: totalPages ≤ 6
Tampilkan semua halaman:
```
1 2 3 4 5 6
```

### Kasus 2: totalPages > 6 dengan currentPage di awal
```
currentPage = 1: 1 2 3 ... 20
currentPage = 2: 1 2 3 ... 20
currentPage = 3: 1 2 3 ... 20
```

### Kasus 3: totalPages > 6 dengan currentPage di tengah
```
currentPage = 5: 1 ... 4 5 6 ... 20
currentPage = 10: 1 ... 9 10 11 ... 20
```

### Kasus 4: totalPages > 6 dengan currentPage di akhir
```
currentPage = 18: 1 ... 17 18 19 20
currentPage = 19: 1 ... 17 18 19 20
currentPage = 20: 1 ... 18 19 20
```

## Kustomisasi

### Custom Styling
```tsx
<div className="flex justify-center mt-8">
  <DataTablePagination
    currentPage={currentPage}
    totalPages={totalPages}
    onPageChange={setCurrentPage}
    showPreviousNext={true}
  />
</div>
```

### Custom Class pada Container
```tsx
<div className="bg-gray-50 p-4 rounded-lg">
  <DataTablePagination
    currentPage={currentPage}
    totalPages={totalPages}
    onPageChange={setCurrentPage}
    showPreviousNext={true}
  />
</div>
```

## Best Practices

### 1. Sinkronkan dengan Data
Pastikan `totalPages` dihitung dari total data dan items per page:
```tsx
const totalPages = Math.ceil(totalItems / itemsPerPage);
```

### 2. Handle Page Change
Implementasikan handler yang mengupdate state dan fetch data:
```tsx
const handlePageChange = (page: number) => {
  setCurrentPage(page);
  fetchData(page, pageSize);
};
```

### 3. Reset ke Halaman Pertama saat Filter Berubah
```tsx
const handleSearch = (searchTerm: string) => {
  setSearch(searchTerm);
  setCurrentPage(1); // Reset to first page
  fetchData(1, pageSize, searchTerm);
};
```

### 4. Disable Tombol pada Edge
Tombol Previous/Next otomatis dinonaktifkan pada halaman pertama/terakhir.

## Troubleshooting

### Pagination Tidak Muncul
- Pastikan `totalPages` > 1
- Periksa apakah `showPreviousNext` di-set dengan benar

### Halaman Aktif Tidak Terlihat
- Pastikan `currentPage` berada dalam rentang 1 - `totalPages`
- Periksa apakah `onPageChange` mengupdate state dengan benar

### Ellipsis Tidak Muncul
- Ellipsis hanya muncul jika `totalPages > 6`
- Pastikan ada halaman yang disembunyikan

## Referensi

- [Shadcn Pagination Documentation](https://ui.shadcn.com/docs/components/pagination)
- [TanStack Table Pagination](https://tanstack.com/table/v8/docs/guide/pagination)
- [Pagination Best Practices](https://www.smashingmagazine.com/2017/05/pagination-infinite-scrolling/)
