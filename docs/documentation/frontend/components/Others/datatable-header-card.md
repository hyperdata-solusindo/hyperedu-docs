---
title: "DataTableHeaderCard"
sidebar_label: "DataTableHeaderCard"
---

# DataTableHeaderCard

**Lokasi:** `components/custom/datatable-header-card.tsx`

## Deskripsi

Komponen header card untuk data table dengan informasi ringkasan, search, dan sort. DataTableHeaderCard menyediakan antarmuka yang konsisten untuk menampilkan judul tabel, total item, input pencarian, dan dropdown sorting dalam satu container yang terintegrasi.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `DataTableHeaderCardRoot` | Container utama dengan border, shadow, dan padding |
| `DataTableHeaderInfo` | Section untuk judul dan total item |
| `DataTableHeaderActions` | Wrapper untuk tombol aksi (search, sort, dll) |
| `DataTableHeaderSearch` | Input pencarian dengan ikon search |
| `DataTableHeaderSelect` | Dropdown select untuk sorting/filter |
| `DataTableHeaderCard` | Komponen utama yang menggabungkan semua bagian |

---

## DataTableHeaderCardRoot

Container utama untuk header card.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `children` | `React.ReactNode` | Tidak | - | Konten header card |
| `className` | `string` | Tidak | - | Custom CSS classes |

### Styling Default
- Background: putih (`bg-white`)
- Border: slate-200 dengan radius xl (`border border-slate-200 rounded-xl`)
- Padding: `p-3`
- Layout: flex wrap dengan justify-between (`flex flex-wrap items-center justify-between gap-4`)
- Transition: `transition-all`

---

## DataTableHeaderInfo

Section untuk menampilkan judul dan total item.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `title` | `string` | Ya | - | Judul tabel |
| `totalLabel` | `string` | Ya | - | Label untuk total item (contoh: "Total Users") |
| `totalItems` | `number` | Ya | - | Jumlah total item |
| `icon` | `React.ReactNode` | Tidak | `<Database />` | Ikon untuk section info |
| `className` | `string` | Tidak | - | Custom CSS classes |

### Styling Default
- Container: flex dengan gap 4 (`flex items-center gap-4`)
- Icon container: `w-12 h-12 bg-indigo-50 rounded-lg`
- Title: `text-[#1E293B] text-base font-black tracking-tight uppercase`
- Total info: flex dengan icon Database dan teks kecil

---

## DataTableHeaderActions

Wrapper untuk tombol aksi.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `children` | `React.ReactNode` | Tidak | - | Tombol aksi |
| `className` | `string` | Tidak | - | Custom CSS classes |

### Styling Default
- Layout: flex dengan gap 3 dan wrap (`flex items-center flex-wrap gap-3`)

---

## DataTableHeaderSearch

Input pencarian dengan ikon search.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `value` | `string` | Ya | - | Nilai pencarian |
| `onChange` | `(val: string) => void` | Ya | - | Handler perubahan pencarian |
| `placeholder` | `string` | Tidak | `"Search..."` | Placeholder input |
| `disabled` | `boolean` | Tidak | `false` | Menonaktifkan input |
| `className` | `string` | Tidak | - | Custom CSS classes |

### Styling Default
- Ikon search: absolute positioned di kiri
- Input: `w-full sm:w-[300px] md:w-[380px] h-8 pl-10 pr-4`
- Border: `border-slate-300 rounded-full`
- Focus: `focus:ring-1 focus:ring-indigo-500`

---

## DataTableHeaderSelect

Dropdown select untuk sorting atau filter.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `value` | `string` | Ya | - | Nilai yang dipilih |
| `onChange` | `(val: string) => void` | Ya | - | Handler perubahan nilai |
| `options` | `{ value: string; label: string }[]` | Ya | - | Opsi select |
| `placeholder` | `string` | Ya | - | Placeholder select |
| `disabled` | `boolean` | Tidak | `false` | Menonaktifkan select |
| `className` | `string` | Tidak | - | Custom CSS classes |

### Styling Default
- Width: `w-[160px]`
- Height: `h-8`
- Border: `border-slate-300 rounded-xl`
- Text: `text-xs font-semibold text-slate-600`

---

## DataTableHeaderCard

Komponen utama yang menggabungkan semua bagian.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `title` | `string` | Ya | - | Judul tabel |
| `totalItems` | `number` | Ya | - | Jumlah total item |
| `totalLabel` | `string` | Ya | - | Label total item |
| `onSearch` | `(val: string) => void` | Ya | - | Handler pencarian |
| `searchValue` | `string` | Ya | - | Nilai pencarian |
| `searchPlaceholder` | `string` | Tidak | - | Placeholder input search |
| `onSortChange` | `(val: string) => void` | Ya | - | Handler perubahan sorting |
| `sortValue` | `string` | Ya | - | Nilai sorting yang dipilih |
| `sortOptions` | `{ value: string; label: string }[]` | Ya | - | Opsi sorting |
| `icon` | `React.ReactNode` | Tidak | `<Database />` | Ikon untuk section info |
| `isLoading` | `boolean` | Tidak | `false` | State loading |

---

## Contoh Penggunaan

### Basic Usage
```tsx
import { DataTableHeaderCard } from "@/components/custom/datatable-header-card";

function UsersPage() {
  const [search, setSearch] = useState("");
  const [sort, setSort] = useState("name_asc");

  const sortOptions = [
    { value: "name_asc", label: "Name A-Z" },
    { value: "name_desc", label: "Name Z-A" },
    { value: "date_asc", label: "Date Oldest" },
    { value: "date_desc", label: "Date Newest" },
  ];

  return (
    <DataTableHeaderCard
      title="Users"
      totalItems={156}
      totalLabel="Total Users"
      onSearch={setSearch}
      searchValue={search}
      searchPlaceholder="Search users..."
      onSortChange={setSort}
      sortValue={sort}
      sortOptions={sortOptions}
    />
  );
}
```

### Dengan Custom Icon
```tsx
<DataTableHeaderCard
  title="Products"
  totalItems={42}
  totalLabel="Total Products"
  onSearch={setSearch}
  searchValue={search}
  onSortChange={setSort}
  sortValue={sort}
  sortOptions={sortOptions}
  icon={<PackageIcon size={24} className="text-indigo-900" />}
/>
```

### Dengan Loading State
```tsx
<DataTableHeaderCard
  title="Orders"
  totalItems={orderCount}
  totalLabel="Total Orders"
  onSearch={setSearch}
  searchValue={search}
  onSortChange={setSort}
  sortValue={sort}
  sortOptions={sortOptions}
  isLoading={isLoading}
/>
```

### Dalam Layout Halaman
```tsx
function Dashboard() {
  const [search, setSearch] = useState("");
  const [sort, setSort] = useState("date_desc");

  return (
    <div className="space-y-6">
      <DataTableHeaderCard
        title="Recent Orders"
        totalItems={orders.length}
        totalLabel="Total Orders"
        onSearch={setSearch}
        searchValue={search}
        searchPlaceholder="Search by order ID..."
        onSortChange={setSort}
        sortValue={sort}
        sortOptions={[
          { value: "date_desc", label: "Newest First" },
          { value: "date_asc", label: "Oldest First" },
          { value: "amount_desc", label: "Highest Amount" },
          { value: "amount_asc", label: "Lowest Amount" },
        ]}
      />
      
      <OrdersTable 
        data={filteredOrders} 
        search={search} 
        sort={sort}
      />
    </div>
  );
}
```

### Dengan Search dan Filter Tambahan
```tsx
function ProductsPage() {
  const [search, setSearch] = useState("");
  const [category, setCategory] = useState("all");
  const [sort, setSort] = useState("name_asc");

  return (
    <div className="space-y-4">
      <DataTableHeaderCard
        title="Products"
        totalItems={products.length}
        totalLabel="Total Products"
        onSearch={setSearch}
        searchValue={search}
        onSortChange={setSort}
        sortValue={sort}
        sortOptions={[
          { value: "name_asc", label: "Name A-Z" },
          { value: "name_desc", label: "Name Z-A" },
          { value: "price_asc", label: "Price Low to High" },
          { value: "price_desc", label: "Price High to Low" },
        ]}
      />
      
      <DataTableHeaderActions>
        <DataTableHeaderSelect
          value={category}
          onChange={setCategory}
          options={[
            { value: "all", label: "All Categories" },
            { value: "electronics", label: "Electronics" },
            { value: "clothing", label: "Clothing" },
            { value: "books", label: "Books" },
          ]}
          placeholder="Category"
        />
      </DataTableHeaderActions>
      
      <ProductsTable 
        data={filteredProducts} 
        search={search} 
        category={category}
        sort={sort}
      />
    </div>
  );
}
```

### Dengan Custom Styling
```tsx
<DataTableHeaderCardRoot className="bg-gray-50 border-gray-200">
  <DataTableHeaderInfo
    title="Archived Users"
    totalItems={23}
    totalLabel="Archived Users"
    icon={<ArchiveIcon size={24} className="text-gray-600" />}
    className="text-gray-700"
  />
  
  <DataTableHeaderActions>
    <DataTableHeaderSearch
      value={search}
      onChange={setSearch}
      placeholder="Search archived users..."
      className="[&_input]:bg-white"
    />
  </DataTableHeaderActions>
</DataTableHeaderCardRoot>
```

## Best Practices

### 1. Gunakan untuk Semua Halaman dengan Tabel
DataTableHeaderCard memberikan konsistensi visual untuk semua halaman yang menampilkan data tabel.

### 2. Label Total yang Jelas
Gunakan `totalLabel` yang informatif:
```tsx
// ✅ Baik
totalLabel="Total Users"
totalLabel="Total Orders"

// ❌ Hindari
totalLabel="Total"
```

### 3. Search Placeholder yang Informatif
```tsx
// ✅ Baik
searchPlaceholder="Search by name or email..."

// ❌ Hindari
searchPlaceholder="Search..."
```

### 4. Loading State
Gunakan `isLoading` untuk mencegah interaksi saat data sedang dimuat:
```tsx
<DataTableHeaderCard
  isLoading={isFetching}
  // ... props lainnya
/>
```

## Troubleshooting

### Loading Overlay Tidak Muncul
- Pastikan `isLoading` prop di-set ke `true`
- Periksa apakah `className="relative overflow-hidden"` diterapkan

### Search Tidak Berfungsi
- Pastikan `onSearch` handler mengupdate state
- Periksa apakah `searchValue` terikat dengan benar

### Select Tidak Muncul Opsi
- Pastikan `options` array memiliki format yang benar (`{ value, label }`)

## Referensi

- [Shadcn Select Documentation](https://ui.shadcn.com/docs/components/select)
- [Tailwind Merge](https://github.com/dcastil/tailwind-merge)
- [Lucide Icons](https://lucide.dev/icons/)
