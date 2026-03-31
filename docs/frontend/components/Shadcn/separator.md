---
title: "Separator"
sidebar_label: "Separator"
---

# Separator

**Lokasi:** `components/ui/separator.tsx`  
**Dokumentasi Resmi:** [Shadcn Separator](https://ui.shadcn.com/docs/components/separator)

## Deskripsi

Komponen garis pemisah horizontal atau vertikal, dibangun di atas Radix UI Separator. Separator digunakan untuk memisahkan konten secara visual menjadi bagian-bagian yang berbeda.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `orientation` | `"horizontal" \| "vertical"` | `"horizontal"` | Orientasi separator |
| `decorative` | `boolean` | `true` | Apakah separator bersifat dekoratif (untuk aksesibilitas) |
| `className` | `string` | - | Custom CSS classes tambahan |

## Contoh Penggunaan

### Basic Separator
```tsx
import { Separator } from "@/components/ui/separator";

<div className="space-y-4">
  <div>Content above</div>
  <Separator />
  <div>Content below</div>
</div>
```

### Separator Horizontal dengan Label
```tsx
<div className="relative">
  <div className="absolute inset-0 flex items-center">
    <Separator className="w-full" />
  </div>
  <div className="relative flex justify-center text-xs uppercase">
    <span className="bg-background px-2 text-muted-foreground">
      Or continue with
    </span>
  </div>
</div>
```

### Separator Vertikal
```tsx
<div className="flex h-10 items-center gap-4">
  <span>Item 1</span>
  <Separator orientation="vertical" />
  <span>Item 2</span>
  <Separator orientation="vertical" />
  <span>Item 3</span>
</div>
```

### Separator dalam Menu
```tsx
<div className="space-y-2">
  <button className="w-full text-left px-4 py-2 hover:bg-muted">Profile</button>
  <button className="w-full text-left px-4 py-2 hover:bg-muted">Settings</button>
  <Separator />
  <button className="w-full text-left px-4 py-2 hover:bg-muted text-red-600">
    Logout
  </button>
</div>
```

### Separator dalam Card
```tsx
<Card>
  <CardHeader>
    <CardTitle>Settings</CardTitle>
  </CardHeader>
  <CardContent className="space-y-4">
    <div className="space-y-2">
      <Label>General Settings</Label>
      <Input placeholder="Setting value" />
    </div>
    <Separator />
    <div className="space-y-2">
      <Label>Advanced Settings</Label>
      <Input placeholder="Advanced value" />
    </div>
  </CardContent>
</Card>
```

### Separator dengan Margin Kustom
```tsx
<Separator className="my-4" />
<Separator className="my-8" />
<Separator className="my-2" />
```

### Separator dengan Warna Kustom
```tsx
<Separator className="bg-primary" />
<Separator className="bg-destructive" />
<Separator className="bg-gradient-to-r from-primary to-secondary" />
```

### Separator dalam Sidebar
```tsx
<SidebarMenu>
  <SidebarMenuItem>
    <SidebarMenuButton>Dashboard</SidebarMenuButton>
  </SidebarMenuItem>
  <SidebarMenuItem>
    <SidebarMenuButton>Analytics</SidebarMenuButton>
  </SidebarMenuItem>
  <SidebarSeparator />
  <SidebarMenuItem>
    <SidebarMenuButton>Settings</SidebarMenuButton>
  </SidebarMenuItem>
</SidebarMenu>
```

### Separator dalam Form
```tsx
<form className="space-y-6">
  <div className="space-y-4">
    <h3 className="text-lg font-medium">Personal Information</h3>
    <Input placeholder="Full name" />
    <Input placeholder="Email" />
  </div>
  
  <Separator />
  
  <div className="space-y-4">
    <h3 className="text-lg font-medium">Address Information</h3>
    <Input placeholder="Street address" />
    <Input placeholder="City" />
  </div>
  
  <Separator />
  
  <Button type="submit">Submit</Button>
</form>
```

### Separator dalam Dropdown Menu
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button>Menu</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuItem>Profile</DropdownMenuItem>
    <DropdownMenuItem>Settings</DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem variant="destructive">Logout</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Separator dalam List
```tsx
<div className="divide-y">
  <div className="py-4">
    <p className="font-medium">Item 1</p>
    <p className="text-sm text-muted-foreground">Description for item 1</p>
  </div>
  <div className="py-4">
    <p className="font-medium">Item 2</p>
    <p className="text-sm text-muted-foreground">Description for item 2</p>
  </div>
  <div className="py-4">
    <p className="font-medium">Item 3</p>
    <p className="text-sm text-muted-foreground">Description for item 3</p>
  </div>
</div>
```

### Separator dengan Dashed Style (Custom)
```tsx
<Separator className="border-dashed" />
<Separator className="border-dotted" />
<Separator className="border-2" />
```

### Separator dengan Spacer
```tsx
<div className="flex items-center gap-4">
  <span>Left</span>
  <Separator className="flex-1" />
  <span>Center</span>
  <Separator className="flex-1" />
  <span>Right</span>
</div>
```

### Separator dalam Tab Navigation
```tsx
<div className="space-y-4">
  <div className="flex gap-6">
    <button className="pb-2 border-b-2 border-primary">Overview</button>
    <button className="pb-2 text-muted-foreground">Details</button>
    <button className="pb-2 text-muted-foreground">Reviews</button>
  </div>
  <Separator />
  <div>
    <p>Content for selected tab</p>
  </div>
</div>
```

### Separator dengan Icon
```tsx
<div className="relative">
  <div className="absolute inset-0 flex items-center">
    <Separator className="w-full" />
  </div>
  <div className="relative flex justify-center">
    <span className="bg-background px-2 text-muted-foreground">
      <HeartIcon className="h-4 w-4" />
    </span>
  </div>
</div>
```

## Kustomisasi

### Custom Thickness
```tsx
<Separator className="h-px bg-border" />  // Default
<Separator className="h-0.5 bg-border" /> // Lebih tebal
<Separator className="h-1 bg-primary" />  // Sangat tebal
```

### Custom Style
```tsx
<Separator className="bg-gradient-to-r from-transparent via-primary to-transparent" />
<Separator className="bg-primary/20" />
```

## Best Practices

### 1. Gunakan untuk Memisahkan Konten
Separator membantu memisahkan bagian yang berbeda secara visual:
```tsx
// ✅ Baik: Memisahkan section
<Section>
  <Content />
  <Separator />
  <OtherContent />
</Section>

// ❌ Hindari: Terlalu banyak separator
<Item />
<Separator />
<Item />
<Separator />
<Item />
```

### 2. Gunakan di Menu
Separator membantu memisahkan grup menu yang berbeda:
```tsx
<Menu>
  <MenuItem>Edit</MenuItem>
  <MenuItem>Copy</MenuItem>
  <Separator />
  <MenuItem>Delete</MenuItem>
</Menu>
```

### 3. Perhatikan Aksesibilitas
- Separator bersifat dekoratif secara default (`decorative={true}`)
- Jika memiliki makna semantik, set `decorative={false}`

### 4. Konsisten dengan Spacing
Gunakan margin yang konsisten di sekitar separator:
```tsx
// ✅ Baik: Spacing konsisten
<Separator className="my-4" />

// ❌ Hindari: Spacing berbeda di setiap penggunaan
```

## Troubleshooting

### Separator Tidak Terlihat
- Periksa warna background separator
- Pastikan ada konten di sekitar separator
- Cek z-index jika ada elemen lain yang menutupi

### Separator Vertikal Tidak Memiliki Tinggi
- Pastikan parent memiliki height
- Gunakan `h-full` pada parent jika diperlukan:

```tsx
<div className="h-10 flex items-center">
  <span>Left</span>
  <Separator orientation="vertical" className="h-8" />
  <span>Right</span>
</div>
```

## Referensi

- [Shadcn Separator Documentation](https://ui.shadcn.com/docs/components/separator)
- [Radix UI Separator](https://www.radix-ui.com/primitives/docs/components/separator)
- [MDN hr Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/hr)
