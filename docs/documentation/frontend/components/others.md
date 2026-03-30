---
sidebar_position: 3
---

# Custom Components (Others)

## Overview

Dokumentasi ini menyediakan panduan lengkap untuk komponen kustom yang digunakan dalam aplikasi frontend HyperEdu. Komponen-komponen ini dirancang untuk dapat digunakan kembali (reusable), mudah dipelihara (maintainable), dan mengikuti sistem desain serta praktik terbaik proyek.

Komponen-komponen ini merupakan pengembangan dari komponen UI dasar (Shadcn) yang telah dikustomisasi untuk kebutuhan spesifik aplikasi HyperEdu.

---

## Daftar Komponen

| No | Komponen | Lokasi | Deskripsi |
|----|----------|--------|-----------|
| 1 | [AppSidebar](#appsidebar) | `components/custom/app-sidebar.tsx` | Sidebar navigasi utama |
| 2 | [AsyncCombobox](#asynccombobox) | `components/custom/async-combobox.tsx` | Combobox asinkron dengan search & infinite scroll |
| 3 | [AsyncSelect](#asyncselect) | `components/custom/async-select.tsx` | Select asinkron dengan search & pagination |
| 4 | [ConfirmPopover](#confirmpopover) | `components/custom/confirm-popover.tsx` | Popover konfirmasi aksi |
| 5 | [DataTable](#datatable) | `components/custom/datatable.tsx` | Tabel data dengan sorting, filter, pagination |
| 6 | [DataTable Components](#datatable-components) | `components/custom/` | Komponen pendukung DataTable |
| 7 | [DatePicker Components](#datepicker-components) | `components/custom/` | Komponen pemilih tanggal |
| 8 | [DeletePopover](#deletepopover) | `components/custom/delete-popover.tsx` | Popover konfirmasi hapus |
| 9 | [Form Components](#form-components) | `components/custom/form-card.tsx` | Komponen form dengan styling konsisten |
| 10 | [FormHeaderCard](#formheadercard) | `components/custom/form-header-card.tsx` | Header untuk halaman form |
| 11 | [FormModal](#formmodal) | `components/custom/form-modal.tsx` | Modal dialog untuk form |
| 12 | [FormNavTab](#formnavtab) | `components/custom/form-navtab.tsx` | Tab navigasi untuk form multi-bagian |
| 13 | [IconCombobox](#iconcombobox) | `components/custom/icon-combobox.tsx` | Combobox pemilih ikon Lucide |
| 14 | [LazyIcon](#lazyicon) | `components/custom/lazy-icon.tsx` | Ikon dengan lazy loading |
| 15 | [LoadingScreen](#loadingscreen) | `components/custom/loading-screen.tsx` | Fullscreen loading overlay |
| 16 | [ProtectedAccess](#protectedaccess) | `components/custom/protected-access.tsx` | Kontrol akses berbasis fitur |
| 17 | [Section Components](#section-components) | `components/custom/section.tsx` | Komponen layout halaman |
| 18 | [Sortable](#sortable) | `components/custom/sortable.tsx` | Drag-and-drop sorting |

---

## AppSidebar

**Lokasi:** `components/custom/app-sidebar.tsx`

### Deskripsi
Komponen navigasi sidebar utama yang menampilkan struktur menu aplikasi dengan bagian yang dapat dilipat (collapsible sections), pelacakan status aktif, dan informasi pengguna.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `items` | `MenuItem[]` | Tidak | `[]` | Array item menu yang akan ditampilkan |
| `activeMenus` | `string[]` | Tidak | `[]` | Array link menu yang aktif |
| `isLoading` | `boolean` | Tidak | `false` | Status loading sidebar |
| `isLogOuting` | `boolean` | Tidak | `false` | Status proses logout |
| `onClickLogout` | `() => void` | Tidak | - | Handler klik logout |
| `onNavigateHandler` | `(link: string, deepLevel: number) => void` | Ya | - | Fungsi navigasi |

### Fitur
- Struktur menu bertingkat dengan ikon
- Indikasi visual menu aktif dengan border kiri
- Dukungan menu bersarang (unlimited depth)
- Tampilan profil pengguna dengan role dan waktu saat ini
- State loading dengan skeleton
- Desain responsif dengan mode collapse

### Contoh Penggunaan
```tsx
<AppSidebar
  items={menuItems}
  activeMenus={['/dashboard', '/courses']}
  isLoading={false}
  onNavigateHandler={(link, level) => console.log(link, level)}
/>
```

---

## AsyncCombobox

**Lokasi:** `components/custom/async-combobox.tsx`

### Deskripsi
Komponen combobox asinkron yang mendukung pencarian, state loading, infinite scrolling, dan multiple selection.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `refSearch` | `RefObject<HTMLInputElement>` | Tidak | - | Referensi ke input pencarian |
| `refLoadMore` | `RefObject<HTMLDivElement>` | Tidak | - | Referensi untuk infinite scroll |
| `open` | `boolean` | Ya | - | Mengontrol visibilitas dropdown |
| `placeholder` | `string` | Tidak | - | Placeholder input |
| `value` | `any` | Tidak | - | Nilai yang dipilih |
| `disabled` | `boolean` | Tidak | `false` | Menonaktifkan komponen |
| `defaultValue` | `any` | Tidak | - | Nilai default |
| `multiple` | `boolean` | Tidak | `false` | Mengaktifkan multiple selection |
| `showClear` | `boolean` | Tidak | `true` | Menampilkan tombol clear |
| `isLoading` | `boolean` | Ya | - | State loading |
| `items` | `TData[]` | Ya | - | Array item yang ditampilkan |
| `modal` | `boolean` | Tidak | `false` | Render dalam konteks modal |
| `onOpenChange` | `(value: boolean) => void` | Ya | - | Handler perubahan open state |
| `onValueChange` | `(value: any) => void` | Tidak | - | Handler perubahan nilai |
| `onSearch` | `(value: any) => void` | Ya | - | Handler pencarian |
| `renderItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item |
| `renderSelectedItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item terpilih |

### Fitur
- Memuat data secara asinkron
- Pencarian dengan debounce (ditangani eksternal)
- Infinite scrolling dengan observer
- Dukungan multiple selection
- Custom rendering untuk item dan nilai terpilih
- Tombol clear untuk reset pilihan

---

## AsyncSelect

**Lokasi:** `components/custom/async-select.tsx`

### Deskripsi
Komponen select asinkron komprehensif dengan pencarian, state loading, pagination, dan multiple selection.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `multiple` | `boolean` | Tidak | `false` | Mengaktifkan multiple selection |
| `disabled` | `boolean` | Tidak | `false` | Menonaktifkan komponen |
| `open` | `boolean` | Tidak | `false` | Mengontrol visibilitas dropdown |
| `showClear` | `boolean` | Tidak | `false` | Menampilkan tombol clear |
| `items` | `TData[]` | Tidak | `[]` | Array item yang ditampilkan |
| `value` | `any` | Tidak | - | Nilai yang dipilih |
| `defaultValue` | `any` | Tidak | - | Nilai default |
| `isFetching` | `boolean` | Tidak | `false` | State fetching |
| `isLoading` | `boolean` | Tidak | `false` | State loading |
| `isFetchingNextPage` | `boolean` | Tidak | `false` | State fetching halaman berikutnya |
| `placeholder` | `string` | Tidak | `"Choose select"` | Placeholder input |
| `variant` | `"default"` | Tidak | `"default"` | Varian visual |
| `size` | `"default" \| "sm" \| "xs"` | Tidak | `"default"` | Ukuran komponen |
| `onIntersectingScroll` | `() => void` | Tidak | - | Handler infinite scroll |
| `onRefetch` | `() => void` | Tidak | - | Handler refetch |
| `onSearch` | `(value: string) => void` | Tidak | - | Handler pencarian |
| `onChange` | `(value: any) => void` | Tidak | - | Handler perubahan nilai |
| `onOpenChange` | `(value: boolean) => void` | Tidak | - | Handler perubahan open state |
| `renderItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item |
| `renderSelectedItem` | `(value: TData) => React.ReactNode` | Tidak | - | Custom render item terpilih |

### Fitur
- Pencarian dengan debounce bawaan
- Infinite scrolling dengan intersection observer
- State loading untuk initial load dan pagination
- Multiple size variants
- Styling kustom menggunakan class-variance-authority

### Contoh Penggunaan
```tsx
<AsyncSelect
  items={users}
  value={selectedUser}
  onChange={setSelectedUser}
  onSearch={handleSearch}
  placeholder="Pilih pengguna..."
  isFetching={isLoading}
/>
```

---

## ConfirmPopover

**Lokasi:** `components/custom/confirm-popover.tsx`

### Deskripsi
Komponen popover konfirmasi yang membutuhkan konfirmasi pengguna sebelum melakukan suatu tindakan.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `title` | `string` | Tidak | `"Confirmation?"` | Judul popover |
| `description` | `string` | Tidak | `"Are you sure to delete this data?"` | Pesan konfirmasi |
| `onConfirm` | `() => Promise<void> \| void` | Ya | - | Handler aksi konfirmasi |
| `onCancel` | `() => Promise<void> \| void` | Ya | - | Handler aksi batal |
| `onSuccess` | `() => void` | Tidak | `() => {}` | Callback setelah sukses |
| `open` | `boolean` | Tidak | - | Mengontrol visibilitas popover |
| `isLoading` | `boolean` | Tidak | - | State loading |
| `children` | `React.ReactNode` | Tidak | - | Elemen trigger |

### Contoh Penggunaan
```tsx
<ConfirmPopover
  title="Hapus Data?"
  description="Apakah Anda yakin ingin menghapus data ini?"
  onConfirm={handleDelete}
  onCancel={() => setIsOpen(false)}
>
  <Button>Hapus</Button>
</ConfirmPopover>
```

---

## DataTable

**Lokasi:** `components/custom/datatable.tsx`

### Deskripsi
Komponen tabel data yang powerful dengan fitur sorting, filtering, pagination, dan column visibility, menggunakan TanStack Table.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `columns` | `ColumnDef<TData, TValue>[]` | Ya | - | Definisi kolom tabel |
| `data` | `TData[]` | Ya | - | Data yang ditampilkan |
| `pagination` | `PaginationState` | Ya | - | State pagination |
| `defaultSorting` | `SortingState` | Tidak | `[]` | Default sorting |
| `rowCount` | `number` | Ya | - | Jumlah total baris |
| `totalItems` | `number` | Tidak | - | Jumlah total item |
| `isLoading` | `boolean` | Tidak | `false` | State loading |
| `lengths` | `number[]` | Tidak | `[10, 25, 50, 100]` | Opsi rows per page |
| `title` | `React.ReactNode` | Tidak | - | Judul tabel |
| `actions` | `React.ReactNode[]` | Tidak | - | Tombol aksi |
| `onRefresh` | `() => Promise<void> \| void` | Tidak | - | Handler refresh data |
| `hideColumnsButton` | `boolean` | Tidak | `true` | Sembunyikan tombol columns |
| `hideSearch` | `boolean` | Tidak | `true` | Sembunyikan pencarian |
| `hideActions` | `boolean` | Tidak | `false` | Sembunyikan actions |

### Fitur
- Sorting manual dengan query params
- Filtering global
- Column visibility toggle
- Pagination manual
- Loading overlay
- Responsive design

### Contoh Penggunaan
```tsx
<DataTable
  columns={columns}
  data={data}
  pagination={pagination}
  rowCount={totalRows}
  totalItems={totalItems}
  isLoading={isLoading}
  onRefresh={refetch}
  title="Daftar Pengguna"
/>
```

---

## DataTable Components

### DataTableHeader

**Lokasi:** `components/custom/datatable-header.tsx`

Komponen untuk header kolom tabel dengan dukungan sorting.

```tsx
<DataTableHeader info={headerContext}>
  {columnTitle}
</DataTableHeader>
```

### DataTablePagination

**Lokasi:** `components/custom/datatable-pagination.tsx`

Komponen pagination untuk navigasi halaman.

### DataTableHeaderCard

**Lokasi:** `components/custom/datatable-header-card.tsx`

Komponen header card untuk data table dengan informasi ringkasan.

```tsx
<DataTableHeaderCard
  title="Data Mahasiswa"
  totalItems={totalStudents}
  totalLabel="Total Mahasiswa"
  onSearch={handleSearch}
  searchValue={search}
  onSortChange={handleSortChange}
  sortValue={sort}
  sortOptions={[
    { value: 'name:asc', label: 'Nama A-Z' },
    { value: 'name:desc', label: 'Nama Z-A' }
  ]}
/>
```

### DataTableCell

**Lokasi:** `components/custom/datatables-cell.tsx`

Komponen wrapper untuk cell tabel dengan styling konsisten.

---

## DatePicker Components

### DatePickerSimple

**Lokasi:** `components/custom/date-of-birth.tsx`

Date picker sederhana untuk input tanggal lahir.

### DatePickerInput

**Lokasi:** `components/custom/date-picker.tsx` & `input-date.tsx`

Date picker yang lebih kompleks dengan input teks dan kalender dropdown.

#### Props untuk DatePickerInput
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `value` | `Date` | - | Nilai tanggal yang dipilih |
| `onChange` | `(date?: Date) => void` | - | Handler perubahan tanggal |
| `disabled` | `boolean` | `false` | Menonaktifkan komponen |
| `placeholder` | `string` | `"June 01, 2025"` | Placeholder input |

---

## DeletePopover

**Lokasi:** `components/custom/delete-popover.tsx`

### Deskripsi
Popover konfirmasi khusus untuk aksi hapus data.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `onConfirm` | `() => Promise<void> \| void` | Ya | - | Handler aksi hapus |
| `onSuccess` | `() => void` | Tidak | `() => {}` | Callback setelah sukses |
| `isLoading` | `boolean` | Tidak | - | State loading |

---

## Form Components

**Lokasi:** `components/custom/form-card.tsx`

### Deskripsi
Kumpulan komponen form dengan styling konsisten untuk membuat formulir.

### Komponen yang Tersedia

#### FormCard
Container utama untuk form dengan border dan shadow.

```tsx
<FormCard title="Informasi Pribadi">
  <FormRow cols={2}>
    <FormField label="Nama" required>
      <FormInput placeholder="Masukkan nama" />
    </FormField>
    <FormField label="Email">
      <FormInput type="email" placeholder="email@example.com" />
    </FormField>
  </FormRow>
</FormCard>
```

#### FormField
Wrapper untuk field form dengan label dan error message.

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `label` | `string` | - | Label field |
| `required` | `boolean` | `false` | Menampilkan indikator required |
| `error` | `string` | - | Pesan error |

#### FormInput
Input text dengan styling konsisten.

#### FormSelect
Dropdown select dengan styling konsisten.

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `options` | `Array<{ value: string; label: string }>` | `[]` | Opsi select |

#### FormTextarea
Textarea dengan styling konsisten.

#### FormRow
Grid layout untuk form fields.

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `cols` | `2 \| 3 \| 4` | `2` | Jumlah kolom grid |

---

## FormHeaderCard

**Lokasi:** `components/custom/form-header-card.tsx`

### Deskripsi
Header card untuk halaman form dengan tombol navigasi dan aksi.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `title` | `string` | Ya | - | Judul form |
| `subtitle` | `string` | Tidak | - | Subjudul |
| `mode` | `"create" \| "edit" \| "view"` | Ya | - | Mode form |
| `isLoading` | `boolean` | Tidak | `false` | State loading |
| `onAddNew` | `() => void` | Tidak | - | Handler tambah baru |
| `onSubmit` | `() => void` | Tidak | - | Handler submit |
| `onCancel` | `() => void` | Tidak | - | Handler batal |
| `onBack` | `() => void` | Tidak | - | Handler back |
| `backTo` | `string` | Tidak | - | URL navigasi back |
| `addButtonText` | `string` | Tidak | `"Add New"` | Teks tombol tambah |
| `submitButtonText` | `string` | Tidak | - | Teks tombol submit |
| `hideAddButton` | `boolean` | Tidak | `false` | Sembunyikan tombol tambah |
| `hideSubmitButton` | `boolean` | Tidak | `false` | Sembunyikan tombol submit |
| `hideCancelButton` | `boolean` | Tidak | `true` | Sembunyikan tombol batal |

### Contoh Penggunaan
```tsx
<FormHeaderCard
  title="Tambah Pengguna"
  mode="create"
  isLoading={isSubmitting}
  onSubmit={handleSubmit}
  onCancel={handleCancel}
  backTo="/users"
/>
```

---

## FormModal

**Lokasi:** `components/custom/form-modal.tsx`

### Deskripsi
Modal dialog untuk form dengan header, body, dan footer.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `isOpen` | `boolean` | Ya | - | Status modal terbuka |
| `onClose` | `() => void` | Tidak | - | Handler tutup modal |
| `title` | `string` | Ya | - | Judul modal |
| `subtitle` | `string` | Tidak | - | Subjudul |
| `children` | `React.ReactNode` | Ya | - | Konten modal |
| `footer` | `React.ReactNode` | Tidak | - | Footer modal |
| `size` | `"xs" \| "sm" \| "md" \| "lg" \| "xl" \| "2xl" \| "4xl" \| "full"` | Tidak | `"sm"` | Ukuran modal |
| `variant` | `"default" \| "sticky-footer"` | Tidak | `"default"` | Varian modal |

### FormModalFooter
Komponen footer untuk modal dengan tombol aksi.

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `onCancel` | `() => void` | - | Handler batal |
| `onSubmit` | `() => void` | - | Handler submit |
| `cancelText` | `string` | `"CANCEL"` | Teks tombol batal |
| `submitText` | `string` | `"SAVE"` | Teks tombol submit |
| `isLoading` | `boolean` | `false` | State loading |
| `hideCancel` | `boolean` | `false` | Sembunyikan tombol batal |
| `formId` | `string` | - | ID form untuk submit |

---

## FormNavTab

**Lokasi:** `components/custom/form-navtab.tsx`

### Deskripsi
Tab navigasi untuk form dengan beberapa bagian.

### Props

| Prop | Tipe | Required | Deskripsi |
|------|------|----------|-----------|
| `items` | `NavTabItem[]` | Ya | Array tab items |
| `activeTab` | `string` | Ya | ID tab yang aktif |
| `onTabChange` | `(tabId: string) => void` | Ya | Handler perubahan tab |

### Contoh Penggunaan
```tsx
<FormNavTab
  items={STUDENT_FORM_TABS}
  activeTab={activeTab}
  onTabChange={setActiveTab}
/>
```

---

## IconCombobox

**Lokasi:** `components/custom/icon-combobox.tsx`

### Deskripsi
Combobox untuk memilih ikon dari library Lucide React.

### Props

| Prop | Tipe | Deskripsi |
|------|------|-----------|
| `defaultValue` | `any` | Nilai default ikon |
| `onChange` | `(value: any) => void` | Handler perubahan ikon |

### Fitur
- Pencarian ikon secara real-time
- Infinite scroll untuk memuat lebih banyak ikon
- Preview ikon dalam dropdown

---

## LazyIcon

**Lokasi:** `components/custom/lazy-icon.tsx`

### Deskripsi
Komponen untuk memuat ikon Lucide secara lazy (code splitting) untuk optimalisasi performa.

### Props

| Prop | Tipe | Deskripsi |
|------|------|-----------|
| `name` | `string` | Nama ikon (PascalCase) |
| `...props` | `LucideProps` | Props ikon lainnya |

### Fitur
- Code splitting per ikon
- Fallback skeleton saat loading
- Otomatis konversi PascalCase ke kebab-case

### Contoh Penggunaan
```tsx
<LazyIcon name="Home" size={16} className="text-blue-500" />
```

---

## LoadingScreen

**Lokasi:** `components/custom/loading-screen.tsx`

### Deskripsi
Fullscreen loading overlay dengan animasi.

### Props

| Prop | Tipe | Deskripsi |
|------|------|-----------|
| `show` | `boolean` | Status tampilan loading screen |

---

## ProtectedAccess

**Lokasi:** `components/custom/protected-access.tsx`

### Deskripsi
Komponen untuk mengontrol akses berdasarkan fitur dan izin pengguna.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `route` | `string` | - | Route yang diakses |
| `feature` | `string` | - | Kode fitur yang diperlukan |
| `isPage` | `boolean` | `false` | Apakah ini halaman (menampilkan unauthorized page) |
| `children` | `React.ReactNode` | - | Konten yang dilindungi |

### Contoh Penggunaan
```tsx
<ProtectedAccess route="/users" feature="view">
  <UserList />
</ProtectedAccess>
```

---

## Section Components

**Lokasi:** `components/custom/section.tsx`

### Deskripsi
Kumpulan komponen untuk layout halaman dengan struktur konsisten.

### Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `SectionPage` | Container utama untuk halaman |
| `SectionPageHeader` | Header section dengan layout flex responsif |
| `SectionPageHeaderInfo` | Container untuk informasi judul dan deskripsi |
| `SectionPageHeaderTitle` | Judul utama halaman |
| `SectionPageHeaderDescription` | Deskripsi atau subtitle halaman |
| `SectionPageHeaderActions` | Container untuk tombol aksi |
| `SectionPageContent` | Container untuk konten utama halaman |

### Contoh Penggunaan
```tsx
<SectionPage>
  <SectionPageHeader>
    <SectionPageHeaderInfo>
      <SectionPageHeaderTitle>Dashboard</SectionPageHeaderTitle>
      <SectionPageHeaderDescription>Ringkasan data</SectionPageHeaderDescription>
    </SectionPageHeaderInfo>
    <SectionPageHeaderActions>
      <Button>Export</Button>
    </SectionPageHeaderActions>
  </SectionPageHeader>
  <SectionPageContent>
    {/* Konten halaman */}
  </SectionPageContent>
</SectionPage>
```

---

## Sortable

**Lokasi:** `components/custom/sortable.tsx`

### Deskripsi
Komponen untuk drag-and-drop sorting menggunakan dnd-kit-sortable-tree.

### Props

| Prop | Tipe | Deskripsi |
|------|------|-----------|
| `items` | `TreeItems<MinimalTreeItemData>` | Data items yang dapat di-sort |
| `onItemsChanged` | `(items: TreeItems<MinimalTreeItemData>) => void` | Handler perubahan urutan |

### Tipe Data
```typescript
type MinimalTreeItemData = {
  name: string;
  icon: string;
};
```

### Fitur
- Drag and drop sorting
- Ikon untuk setiap item
- Visual feedback saat drag
- Handle custom styling

### Contoh Penggunaan
```tsx
<Sortable
  items={menuItems}
  onItemsChanged={handleReorder}
/>
```

---

## Praktik Terbaik Penggunaan

### 1. State Management
- Gunakan state lokal untuk data komponen sederhana
- Gunakan Redux untuk state global yang digunakan banyak komponen
- Gunakan React Query untuk data server

### 2. Performance
- Gunakan `React.memo` untuk komponen yang sering re-render
- Gunakan `useMemo` dan `useCallback` untuk nilai dan fungsi yang mahal
- Manfaatkan lazy loading untuk ikon dan komponen besar

### 3. Accessibility
- Pastikan semua komponen interaktif memiliki label yang sesuai
- Gunakan semantic HTML
- Dukung keyboard navigation

### 4. Styling
- Gunakan Tailwind utility classes
- Ikuti sistem desain yang telah ditentukan
- Gunakan `cn()` atau `twMerge()` untuk conditional classes

---

## Menambahkan Komponen Baru

1. Buat file komponen baru di `src/components/custom/`
2. Ikuti struktur komponen dengan TypeScript
3. Definisikan props dengan interface
4. Export komponen dari file
5. Update dokumentasi ini dengan informasi komponen baru

### Template Komponen
```tsx
import { cn } from "@/lib/utils";

interface NewComponentProps {
  className?: string;
  children?: React.ReactNode;
}

export const NewComponent = ({ className, children }: NewComponentProps) => {
  return (
    <div className={cn("base-styles", className)}>
      {children}
    </div>
  );
};
```