---
title: "Badge"
sidebar_label: "Badge"
---

# Badge

**Lokasi:** `components/ui/badge.tsx`  
**Dokumentasi Resmi:** [Shadcn Badge](https://ui.shadcn.com/docs/components/badge)

## Deskripsi

Komponen untuk menampilkan status, label, atau indikator kecil dengan berbagai varian warna. Badge digunakan untuk memberikan informasi tambahan yang ringkas, seperti status item, jumlah notifikasi, atau label kategori.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "secondary" \| "destructive" \| "outline" \| "success" \| "warning" \| "info"` | `"default"` | Varian warna badge |
| `className` | `string` | - | Custom CSS classes tambahan |

## Varian

### Varian Default Shadcn
| Varian | Warna | Penggunaan |
|--------|-------|------------|
| **default** | Biru solid | Label utama, informasi umum |
| **secondary** | Abu-abu | Label sekunder, informasi kurang penting |
| **destructive** | Merah solid | Error, peringatan serius |
| **outline** | Border dengan teks berwarna | Label dengan tampilan minimalis |

### Varian Kustom HyperEdu
| Varian | Warna | Penggunaan |
|--------|-------|------------|
| **success** | Hijau pastel (`#E8F8EE` / `#2D9B63`) | Status aktif, sukses, verified |
| **warning** | Merah pastel (`#FEECEB` / `#EB5757`) | Status non-aktif, pending, warning |
| **info** | Abu-abu pastel (`#F1F5F9` / `#64748B`) | Informasi netral, class, label biasa |

## Contoh Penggunaan

### Basic Badges (Semua Varian)
```tsx
<div className="flex flex-wrap gap-2">
  <Badge>Default</Badge>
  <Badge variant="secondary">Secondary</Badge>
  <Badge variant="destructive">Destructive</Badge>
  <Badge variant="outline">Outline</Badge>
  <Badge variant="success">Success</Badge>
  <Badge variant="warning">Warning</Badge>
  <Badge variant="info">Info</Badge>
</div>
```

### Status Badges
```tsx
<div className="flex gap-2">
  <Badge variant="success">Active</Badge>
  <Badge variant="warning">Inactive</Badge>
  <Badge variant="info">Pending</Badge>
  <Badge variant="destructive">Blocked</Badge>
</div>
```

### Badge dengan Ikon
```tsx
<Badge variant="success" className="gap-1">
  <CheckCircleIcon className="h-3 w-3" />
  Verified
</Badge>

<Badge variant="warning" className="gap-1">
  <AlertTriangleIcon className="h-3 w-3" />
  Expired
</Badge>

<Badge variant="info" className="gap-1">
  <InfoIcon className="h-3 w-3" />
  New
</Badge>
```

### Dalam Tabel
```tsx
<Table>
  <TableHeader>
    <TableRow>
      <TableHead>Name</TableHead>
      <TableHead>Status</TableHead>
      <TableHead>Role</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {users.map((user) => (
      <TableRow key={user.id}>
        <TableCell>{user.name}</TableCell>
        <TableCell>
          <Badge variant={user.status === 'active' ? 'success' : 'warning'}>
            {user.status}
          </Badge>
        </TableCell>
        <TableCell>
          <Badge variant="outline">{user.role}</Badge>
        </TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### Badge pada Card
```tsx
<Card className="relative">
  <CardHeader>
    <div className="flex justify-between items-start">
      <CardTitle>Product Name</CardTitle>
      <Badge variant="success">In Stock</Badge>
    </div>
    <CardDescription>Product description here</CardDescription>
  </CardHeader>
  <CardContent>
    <p>$99.99</p>
  </CardContent>
</Card>
```

### Badge untuk Notifikasi
```tsx
<Button variant="ghost" size="icon" className="relative">
  <BellIcon className="h-5 w-5" />
  <Badge className="absolute -top-1 -right-1 px-1.5 py-0 text-[10px] min-w-[18px]">
    3
  </Badge>
</Button>
```

### Badge dengan Custom Warna
```tsx
<Badge className="bg-purple-100 text-purple-800 hover:bg-purple-200">
  Premium
</Badge>

<Badge className="bg-orange-100 text-orange-800 hover:bg-orange-200">
  Featured
</Badge>

<Badge className="bg-cyan-100 text-cyan-800 hover:bg-cyan-200">
  Beta
</Badge>
```

## Kustomisasi

### Menambahkan Varian Baru
Untuk menambahkan varian baru, modifikasi `badgeVariants` di `badge.tsx`:

```typescript
const badgeVariants = cva(
  "inline-flex items-center rounded-full border px-3 py-1 text-[10px] font-bold uppercase tracking-wider transition-colors",
  {
    variants: {
      variant: {
        // ... existing variants
        premium: "border-transparent bg-gradient-to-r from-yellow-400 to-yellow-600 text-white hover:from-yellow-500 hover:to-yellow-700",
        draft: "border-transparent bg-gray-100 text-gray-600 hover:bg-gray-200",
      },
    },
  }
);
```

### Custom Styling dengan className
```tsx
<Badge className="rounded-sm px-2 py-0.5 text-[9px] font-mono">
  Code: 123
</Badge>

<Badge className="border-2 border-dashed border-primary text-primary">
  Custom Style
</Badge>
```

## Best Practices

### 1. Pilih Varian yang Tepat
- **success**: Untuk status positif (active, completed, verified)
- **warning**: Untuk status negatif (inactive, expired, pending)
- **info**: Untuk status netral (new, updated, info)
- **destructive**: Untuk error atau aksi berbahaya

### 2. Gunakan untuk Informasi Ringkas
Badge sebaiknya hanya berisi 1-3 kata untuk menjaga readability:
```tsx
// ✅ Baik
<Badge variant="success">Active</Badge>

// ❌ Hindari: Terlalu panjang
<Badge variant="success">This item is currently active and available</Badge>
```

### 3. Konsistensi Warna
Gunakan varian yang konsisten di seluruh aplikasi:
- Status aktif selalu `success` (hijau)
- Status non-aktif selalu `warning` (merah)
- Status pending selalu `info` (abu-abu)

### 4. Aksesibilitas
Pastikan kontras warna cukup untuk readability:
```tsx
// Periksa kontras warna
<Badge variant="success">Active</Badge>  // Hijau gelap di background hijau muda
<Badge variant="warning">Inactive</Badge> // Merah gelap di background merah muda
```

### 5. Responsive
Badge secara otomatis menyesuaikan ukuran dengan teks:
```tsx
<Badge variant="outline" className="text-xs md:text-sm">
  Responsive Badge
</Badge>
```

## Troubleshooting

### Badge Tidak Terlihat
- Periksa apakah varian yang digunakan memiliki warna background yang sesuai
- Cek apakah ada CSS yang menimpa styling badge

### Teks Badge Terpotong
- Gunakan `whitespace-nowrap` untuk mencegah wrap
- Atau gunakan `truncate` untuk teks panjang:

```tsx
<Badge className="max-w-[100px] truncate">
  Very long text that will be truncated
</Badge>
```

### Badge Tidak Rounded
Badge memiliki `rounded-full` secara default. Untuk mengubah:

```tsx
<Badge className="rounded-sm">Semi-rounded</Badge>
<Badge className="rounded-none">Square</Badge>
```

## Referensi

- [Shadcn Badge Documentation](https://ui.shadcn.com/docs/components/badge)
- [Radix UI Badge](https://www.radix-ui.com/primitives/docs/components/badge)
- [Tailwind Border Radius](https://tailwindcss.com/docs/border-radius)
