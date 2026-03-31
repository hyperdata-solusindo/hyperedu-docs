---
title: "LazyIcon"
sidebar_label: "LazyIcon"
---

# LazyIcon

**Lokasi:** `components/custom/lazy-icon.tsx`

## Deskripsi

Komponen untuk memuat ikon Lucide secara lazy (code splitting) untuk optimalisasi performa. LazyIcon memuat ikon hanya saat diperlukan, mengurangi ukuran bundle awal dan meningkatkan waktu muat halaman.

## Fitur

- Code splitting per ikon (hanya ikon yang digunakan yang dimuat)
- Fallback skeleton saat loading
- Otomatis konversi PascalCase ke kebab-case untuk dynamic import
- Dukungan penuh props LucideIcon
- Menampilkan placeholder jika ikon tidak ditemukan

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `name` | `string` | Ya | - | Nama ikon dalam format PascalCase (contoh: "Home", "User", "Settings") |
| `...props` | `LucideProps` | Tidak | - | Props ikon lainnya (size, color, className, dll) |

## Konversi Nama Ikon

LazyIcon secara otomatis mengkonversi nama ikon dari PascalCase ke kebab-case:

| PascalCase | kebab-case |
|------------|-----------|
| `Home` | `home` |
| `User` | `user` |
| `Settings` | `settings` |
| `AlignLeft` | `align-left` |
| `ArrowUpDown` | `arrow-up-down` |
| `LayoutDashboard` | `layout-dashboard` |

## Contoh Penggunaan

### Basic Usage
```tsx
import { LazyIcon } from "@/components/custom/lazy-icon";

<LazyIcon name="Home" size={24} className="text-primary" />
```

### Dengan Berbagai Ukuran
```tsx
<div className="flex gap-4 items-center">
  <LazyIcon name="Star" size={16} />
  <LazyIcon name="Star" size={20} />
  <LazyIcon name="Star" size={24} />
  <LazyIcon name="Star" size={32} />
</div>
```

### Dengan Warna
```tsx
<div className="flex gap-4">
  <LazyIcon name="Heart" className="text-red-500" />
  <LazyIcon name="Heart" className="text-blue-500" />
  <LazyIcon name="Heart" className="text-green-500" />
  <LazyIcon name="Heart" className="text-yellow-500" />
</div>
```

### Dalam Button
```tsx
<Button>
  <LazyIcon name="Plus" size={16} className="mr-2" />
  Add Item
</Button>
```

### Dalam Menu Sidebar
```tsx
<SidebarMenuButton>
  <LazyIcon name={item.icon} size={18} />
  <span>{item.name}</span>
</SidebarMenuButton>
```

### Fallback untuk Ikon Tidak Ditemukan
```tsx
// Jika ikon tidak ditemukan, akan menampilkan div kosong
<LazyIcon name="NonExistentIcon" size={24} />
// Output: <div className="w-4 h-4 rounded" />
```

### Dengan Loading State
```tsx
function IconWithLoading() {
  const [showIcon, setShowIcon] = useState(false);

  return (
    <div>
      <Button onClick={() => setShowIcon(!showIcon)}>
        Toggle Icon
      </Button>
      {showIcon && (
        <LazyIcon name="RefreshCw" size={24} className="ml-2 animate-spin" />
      )}
    </div>
  );
}
```

### Dalam Tabel
```tsx
<Table>
  <TableBody>
    {menuItems.map((item) => (
      <TableRow key={item.id}>
        <TableCell>
          <LazyIcon name={item.icon} size={16} />
        </TableCell>
        <TableCell>{item.name}</TableCell>
        <TableCell>{item.link}</TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### Dalam Card
```tsx
<Card>
  <CardHeader>
    <div className="flex items-center gap-3">
      <div className="p-2 bg-primary/10 rounded-lg">
        <LazyIcon name="Settings" size={20} className="text-primary" />
      </div>
      <CardTitle>Settings</CardTitle>
    </div>
  </CardHeader>
  <CardContent>
    <p>Configure your application preferences</p>
  </CardContent>
</Card>
```

### Dengan Conditional Rendering
```tsx
function StatusIcon({ status }: { status: string }) {
  const iconMap = {
    active: "CheckCircle",
    inactive: "XCircle",
    pending: "Clock",
    error: "AlertCircle",
  };

  const iconName = iconMap[status as keyof typeof iconMap] || "HelpCircle";

  return (
    <LazyIcon 
      name={iconName} 
      size={16}
      className={
        status === "active" ? "text-green-500" :
        status === "inactive" ? "text-red-500" :
        status === "pending" ? "text-yellow-500" :
        "text-gray-500"
      }
    />
  );
}
```

## Best Practices

### 1. Gunakan untuk Semua Ikon
Gunakan LazyIcon untuk semua ikon di aplikasi untuk optimalisasi bundle size:
```tsx
// ✅ Baik: Lazy loading
<LazyIcon name="Home" size={20} />

// ❌ Hindari: Import langsung (menambah bundle size)
import { Home } from "lucide-react";
<Home size={20} />
```

### 2. Ukuran Ikon yang Konsisten
Gunakan ukuran ikon yang konsisten di seluruh aplikasi:
```tsx
// Icon sizes:
// 14px - small (toolbar, badges)
// 16px - default (buttons, menu items)
// 20px - medium (card icons)
// 24px - large (hero sections)
<LazyIcon name="Star" size={16} />
```

### 3. Warna Ikon dengan Tailwind
Gunakan Tailwind classes untuk warna:
```tsx
<LazyIcon name="Heart" className="text-red-500" />
```

### 4. Loading Fallback
LazyIcon sudah memiliki fallback skeleton, tidak perlu menambahkan loading state manual.

## Performance Considerations

### Bundle Size Optimization
- Ikon dimuat secara terpisah menggunakan dynamic imports
- Hanya ikon yang digunakan yang akan dimuat ke bundle
- Mengurangi initial bundle size secara signifikan

### Code Splitting
```typescript
// Setiap ikon memiliki chunk terpisah
const iconImport = dynamicIconImports[kebabName];
return lazy(iconImport);
```

## Troubleshooting

### Ikon Tidak Muncul
- Periksa apakah nama ikon sesuai dengan nama di [Lucide Icons](https://lucide.dev/icons/)
- Pastikan nama ikon dalam format PascalCase
- Cek console untuk error loading

### Loading Lambat
- Ikon dimuat secara lazy, mungkin ada delay kecil pertama kali
- Tidak masalah karena hanya terjadi sekali per ikon

### Ikon Tidak Terlihat di Production
- Pastikan build process mendukung dynamic imports
- Periksa konfigurasi Vite untuk code splitting

## Referensi

- [Lucide Icons](https://lucide.dev/icons/)
- [Lucide Dynamic Imports](https://lucide.dev/guide/packages/lucide-react#dynamic-imports)
- [React.lazy Documentation](https://react.dev/reference/react/lazy)
- [Vite Code Splitting](https://vitejs.dev/guide/features.html#code-splitting)
