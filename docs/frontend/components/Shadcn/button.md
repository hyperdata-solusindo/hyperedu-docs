---
title: "Button"
sidebar_label: "Button"
---

# Button

**Lokasi:** `components/ui/button.tsx`  
**Dokumentasi Resmi:** [Shadcn Button](https://ui.shadcn.com/docs/components/button)

## Deskripsi

Komponen button dengan berbagai varian, ukuran, dan dukungan `asChild` untuk rendering sebagai link. Button adalah komponen interaktif fundamental yang digunakan untuk trigger aksi seperti submit form, navigasi, atau menjalankan fungsi.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "destructive" \| "outline" \| "secondary" \| "ghost" \| "link" \| "danger"` | `"default"` | Varian visual button |
| `size` | `"default" \| "xs" \| "sm" \| "lg" \| "icon" \| "icon-xs" \| "icon-sm" \| "icon-lg"` | `"default"` | Ukuran button |
| `asChild` | `boolean` | `false` | Render sebagai child component (untuk Link dari React Router) |
| `disabled` | `boolean` | `false` | Menonaktifkan button |

## Varian

| Varian | Deskripsi | Penggunaan |
|--------|-----------|------------|
| **default** | Primary button dengan background solid biru | Aksi utama (submit, save, create) |
| **destructive** | Warna merah dengan background solid | Aksi berbahaya seperti hapus data |
| **outline** | Border dengan background transparan | Aksi sekunder, tombol cancel |
| **secondary** | Warna abu-abu | Aksi alternatif yang tidak terlalu menonjol |
| **ghost** | Tanpa background, muncul saat hover | Aksi di dalam card, toolbar, atau menu |
| **link** | Tampilan seperti link teks biasa | Navigasi yang tampil seperti link |
| **danger** | Warna merah solid lebih mencolok | Aksi berbahaya yang perlu penekanan lebih |

## Ukuran

| Ukuran | Height | Penggunaan |
|--------|--------|------------|
| **xs** | `h-7` | Tombol kecil di dalam tabel atau card yang padat |
| **sm** | `h-8` | Tombol sekunder compact |
| **default** | `h-9` | Tombol standar untuk kebanyakan kasus |
| **lg** | `h-10` | Tombol besar untuk Call-to-Action (CTA) |
| **icon** | `size-9` | Tombol ikon standar |
| **icon-xs** | `size-6` | Tombol ikon sangat kecil (untuk toolbar padat) |
| **icon-sm** | `size-8` | Tombol ikon kecil |
| **icon-lg** | `size-10` | Tombol ikon besar |

## Contoh Penggunaan

### Basic Buttons (Semua Varian)
```tsx
<div className="flex flex-wrap gap-2">
  <Button variant="default">Save</Button>
  <Button variant="destructive">Delete</Button>
  <Button variant="outline">Cancel</Button>
  <Button variant="secondary">Settings</Button>
  <Button variant="ghost">Learn More</Button>
  <Button variant="link">Go to Dashboard</Button>
  <Button variant="danger">Delete Permanently</Button>
</div>
```

### Ukuran Button
```tsx
<div className="flex items-center gap-2">
  <Button size="xs">Extra Small</Button>
  <Button size="sm">Small</Button>
  <Button size="default">Default</Button>
  <Button size="lg">Large</Button>
</div>
```

### Icon Buttons (Tanpa Teks)
```tsx
<div className="flex gap-2">
  <Button size="icon">
    <HeartIcon className="h-4 w-4" />
  </Button>

  <Button size="icon-xs">
    <PlusIcon className="h-3 w-3" />
  </Button>

  <Button size="icon-sm">
    <EditIcon className="h-4 w-4" />
  </Button>

  <Button size="icon-lg">
    <TrashIcon className="h-5 w-5" />
  </Button>
</div>
```

### Button dengan Ikon dan Teks
```tsx
<div className="flex flex-wrap gap-2">
  <Button>
    <PlusIcon className="mr-2 h-4 w-4" />
    Add New
  </Button>

  <Button variant="outline">
    <DownloadIcon className="mr-2 h-4 w-4" />
    Export Data
  </Button>

  <Button variant="destructive">
    <TrashIcon className="mr-2 h-4 w-4" />
    Delete
  </Button>
</div>
```

### Loading State
```tsx
<Button disabled>
  <Loader2Icon className="mr-2 h-4 w-4 animate-spin" />
  Saving...
</Button>
```

### As Child dengan React Router (Navigasi)
```tsx
import { Link } from 'react-router-dom';

<Button asChild>
  <Link to="/dashboard">Go to Dashboard</Link>
</Button>
```

### Full Width Button
```tsx
<Button className="w-full">Sign In</Button>
```

### Button Group (Action Footer)
```tsx
<div className="flex justify-end gap-2 border-t pt-4">
  <Button variant="outline">Cancel</Button>
  <Button>Save Changes</Button>
</div>
```

## Kustomisasi

### Custom Variant
```tsx
// Untuk menambahkan varian baru, modifikasi buttonVariants di button.tsx
const buttonVariants = cva(
  // ... base styles
  {
    variants: {
      variant: {
        // ... existing variants
        gradient: "bg-gradient-to-r from-purple-500 to-pink-500 text-white hover:from-purple-600 hover:to-pink-600",
      },
    },
  }
);

// Penggunaan
<Button variant="gradient">Premium Feature</Button>
```

### Custom Styling dengan className
```tsx
<Button className="bg-indigo-600 hover:bg-indigo-700 text-white shadow-lg rounded-full">
  Custom Button
</Button>
```

## Best Practices

### 1. Pilih Varian yang Tepat
- **default**: Hanya satu button utama per halaman/form
- **outline**: Untuk aksi sekunder yang tidak terlalu penting
- **destructive/danger**: Untuk aksi yang tidak dapat di-undo

### 2. Loading State
Selalu tampilkan loading state untuk aksi async:
```tsx
const [isLoading, setIsLoading] = useState(false);

const handleSubmit = async () => {
  setIsLoading(true);
  try {
    await saveData();
  } finally {
    setIsLoading(false);
  }
};

<Button onClick={handleSubmit} disabled={isLoading}>
  {isLoading ? <Loader2Icon className="mr-2 h-4 w-4 animate-spin" /> : null}
  {isLoading ? "Saving..." : "Save"}
</Button>
```

### 3. Aksesibilitas
- Berikan `aria-label` untuk icon-only button
- Pastikan kontras warna memenuhi standar WCAG
- Gunakan `asChild` untuk link agar tetap accessible

```tsx
<Button size="icon" aria-label="Delete item">
  <TrashIcon className="h-4 w-4" />
</Button>
```

### 4. Konsistensi Ukuran
Gunakan ukuran yang konsisten di seluruh aplikasi:
- `xs` untuk tombol dalam tabel
- `sm` untuk tombol di card atau modal
- `default` untuk tombol utama
- `lg` untuk CTA di halaman landing

## Troubleshooting

### Button Tidak Bisa Diklik
- Pastikan tidak ada `disabled={true}`
- Periksa apakah ada elemen lain yang menutupi button
- Cek apakah fungsi handler sudah di-pass dengan benar

### Icon Tidak Muncul
- Pastikan icon di-import dengan benar dari `lucide-react`
- Cek ukuran icon: gunakan `h-4 w-4` untuk ukuran standar

### Button Berubah Ukuran Saat Loading
- Gunakan fixed width untuk mencegah layout shift:
```tsx
<Button className="min-w-[100px]">
  {isLoading ? "Loading..." : "Submit"}
</Button>
```

## Referensi

- [Shadcn Button Documentation](https://ui.shadcn.com/docs/components/button)
- [Radix UI Button](https://www.radix-ui.com/primitives/docs/components/button)
- [Lucide Icons](https://lucide.dev/icons/)
