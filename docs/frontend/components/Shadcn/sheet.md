---
title: "Sheet"
sidebar_label: "Sheet"
---

# Sheet

**Lokasi:** `components/ui/sheet.tsx`  
**Dokumentasi Resmi:** [Shadcn Sheet](https://ui.shadcn.com/docs/components/sheet)

## Deskripsi

Komponen drawer/sheet yang muncul dari sisi layar, dibangun di atas Radix UI Dialog. Sheet digunakan untuk menampilkan konten tambahan seperti form, menu, atau detail informasi tanpa meninggalkan halaman utama. Berbeda dengan Dialog, Sheet muncul dari tepi layar dan lebih cocok untuk konten yang lebih panjang.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Sheet` | Container utama sheet |
| `SheetTrigger` | Trigger untuk membuka sheet (button atau elemen lain) |
| `SheetContent` | Konten utama sheet yang muncul dari sisi layar |
| `SheetHeader` | Header section sheet |
| `SheetTitle` | Judul sheet |
| `SheetDescription` | Deskripsi sheet |
| `SheetFooter` | Footer section untuk tombol aksi |
| `SheetClose` | Tombol close |
| `SheetOverlay` | Overlay gelap di belakang sheet |

## Props

### SheetContent
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `side` | `"top" \| "right" \| "bottom" \| "left"` | `"right"` | Posisi sheet muncul dari sisi mana |
| `showCloseButton` | `boolean` | `true` | Menampilkan tombol close di pojok kanan atas |
| `className` | `string` | - | Custom CSS classes |

## Contoh Penggunaan

### Basic Sheet
```tsx
import {
  Sheet,
  SheetContent,
  SheetDescription,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from "@/components/ui/sheet";

<Sheet>
  <SheetTrigger asChild>
    <Button variant="outline">Open Sheet</Button>
  </SheetTrigger>
  <SheetContent>
    <SheetHeader>
      <SheetTitle>Edit Profile</SheetTitle>
      <SheetDescription>
        Make changes to your profile here. Click save when you're done.
      </SheetDescription>
    </SheetHeader>
    <div className="py-4">
      <p>Sheet content goes here</p>
    </div>
  </SheetContent>
</Sheet>
```

### Sheet dengan Berbagai Posisi
```tsx
<div className="flex gap-4">
  {/* Left Side */}
  <Sheet>
    <SheetTrigger asChild>
      <Button variant="outline">Left</Button>
    </SheetTrigger>
    <SheetContent side="left">
      <SheetHeader>
        <SheetTitle>Left Sheet</SheetTitle>
        <SheetDescription>Sheet appears from left side</SheetDescription>
      </SheetHeader>
      <div className="py-4">Content</div>
    </SheetContent>
  </Sheet>

  {/* Right Side (Default) */}
  <Sheet>
    <SheetTrigger asChild>
      <Button variant="outline">Right</Button>
    </SheetTrigger>
    <SheetContent side="right">
      <SheetHeader>
        <SheetTitle>Right Sheet</SheetTitle>
        <SheetDescription>Sheet appears from right side</SheetDescription>
      </SheetHeader>
      <div className="py-4">Content</div>
    </SheetContent>
  </Sheet>

  {/* Top Side */}
  <Sheet>
    <SheetTrigger asChild>
      <Button variant="outline">Top</Button>
    </SheetTrigger>
    <SheetContent side="top">
      <SheetHeader>
        <SheetTitle>Top Sheet</SheetTitle>
        <SheetDescription>Sheet appears from top side</SheetDescription>
      </SheetHeader>
      <div className="py-4">Content</div>
    </SheetContent>
  </Sheet>

  {/* Bottom Side */}
  <Sheet>
    <SheetTrigger asChild>
      <Button variant="outline">Bottom</Button>
    </SheetTrigger>
    <SheetContent side="bottom">
      <SheetHeader>
        <SheetTitle>Bottom Sheet</SheetTitle>
        <SheetDescription>Sheet appears from bottom side</SheetDescription>
      </SheetHeader>
      <div className="py-4">Content</div>
    </SheetContent>
  </Sheet>
</div>
```

### Controlled Sheet
```tsx
function ControlledSheet() {
  const [open, setOpen] = useState(false);

  return (
    <Sheet open={open} onOpenChange={setOpen}>
      <SheetTrigger asChild>
        <Button>Open Controlled Sheet</Button>
      </SheetTrigger>
      <SheetContent>
        <SheetHeader>
          <SheetTitle>Settings</SheetTitle>
          <SheetDescription>Configure your application settings</SheetDescription>
        </SheetHeader>
        <div className="py-4 space-y-4">
          <div className="flex items-center justify-between">
            <Label htmlFor="notifications">Notifications</Label>
            <Switch id="notifications" />
          </div>
          <div className="flex items-center justify-between">
            <Label htmlFor="dark-mode">Dark Mode</Label>
            <Switch id="dark-mode" />
          </div>
        </div>
        <SheetFooter>
          <Button variant="outline" onClick={() => setOpen(false)}>Cancel</Button>
          <Button onClick={() => setOpen(false)}>Save Changes</Button>
        </SheetFooter>
      </SheetContent>
    </Sheet>
  );
}
```

### Sheet dengan Form
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const formSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email address"),
});

function FormSheet() {
  const [open, setOpen] = useState(false);
  const form = useForm({
    resolver: zodResolver(formSchema),
    defaultValues: { name: "", email: "" },
  });

  const onSubmit = (data) => {
    console.log(data);
    setOpen(false);
    form.reset();
  };

  return (
    <Sheet open={open} onOpenChange={setOpen}>
      <SheetTrigger asChild>
        <Button>Add User</Button>
      </SheetTrigger>
      <SheetContent className="sm:max-w-md">
        <SheetHeader>
          <SheetTitle>Add New User</SheetTitle>
          <SheetDescription>
            Fill in the user details below. Click save when you're done.
          </SheetDescription>
        </SheetHeader>
        <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4 py-4">
          <div className="space-y-1.5">
            <Label htmlFor="name">Name</Label>
            <Input
              id="name"
              placeholder="John Doe"
              {...form.register("name")}
            />
            {form.formState.errors.name && (
              <p className="text-xs text-red-500">{form.formState.errors.name.message}</p>
            )}
          </div>
          <div className="space-y-1.5">
            <Label htmlFor="email">Email</Label>
            <Input
              id="email"
              type="email"
              placeholder="john@example.com"
              {...form.register("email")}
            />
            {form.formState.errors.email && (
              <p className="text-xs text-red-500">{form.formState.errors.email.message}</p>
            )}
          </div>
          <SheetFooter>
            <Button variant="outline" type="button" onClick={() => setOpen(false)}>
              Cancel
            </Button>
            <Button type="submit">Save User</Button>
          </SheetFooter>
        </form>
      </SheetContent>
    </Sheet>
  );
}
```

### Sheet dengan Scroll Area (Untuk Konten Panjang)
```tsx
import { ScrollArea } from "@/components/ui/scroll-area";

<Sheet>
  <SheetTrigger asChild>
    <Button>View Details</Button>
  </SheetTrigger>
  <SheetContent className="w-full sm:max-w-lg">
    <SheetHeader>
      <SheetTitle>Order Details</SheetTitle>
      <SheetDescription>Complete information about this order</SheetDescription>
    </SheetHeader>
    <ScrollArea className="h-[calc(100vh-120px)] mt-4">
      <div className="space-y-6 pr-4">
        <div>
          <h4 className="font-semibold mb-2">Customer Information</h4>
          <p className="text-sm">John Doe</p>
          <p className="text-sm">john@example.com</p>
          <p className="text-sm">+62 812 3456 7890</p>
        </div>
        <div>
          <h4 className="font-semibold mb-2">Order Items</h4>
          {Array.from({ length: 20 }).map((_, i) => (
            <div key={i} className="flex justify-between py-2 border-b">
              <span>Product {i + 1}</span>
              <span>$49.99</span>
            </div>
          ))}
        </div>
        <div>
          <h4 className="font-semibold mb-2">Shipping Address</h4>
          <p className="text-sm">123 Main Street</p>
          <p className="text-sm">Jakarta, Indonesia 12345</p>
        </div>
      </div>
    </ScrollArea>
    <SheetFooter className="mt-4">
      <Button className="w-full">Print Order</Button>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

### Sheet dengan Menu Navigasi
```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button variant="ghost" size="icon">
      <MenuIcon className="h-5 w-5" />
    </Button>
  </SheetTrigger>
  <SheetContent side="left" className="w-[300px] sm:w-[350px]">
    <SheetHeader>
      <SheetTitle>Menu</SheetTitle>
    </SheetHeader>
    <nav className="flex flex-col gap-2 mt-6">
      <Button variant="ghost" className="w-full justify-start">
        <HomeIcon className="mr-2 h-4 w-4" />
        Dashboard
      </Button>
      <Button variant="ghost" className="w-full justify-start">
        <UsersIcon className="mr-2 h-4 w-4" />
        Users
      </Button>
      <Button variant="ghost" className="w-full justify-start">
        <SettingsIcon className="mr-2 h-4 w-4" />
        Settings
      </Button>
      <Button variant="ghost" className="w-full justify-start">
        <HelpCircleIcon className="mr-2 h-4 w-4" />
        Help
      </Button>
      <div className="h-px bg-border my-2" />
      <Button variant="ghost" className="w-full justify-start text-red-600">
        <LogOutIcon className="mr-2 h-4 w-4" />
        Logout
      </Button>
    </nav>
  </SheetContent>
</Sheet>
```

### Sheet dengan Custom Width
```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button>Custom Width</Button>
  </SheetTrigger>
  <SheetContent className="w-[500px] sm:w-[600px]">
    <SheetHeader>
      <SheetTitle>Custom Size Sheet</SheetTitle>
      <SheetDescription>This sheet has custom width</SheetDescription>
    </SheetHeader>
    <div className="py-4">
      <p>Content with custom width</p>
    </div>
  </SheetContent>
</Sheet>
```

### Sheet Tanpa Tombol Close
```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button>No Close Button</Button>
  </SheetTrigger>
  <SheetContent showCloseButton={false}>
    <SheetHeader>
      <SheetTitle>No Close Button</SheetTitle>
      <SheetDescription>This sheet doesn't have a close button</SheetDescription>
    </SheetHeader>
    <div className="py-4">
      <p>You must click outside to close</p>
    </div>
    <SheetFooter>
      <Button className="w-full">Action Button</Button>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

### Sheet dengan Multiple Steps (Wizard)
```tsx
function WizardSheet() {
  const [step, setStep] = useState(1);
  const [open, setOpen] = useState(false);

  const steps = [
    {
      title: "Step 1: Basic Information",
      description: "Enter your basic details",
      content: (
        <div className="space-y-4">
          <Input placeholder="Name" />
          <Input placeholder="Email" type="email" />
        </div>
      ),
    },
    {
      title: "Step 2: Additional Details",
      description: "Provide more information",
      content: (
        <div className="space-y-4">
          <Input placeholder="Company" />
          <Input placeholder="Position" />
        </div>
      ),
    },
    {
      title: "Step 3: Review",
      description: "Confirm your information",
      content: (
        <div className="space-y-2">
          <p><strong>Name:</strong> John Doe</p>
          <p><strong>Email:</strong> john@example.com</p>
          <p><strong>Company:</strong> Acme Inc</p>
        </div>
      ),
    },
  ];

  const handleNext = () => setStep(prev => Math.min(prev + 1, steps.length));
  const handlePrev = () => setStep(prev => Math.max(prev - 1, 1));
  const handleFinish = () => setOpen(false);

  return (
    <Sheet open={open} onOpenChange={setOpen}>
      <SheetTrigger asChild>
        <Button>Wizard Sheet</Button>
      </SheetTrigger>
      <SheetContent className="sm:max-w-md">
        <SheetHeader>
          <SheetTitle>{steps[step - 1].title}</SheetTitle>
          <SheetDescription>{steps[step - 1].description}</SheetDescription>
        </SheetHeader>
        <div className="py-4">
          {steps[step - 1].content}
        </div>
        <SheetFooter>
          <div className="flex justify-between w-full">
            <Button variant="outline" onClick={handlePrev} disabled={step === 1}>
              Previous
            </Button>
            {step === steps.length ? (
              <Button onClick={handleFinish}>Finish</Button>
            ) : (
              <Button onClick={handleNext}>Next</Button>
            )}
          </div>
        </SheetFooter>
      </SheetContent>
    </Sheet>
  );
}
```

## Kustomisasi

### Custom Styling
```tsx
<SheetContent className="bg-primary text-primary-foreground">
  <SheetHeader>
    <SheetTitle className="text-primary-foreground">Custom Sheet</SheetTitle>
  </SheetHeader>
</SheetContent>
```

### Custom Overlay
```tsx
// Overlay styling dapat dimodifikasi melalui CSS global
<Sheet>
  <SheetContent />
</Sheet>
```

## Best Practices

### 1. Pilih Sisi yang Tepat
- **Right**: Untuk form, detail, atau pengaturan (paling umum)
- **Left**: Untuk menu navigasi
- **Bottom**: Untuk action sheet pada mobile
- **Top**: Untuk notifikasi atau alert

### 2. Gunakan untuk Konten yang Tidak Esensial
Sheet cocok untuk konten tambahan yang tidak menghalangi akses ke konten utama.

### 3. Responsive Design
Sheet sudah responsive secara default, dengan lebar yang menyesuaikan di mobile.

### 4. Keyboard Accessibility
- `Esc` menutup sheet
- `Tab` navigasi antar elemen dalam sheet

## Troubleshooting

### Sheet Tidak Tertutup
- Pastikan `onOpenChange` di-handle dengan benar
- Periksa apakah ada kondisi yang mencegah close

### Sheet Terpotong
- Periksa apakah parent memiliki overflow: hidden
- Gunakan `ScrollArea` untuk konten panjang

## Referensi

- [Shadcn Sheet Documentation](https://ui.shadcn.com/docs/components/sheet)
- [Radix UI Dialog](https://www.radix-ui.com/primitives/docs/components/dialog)
- [Material Design Bottom Sheets](https://material.io/components/sheets-bottom)
