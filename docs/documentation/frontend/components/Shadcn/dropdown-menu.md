---
title: "Dropdown Menu"
sidebar_label: "Dropdown Menu"
---

# Dropdown Menu

**Lokasi:** `components/ui/dropdown-menu.tsx`  
**Dokumentasi Resmi:** [Shadcn Dropdown Menu](https://ui.shadcn.com/docs/components/dropdown-menu)

## Deskripsi

Komponen dropdown menu dengan submenu, checkbox items, dan radio items, dibangun di atas Radix UI Dropdown Menu. Dropdown Menu digunakan untuk menampilkan daftar aksi atau opsi yang dapat dipilih pengguna, seperti menu konteks, menu aksi, atau menu pengaturan.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `DropdownMenu` | Container utama dropdown menu |
| `DropdownMenuTrigger` | Trigger untuk membuka dropdown (button atau elemen lain) |
| `DropdownMenuContent` | Konten dropdown |
| `DropdownMenuItem` | Item menu individual |
| `DropdownMenuGroup` | Group items dalam dropdown |
| `DropdownMenuLabel` | Label untuk group |
| `DropdownMenuSeparator` | Separator antar items |
| `DropdownMenuCheckboxItem` | Item checkbox (dapat dicentang) |
| `DropdownMenuRadioGroup` | Group untuk radio items |
| `DropdownMenuRadioItem` | Item radio (mutually exclusive) |
| `DropdownMenuSub` | Submenu |
| `DropdownMenuSubTrigger` | Trigger untuk submenu |
| `DropdownMenuSubContent` | Konten submenu |
| `DropdownMenuShortcut` | Shortcut indicator (seperti ⌘K) |

## Props

### DropdownMenuItem
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "destructive"` | `"default"` | Varian item (destructive untuk warna merah) |
| `inset` | `boolean` | `false` | Memberikan padding kiri ekstra untuk item |

## Contoh Penggunaan

### Basic Dropdown Menu
```tsx
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";

<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button variant="outline">Open Menu</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuLabel>My Account</DropdownMenuLabel>
    <DropdownMenuSeparator />
    <DropdownMenuItem>Profile</DropdownMenuItem>
    <DropdownMenuItem>Settings</DropdownMenuItem>
    <DropdownMenuItem>Billing</DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem variant="destructive">Logout</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Dropdown Menu dengan Ikon
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button variant="ghost" size="icon">
      <MoreHorizontalIcon className="h-4 w-4" />
    </Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent align="end">
    <DropdownMenuItem className="gap-2">
      <UserIcon className="h-4 w-4" />
      <span>View Profile</span>
    </DropdownMenuItem>
    <DropdownMenuItem className="gap-2">
      <EditIcon className="h-4 w-4" />
      <span>Edit</span>
    </DropdownMenuItem>
    <DropdownMenuItem className="gap-2">
      <CopyIcon className="h-4 w-4" />
      <span>Duplicate</span>
    </DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem className="gap-2 text-red-600">
      <TrashIcon className="h-4 w-4" />
      <span>Delete</span>
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Dropdown Menu dengan Shortcut
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button>Actions</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuItem>
      <span>New Tab</span>
      <DropdownMenuShortcut>⌘T</DropdownMenuShortcut>
    </DropdownMenuItem>
    <DropdownMenuItem>
      <span>New Window</span>
      <DropdownMenuShortcut>⌘N</DropdownMenuShortcut>
    </DropdownMenuItem>
    <DropdownMenuItem>
      <span>Save</span>
      <DropdownMenuShortcut>⌘S</DropdownMenuShortcut>
    </DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem>
      <span>Print</span>
      <DropdownMenuShortcut>⌘P</DropdownMenuShortcut>
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Dropdown Menu dengan Checkbox Items
```tsx
function CheckboxDropdown() {
  const [showStatusBar, setShowStatusBar] = useState(true);
  const [showActivityBar, setShowActivityBar] = useState(false);
  const [showPanel, setShowPanel] = useState(false);

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="outline">View Options</Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent>
        <DropdownMenuLabel>Appearance</DropdownMenuLabel>
        <DropdownMenuSeparator />
        <DropdownMenuCheckboxItem
          checked={showStatusBar}
          onCheckedChange={setShowStatusBar}
        >
          Status Bar
        </DropdownMenuCheckboxItem>
        <DropdownMenuCheckboxItem
          checked={showActivityBar}
          onCheckedChange={setShowActivityBar}
        >
          Activity Bar
        </DropdownMenuCheckboxItem>
        <DropdownMenuCheckboxItem
          checked={showPanel}
          onCheckedChange={setShowPanel}
        >
          Panel
        </DropdownMenuCheckboxItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}
```

### Dropdown Menu dengan Radio Items
```tsx
function RadioDropdown() {
  const [theme, setTheme] = useState("light");

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="outline">Theme: {theme}</Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent>
        <DropdownMenuLabel>Select Theme</DropdownMenuLabel>
        <DropdownMenuSeparator />
        <DropdownMenuRadioGroup value={theme} onValueChange={setTheme}>
          <DropdownMenuRadioItem value="light">
            Light
          </DropdownMenuRadioItem>
          <DropdownMenuRadioItem value="dark">
            Dark
          </DropdownMenuRadioItem>
          <DropdownMenuRadioItem value="system">
            System
          </DropdownMenuRadioItem>
        </DropdownMenuRadioGroup>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}
```

### Dropdown Menu dengan Submenu
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button>File</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuItem>New File</DropdownMenuItem>
    <DropdownMenuItem>Open...</DropdownMenuItem>
    <DropdownMenuSub>
      <DropdownMenuSubTrigger>Recent Files</DropdownMenuSubTrigger>
      <DropdownMenuSubContent>
        <DropdownMenuItem>file1.tsx</DropdownMenuItem>
        <DropdownMenuItem>file2.tsx</DropdownMenuItem>
        <DropdownMenuItem>file3.tsx</DropdownMenuItem>
        <DropdownMenuSeparator />
        <DropdownMenuItem>Clear Recent</DropdownMenuItem>
      </DropdownMenuSubContent>
    </DropdownMenuSub>
    <DropdownMenuSeparator />
    <DropdownMenuItem>Save</DropdownMenuItem>
    <DropdownMenuItem>Save As...</DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem variant="destructive">Exit</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Dropdown Menu dalam Tabel (Row Actions)
```tsx
function TableRowActions({ row }) {
  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="ghost" size="icon" className="h-8 w-8">
          <MoreHorizontalIcon className="h-4 w-4" />
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="end">
        <DropdownMenuItem onClick={() => onView(row)}>
          View Details
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => onEdit(row)}>
          Edit
        </DropdownMenuItem>
        <DropdownMenuSeparator />
        <DropdownMenuItem 
          onClick={() => onDelete(row)} 
          variant="destructive"
        >
          Delete
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}
```

### Dropdown Menu dengan Custom Trigger (Avatar)
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Avatar className="cursor-pointer">
      <AvatarImage src="/avatar.jpg" />
      <AvatarFallback>JD</AvatarFallback>
    </Avatar>
  </DropdownMenuTrigger>
  <DropdownMenuContent align="end" className="w-56">
    <DropdownMenuLabel className="font-normal">
      <div className="flex flex-col space-y-1">
        <p className="text-sm font-medium">John Doe</p>
        <p className="text-xs text-muted-foreground">john@example.com</p>
      </div>
    </DropdownMenuLabel>
    <DropdownMenuSeparator />
    <DropdownMenuItem>Profile</DropdownMenuItem>
    <DropdownMenuItem>Settings</DropdownMenuItem>
    <DropdownMenuItem>Billing</DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem variant="destructive">Log out</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Dropdown Menu dengan Inset Items
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button>Menu</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuItem inset>Show All</DropdownMenuItem>
    <DropdownMenuItem inset>Show Unread</DropdownMenuItem>
    <DropdownMenuItem inset>Show Starred</DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem inset>Settings</DropdownMenuItem>
    <DropdownMenuItem inset>Help</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Dropdown Menu dengan Disabled Items
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button>Actions</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuItem>Edit</DropdownMenuItem>
    <DropdownMenuItem disabled>Delete (Not Available)</DropdownMenuItem>
    <DropdownMenuItem>Duplicate</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

## Kustomisasi

### Custom Styling
```tsx
<DropdownMenuContent className="bg-primary text-primary-foreground">
  <DropdownMenuItem>Custom Item</DropdownMenuItem>
</DropdownMenuContent>
```

### Custom Width
```tsx
<DropdownMenuContent className="w-56">
  {/* content */}
</DropdownMenuContent>
```

### Custom Alignment
```tsx
<DropdownMenuContent align="start">
  {/* aligns to start */}
</DropdownMenuContent>

<DropdownMenuContent align="center">
  {/* aligns to center */}
</DropdownMenuContent>

<DropdownMenuContent align="end">
  {/* aligns to end */}
</DropdownMenuContent>
```

## Best Practices

### 1. Gunakan untuk Aksi yang Relevan
Dropdown menu cocok untuk aksi sekunder yang tidak perlu selalu terlihat:
```tsx
// ✅ Baik: Aksi sekunder dalam dropdown
<DropdownMenu>
  <DropdownMenuItem>Edit</DropdownMenuItem>
  <DropdownMenuItem>Delete</DropdownMenuItem>
</DropdownMenu>

// ❌ Hindari: Aksi utama yang penting
// Sebaiknya aksi "Save" ditampilkan sebagai button utama
```

### 2. Ikon untuk Meningkatkan Pengenalan
Gunakan ikon untuk membantu pengguna mengenali aksi lebih cepat:
```tsx
<DropdownMenuItem className="gap-2">
  <EditIcon className="h-4 w-4" />
  <span>Edit</span>
</DropdownMenuItem>
```

### 3. Shortcut untuk Power Users
Tambahkan shortcut untuk aksi yang sering digunakan.

### 4. Destructive Actions dengan Varian
Gunakan variant destructive untuk aksi berbahaya:
```tsx
<DropdownMenuItem variant="destructive">Delete</DropdownMenuItem>
```

## Troubleshooting

### Dropdown Tidak Muncul
- Pastikan `DropdownMenuTrigger` dan `DropdownMenuContent` di-render
- Periksa z-index tidak tertimpa

### Item Tidak Dapat Diklik
- Pastikan tidak ada `disabled` prop
- Periksa event handler tidak error

### Checkbox/Radio Tidak Berfungsi
- Pastikan menggunakan state management yang benar
- Periksa `onCheckedChange` dan `onValueChange` diimplementasikan

## Referensi

- [Shadcn Dropdown Menu Documentation](https://ui.shadcn.com/docs/components/dropdown-menu)
- [Radix UI Dropdown Menu](https://www.radix-ui.com/primitives/docs/components/dropdown-menu)
- [WAI-ARIA Menu Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu/)
