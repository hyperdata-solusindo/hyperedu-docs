---
sidebar_position: 3
---

# UI Components

## Overview

Dokumentasi ini menyediakan panduan lengkap untuk komponen UI dasar yang digunakan dalam aplikasi frontend HyperEdu. Komponen-komponen ini dibangun di atas Radix UI primitives dan menggunakan Tailwind CSS untuk styling, dengan fokus pada aksesibilitas, konsistensi, dan kemudahan penggunaan.

Komponen-komponen ini merupakan fondasi dari sistem desain HyperEdu dan digunakan di seluruh aplikasi untuk membangun antarmuka yang konsisten.

---

## Daftar Komponen

1. [Badge](#badge)
2. [Breadcrumb](#breadcrumb)
3. [Button](#button)
4. [Calendar](#calendar)
5. [Card](#card)
6. [Checkbox](#checkbox)
7. [Collapsible](#collapsible)
8. [Combobox](#combobox)
9. [Dialog](#dialog)
10. [Dropdown Menu](#dropdown-menu)
11. [Field](#field)
12. [Input](#input)
13. [Input Group](#input-group)
14. [Item](#item)
15. [Label](#label)
16. [Navigation Menu](#navigation-menu)
17. [Pagination](#pagination)
18. [Popover](#popover)
19. [Scroll Area](#scroll-area)
20. [Select](#select)
21. [Separator](#separator)
22. [Sheet](#sheet)
23. [Sidebar](#sidebar)
24. [Skeleton](#skeleton)
25. [Sonner (Toaster)](#sonner-toaster)
26. [Table](#table)
27. [Textarea](#textarea)
28. [Tooltip](#tooltip)

---

## Badge

**Lokasi:** `components/ui/badge.tsx`

### Deskripsi
Komponen untuk menampilkan status, label, atau indikator kecil dengan berbagai varian warna.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "secondary" \| "destructive" \| "outline" \| "success" \| "warning" \| "info"` | `"default"` | Varian warna badge |
| `className` | `string` | - | Custom CSS classes |

### Varian
- **default**: Warna primary (biru)
- **secondary**: Warna abu-abu
- **destructive**: Warna merah untuk status error/hapus
- **outline**: Border dengan teks berwarna
- **success**: Hijau pastel untuk status aktif/sukses
- **warning**: Merah pastel untuk status non-aktif
- **info**: Abu-abu untuk status netral

### Contoh Penggunaan
```tsx
<Badge variant="success">Active</Badge>
<Badge variant="warning">Inactive</Badge>
<Badge variant="info">Pending</Badge>
```

---

## Breadcrumb

**Lokasi:** `components/ui/breadcrumb.tsx`

### Deskripsi
Komponen navigasi untuk menunjukkan lokasi pengguna dalam hierarki halaman.

### Komponen yang Tersedia
- `Breadcrumb`: Container utama
- `BreadcrumbList`: Container untuk daftar breadcrumb
- `BreadcrumbItem`: Item individual
- `BreadcrumbLink`: Link navigasi
- `BreadcrumbPage`: Halaman saat ini (bukan link)
- `BreadcrumbSeparator`: Pemisah antar item
- `BreadcrumbEllipsis`: Indikator item yang di-singkat

### Contoh Penggunaan
```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbLink href="/dashboard">Dashboard</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Users</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

---

## Button

**Lokasi:** `components/ui/button.tsx`

### Deskripsi
Komponen button dengan berbagai varian, ukuran, dan dukungan asChild untuk rendering sebagai link.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "destructive" \| "outline" \| "secondary" \| "ghost" \| "link" \| "danger"` | `"default"` | Varian button |
| `size` | `"default" \| "xs" \| "sm" \| "lg" \| "icon" \| "icon-xs" \| "icon-sm" \| "icon-lg"` | `"default"` | Ukuran button |
| `asChild` | `boolean` | `false` | Render sebagai child component (untuk Link) |
| `disabled` | `boolean` | `false` | Menonaktifkan button |

### Varian
- **default**: Primary button dengan background solid
- **destructive**: Warna merah untuk aksi berbahaya
- **outline**: Border dengan background transparan
- **secondary**: Warna abu-abu
- **ghost**: Tanpa background, muncul saat hover
- **link**: Tampilan seperti link teks
- **danger**: Warna merah solid

### Ukuran
- **xs**: Extra small (h-7)
- **sm**: Small (h-8)
- **default**: Default (h-9)
- **lg**: Large (h-10)
- **icon**: Square button untuk ikon (size-9)
- **icon-xs**: Extra small icon button (size-6)
- **icon-sm**: Small icon button (size-8)
- **icon-lg**: Large icon button (size-10)

### Contoh Penggunaan
```tsx
<Button variant="default">Save</Button>
<Button variant="outline" size="sm">Cancel</Button>
<Button asChild>
  <Link to="/dashboard">Go to Dashboard</Link>
</Button>
<Button variant="destructive" size="icon-xs">
  <Trash2 className="h-3 w-3" />
</Button>
```

---

## Calendar

**Lokasi:** `components/ui/calendar.tsx`

### Deskripsi
Komponen kalender untuk memilih tanggal, built on top of react-day-picker.

### Props
Menggunakan props dari `react-day-picker` dengan tambahan:

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `buttonVariant` | `ButtonProps["variant"]` | `"ghost"` | Varian button untuk navigasi |

### Fitur
- Single date selection
- Range selection support
- Dropdown untuk bulan/tahun
- Customizable styling
- Keyboard navigation

### Contoh Penggunaan
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  captionLayout="dropdown"
/>
```

---

## Card

**Lokasi:** `components/ui/card.tsx`

### Deskripsi
Komponen container dengan border, shadow, dan struktur yang konsisten.

### Komponen yang Tersedia
- `Card`: Container utama
- `CardHeader`: Header section
- `CardTitle`: Judul card
- `CardDescription`: Deskripsi/subjudul
- `CardAction`: Area untuk aksi (tombol)
- `CardContent`: Konten utama
- `CardFooter`: Footer section

### Contoh Penggunaan
```tsx
<Card>
  <CardHeader>
    <CardTitle>User Profile</CardTitle>
    <CardDescription>Manage your account settings</CardDescription>
  </CardHeader>
  <CardContent>
    <p>Content goes here</p>
  </CardContent>
  <CardFooter>
    <Button>Save Changes</Button>
  </CardFooter>
</Card>
```

---

## Checkbox

**Lokasi:** `components/ui/checkbox.tsx`

### Deskripsi
Komponen checkbox dengan styling konsisten, dibangun di atas Radix UI Checkbox.

### Props
Menggunakan props dari `@radix-ui/react-checkbox`

| Prop | Tipe | Deskripsi |
|------|------|-----------|
| `checked` | `boolean \| "indeterminate"` | Status checkbox |
| `disabled` | `boolean` | Menonaktifkan checkbox |

### Contoh Penggunaan
```tsx
<Checkbox
  checked={isChecked}
  onCheckedChange={setIsChecked}
  disabled={isLoading}
/>
```

---

## Collapsible

**Lokasi:** `components/ui/collapsible.tsx`

### Deskripsi
Komponen untuk area yang dapat dilipat/dibuka.

### Komponen yang Tersedia
- `Collapsible`: Container utama
- `CollapsibleTrigger`: Trigger untuk membuka/menutup
- `CollapsibleContent`: Konten yang dilipat

### Contoh Penggunaan
```tsx
<Collapsible open={isOpen} onOpenChange={setIsOpen}>
  <CollapsibleTrigger asChild>
    <Button>Toggle Content</Button>
  </CollapsibleTrigger>
  <CollapsibleContent>
    <p>Hidden content that appears when expanded</p>
  </CollapsibleContent>
</Collapsible>
```

---

## Combobox

**Lokasi:** `components/ui/combobox.tsx`

### Deskripsi
Komponen combobox dengan fitur pencarian, multiple selection, dan chips, dibangun di atas Base UI Combobox.

### Komponen yang Tersedia
- `Combobox`: Container utama
- `ComboboxInput`: Input dengan trigger dan clear button
- `ComboboxContent`: Dropdown content
- `ComboboxList`: List item container
- `ComboboxItem`: Item individual
- `ComboboxGroup`: Group items
- `ComboboxLabel`: Label untuk group
- `ComboboxEmpty`: Pesan saat tidak ada item
- `ComboboxSeparator`: Separator
- `ComboboxChips`: Container untuk multiple selection chips
- `ComboboxChip`: Chip untuk item terpilih
- `ComboboxTrigger`: Trigger button
- `ComboboxValue`: Nilai yang terpilih
- `ComboboxObserver`: Observer untuk infinite scroll

### Fitur
- Pencarian real-time
- Multiple selection dengan chips
- Infinite scrolling
- Keyboard navigation
- Custom rendering

### Contoh Penggunaan (Single Selection)
```tsx
<Combobox>
  <ComboboxInput placeholder="Search..." />
  <ComboboxContent>
    <ComboboxList>
      {items.map((item) => (
        <ComboboxItem key={item.id} value={item}>
          {item.name}
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

### Contoh Penggunaan (Multiple Selection)
```tsx
<Combobox multiple>
  <ComboboxChips placeholder="Select items...">
    <ComboboxChipsInput />
  </ComboboxChips>
  <ComboboxContent>
    <ComboboxList>
      {items.map((item) => (
        <ComboboxItem key={item.id} value={item}>
          {item.name}
        </ComboboxItem>
      ))}
    </ComboboxList>
  </ComboboxContent>
</Combobox>
```

---

## Dialog

**Lokasi:** `components/ui/dialog.tsx`

### Deskripsi
Komponen modal dialog dengan overlay, dibangun di atas Radix UI Dialog.

### Komponen yang Tersedia
- `Dialog`: Container utama
- `DialogTrigger`: Trigger untuk membuka dialog
- `DialogContent`: Konten dialog
- `DialogHeader`: Header section
- `DialogTitle`: Judul dialog
- `DialogDescription`: Deskripsi dialog
- `DialogFooter`: Footer dengan tombol aksi
- `DialogClose`: Tombol close

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `showCloseButton` | `boolean` | `true` | Menampilkan tombol close di pojok |

### Contoh Penggunaan
```tsx
<Dialog open={isOpen} onOpenChange={setIsOpen}>
  <DialogTrigger asChild>
    <Button>Open Dialog</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Confirm Action</DialogTitle>
      <DialogDescription>Are you sure you want to proceed?</DialogDescription>
    </DialogHeader>
    <div className="py-4">
      <p>Dialog content goes here</p>
    </div>
    <DialogFooter>
      <Button variant="outline" onClick={() => setIsOpen(false)}>Cancel</Button>
      <Button onClick={handleConfirm}>Confirm</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

---

## Dropdown Menu

**Lokasi:** `components/ui/dropdown-menu.tsx`

### Deskripsi
Komponen dropdown menu dengan submenu, checkbox items, dan radio items, dibangun di atas Radix UI Dropdown Menu.

### Komponen yang Tersedia
- `DropdownMenu`: Container utama
- `DropdownMenuTrigger`: Trigger
- `DropdownMenuContent`: Konten dropdown
- `DropdownMenuItem`: Item menu
- `DropdownMenuGroup`: Group items
- `DropdownMenuLabel`: Label untuk group
- `DropdownMenuSeparator`: Separator
- `DropdownMenuCheckboxItem`: Item checkbox
- `DropdownMenuRadioGroup`: Group radio items
- `DropdownMenuRadioItem`: Item radio
- `DropdownMenuSub`: Submenu
- `DropdownMenuShortcut`: Shortcut indicator

### Contoh Penggunaan
```tsx
<DropdownMenu>
  <DropdownMenuTrigger asChild>
    <Button>Actions</Button>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuLabel>My Account</DropdownMenuLabel>
    <DropdownMenuSeparator />
    <DropdownMenuItem>Profile</DropdownMenuItem>
    <DropdownMenuItem>Settings</DropdownMenuItem>
    <DropdownMenuSeparator />
    <DropdownMenuItem variant="destructive">Delete</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

---

## Field

**Lokasi:** `components/ui/field.tsx`

### Deskripsi
Komponen untuk membungkus form field dengan label, deskripsi, dan error message.

### Komponen yang Tersedia
- `FieldSet`: Container untuk group field
- `FieldLegend`: Legend untuk fieldset
- `FieldGroup`: Group fields
- `Field`: Container utama field
- `FieldContent`: Konten field
- `FieldLabel`: Label field
- `FieldDescription`: Deskripsi field
- `FieldError`: Pesan error
- `FieldSeparator`: Separator antar field
- `FieldTitle`: Title untuk field

### Props Field

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `orientation` | `"vertical" \| "horizontal" \| "responsive"` | `"vertical"` | Orientasi layout |

### Contoh Penggunaan
```tsx
<Field>
  <FieldLabel>Email Address</FieldLabel>
  <FieldContent>
    <Input type="email" placeholder="user@example.com" />
    <FieldDescription>We'll never share your email.</FieldDescription>
    <FieldError>Email is required</FieldError>
  </FieldContent>
</Field>
```

---

## Input

**Lokasi:** `components/ui/input.tsx`

### Deskripsi
Komponen input text dengan berbagai ukuran dan varian.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default"` | `"default"` | Varian input |
| `inputSize` | `"default" \| "sm" \| "xs"` | `"default"` | Ukuran input |

### Contoh Penggunaan
```tsx
<Input 
  type="text" 
  placeholder="Enter your name" 
  value={value}
  onChange={(e) => setValue(e.target.value)}
  inputSize="sm"
/>
```

---

## Input Group

**Lokasi:** `components/ui/input-group.tsx`

### Deskripsi
Komponen untuk mengelompokkan input dengan addon (ikon, tombol, teks).

### Komponen yang Tersedia
- `InputGroup`: Container utama
- `InputGroupAddon`: Addon di kiri/kanan
- `InputGroupButton`: Tombol dalam input group
- `InputGroupText`: Teks dalam input group
- `InputGroupInput`: Input utama
- `InputGroupTextarea`: Textarea dalam input group

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "noOutline"` | `"default"` | Varian input group |
| `size` | `"default" \| "sm" \| "xs"` | `"xs"` | Ukuran |

### Contoh Penggunaan
```tsx
<InputGroup>
  <InputGroupAddon align="inline-start">
    <Search className="h-4 w-4" />
  </InputGroupAddon>
  <InputGroupInput placeholder="Search..." />
</InputGroup>
```

---

## Item

**Lokasi:** `components/ui/item.tsx`

### Deskripsi
Komponen untuk menampilkan item dalam list dengan media, konten, dan aksi.

### Komponen yang Tersedia
- `ItemGroup`: Container untuk group item
- `Item`: Item individual
- `ItemMedia`: Area untuk media (ikon, gambar)
- `ItemContent`: Konten utama
- `ItemTitle`: Judul item
- `ItemDescription`: Deskripsi
- `ItemActions`: Area untuk aksi
- `ItemSeparator`: Separator
- `ItemHeader`: Header item
- `ItemFooter`: Footer item

### Contoh Penggunaan
```tsx
<Item>
  <ItemMedia variant="icon">
    <UserIcon className="h-5 w-5" />
  </ItemMedia>
  <ItemContent>
    <ItemTitle>John Doe</ItemTitle>
    <ItemDescription>john.doe@example.com</ItemDescription>
  </ItemContent>
  <ItemActions>
    <Button size="xs">Edit</Button>
  </ItemActions>
</Item>
```

---

## Label

**Lokasi:** `components/ui/label.tsx`

### Deskripsi
Komponen label untuk form fields, dibangun di atas Radix UI Label.

### Contoh Penggunaan
```tsx
<Label htmlFor="name">Name</Label>
<Input id="name" type="text" />
```

---

## Navigation Menu

**Lokasi:** `components/ui/navigation-menu.tsx`

### Deskripsi
Komponen menu navigasi dengan dropdown, dibangun di atas Radix UI Navigation Menu.

### Komponen yang Tersedia
- `NavigationMenu`: Container utama
- `NavigationMenuList`: Daftar menu items
- `NavigationMenuItem`: Item menu
- `NavigationMenuTrigger`: Trigger untuk dropdown
- `NavigationMenuContent`: Konten dropdown
- `NavigationMenuLink`: Link dalam menu
- `NavigationMenuIndicator`: Indikator

### Contoh Penggunaan
```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Products</NavigationMenuTrigger>
      <NavigationMenuContent>
        <ul className="grid w-48 gap-1 p-2">
          <li>
            <NavigationMenuLink href="/product1">Product 1</NavigationMenuLink>
          </li>
        </ul>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

---

## Pagination

**Lokasi:** `components/ui/pagination.tsx`

### Deskripsi
Komponen pagination untuk navigasi halaman.

### Komponen yang Tersedia
- `Pagination`: Container utama
- `PaginationContent`: Container untuk items
- `PaginationItem`: Item individual
- `PaginationLink`: Link halaman
- `PaginationPrevious`: Tombol previous
- `PaginationNext`: Tombol next
- `PaginationEllipsis`: Indikator halaman tersembunyi

### Contoh Penggunaan
```tsx
<Pagination>
  <PaginationContent>
    <PaginationItem>
      <PaginationPrevious href="#" />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#" isActive>1</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">2</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationNext href="#" />
    </PaginationItem>
  </PaginationContent>
</Pagination>
```

---

## Popover

**Lokasi:** `components/ui/popover.tsx`

### Deskripsi
Komponen popover untuk menampilkan konten sementara, dibangun di atas Radix UI Popover.

### Komponen yang Tersedia
- `Popover`: Container utama
- `PopoverTrigger`: Trigger
- `PopoverContent`: Konten popover
- `PopoverAnchor`: Anchor positioning
- `PopoverHeader`: Header
- `PopoverTitle`: Title
- `PopoverDescription`: Description

### Contoh Penggunaan
```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button>Open Popover</Button>
  </PopoverTrigger>
  <PopoverContent className="w-80">
    <PopoverHeader>
      <PopoverTitle>Settings</PopoverTitle>
      <PopoverDescription>Configure your preferences</PopoverDescription>
    </PopoverHeader>
    <div className="py-2">
      {/* Content */}
    </div>
  </PopoverContent>
</Popover>
```

---

## Scroll Area

**Lokasi:** `components/ui/scroll-area.tsx`

### Deskripsi
Komponen area scroll dengan styling custom, dibangun di atas Radix UI Scroll Area.

### Contoh Penggunaan
```tsx
<ScrollArea className="h-96">
  <div className="p-4">
    {/* Long content */}
    {Array.from({ length: 50 }).map((_, i) => (
      <p key={i}>Item {i + 1}</p>
    ))}
  </div>
</ScrollArea>
```

---

## Select

**Lokasi:** `components/ui/select.tsx`

### Deskripsi
Komponen select dropdown dengan group dan scroll, dibangun di atas Radix UI Select.

### Komponen yang Tersedia
- `Select`: Container utama
- `SelectTrigger`: Trigger button
- `SelectValue`: Nilai yang terpilih
- `SelectContent`: Konten dropdown
- `SelectGroup`: Group items
- `SelectItem`: Item individual
- `SelectLabel`: Label group
- `SelectSeparator`: Separator
- `SelectScrollUpButton`: Scroll up button
- `SelectScrollDownButton`: Scroll down button

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `size` | `"xs" \| "sm" \| "default"` | `"default"` | Ukuran trigger |

### Contoh Penggunaan
```tsx
<Select value={value} onValueChange={setValue}>
  <SelectTrigger className="w-48">
    <SelectValue placeholder="Select option" />
  </SelectTrigger>
  <SelectContent>
    <SelectGroup>
      <SelectLabel>Options</SelectLabel>
      <SelectItem value="1">Option 1</SelectItem>
      <SelectItem value="2">Option 2</SelectItem>
    </SelectGroup>
  </SelectContent>
</Select>
```

---

## Separator

**Lokasi:** `components/ui/separator.tsx`

### Deskripsi
Komponen garis pemisah horizontal atau vertikal, dibangun di atas Radix UI Separator.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `orientation` | `"horizontal" \| "vertical"` | `"horizontal"` | Orientasi separator |

### Contoh Penggunaan
```tsx
<Separator />
<Separator orientation="vertical" className="h-10" />
```

---

## Sheet

**Lokasi:** `components/ui/sheet.tsx`

### Deskripsi
Komponen drawer/sheet yang muncul dari sisi layar, dibangun di atas Radix UI Dialog.

### Komponen yang Tersedia
- `Sheet`: Container utama
- `SheetTrigger`: Trigger
- `SheetContent`: Konten sheet
- `SheetHeader`: Header
- `SheetTitle`: Title
- `SheetDescription`: Description
- `SheetFooter`: Footer
- `SheetClose`: Tombol close

### Props SheetContent

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `side` | `"top" \| "right" \| "bottom" \| "left"` | `"right"` | Posisi sheet |
| `showCloseButton` | `boolean` | `true` | Menampilkan tombol close |

### Contoh Penggunaan
```tsx
<Sheet open={isOpen} onOpenChange={setIsOpen}>
  <SheetTrigger asChild>
    <Button>Open Sheet</Button>
  </SheetTrigger>
  <SheetContent side="right">
    <SheetHeader>
      <SheetTitle>Edit Profile</SheetTitle>
      <SheetDescription>Make changes to your profile here.</SheetDescription>
    </SheetHeader>
    <div className="flex-1 py-4">
      {/* Content */}
    </div>
    <SheetFooter>
      <Button variant="outline" onClick={() => setIsOpen(false)}>Cancel</Button>
      <Button onClick={handleSave}>Save Changes</Button>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

---

## Sidebar

**Lokasi:** `components/ui/sidebar.tsx`

### Deskripsi
Komponen sidebar lengkap dengan support collapsible, responsive, dan mobile view.

### Komponen yang Tersedia
- `SidebarProvider`: Provider untuk state sidebar
- `Sidebar`: Container utama sidebar
- `SidebarTrigger`: Tombol toggle sidebar
- `SidebarHeader`: Header section
- `SidebarContent`: Konten scrollable
- `SidebarFooter`: Footer section
- `SidebarMenu`: Menu container
- `SidebarMenuItem`: Item menu
- `SidebarMenuButton`: Tombol menu
- `SidebarMenuSub`: Submenu
- `SidebarMenuSubButton`: Tombol submenu
- `SidebarRail`: Rail untuk toggle
- `SidebarInset`: Container konten utama

### Hooks
- `useSidebar`: Hook untuk mengakses state sidebar

### Contoh Penggunaan
```tsx
<SidebarProvider defaultOpen={true}>
  <Sidebar>
    <SidebarHeader>Logo</SidebarHeader>
    <SidebarContent>
      <SidebarMenu>
        <SidebarMenuItem>
          <SidebarMenuButton asChild>
            <Link to="/dashboard">Dashboard</Link>
          </SidebarMenuButton>
        </SidebarMenuItem>
      </SidebarMenu>
    </SidebarContent>
  </Sidebar>
  <SidebarInset>
    <header className="flex items-center p-4">
      <SidebarTrigger />
      <h1>Page Title</h1>
    </header>
    <main className="p-4">Content goes here</main>
  </SidebarInset>
</SidebarProvider>
```

---

## Skeleton

**Lokasi:** `components/ui/skeleton.tsx`

### Deskripsi
Komponen placeholder untuk loading state dengan animasi pulse.

### Contoh Penggunaan
```tsx
<Skeleton className="h-4 w-24" />
<Skeleton className="h-10 w-full" />
```

---

## Sonner (Toaster)

**Lokasi:** `components/ui/sonner.tsx`

### Deskripsi
Komponen toast notification menggunakan library sonner dengan ikon custom.

### Contoh Penggunaan
```tsx
// Dalam root layout
<Toaster position="top-right" />

// Trigger toast
import { toast } from "sonner";

toast.success("Operation successful!");
toast.error("Something went wrong");
toast.loading("Loading...");
```

---

## Table

**Lokasi:** `components/ui/table.tsx`

### Deskripsi
Komponen table dengan styling konsisten untuk menampilkan data tabular.

### Komponen yang Tersedia
- `Table`: Container utama
- `TableHeader`: Header table
- `TableBody`: Body table
- `TableFooter`: Footer table
- `TableRow`: Baris
- `TableHead`: Header cell
- `TableCell`: Data cell
- `TableCaption`: Caption table

### Contoh Penggunaan
```tsx
<Table>
  <TableHeader>
    <TableRow>
      <TableHead>Name</TableHead>
      <TableHead>Email</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {users.map((user) => (
      <TableRow key={user.id}>
        <TableCell>{user.name}</TableCell>
        <TableCell>{user.email}</TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

---

## Textarea

**Lokasi:** `components/ui/textarea.tsx`

### Deskripsi
Komponen textarea dengan styling konsisten.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default"` | `"default"` | Varian textarea |
| `size` | `"default" \| "sm" \| "xs"` | `"default"` | Ukuran textarea |

### Contoh Penggunaan
```tsx
<Textarea 
  placeholder="Enter description"
  value={value}
  onChange={(e) => setValue(e.target.value)}
  rows={4}
/>
```

---

## Tooltip

**Lokasi:** `components/ui/tooltip.tsx`

### Deskripsi
Komponen tooltip dengan styling custom, dibangun di atas Radix UI Tooltip.

### Komponen yang Tersedia
- `TooltipProvider`: Provider dengan delay configuration
- `Tooltip`: Container utama
- `TooltipTrigger`: Trigger
- `TooltipContent`: Konten tooltip

### Contoh Penggunaan
```tsx
<Tooltip>
  <TooltipTrigger asChild>
    <Button>Hover me</Button>
  </TooltipTrigger>
  <TooltipContent>
    <p>This is a tooltip</p>
  </TooltipContent>
</Tooltip>
```

---

## Praktik Terbaik

### 1. Aksesibilitas
- Selalu gunakan `asChild` ketika merender Link dengan Button
- Berikan `aria-label` pada ikon button
- Gunakan semantic HTML yang sesuai

### 2. Performance
- Gunakan lazy loading untuk komponen yang tidak terlihat
- Manfaatkan React.memo untuk komponen yang sering re-render
- Hindari inline styles yang tidak perlu

### 3. Konsistensi
- Gunakan varian dan ukuran yang sudah ditentukan
- Ikuti sistem desain yang telah ditetapkan
- Gunakan utility classes Tailwind dengan konsisten

### 4. Customization
- Gunakan prop `className` untuk override styling
- Kombinasikan dengan `cn()` untuk conditional classes
- Jangan mengubah struktur dasar komponen

---

## Menambahkan Komponen Baru

Untuk menambahkan komponen UI baru:

1. Buat file di `src/components/ui/`
2. Gunakan Radix UI primitives jika memungkinkan
3. Implementasikan styling dengan Tailwind CSS
4. Export komponen dari file
5. Update dokumentasi ini

### Template Komponen
```tsx
import * as React from "react";
import * as Primitive from "@radix-ui/react-primitive";
import { cn } from "@/lib/utils";

interface NewComponentProps extends React.ComponentProps<typeof Primitive.Root> {
  variant?: "default" | "secondary";
  size?: "sm" | "md" | "lg";
}

function NewComponent({
  className,
  variant = "default",
  size = "md",
  ...props
}: NewComponentProps) {
  return (
    <Primitive.Root
      data-slot="new-component"
      className={cn(
        "base-styles",
        variant === "default" && "default-styles",
        size === "sm" && "sm-styles",
        className
      )}
      {...props}
    />
  );
}

export { NewComponent };
```