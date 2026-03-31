---
title: "ConfirmPopover"
sidebar_label: "ConfirmPopover"
---

# ConfirmPopover

**Lokasi:** `components/custom/confirm-popover.tsx`

## Deskripsi

Komponen popover konfirmasi yang membutuhkan konfirmasi pengguna sebelum melakukan suatu tindakan. ConfirmPopover digunakan untuk aksi-aksi yang memerlukan konfirmasi seperti penghapusan data, pembatalan, atau tindakan yang tidak dapat di-undo.

## Fitur

- Popover konfirmasi yang muncul di dekat trigger
- Judul dan deskripsi yang dapat dikustomisasi
- Handler untuk konfirmasi dan pembatalan
- Loading state untuk aksi asinkron
- Callback onSuccess setelah konfirmasi berhasil
- Mendukung aksi async dengan Promise
- Styling konsisten dengan sistem desain

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `title` | `string` | Tidak | `"Confirmation?"` | Judul popover |
| `description` | `string` | Tidak | `"Are you sure to delete this data?"` | Pesan konfirmasi |
| `onConfirm` | `() => Promise<void> \| void` | Ya | - | Handler aksi konfirmasi |
| `onCancel` | `() => Promise<void> \| void` | Ya | - | Handler aksi batal |
| `onSuccess` | `() => void` | Tidak | `() => {}` | Callback setelah sukses |
| `open` | `boolean` | Tidak | - | Mengontrol visibilitas popover |
| `isLoading` | `boolean` | Tidak | - | State loading |
| `children` | `React.ReactNode` | Tidak | - | Elemen trigger (tombol, ikon, dll) |

## Contoh Penggunaan

### Basic Usage
```tsx
import { ConfirmPopover } from "@/components/custom/confirm-popover";
import { Button } from "@/components/ui/button";

<ConfirmPopover
  onConfirm={() => console.log("Confirmed!")}
  onCancel={() => console.log("Cancelled!")}
>
  <Button variant="destructive">Delete Item</Button>
</ConfirmPopover>
```

### Dengan Custom Title dan Description
```tsx
<ConfirmPopover
  title="Hapus Pengguna?"
  description="Pengguna ini akan dihapus secara permanen. Tindakan ini tidak dapat dibatalkan."
  onConfirm={handleDelete}
  onCancel={() => setOpen(false)}
>
  <Button variant="destructive">Hapus Pengguna</Button>
</ConfirmPopover>
```

### Dengan State Controlled
```tsx
function DeleteButton() {
  const [open, setOpen] = useState(false);
  const [isLoading, setIsLoading] = useState(false);

  const handleDelete = async () => {
    setIsLoading(true);
    try {
      await api.delete("/users/1");
      toast.success("User deleted successfully");
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <ConfirmPopover
      open={open}
      onOpenChange={setOpen}
      title="Hapus Data?"
      description="Data ini akan dihapus secara permanen."
      onConfirm={handleDelete}
      onCancel={() => setOpen(false)}
      isLoading={isLoading}
    >
      <Button variant="destructive">Hapus</Button>
    </ConfirmPopover>
  );
}
```

### Dengan Ikon sebagai Trigger
```tsx
<ConfirmPopover
  title="Hapus Item?"
  description="Item ini akan dihapus dari keranjang."
  onConfirm={handleRemove}
  onCancel={() => {}}
>
  <Button variant="ghost" size="icon">
    <TrashIcon className="h-4 w-4" />
  </Button>
</ConfirmPopover>
```

### Dengan Link sebagai Trigger
```tsx
<ConfirmPopover
  title="Keluar?"
  description="Apakah Anda yakin ingin keluar dari akun ini?"
  onConfirm={handleLogout}
  onCancel={() => {}}
>
  <Link to="#" className="text-red-600">
    Logout
  </Link>
</ConfirmPopover>
```

### Dengan Async Delete Operation
```tsx
function DeleteUserButton({ userId, onSuccess }) {
  const [isLoading, setIsLoading] = useState(false);

  const handleDelete = async () => {
    setIsLoading(true);
    try {
      await api.delete(`/users/${userId}`);
      toast.success("User deleted successfully");
      onSuccess?.();
    } catch (error) {
      toast.error("Failed to delete user");
      throw error;
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <ConfirmPopover
      title="Hapus Pengguna?"
      description="Pengguna akan dihapus secara permanen. Tindakan ini tidak dapat dibatalkan."
      onConfirm={handleDelete}
      onCancel={() => {}}
      isLoading={isLoading}
    >
      <Button variant="destructive" size="sm">
        Hapus
      </Button>
    </ConfirmPopover>
  );
}
```

### Dalam Tabel (Row Actions)
```tsx
function UserTableRow({ user }) {
  const handleDelete = async () => {
    await api.delete(`/users/${user.id}`);
    toast.success(`User ${user.name} deleted`);
  };

  return (
    <TableRow>
      <TableCell>{user.name}</TableCell>
      <TableCell>{user.email}</TableCell>
      <TableCell>
        <div className="flex gap-2">
          <Button variant="ghost" size="icon">
            <EditIcon className="h-4 w-4" />
          </Button>
          <ConfirmPopover
            title={`Hapus ${user.name}?`}
            description="Data pengguna akan dihapus secara permanen."
            onConfirm={handleDelete}
            onCancel={() => {}}
          >
            <Button variant="ghost" size="icon">
              <TrashIcon className="h-4 w-4" />
            </Button>
          </ConfirmPopover>
        </div>
      </TableCell>
    </TableRow>
  );
}
```

### Dengan Multiple Confirmation Steps
```tsx
function CriticalActionButton() {
  const [step, setStep] = useState(1);
  const [open, setOpen] = useState(false);

  const handleConfirm = async () => {
    if (step === 1) {
      setStep(2);
      return;
    }
    await performCriticalAction();
    setOpen(false);
    setStep(1);
  };

  return (
    <ConfirmPopover
      open={open}
      onOpenChange={setOpen}
      title={step === 1 ? "Konfirmasi Tindakan" : "Konfirmasi Akhir"}
      description={
        step === 1
          ? "Tindakan ini akan mempengaruhi semua data. Lanjutkan?"
          : "Apakah Anda benar-benar yakin? Tindakan ini TIDAK dapat dibatalkan."
      }
      onConfirm={handleConfirm}
      onCancel={() => {
        setOpen(false);
        setStep(1);
      }}
    >
      <Button variant="destructive">Tindakan Kritis</Button>
    </ConfirmPopover>
  );
}
```

### Dengan Custom Styling
```tsx
<ConfirmPopover
  title="Konfirmasi"
  description="Apakah Anda yakin?"
  onConfirm={handleConfirm}
  onCancel={handleCancel}
>
  <Button 
    variant="outline" 
    className="border-red-500 text-red-500 hover:bg-red-50"
  >
    Hapus
  </Button>
</ConfirmPopover>
```

## Best Practices

### 1. Gunakan untuk Aksi yang Tidak Dapat Di-undo
```tsx
// ✅ Baik: Aksi destruktif yang perlu konfirmasi
<ConfirmPopover onConfirm={deleteUser}>
  <Button>Hapus User</Button>
</ConfirmPopover>

// ❌ Hindari: Aksi yang tidak berbahaya
<ConfirmPopover onConfirm={downloadFile}>
  <Button>Download</Button>
</ConfirmPopover>
```

### 2. Deskripsi yang Jelas
Berikan deskripsi yang informatif tentang konsekuensi tindakan:
```tsx
// ✅ Baik: Deskripsi jelas
description="Data akan dihapus permanen dan tidak dapat dikembalikan."

// ❌ Hindari: Deskripsi tidak jelas
description="Are you sure?"
```

### 3. Handle Async Operations
Gunakan Promise untuk aksi asinkron:
```tsx
const handleDelete = async () => {
  try {
    await api.delete(id);
    toast.success("Deleted");
  } catch (error) {
    toast.error("Failed");
    throw error; // Penting untuk menampilkan error di popover
  }
};
```

### 4. Loading State
Selalu gunakan loading state untuk aksi asinkron:
```tsx
<ConfirmPopover
  isLoading={isDeleting}
  onConfirm={handleDelete}
  // ...
>
  <Button disabled={isDeleting}>Hapus</Button>
</ConfirmPopover>
```

## Troubleshooting

### Popover Tidak Muncul
- Pastikan `children` di-render dengan benar
- Periksa apakah ada elemen yang menutupi trigger

### onSuccess Tidak Terpanggil
- Pastikan `onConfirm` mengembalikan Promise yang resolve
- Periksa apakah ada error yang menghentikan eksekusi

### Loading State Tidak Muncul
- Pastikan `isLoading` prop di-set dengan benar
- Periksa apakah state loading di-update sebelum aksi

## Referensi

- [Shadcn Popover Documentation](https://ui.shadcn.com/docs/components/popover)
- [Radix UI Popover](https://www.radix-ui.com/primitives/docs/components/popover)
