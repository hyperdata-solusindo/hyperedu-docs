---
title: "FormModal"
sidebar_label: "FormModal"
---

# FormModal

**Lokasi:** `components/custom/form-modal.tsx`

## Deskripsi

Modal dialog untuk form dengan header, body, dan footer yang terstruktur. FormModal menyediakan container modal yang optimal untuk menampilkan form, dengan dukungan berbagai ukuran, sticky footer, dan integrasi yang baik dengan React Hook Form.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `FormModal` | Container utama modal dengan header, content, dan footer |
| `FormModalFooter` | Footer dengan tombol cancel dan submit |

---

## FormModal

Container utama modal dengan header, content, dan footer.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `isOpen` | `boolean` | Ya | - | Status modal terbuka |
| `onClose` | `() => void` | Tidak | - | Handler tutup modal |
| `title` | `string` | Ya | - | Judul modal |
| `subtitle` | `string` | Tidak | - | Subjudul modal |
| `children` | `React.ReactNode` | Ya | - | Konten modal |
| `footer` | `React.ReactNode` | Tidak | - | Footer modal (override default) |
| `size` | `"xs" \| "sm" \| "md" \| "lg" \| "xl" \| "2xl" \| "4xl" \| "full"` | Tidak | `"sm"` | Ukuran modal |
| `variant` | `"default" \| "sticky-footer"` | Tidak | `"default"` | Varian modal |
| `className` | `string` | Tidak | - | Custom CSS classes |

### Ukuran Modal

| Ukuran | Max Width | Penggunaan |
|--------|-----------|------------|
| `xs` | `sm:max-w-xs` | Modal sangat kecil (konfirmasi) |
| `sm` | `sm:max-w-sm` | Modal kecil (form singkat) |
| `md` | `sm:max-w-md` | Modal sedang (form standar) |
| `lg` | `sm:max-w-lg` | Modal besar |
| `xl` | `sm:max-w-xl` | Modal extra large |
| `2xl` | `sm:max-w-2xl` | Modal 2x large |
| `4xl` | `sm:max-w-4xl` | Modal sangat besar |
| `full` | `sm:max-w-full` | Modal full width |

### Varian

| Varian | Deskripsi |
|--------|-----------|
| `default` | Konten normal dengan scroll jika diperlukan |
| `sticky-footer` | Footer tetap di bawah, konten scrollable |

---

## FormModalFooter

Footer untuk modal dengan tombol cancel dan submit.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `onCancel` | `() => void` | Tidak | - | Handler tombol cancel |
| `onSubmit` | `() => void` | Tidak | - | Handler tombol submit |
| `cancelText` | `string` | Tidak | `"CANCEL"` | Teks tombol cancel |
| `submitText` | `string` | Tidak | `"SAVE"` | Teks tombol submit |
| `isLoading` | `boolean` | Tidak | `false` | State loading |
| `hideCancel` | `boolean` | Tidak | `false` | Sembunyikan tombol cancel |
| `formId` | `string` | Tidak | - | ID form untuk submit (mengikat tombol submit dengan form) |

---

## Contoh Penggunaan

### Basic Modal
```tsx
import { FormModal } from "@/components/custom/form-modal";

function BasicModal() {
  const [open, setOpen] = useState(false);

  return (
    <>
      <Button onClick={() => setOpen(true)}>Open Modal</Button>
      <FormModal
        isOpen={open}
        onClose={() => setOpen(false)}
        title="Add New Item"
        subtitle="Fill in the details below"
      >
        <div className="space-y-4">
          <Input placeholder="Name" />
          <Input placeholder="Description" />
        </div>
      </FormModal>
    </>
  );
}
```

### Dengan Form dan Footer
```tsx
import { FormModal, FormModalFooter } from "@/components/custom/form-modal";

function FormModalExample() {
  const [open, setOpen] = useState(false);
  const [isLoading, setIsLoading] = useState(false);

  const handleSubmit = async () => {
    setIsLoading(true);
    await saveData();
    setIsLoading(false);
    setOpen(false);
  };

  return (
    <FormModal
      isOpen={open}
      onClose={() => setOpen(false)}
      title="Create User"
      subtitle="Enter user information"
      footer={
        <FormModalFooter
          onCancel={() => setOpen(false)}
          onSubmit={handleSubmit}
          isLoading={isLoading}
        />
      }
    >
      <div className="space-y-4">
        <Input placeholder="Name" />
        <Input placeholder="Email" type="email" />
        <Input placeholder="Role" />
      </div>
    </FormModal>
  );
}
```

### Dengan React Hook Form
```tsx
import { useForm } from "react-hook-form";
import { FormModal, FormModalFooter } from "@/components/custom/form-modal";

function FormModalWithRHF() {
  const [open, setOpen] = useState(false);
  const { register, handleSubmit, reset, formState: { isSubmitting } } = useForm();

  const onSubmit = async (data) => {
    await api.post("/users", data);
    reset();
    setOpen(false);
  };

  return (
    <FormModal
      isOpen={open}
      onClose={() => setOpen(false)}
      title="Add User"
      subtitle="Fill in user details"
      footer={
        <FormModalFooter
          onCancel={() => setOpen(false)}
          onSubmit={handleSubmit(onSubmit)}
          isLoading={isSubmitting}
          formId="user-form"
        />
      }
    >
      <form id="user-form" onSubmit={handleSubmit(onSubmit)} className="space-y-4">
        <Input placeholder="Name" {...register("name")} />
        <Input placeholder="Email" type="email" {...register("email")} />
        <Select {...register("role")}>
          <option value="admin">Admin</option>
          <option value="user">User</option>
        </Select>
      </form>
    </FormModal>
  );
}
```

### Dengan Ukuran Berbeda
```tsx
// Modal kecil untuk konfirmasi
<FormModal size="xs" title="Confirm Delete">
  <p>Are you sure?</p>
</FormModal>

// Modal besar untuk form kompleks
<FormModal size="2xl" title="Advanced Settings">
  {/* complex form content */}
</FormModal>

// Modal full width
<FormModal size="full" title="Full Screen Editor">
  {/* full width content */}
</FormModal>
```

### Dengan Sticky Footer
```tsx
<FormModal
  variant="sticky-footer"
  title="Long Form"
  subtitle="This form has many fields"
  footer={<FormModalFooter onSubmit={handleSubmit} />}
>
  <div className="space-y-4">
    {Array.from({ length: 20 }).map((_, i) => (
      <Input key={i} placeholder={`Field ${i + 1}`} />
    ))}
  </div>
</FormModal>
```

### Tanpa Cancel Button
```tsx
<FormModal
  title="Important Update"
  footer={
    <FormModalFooter
      onSubmit={handleSubmit}
      hideCancel
      submitText="Confirm"
    />
  }
>
  <p>This action cannot be undone.</p>
</FormModal>
```

### Dengan Custom Footer
```tsx
<FormModal
  title="Custom Actions"
  footer={
    <div className="flex justify-between gap-2">
      <Button variant="outline">Save as Draft</Button>
      <Button>Publish</Button>
      <Button variant="destructive">Delete</Button>
    </div>
  }
>
  {/* content */}
</FormModal>
```

### Dalam Komponen Terpisah
```tsx
function CreateUserModal() {
  const [open, setOpen] = useState(false);
  const form = useForm();

  const handleSuccess = () => {
    toast.success("User created");
    setOpen(false);
    form.reset();
  };

  return (
    <>
      <Button onClick={() => setOpen(true)}>Add User</Button>
      
      <FormModal
        isOpen={open}
        onClose={() => setOpen(false)}
        title="Create User"
        subtitle="Enter user information"
        footer={
          <FormModalFooter
            onCancel={() => setOpen(false)}
            onSubmit={form.handleSubmit(handleSuccess)}
            isLoading={form.formState.isSubmitting}
          />
        }
      >
        <form onSubmit={form.handleSubmit(handleSuccess)} className="space-y-4">
          <div className="space-y-1.5">
            <Label>Name</Label>
            <Input {...form.register("name", { required: true })} />
          </div>
          <div className="space-y-1.5">
            <Label>Email</Label>
            <Input type="email" {...form.register("email")} />
          </div>
        </form>
      </FormModal>
    </>
  );
}
```

### Dengan Konfirmasi Sebelum Close
```tsx
function ModalWithConfirm() {
  const [open, setOpen] = useState(false);
  const [showConfirm, setShowConfirm] = useState(false);
  const [hasChanges, setHasChanges] = useState(false);
  const form = useForm();

  const handleClose = () => {
    if (hasChanges) {
      setShowConfirm(true);
    } else {
      setOpen(false);
    }
  };

  return (
    <>
      <FormModal
        isOpen={open}
        onClose={handleClose}
        title="Edit Profile"
        footer={
          <FormModalFooter
            onCancel={handleClose}
            onSubmit={form.handleSubmit(handleSubmit)}
          />
        }
      >
        <form onChange={() => setHasChanges(true)}>
          <Input {...form.register("name")} />
        </form>
      </FormModal>

      <AlertDialog open={showConfirm} onOpenChange={setShowConfirm}>
        <AlertDialogContent>
          <AlertDialogHeader>
            <AlertDialogTitle>Unsaved Changes</AlertDialogTitle>
            <AlertDialogDescription>
              You have unsaved changes. Are you sure you want to close?
            </AlertDialogDescription>
          </AlertDialogHeader>
          <AlertDialogFooter>
            <AlertDialogCancel>Continue Editing</AlertDialogCancel>
            <AlertDialogAction onClick={() => setOpen(false)}>
              Discard Changes
            </AlertDialogAction>
          </AlertDialogFooter>
        </AlertDialogContent>
      </AlertDialog>
    </>
  );
}
```

## Best Practices

### 1. Gunakan Ukuran yang Tepat
```tsx
// ✅ Baik: Ukuran sesuai konten
<FormModal size="sm"> {/* konten pendek */} </FormModal>
<FormModal size="lg"> {/* konten panjang */} </FormModal>

// ❌ Hindari: Ukuran terlalu besar untuk konten kecil
<FormModal size="4xl"> {/* konten pendek */} </FormModal>
```

### 2. Sticky Footer untuk Form Panjang
Gunakan `variant="sticky-footer"` untuk form dengan banyak field agar footer selalu terlihat.

### 3. Reset Form on Close
```tsx
const handleClose = () => {
  form.reset();
  setOpen(false);
};
```

### 4. Handle Loading State
Gunakan `isLoading` pada `FormModalFooter` untuk mencegah double submit.

### 5. Form ID untuk Submit
Gunakan `formId` untuk menghubungkan tombol submit dengan form:
```tsx
<form id="my-form">...</form>
<FormModalFooter formId="my-form" onSubmit={handleSubmit} />
```

## Troubleshooting

### Modal Tidak Tertutup
- Pastikan `onClose` diimplementasikan dengan benar
- Periksa apakah ada kondisi yang mencegah close

### Konten Terpotong
- Gunakan `variant="sticky-footer"` untuk form panjang
- Pastikan tidak ada CSS yang membatasi height

## Referensi

- [Shadcn Dialog Documentation](https://ui.shadcn.com/docs/components/dialog)
- [React Hook Form](https://react-hook-form.com/)
- [Radix UI Dialog](https://www.radix-ui.com/primitives/docs/components/dialog)
