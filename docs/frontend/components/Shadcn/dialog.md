---
title: "Dialog"
sidebar_label: "Dialog"
---

# Dialog

**Lokasi:** `components/ui/dialog.tsx`  
**Dokumentasi Resmi:** [Shadcn Dialog](https://ui.shadcn.com/docs/components/dialog)

## Deskripsi

Komponen modal dialog dengan overlay, dibangun di atas Radix UI Dialog. Dialog digunakan untuk menampilkan konten yang memerlukan interaksi pengguna sebelum dapat melanjutkan, seperti form konfirmasi, formulir input, atau informasi penting.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Dialog` | Container utama dialog |
| `DialogTrigger` | Trigger untuk membuka dialog (button atau elemen lain) |
| `DialogContent` | Konten utama dialog |
| `DialogHeader` | Header section dialog |
| `DialogTitle` | Judul dialog |
| `DialogDescription` | Deskripsi dialog |
| `DialogFooter` | Footer section untuk tombol aksi |
| `DialogClose` | Tombol close |
| `DialogOverlay` | Overlay gelap di belakang dialog |

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `open` | `boolean` | - | Kontrol visibilitas dialog (controlled) |
| `defaultOpen` | `boolean` | `false` | Status awal dialog (uncontrolled) |
| `onOpenChange` | `(open: boolean) => void` | - | Callback ketika dialog dibuka/ditutup |
| `showCloseButton` | `boolean` | `true` | Menampilkan tombol close di pojok kanan atas |
| `className` | `string` | - | Custom CSS classes untuk DialogContent |

## Contoh Penggunaan

### Basic Dialog
```tsx
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog";

<Dialog>
  <DialogTrigger asChild>
    <Button>Open Dialog</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Are you absolutely sure?</DialogTitle>
      <DialogDescription>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </DialogDescription>
    </DialogHeader>
    <div className="flex justify-end gap-2 mt-4">
      <Button variant="outline">Cancel</Button>
      <Button variant="destructive">Delete Account</Button>
    </div>
  </DialogContent>
</Dialog>
```

### Controlled Dialog
```tsx
function ControlledDialog() {
  const [open, setOpen] = useState(false);

  return (
    <Dialog open={open} onOpenChange={setOpen}>
      <DialogTrigger asChild>
        <Button>Open Controlled Dialog</Button>
      </DialogTrigger>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>Edit Profile</DialogTitle>
          <DialogDescription>
            Make changes to your profile here. Click save when you're done.
          </DialogDescription>
        </DialogHeader>
        <div className="space-y-4 py-4">
          <div className="space-y-1.5">
            <Label htmlFor="name">Name</Label>
            <Input id="name" defaultValue="John Doe" />
          </div>
          <div className="space-y-1.5">
            <Label htmlFor="email">Email</Label>
            <Input id="email" defaultValue="john@example.com" />
          </div>
        </div>
        <DialogFooter>
          <Button variant="outline" onClick={() => setOpen(false)}>Cancel</Button>
          <Button onClick={() => setOpen(false)}>Save Changes</Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}
```

### Dialog dengan Form
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const formSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email address"),
});

function FormDialog() {
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
    <Dialog open={open} onOpenChange={setOpen}>
      <DialogTrigger asChild>
        <Button>Add User</Button>
      </DialogTrigger>
      <DialogContent className="sm:max-w-[425px]">
        <DialogHeader>
          <DialogTitle>Add New User</DialogTitle>
          <DialogDescription>
            Fill in the user details below. Click save when you're done.
          </DialogDescription>
        </DialogHeader>
        <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
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
          <DialogFooter>
            <Button variant="outline" type="button" onClick={() => setOpen(false)}>
              Cancel
            </Button>
            <Button type="submit">Save User</Button>
          </DialogFooter>
        </form>
      </DialogContent>
    </Dialog>
  );
}
```

### Konfirmasi Dialog
```tsx
function ConfirmDialog() {
  const [open, setOpen] = useState(false);
  const [isDeleting, setIsDeleting] = useState(false);

  const handleDelete = async () => {
    setIsDeleting(true);
    await new Promise(resolve => setTimeout(resolve, 1500)); // Simulasi API call
    setIsDeleting(false);
    setOpen(false);
    // toast.success("Item deleted successfully");
  };

  return (
    <Dialog open={open} onOpenChange={setOpen}>
      <DialogTrigger asChild>
        <Button variant="destructive">Delete Item</Button>
      </DialogTrigger>
      <DialogContent className="sm:max-w-[400px]">
        <DialogHeader>
          <DialogTitle>Confirm Deletion</DialogTitle>
          <DialogDescription>
            Are you sure you want to delete this item? This action cannot be undone.
          </DialogDescription>
        </DialogHeader>
        <div className="flex justify-end gap-2 mt-4">
          <Button variant="outline" onClick={() => setOpen(false)} disabled={isDeleting}>
            Cancel
          </Button>
          <Button variant="destructive" onClick={handleDelete} disabled={isDeleting}>
            {isDeleting ? (
              <>
                <Loader2Icon className="mr-2 h-4 w-4 animate-spin" />
                Deleting...
              </>
            ) : (
              "Yes, Delete"
            )}
          </Button>
        </div>
      </DialogContent>
    </Dialog>
  );
}
```

### Dialog dengan Scroll Area (Untuk Konten Panjang)
```tsx
import { ScrollArea } from "@/components/ui/scroll-area";

<Dialog>
  <DialogTrigger asChild>
    <Button>View Terms</Button>
  </DialogTrigger>
  <DialogContent className="max-h-[80vh]">
    <DialogHeader>
      <DialogTitle>Terms and Conditions</DialogTitle>
      <DialogDescription>Please read the terms carefully</DialogDescription>
    </DialogHeader>
    <ScrollArea className="h-[400px] pr-4">
      <div className="space-y-4">
        <h4 className="font-semibold">1. Acceptance of Terms</h4>
        <p className="text-sm text-muted-foreground">
          By accessing and using this service, you accept and agree to be bound by the terms
          and provision of this agreement...
        </p>
        {/* More content */}
      </div>
    </ScrollArea>
    <DialogFooter>
      <Button variant="outline">Decline</Button>
      <Button>Accept</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

### Dialog dengan Multiple Steps (Wizard)
```tsx
function WizardDialog() {
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
    <Dialog open={open} onOpenChange={setOpen}>
      <DialogTrigger asChild>
        <Button>Start Wizard</Button>
      </DialogTrigger>
      <DialogContent className="sm:max-w-[500px]">
        <DialogHeader>
          <DialogTitle>{steps[step - 1].title}</DialogTitle>
          <DialogDescription>{steps[step - 1].description}</DialogDescription>
        </DialogHeader>
        <div className="py-4">
          {steps[step - 1].content}
        </div>
        <DialogFooter>
          <div className="flex justify-between w-full">
            <Button
              variant="outline"
              onClick={handlePrev}
              disabled={step === 1}
            >
              Previous
            </Button>
            {step === steps.length ? (
              <Button onClick={handleFinish}>Finish</Button>
            ) : (
              <Button onClick={handleNext}>Next</Button>
            )}
          </div>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}
```

### Tanpa Tombol Close
```tsx
<Dialog>
  <DialogContent showCloseButton={false}>
    <DialogHeader>
      <DialogTitle>Important Message</DialogTitle>
      <DialogDescription>This dialog cannot be closed by clicking the X button</DialogDescription>
    </DialogHeader>
    <div className="py-4">
      <p>You must click the button below to close this dialog.</p>
    </div>
    <DialogFooter>
      <Button>I Understand</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

## Kustomisasi

### Custom Width
```tsx
<DialogContent className="sm:max-w-3xl">
  {/* Wide dialog */}
</DialogContent>

<DialogContent className="sm:max-w-xs">
  {/* Small dialog */}
</DialogContent>
```

### Custom Positioning
```tsx
<DialogContent className="top-[20%] translate-y-0">
  {/* Dialog positioned lower */}
</DialogContent>
```

### Custom Animation
Dialog sudah memiliki animasi bawaan dari Radix UI. Untuk kustomisasi, override className:

```tsx
<DialogContent className="data-[state=open]:animate-none data-[state=closed]:animate-none">
  {/* No animation */}
</DialogContent>
```

## Best Practices

### 1. Gunakan untuk Interaksi yang Penting
Dialog harus digunakan untuk interaksi yang memerlukan perhatian pengguna, bukan untuk konten biasa:
```tsx
// ✅ Baik: Konfirmasi aksi penting
<Dialog>Confirm delete?</Dialog>

// ❌ Hindari: Informasi umum
<Dialog>View profile</Dialog> // Gunakan Sheet atau Popover untuk ini
```

### 2. Fokus pada Form
Saat membuka dialog dengan form, fokus otomatis pada input pertama:
```tsx
// Input pertama akan otomatis mendapatkan fokus
<DialogContent>
  <Input autoFocus />
</DialogContent>
```

### 3. Keyboard Accessibility
- `Esc` menutup dialog
- `Tab` navigasi antar elemen dalam dialog
- `Enter` untuk submit jika form

### 4. Handle Close dengan Benar
```tsx
// ✅ Baik: Reset form saat close
const handleOpenChange = (open: boolean) => {
  if (!open) {
    form.reset();
  }
  setOpen(open);
};

// ❌ Hindari: State tetap tersimpan
```

## Troubleshooting

### Dialog Tidak Tertutup
- Pastikan `onOpenChange` di-handle dengan benar
- Periksa apakah ada kondisi yang mencegah close

### Konten Terpotong
- Gunakan `max-h-[80vh]` pada DialogContent untuk konten panjang
- Kombinasikan dengan ScrollArea

### Overlay Tidak Muncul
- Pastikan DialogContent di-render dalam Portal (sudah default)
- Periksa z-index tidak tertimpa CSS lain

## Referensi

- [Shadcn Dialog Documentation](https://ui.shadcn.com/docs/components/dialog)
- [Radix UI Dialog](https://www.radix-ui.com/primitives/docs/components/dialog)
- [MDN Dialog Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)
