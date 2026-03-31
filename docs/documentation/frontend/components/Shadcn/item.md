---
title: "Item"
sidebar_label: "Item"
---

# Item

**Lokasi:** `components/ui/item.tsx`  
**Dokumentasi Resmi:** - (Custom Component)

## Deskripsi

Komponen untuk menampilkan item dalam list dengan media, konten, dan aksi. Item menyediakan struktur yang konsisten untuk menampilkan data dalam format list, seperti daftar pengguna, produk, atau notifikasi, dengan dukungan media (ikon/gambar), konten utama, dan area aksi.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `ItemGroup` | Container untuk group item |
| `Item` | Item individual |
| `ItemMedia` | Area untuk media (ikon, gambar) |
| `ItemContent` | Konten utama item |
| `ItemTitle` | Judul item |
| `ItemDescription` | Deskripsi item |
| `ItemActions` | Area untuk tombol aksi |
| `ItemSeparator` | Separator antar item |
| `ItemHeader` | Header item |
| `ItemFooter` | Footer item |

## Props

### Item
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "outline" \| "muted"` | `"default"` | Varian visual item |
| `size` | `"default" \| "sm"` | `"default"` | Ukuran item |
| `asChild` | `boolean` | `false` | Render sebagai child component (untuk Link) |

### ItemMedia
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "icon" \| "image"` | `"default"` | Varian media |

## Contoh Penggunaan

### Basic Item
```tsx
import {
  Item,
  ItemContent,
  ItemDescription,
  ItemTitle,
} from "@/components/ui/item";

<Item>
  <ItemContent>
    <ItemTitle>John Doe</ItemTitle>
    <ItemDescription>john.doe@example.com</ItemDescription>
  </ItemContent>
</Item>
```

### Item dengan Media Icon
```tsx
<Item>
  <ItemMedia variant="icon">
    <UserIcon className="h-5 w-5" />
  </ItemMedia>
  <ItemContent>
    <ItemTitle>John Doe</ItemTitle>
    <ItemDescription>Administrator</ItemDescription>
  </ItemContent>
</Item>
```

### Item dengan Media Image
```tsx
<Item>
  <ItemMedia variant="image">
    <img 
      src="/avatar.jpg" 
      alt="User avatar" 
      className="h-10 w-10 object-cover"
    />
  </ItemMedia>
  <ItemContent>
    <ItemTitle>John Doe</ItemTitle>
    <ItemDescription>Product Designer</ItemDescription>
  </ItemContent>
</Item>
```

### Item dengan Actions
```tsx
<Item>
  <ItemMedia variant="icon">
    <FileIcon className="h-5 w-5" />
  </ItemMedia>
  <ItemContent>
    <ItemTitle>document.pdf</ItemTitle>
    <ItemDescription>2.5 MB • Uploaded 2 hours ago</ItemDescription>
  </ItemContent>
  <ItemActions>
    <Button variant="ghost" size="icon-xs">
      <DownloadIcon className="h-3 w-3" />
    </Button>
    <Button variant="ghost" size="icon-xs">
      <TrashIcon className="h-3 w-3" />
    </Button>
  </ItemActions>
</Item>
```

### Item dengan Header dan Footer
```tsx
<Item>
  <ItemHeader>
    <ItemTitle>Project Overview</ItemTitle>
    <Badge variant="success">Active</Badge>
  </ItemHeader>
  <ItemContent>
    <ItemDescription>
      This project is currently in development phase.
    </ItemDescription>
  </ItemContent>
  <ItemFooter>
    <div className="text-xs text-muted-foreground">
      Last updated: 2 days ago
    </div>
    <Button size="xs">View Details</Button>
  </ItemFooter>
</Item>
```

### Item Group dengan Separator
```tsx
<ItemGroup>
  <Item>
    <ItemMedia variant="icon">
      <UserIcon className="h-5 w-5" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>John Doe</ItemTitle>
      <ItemDescription>john@example.com</ItemDescription>
    </ItemContent>
  </Item>
  <ItemSeparator />
  <Item>
    <ItemMedia variant="icon">
      <UserIcon className="h-5 w-5" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>Jane Smith</ItemTitle>
      <ItemDescription>jane@example.com</ItemDescription>
    </ItemContent>
  </Item>
  <ItemSeparator />
  <Item>
    <ItemMedia variant="icon">
      <UserIcon className="h-5 w-5" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>Bob Johnson</ItemTitle>
      <ItemDescription>bob@example.com</ItemDescription>
    </ItemContent>
  </Item>
</ItemGroup>
```

### Item Variants
```tsx
<div className="space-y-2">
  <Item variant="default">
    <ItemContent>
      <ItemTitle>Default Item</ItemTitle>
    </ItemContent>
  </Item>
  <Item variant="outline">
    <ItemContent>
      <ItemTitle>Outline Item</ItemTitle>
    </ItemContent>
  </Item>
  <Item variant="muted">
    <ItemContent>
      <ItemTitle>Muted Item</ItemTitle>
    </ItemContent>
  </Item>
</div>
```

### Item Sizes
```tsx
<div className="space-y-2">
  <Item size="default">
    <ItemMedia variant="icon">
      <UserIcon className="h-5 w-5" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>Default Size</ItemTitle>
      <ItemDescription>This is the default item size</ItemDescription>
    </ItemContent>
  </Item>
  <Item size="sm">
    <ItemMedia variant="icon">
      <UserIcon className="h-4 w-4" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>Small Size</ItemTitle>
      <ItemDescription>Compact item size</ItemDescription>
    </ItemContent>
  </Item>
</div>
```

### Item dengan Badge
```tsx
<Item>
  <ItemMedia variant="icon">
    <ShoppingBagIcon className="h-5 w-5" />
  </ItemMedia>
  <ItemContent>
    <div className="flex items-center gap-2">
      <ItemTitle>Order #12345</ItemTitle>
      <Badge variant="warning">Pending</Badge>
    </div>
    <ItemDescription>Total: $299.99 • 3 items</ItemDescription>
  </ItemContent>
  <ItemActions>
    <Button variant="outline" size="xs">Track Order</Button>
  </ItemActions>
</Item>
```

### Item dengan Avatar (Custom Media)
```tsx
<Item>
  <ItemMedia variant="image" className="rounded-full overflow-hidden">
    <Avatar className="h-10 w-10">
      <AvatarImage src="/avatar.jpg" />
      <AvatarFallback>JD</AvatarFallback>
    </Avatar>
  </ItemMedia>
  <ItemContent>
    <ItemTitle>John Doe</ItemTitle>
    <ItemDescription>@johndoe • 1.2k followers</ItemDescription>
  </ItemContent>
  <ItemActions>
    <Button size="xs">Follow</Button>
  </ItemActions>
</Item>
```

### Item sebagai Link (asChild)
```tsx
<Item asChild>
  <Link to="/users/1">
    <ItemMedia variant="icon">
      <UserIcon className="h-5 w-5" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>John Doe</ItemTitle>
      <ItemDescription>View profile →</ItemDescription>
    </ItemContent>
  </Link>
</Item>
```

### Item dengan Checkbox Selection
```tsx
<Item>
  <div className="flex items-center gap-3">
    <Checkbox id="item-1" />
    <ItemMedia variant="icon">
      <FileIcon className="h-5 w-5" />
    </ItemMedia>
  </div>
  <ItemContent>
    <ItemTitle>Document.pdf</ItemTitle>
    <ItemDescription>Last modified: 2 hours ago</ItemDescription>
  </ItemContent>
  <ItemActions>
    <Button variant="ghost" size="icon-xs">
      <MoreHorizontalIcon className="h-3 w-3" />
    </Button>
  </ItemActions>
</Item>
```

### Item Group untuk Notification List
```tsx
<ItemGroup>
  <div className="px-4 py-2 bg-muted/50">
    <span className="text-xs font-medium">Today</span>
  </div>
  <Item>
    <ItemMedia variant="icon">
      <BellIcon className="h-5 w-5" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>New message from John</ItemTitle>
      <ItemDescription>2 minutes ago</ItemDescription>
    </ItemContent>
  </Item>
  <ItemSeparator />
  <div className="px-4 py-2 bg-muted/50">
    <span className="text-xs font-medium">Yesterday</span>
  </div>
  <Item>
    <ItemMedia variant="icon">
      <MailIcon className="h-5 w-5" />
    </ItemMedia>
    <ItemContent>
      <ItemTitle>Your order has been shipped</ItemTitle>
      <ItemDescription>Yesterday at 3:45 PM</ItemDescription>
    </ItemContent>
  </Item>
</ItemGroup>
```

## Kustomisasi

### Custom Styling
```tsx
<Item className="hover:bg-primary/5 transition-colors">
  <ItemContent>
    <ItemTitle>Custom Item</ItemTitle>
  </ItemContent>
</Item>
```

### Custom Media Size
```tsx
<ItemMedia variant="icon" className="h-12 w-12 bg-primary/10 rounded-lg">
  <Icon className="h-6 w-6" />
</ItemMedia>
```

## Best Practices

### 1. Konsisten dengan Variant
Gunakan variant yang konsisten untuk item dengan tingkat kepentingan yang sama.

### 2. Gunakan ItemGroup untuk List
ItemGroup memberikan struktur yang baik untuk daftar item yang panjang.

### 3. Aksesibilitas
- Untuk item yang dapat diklik, gunakan `asChild` dengan Link atau button
- Pastikan focus state terlihat jelas

### 4. Responsive Design
Item secara default responsive, dengan ukuran yang menyesuaikan pada mobile.

## Troubleshooting

### Item Tidak Rata
- Pastikan struktur Item terdiri dari komponen yang benar (ItemMedia, ItemContent, ItemActions)
- Periksa apakah ada margin/padding yang mengganggu

### Media Tidak Sejajar
- ItemMedia secara default memiliki `self-start` untuk menyelaraskan dengan konten
- Untuk alignment berbeda, gunakan `className` pada ItemMedia

## Referensi

- [Radix UI Primitives](https://www.radix-ui.com/)
- [Tailwind CSS Flexbox](https://tailwindcss.com/docs/flex)
