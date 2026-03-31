---
title: "FormHeaderCard"
sidebar_label: "FormHeaderCard"
---

# FormHeaderCard

**Lokasi:** `components/custom/form-header-card.tsx`

## Deskripsi

Header card untuk halaman form dengan tombol navigasi dan aksi. FormHeaderCard menyediakan struktur header yang konsisten untuk halaman create, edit, dan view, dengan tombol back, submit, cancel, dan add new yang dapat dikustomisasi.

## Fitur

- Tombol back untuk navigasi (dengan dukungan `backTo` atau custom handler)
- Mode form (create, edit, view) dengan teks submit yang dinamis
- Loading state untuk operasi async
- Tombol add new untuk mode create
- Tombol submit dan cancel dengan teks yang dapat dikustomisasi
- Dukungan untuk menyembunyikan tombol tertentu
- Styling konsisten dengan sistem desain

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `title` | `string` | Ya | - | Judul form |
| `subtitle` | `string` | Tidak | - | Subjudul form |
| `mode` | `"create" \| "edit" \| "view"` | Ya | - | Mode form (mempengaruhi teks submit) |
| `isLoading` | `boolean` | Tidak | `false` | State loading |
| `onAddNew` | `() => void` | Tidak | - | Handler tombol add new |
| `onSubmit` | `() => void` | Tidak | - | Handler tombol submit |
| `onCancel` | `() => void` | Tidak | - | Handler tombol cancel |
| `onBack` | `() => void` | Tidak | - | Handler tombol back (override default) |
| `backTo` | `string` | Tidak | - | URL untuk navigasi back |
| `addButtonText` | `string` | Tidak | `"Add New"` | Teks tombol add new |
| `submitButtonText` | `string` | Tidak | - | Teks tombol submit (default: "Save Data" untuk create, "Update Data" untuk edit) |
| `cancelButtonText` | `string` | Tidak | `"Cancel"` | Teks tombol cancel |
| `hideAddButton` | `boolean` | Tidak | `false` | Sembunyikan tombol add new |
| `hideSubmitButton` | `boolean` | Tidak | `false` | Sembunyikan tombol submit |
| `hideCancelButton` | `boolean` | Tidak | `true` | Sembunyikan tombol cancel |
| `className` | `string` | Tidak | - | Custom CSS classes |

## Contoh Penggunaan

### Basic Usage (Create Mode)
```tsx
import { FormHeaderCard } from "@/components/custom/form-header-card";

<FormHeaderCard
  title="Create New User"
  subtitle="Fill in the user details below"
  mode="create"
  onSubmit={handleSubmit}
  onCancel={handleCancel}
/>
```

### Edit Mode
```tsx
<FormHeaderCard
  title="Edit User"
  subtitle="Update user information"
  mode="edit"
  onSubmit={handleUpdate}
  onCancel={handleCancel}
/>
```

### View Mode (Read Only)
```tsx
<FormHeaderCard
  title="User Details"
  subtitle="View user information"
  mode="view"
  hideSubmitButton
/>
```

### Dengan Back Navigation
```tsx
<FormHeaderCard
  title="Create Product"
  mode="create"
  backTo="/products"
  onSubmit={handleSubmit}
/>
```

### Dengan Custom Back Handler
```tsx
<FormHeaderCard
  title="Edit Profile"
  mode="edit"
  onBack={() => {
    if (hasChanges) {
      confirmDiscard();
    } else {
      navigate(-1);
    }
  }}
  onSubmit={handleUpdate}
/>
```

### Dengan Add New Button
```tsx
<FormHeaderCard
  title="Products"
  mode="create"
  onAddNew={handleAddNew}
  onSubmit={handleSave}
  addButtonText="Add Product"
/>
```

### Dengan Custom Submit Text
```tsx
<FormHeaderCard
  title="Publish Article"
  mode="create"
  onSubmit={handlePublish}
  submitButtonText="Publish Now"
/>
```

### Dengan Loading State
```tsx
<FormHeaderCard
  title="Save Changes"
  mode="edit"
  isLoading={isSubmitting}
  onSubmit={handleSave}
/>
```

### Tanpa Cancel Button
```tsx
<FormHeaderCard
  title="Quick Add"
  mode="create"
  onSubmit={handleSave}
  hideCancelButton
/>
```

### Dengan Semua Tombol (Custom)
```tsx
<FormHeaderCard
  title="User Management"
  mode="create"
  onAddNew={handleAddNew}
  onSubmit={handleSave}
  onCancel={handleCancel}
  hideCancelButton={false}
  addButtonText="New User"
  submitButtonText="Save User"
  cancelButtonText="Discard"
/>
```

### Dalam Layout Halaman
```tsx
function CreateUserPage() {
  const navigate = useNavigate();
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = async () => {
    setIsSubmitting(true);
    try {
      await createUser(formData);
      toast.success("User created successfully");
      navigate("/users");
    } catch (error) {
      toast.error("Failed to create user");
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <div className="space-y-6">
      <FormHeaderCard
        title="Create New User"
        subtitle="Fill in the user details below"
        mode="create"
        isLoading={isSubmitting}
        onSubmit={handleSubmit}
        onCancel={() => navigate("/users")}
        backTo="/users"
      />
      
      <UserForm
        formData={formData}
        onChange={setFormData}
        isDisabled={isSubmitting}
      />
    </div>
  );
}
```

### Dengan Konfirmasi Sebelum Cancel
```tsx
function EditUserPage() {
  const navigate = useNavigate();
  const [hasChanges, setHasChanges] = useState(false);
  const [showConfirm, setShowConfirm] = useState(false);

  const handleCancel = () => {
    if (hasChanges) {
      setShowConfirm(true);
    } else {
      navigate("/users");
    }
  };

  return (
    <>
      <FormHeaderCard
        title="Edit User"
        mode="edit"
        onCancel={handleCancel}
        onSubmit={handleUpdate}
      />

      <AlertDialog open={showConfirm} onOpenChange={setShowConfirm}>
        <AlertDialogContent>
          <AlertDialogHeader>
            <AlertDialogTitle>Unsaved Changes</AlertDialogTitle>
            <AlertDialogDescription>
              You have unsaved changes. Are you sure you want to leave?
            </AlertDialogDescription>
          </AlertDialogHeader>
          <AlertDialogFooter>
            <AlertDialogCancel>Continue Editing</AlertDialogCancel>
            <AlertDialogAction onClick={() => navigate("/users")}>
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

### 1. Gunakan Mode yang Tepat
Set `mode` sesuai dengan konteks form:
```tsx
// ✅ Baik
mode="create"  // Form tambah data
mode="edit"    // Form edit data
mode="view"    // Hanya melihat data (read-only)

// ❌ Hindari: Mode tidak sesuai
mode="create"  // Untuk halaman edit
```

### 2. Handle Loading State
Gunakan `isLoading` untuk menonaktifkan tombol saat submit:
```tsx
<FormHeaderCard
  isLoading={isSubmitting}
  onSubmit={handleSubmit}
/>
```

### 3. Back Navigation
Gunakan `backTo` untuk navigasi sederhana, atau `onBack` untuk logika custom:
```tsx
// Untuk navigasi sederhana
backTo="/users"

// Untuk logika custom (konfirmasi, dll)
onBack={handleCustomBack}
```

### 4. Cancel vs Back
- `onCancel`: Biasanya untuk membatalkan perubahan dan kembali
- `onBack`: Navigasi back (bisa override dengan `onBack`)

## Troubleshooting

### Tombol Tidak Muncul
- Periksa `hideSubmitButton`, `hideCancelButton`, `hideAddButton` props
- Pastikan mode yang sesuai dengan kebutuhan

### Submit Text Tidak Sesuai
- Untuk `mode="create"` default: "Save Data"
- Untuk `mode="edit"` default: "Update Data"
- Gunakan `submitButtonText` untuk override

## Referensi

- [Shadcn Button Documentation](https://ui.shadcn.com/docs/components/button)
- [Lucide Icons](https://lucide.dev/icons/arrow-left)
- [React Router Navigation](https://reactrouter.com/en/main/hooks/use-navigate)
