---
title: "DeletePopover"
sidebar_label: "DeletePopover"
---

# DeletePopover

**Lokasi:** `components/custom/delete-popover.tsx`

## Deskripsi

Popover konfirmasi khusus untuk aksi hapus data. DeletePopover adalah varian spesifik dari ConfirmPopover yang telah dikustomisasi untuk operasi penghapusan dengan teks dan styling yang sesuai.

## Fitur

- Popover konfirmasi dengan tema destruktif
- Tombol konfirmasi berwarna merah (destructive)
- State loading untuk aksi async
- Callback onSuccess setelah penghapusan berhasil
- Otomatis menutup setelah konfirmasi
- Styling konsisten dengan aksi hapus

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `onConfirm` | `() => Promise<void> \| void` | Ya | - | Handler aksi hapus |
| `onSuccess` | `() => void` | Tidak | `() => {}` | Callback setelah sukses |
| `isLoading` | `boolean` | Tidak | - | State loading |

## Contoh Penggunaan

### Basic Usage
```tsx
import { DeletePopover } from "@/components/custom/delete-popover";
import { Button } from "@/components/ui/button";

<DeletePopover
  onConfirm={() => console.log("Deleted!")}
>
  <Button variant="destructive">Delete</Button>
</DeletePopover>
```

### Dengan API Call
```tsx
function DeleteUserButton({ userId, onUserDeleted }) {
  const [isDeleting, setIsDeleting] = useState(false);

  const handleDelete = async () => {
    setIsDeleting(true);
    try {
      await api.delete(`/users/${userId}`);
      toast.success("User deleted successfully");
      onUserDeleted?.();
    } catch (error) {
      toast.error("Failed to delete user");
      throw error;
    } finally {
      setIsDeleting(false);
    }
  };

  return (
    <DeletePopover
      onConfirm={handleDelete}
      onSuccess={onUserDeleted}
      isLoading={isDeleting}
    >
      <Button variant="destructive" size="sm">
        Delete
      </Button>
    </DeletePopover>
  );
}
```

### Dalam Tabel (Row Actions)
```tsx
function UserTableRow({ user, onDelete }) {
  return (
    <TableRow>
      <TableCell>{user.name}</TableCell>
      <TableCell>{user.email}</TableCell>
      <TableCell>
        <div className="flex gap-2">
          <Button variant="ghost" size="icon">
            <EditIcon className="h-4 w-4" />
          </Button>
          <DeletePopover
            onConfirm={() => onDelete(user.id)}
            onSuccess={() => toast.success(`${user.name} deleted`)}
          >
            <Button variant="ghost" size="icon">
              <TrashIcon className="h-4 w-4 text-destructive" />
            </Button>
          </DeletePopover>
        </div>
      </TableCell>
    </TableRow>
  );
}
```

### Dengan Ikon Saja
```tsx
<DeletePopover onConfirm={handleDelete}>
  <Button variant="ghost" size="icon" className="text-destructive hover:text-destructive">
    <TrashIcon className="h-4 w-4" />
  </Button>
</DeletePopover>
```

### Dalam Card Item
```tsx
function ProductCard({ product, onDelete }) {
  return (
    <Card>
      <CardHeader>
        <div className="flex justify-between items-start">
          <CardTitle>{product.name}</CardTitle>
          <DeletePopover onConfirm={() => onDelete(product.id)}>
            <Button variant="ghost" size="icon" className="h-8 w-8">
              <TrashIcon className="h-4 w-4" />
            </Button>
          </DeletePopover>
        </div>
        <CardDescription>{product.description}</CardDescription>
      </CardHeader>
      <CardContent>
        <p className="text-2xl font-bold">${product.price}</p>
      </CardContent>
    </Card>
  );
}
```

### Dengan Konfirmasi Ganda (Custom)
```tsx
function CriticalDeleteButton() {
  const [confirmText, setConfirmText] = useState("");
  const [open, setOpen] = useState(false);

  const handleDelete = async () => {
    if (confirmText !== "DELETE") {
      toast.error('Please type "DELETE" to confirm');
      return;
    }
    await performCriticalDelete();
    setOpen(false);
  };

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger asChild>
        <Button variant="destructive">Delete All Data</Button>
      </PopoverTrigger>
      <PopoverContent className="w-80">
        <div className="space-y-4">
          <div>
            <h4 className="font-medium">Delete All Data</h4>
            <p className="text-sm text-muted-foreground">
              This action cannot be undone. Type <strong>DELETE</strong> to confirm.
            </p>
          </div>
          <Input
            placeholder="Type DELETE to confirm"
            value={confirmText}
            onChange={(e) => setConfirmText(e.target.value)}
          />
          <div className="flex justify-end gap-2">
            <Button variant="outline" size="sm" onClick={() => setOpen(false)}>
              Cancel
            </Button>
            <Button
              variant="destructive"
              size="sm"
              onClick={handleDelete}
              disabled={confirmText !== "DELETE"}
            >
              Delete Permanently
            </Button>
          </div>
        </div>
      </PopoverContent>
    </Popover>
  );
}
```

### Dalam Modal Konfirmasi (Custom)
```tsx
function DeleteWithModal() {
  const [open, setOpen] = useState(false);
  const [itemToDelete, setItemToDelete] = useState(null);

  const handleDelete = async () => {
    await api.delete(`/items/${itemToDelete.id}`);
    toast.success("Item deleted");
    setOpen(false);
    setItemToDelete(null);
  };

  return (
    <>
      <Button
        variant="destructive"
        onClick={() => {
          setItemToDelete(item);
          setOpen(true);
        }}
      >
        Delete
      </Button>

      <Dialog open={open} onOpenChange={setOpen}>
        <DialogContent>
          <DialogHeader>
            <DialogTitle>Delete {itemToDelete?.name}?</DialogTitle>
            <DialogDescription>
              This action cannot be undone. This will permanently delete the item.
            </DialogDescription>
          </DialogHeader>
          <DialogFooter>
            <Button variant="outline" onClick={() => setOpen(false)}>
              Cancel
            </Button>
            <Button variant="destructive" onClick={handleDelete}>
              Delete
            </Button>
          </DialogFooter>
        </DialogContent>
      </Dialog>
    </>
  );
}
```

## Best Practices

### 1. Gunakan untuk Aksi Hapus Saja
DeletePopover dirancang khusus untuk aksi penghapusan:
```tsx
// ✅ Baik: Aksi hapus
<DeletePopover onConfirm={deleteUser}>Delete User</DeletePopover>

// ❌ Hindari: Aksi lain
<DeletePopover onConfirm={archiveUser}>Archive User</DeletePopover>
```

### 2. Berikan Feedback Setelah Hapus
Selalu berikan feedback setelah aksi hapus berhasil:
```tsx
const handleDelete = async () => {
  await api.delete(id);
  toast.success("Item deleted successfully");
  onSuccess?.(); // Refresh data
};
```

### 3. Handle Error dengan Baik
Tampilkan error jika penghapusan gagal:
```tsx
const handleDelete = async () => {
  try {
    await api.delete(id);
    toast.success("Deleted");
  } catch (error) {
    toast.error(error.message);
    throw error; // Penting untuk popover
  }
};
```

### 4. Loading State
Gunakan loading state untuk mencegah double click:
```tsx
<DeletePopover
  isLoading={isDeleting}
  onConfirm={handleDelete}
>
  <Button disabled={isDeleting}>
    {isDeleting ? "Deleting..." : "Delete"}
  </Button>
</DeletePopover>
```

## Troubleshooting

### Popover Tidak Tertutup Setelah Delete
- Pastikan `onConfirm` mengembalikan Promise yang resolve
- Periksa apakah ada error yang menghentikan eksekusi

### Loading State Tidak Muncul
- Pastikan `isLoading` prop di-set dengan benar
- State loading harus di-set sebelum aksi dimulai

## Referensi

- [Shadcn Popover Documentation](https://ui.shadcn.com/docs/components/popover)
- [Radix UI Popover](https://www.radix-ui.com/primitives/docs/components/popover)
