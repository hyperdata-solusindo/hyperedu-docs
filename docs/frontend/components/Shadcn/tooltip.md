---
title: "Tooltip"
sidebar_label: "Tooltip"
---

# Tooltip

**Lokasi:** `components/ui/tooltip.tsx`  
**Dokumentasi Resmi:** [Shadcn Tooltip](https://ui.shadcn.com/docs/components/tooltip)

## Deskripsi

Komponen tooltip dengan styling custom, dibangun di atas Radix UI Tooltip. Tooltip digunakan untuk menampilkan informasi tambahan saat pengguna mengarahkan kursor ke suatu elemen, memberikan konteks atau petunjuk tanpa mengganggu antarmuka.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `TooltipProvider` | Provider untuk mengatur delay dan konfigurasi global tooltip |
| `Tooltip` | Container utama tooltip |
| `TooltipTrigger` | Trigger yang memicu tooltip (elemen yang di-hover) |
| `TooltipContent` | Konten tooltip yang muncul |

## Props

### TooltipProvider
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `delayDuration` | `number` | `0` | Waktu delay sebelum tooltip muncul (ms) |
| `skipDelayDuration` | `number` | `300` | Waktu delay setelah tooltip tertutup sebelum bisa muncul lagi |

### TooltipContent
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `side` | `"top" \| "right" \| "bottom" \| "left"` | `"top"` | Posisi tooltip relatif terhadap trigger |
| `sideOffset` | `number` | `0` | Jarak dari trigger |
| `align` | `"start" \| "center" \| "end"` | `"center"` | Posisi alignment |
| `className` | `string` | - | Custom CSS classes |

## Contoh Penggunaan

### Basic Tooltip
```tsx
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip";

<TooltipProvider>
  <Tooltip>
    <TooltipTrigger asChild>
      <Button variant="outline">Hover me</Button>
    </TooltipTrigger>
    <TooltipContent>
      <p>This is a tooltip</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

### Tooltip dengan Icon
```tsx
<TooltipProvider>
  <Tooltip>
    <TooltipTrigger asChild>
      <Button variant="ghost" size="icon">
        <HelpCircleIcon className="h-4 w-4" />
      </Button>
    </TooltipTrigger>
    <TooltipContent>
      <p>Click for help documentation</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

### Tooltip dengan Posisi Berbeda
```tsx
<div className="flex gap-4">
  <Tooltip>
    <TooltipTrigger asChild>
      <Button>Top</Button>
    </TooltipTrigger>
    <TooltipContent side="top">
      <p>Tooltip on top</p>
    </TooltipContent>
  </Tooltip>

  <Tooltip>
    <TooltipTrigger asChild>
      <Button>Right</Button>
    </TooltipTrigger>
    <TooltipContent side="right">
      <p>Tooltip on right</p>
    </TooltipContent>
  </Tooltip>

  <Tooltip>
    <TooltipTrigger asChild>
      <Button>Bottom</Button>
    </TooltipTrigger>
    <TooltipContent side="bottom">
      <p>Tooltip on bottom</p>
    </TooltipContent>
  </Tooltip>

  <Tooltip>
    <TooltipTrigger asChild>
      <Button>Left</Button>
    </TooltipTrigger>
    <TooltipContent side="left">
      <p>Tooltip on left</p>
    </TooltipContent>
  </Tooltip>
</div>
```

### Tooltip dengan Konten Multiline
```tsx
<Tooltip>
  <TooltipTrigger asChild>
    <span className="text-sm text-muted-foreground cursor-help">
      What is this?
    </span>
  </TooltipTrigger>
  <TooltipContent className="max-w-xs">
    <p className="text-sm">
      This feature allows you to customize your dashboard layout. You can drag and drop
      widgets to rearrange them according to your preference.
    </p>
  </TooltipContent>
</Tooltip>
```

### Tooltip pada Ikon di Button
```tsx
<Tooltip>
  <TooltipTrigger asChild>
    <Button variant="ghost" size="icon">
      <TrashIcon className="h-4 w-4" />
    </Button>
  </TooltipTrigger>
  <TooltipContent>
    <p>Delete item</p>
  </TooltipContent>
</Tooltip>
```

### Tooltip pada Teks yang Dipotong (Truncated Text)
```tsx
<div className="w-48">
  <Tooltip>
    <TooltipTrigger asChild>
      <p className="truncate">
        This is a very long text that will be truncated if it exceeds the container width
      </p>
    </TooltipTrigger>
    <TooltipContent>
      <p className="max-w-xs">This is a very long text that will be truncated if it exceeds the container width</p>
    </TooltipContent>
  </Tooltip>
</div>
```

### Tooltip dengan Delay
```tsx
<TooltipProvider delayDuration={500}>
  <Tooltip>
    <TooltipTrigger asChild>
      <Button>Hover with delay</Button>
    </TooltipTrigger>
    <TooltipContent>
      <p>Appears after 500ms</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

### Tooltip pada Icon dalam Tabel
```tsx
<Table>
  <TableBody>
    {users.map((user) => (
      <TableRow key={user.id}>
        <TableCell>{user.name}</TableCell>
        <TableCell>{user.email}</TableCell>
        <TableCell>
          <div className="flex gap-1">
            <Tooltip>
              <TooltipTrigger asChild>
                <Button variant="ghost" size="icon-xs">
                  <EyeIcon className="h-3 w-3" />
                </Button>
              </TooltipTrigger>
              <TooltipContent>View details</TooltipContent>
            </Tooltip>

            <Tooltip>
              <TooltipTrigger asChild>
                <Button variant="ghost" size="icon-xs">
                  <EditIcon className="h-3 w-3" />
                </Button>
              </TooltipTrigger>
              <TooltipContent>Edit user</TooltipContent>
            </Tooltip>

            <Tooltip>
              <TooltipTrigger asChild>
                <Button variant="ghost" size="icon-xs">
                  <TrashIcon className="h-3 w-3" />
                </Button>
              </TooltipTrigger>
              <TooltipContent>Delete user</TooltipContent>
            </Tooltip>
          </div>
        </TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### Tooltip pada Badge
```tsx
<Tooltip>
  <TooltipTrigger asChild>
    <Badge variant="success" className="cursor-help">Active</Badge>
  </TooltipTrigger>
  <TooltipContent>
    <p>User is currently active and has access to all features</p>
  </TooltipContent>
</Tooltip>
```

### Tooltip dengan Shortcut Indicator
```tsx
<Tooltip>
  <TooltipTrigger asChild>
    <Button variant="ghost" size="sm">
      <SaveIcon className="h-4 w-4 mr-2" />
      Save
    </Button>
  </TooltipTrigger>
  <TooltipContent className="flex items-center gap-2">
    <span>Save changes</span>
    <kbd className="px-1.5 py-0.5 text-xs bg-muted rounded">⌘S</kbd>
  </TooltipContent>
</Tooltip>
```

### Tooltip Global Provider (Root Layout)
```tsx
// Di root layout atau App.tsx
function App() {
  return (
    <TooltipProvider delayDuration={200}>
      {/* Your app content */}
      <YourRoutes />
    </TooltipProvider>
  );
}
```

### Tooltip dengan Arrow (Custom Styling)
Tooltip sudah memiliki arrow secara default. Untuk custom styling:

```tsx
<TooltipContent className="bg-primary text-primary-foreground">
  <p className="text-sm">Custom colored tooltip</p>
</TooltipContent>
```

## Kustomisasi

### Custom Styling Tooltip
```tsx
<TooltipContent className="bg-red-500 text-white border-none">
  <p>Error tooltip</p>
</TooltipContent>

<TooltipContent className="bg-green-500 text-white border-none">
  <p>Success tooltip</p>
</TooltipContent>
```

### Custom Arrow Color
Arrow secara otomatis mengikuti warna background. Untuk custom:

```tsx
<TooltipContent className="bg-blue-500 text-white">
  <p>Blue tooltip with matching arrow</p>
</TooltipContent>
```

### No Arrow
Tooltip tidak memiliki prop untuk menghilangkan arrow, tetapi bisa dengan CSS:

```tsx
<TooltipContent className="[&_.fill-sidebar-primary]:hidden">
  <p>Tooltip without arrow</p>
</TooltipContent>
```

## Best Practices

### 1. Gunakan untuk Informasi Tambahan
Tooltip cocok untuk informasi yang melengkapi, bukan yang esensial:
```tsx
// ✅ Baik: Informasi tambahan
<Tooltip>Click here to learn more</Tooltip>

// ❌ Hindari: Informasi kritis yang harus dilihat
<Tooltip>Password is required</Tooltip> // Gunakan label error
```

### 2. Teks Singkat dan Jelas
Tooltip sebaiknya berisi teks singkat (1-2 kalimat):
```tsx
// ✅ Baik: Singkat dan jelas
<TooltipContent>Sort by name ascending</TooltipContent>

// ❌ Hindari: Terlalu panjang
<TooltipContent>This button will sort the list of users by their first name in alphabetical order from A to Z</TooltipContent>
```

### 3. Konsisten dengan Delay
Gunakan delay yang konsisten di seluruh aplikasi:
```tsx
<TooltipProvider delayDuration={200}>
  {/* All tooltips have same delay */}
</TooltipProvider>
```

### 4. Aksesibilitas
Tooltip sudah accessible dengan keyboard (focus + hover). Pastikan trigger dapat di-focus.

## Troubleshooting

### Tooltip Tidak Muncul
- Pastikan komponen dibungkus dengan `TooltipProvider`
- Periksa apakah ada elemen yang menutupi trigger
- Cek z-index tidak tertimpa

### Tooltip Terpotong
- Periksa apakah parent memiliki overflow: hidden
- Gunakan side yang berbeda (top, bottom, left, right)

### Delay Tidak Berfungsi
- Pastikan `delayDuration` di-set pada TooltipProvider
- Delay default adalah 0, set sesuai kebutuhan

## Referensi

- [Shadcn Tooltip Documentation](https://ui.shadcn.com/docs/components/tooltip)
- [Radix UI Tooltip](https://www.radix-ui.com/primitives/docs/components/tooltip)
- [MDN Tooltip](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/tooltip_role)
