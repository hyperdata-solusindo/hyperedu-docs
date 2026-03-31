---
title: "Popover"
sidebar_label: "Popover"
---

# Popover

**Lokasi:** `components/ui/popover.tsx`  
**Dokumentasi Resmi:** [Shadcn Popover](https://ui.shadcn.com/docs/components/popover)

## Deskripsi

Komponen popover untuk menampilkan konten sementara yang muncul di atas konten lain, dibangun di atas Radix UI Popover. Popover digunakan untuk menampilkan informasi tambahan, form kecil, atau menu aksi tanpa mengganggu alur utama pengguna.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Popover` | Container utama popover |
| `PopoverTrigger` | Trigger untuk membuka popover (button atau elemen lain) |
| `PopoverContent` | Konten popover |
| `PopoverAnchor` | Anchor positioning untuk popover |
| `PopoverHeader` | Header section popover |
| `PopoverTitle` | Judul popover |
| `PopoverDescription` | Deskripsi popover |

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `open` | `boolean` | - | Kontrol visibilitas popover (controlled) |
| `defaultOpen` | `boolean` | `false` | Status awal popover (uncontrolled) |
| `onOpenChange` | `(open: boolean) => void` | - | Callback ketika popover dibuka/ditutup |
| `align` | `"start" \| "center" \| "end"` | `"center"` | Posisi alignment popover |
| `sideOffset` | `number` | `4` | Jarak dari trigger |
| `className` | `string` | - | Custom CSS classes untuk PopoverContent |

## Contoh Penggunaan

### Basic Popover
```tsx
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";

<Popover>
  <PopoverTrigger asChild>
    <Button variant="outline">Open Popover</Button>
  </PopoverTrigger>
  <PopoverContent className="w-80">
    <div className="space-y-2">
      <h4 className="font-medium leading-none">Dimensions</h4>
      <p className="text-sm text-muted-foreground">
        Set the dimensions for the layer.
      </p>
      <div className="grid gap-2">
        <div className="grid grid-cols-3 items-center gap-4">
          <Label htmlFor="width">Width</Label>
          <Input id="width" defaultValue="100%" className="col-span-2 h-8" />
        </div>
        <div className="grid grid-cols-3 items-center gap-4">
          <Label htmlFor="maxWidth">Max. width</Label>
          <Input id="maxWidth" defaultValue="300px" className="col-span-2 h-8" />
        </div>
      </div>
    </div>
  </PopoverContent>
</Popover>
```

### Controlled Popover
```tsx
function ControlledPopover() {
  const [open, setOpen] = useState(false);

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger asChild>
        <Button>Toggle Popover</Button>
      </PopoverTrigger>
      <PopoverContent className="w-80">
        <div className="space-y-2">
          <h4 className="font-medium">Settings</h4>
          <p className="text-sm text-muted-foreground">
            Configure your preferences.
          </p>
          <div className="flex items-center gap-2 pt-2">
            <Button size="sm" onClick={() => setOpen(false)}>Save</Button>
            <Button variant="outline" size="sm" onClick={() => setOpen(false)}>
              Cancel
            </Button>
          </div>
        </div>
      </PopoverContent>
    </Popover>
  );
}
```

### Popover dengan Form
```tsx
function FormPopover() {
  const [open, setOpen] = useState(false);
  const [name, setName] = useState("");

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log("Submitted:", name);
    setOpen(false);
    setName("");
  };

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger asChild>
        <Button variant="outline">Add New Item</Button>
      </PopoverTrigger>
      <PopoverContent className="w-80" align="start">
        <form onSubmit={handleSubmit} className="space-y-4">
          <div className="space-y-2">
            <h4 className="font-medium leading-none">Create Item</h4>
            <p className="text-sm text-muted-foreground">
              Enter a name for the new item.
            </p>
          </div>
          <div className="space-y-2">
            <Input
              placeholder="Item name"
              value={name}
              onChange={(e) => setName(e.target.value)}
              autoFocus
            />
          </div>
          <div className="flex justify-end gap-2">
            <Button variant="outline" size="sm" type="button" onClick={() => setOpen(false)}>
              Cancel
            </Button>
            <Button size="sm" type="submit">Create</Button>
          </div>
        </form>
      </PopoverContent>
    </Popover>
  );
}
```

### Popover dengan Ikon Trigger
```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button variant="ghost" size="icon">
      <SettingsIcon className="h-4 w-4" />
    </Button>
  </PopoverTrigger>
  <PopoverContent className="w-56" align="end">
    <div className="space-y-2">
      <h4 className="font-medium">Settings</h4>
      <div className="space-y-1">
        <Button variant="ghost" className="w-full justify-start">
          <UserIcon className="mr-2 h-4 w-4" />
          Profile
        </Button>
        <Button variant="ghost" className="w-full justify-start">
          <BellIcon className="mr-2 h-4 w-4" />
          Notifications
        </Button>
        <Button variant="ghost" className="w-full justify-start">
          <LogOutIcon className="mr-2 h-4 w-4" />
          Logout
        </Button>
      </div>
    </div>
  </PopoverContent>
</Popover>
```

### Popover dengan Header dan Deskripsi
```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button variant="outline">Info</Button>
  </PopoverTrigger>
  <PopoverContent className="w-80">
    <PopoverHeader>
      <PopoverTitle>About this feature</PopoverTitle>
      <PopoverDescription>
        This feature allows you to customize your dashboard layout.
      </PopoverDescription>
    </PopoverHeader>
    <div className="mt-4">
      <p className="text-sm">
        You can drag and drop widgets to rearrange them.
      </p>
    </div>
  </PopoverContent>
</Popover>
```

### Popover dengan Scroll Area
```tsx
import { ScrollArea } from "@/components/ui/scroll-area";

<Popover>
  <PopoverTrigger asChild>
    <Button variant="outline">Notifications</Button>
  </PopoverTrigger>
  <PopoverContent className="w-96 p-0" align="end">
    <div className="p-4 border-b">
      <h4 className="font-medium">Notifications</h4>
      <p className="text-sm text-muted-foreground">You have 3 unread messages</p>
    </div>
    <ScrollArea className="h-[300px]">
      <div className="space-y-2 p-4">
        {Array.from({ length: 10 }).map((_, i) => (
          <div key={i} className="flex items-start gap-3 rounded-lg p-2 hover:bg-muted">
            <div className="h-8 w-8 rounded-full bg-primary/10 flex items-center justify-center">
              <BellIcon className="h-4 w-4" />
            </div>
            <div className="flex-1 space-y-1">
              <p className="text-sm font-medium">Notification {i + 1}</p>
              <p className="text-xs text-muted-foreground">
                This is a sample notification message.
              </p>
            </div>
          </div>
        ))}
      </div>
    </ScrollArea>
    <div className="p-4 border-t">
      <Button variant="outline" size="sm" className="w-full">View All</Button>
    </div>
  </PopoverContent>
</Popover>
```

### Popover dengan Menu Navigasi
```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button variant="ghost" size="sm">
      <MenuIcon className="h-4 w-4 mr-2" />
      Menu
    </Button>
  </PopoverTrigger>
  <PopoverContent className="w-48 p-0" align="start">
    <div className="py-1">
      <button className="w-full px-4 py-2 text-left text-sm hover:bg-muted">
        Dashboard
      </button>
      <button className="w-full px-4 py-2 text-left text-sm hover:bg-muted">
        Analytics
      </button>
      <button className="w-full px-4 py-2 text-left text-sm hover:bg-muted">
        Reports
      </button>
      <div className="h-px bg-border my-1" />
      <button className="w-full px-4 py-2 text-left text-sm text-red-600 hover:bg-muted">
        Logout
      </button>
    </div>
  </PopoverContent>
</Popover>
```

### Popover dengan Custom Position
```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button>Different Position</Button>
  </PopoverTrigger>
  <PopoverContent 
    className="w-64" 
    align="end" 
    sideOffset={10}
  >
    <p className="text-sm">
      This popover is aligned to the end with extra offset.
    </p>
  </PopoverContent>
</Popover>
```

### Popover dengan Hover Trigger
```tsx
function HoverPopover() {
  const [open, setOpen] = useState(false);

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger 
        asChild
        onMouseEnter={() => setOpen(true)}
        onMouseLeave={() => setOpen(false)}
      >
        <Button variant="ghost">Hover me</Button>
      </PopoverTrigger>
      <PopoverContent 
        className="w-64" 
        onMouseEnter={() => setOpen(true)}
        onMouseLeave={() => setOpen(false)}
      >
        <p className="text-sm">This popover appears on hover!</p>
      </PopoverContent>
    </Popover>
  );
}
```

## Kustomisasi

### Custom Styling
```tsx
<PopoverContent className="bg-primary text-primary-foreground border-none">
  <p>Custom colored popover</p>
</PopoverContent>
```

### Custom Arrow
Popover secara default tidak memiliki arrow. Untuk menambahkan arrow:

```tsx
<PopoverContent className="relative">
  <div className="absolute -top-2 left-1/2 -translate-x-1/2 w-4 h-4 rotate-45 bg-popover border-t border-l border-border" />
  <p>Popover with arrow</p>
</PopoverContent>
```

### No Padding Content
```tsx
<PopoverContent className="p-0">
  <div className="p-4 border-b">
    <h4>Header</h4>
  </div>
  <div className="p-4">
    <p>Content with custom padding</p>
  </div>
</PopoverContent>
```

## Best Practices

### 1. Gunakan untuk Konten Sekunder
Popover cocok untuk konten tambahan yang tidak esensial:
```tsx
// ✅ Baik: Informasi tambahan
<Popover>Help text, tips, additional options</Popover>

// ❌ Hindari: Form penting atau konfirmasi kritis
<Popover>Delete confirmation</Popover> // Gunakan Dialog untuk ini
```

### 2. Fokus Otomatis
Untuk form dalam popover, gunakan autoFocus:
```tsx
<Input autoFocus placeholder="Type here..." />
```

### 3. Close on Escape
Popover secara otomatis menutup saat tombol Escape ditekan.

### 4. Hindari Popover di Dalam Popover
Menghindari nested popover untuk UX yang lebih baik.

## Troubleshooting

### Popover Terpotong
- Periksa apakah parent element memiliki overflow: hidden
- Gunakan `modal={false}` untuk popover yang keluar dari container

### Popover Tidak Tertutup
- Pastikan `onOpenChange` di-handle dengan benar
- Periksa apakah ada kondisi yang mencegah close

### Posisi Popover Salah
- Gunakan prop `align` dan `sideOffset` untuk menyesuaikan posisi
- Pastikan trigger memiliki posisi yang jelas

## Referensi

- [Shadcn Popover Documentation](https://ui.shadcn.com/docs/components/popover)
- [Radix UI Popover](https://www.radix-ui.com/primitives/docs/components/popover)
- [MDN Popover API](https://developer.mozilla.org/en-US/docs/Web/API/Popover_API)
