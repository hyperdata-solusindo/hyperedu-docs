---
title: "Sonner (Toaster)"
sidebar_label: "Sonner (Toaster)"
---

# Sonner (Toaster)

**Lokasi:** `components/ui/sonner.tsx`  
**Dokumentasi Resmi:** [Sonner](https://sonner.emilkowal.ski/)

## Deskripsi

Komponen toast notification menggunakan library sonner dengan ikon custom. Sonner digunakan untuk menampilkan notifikasi non-intrusif seperti pesan sukses, error, atau informasi yang muncul sebentar dan menghilang.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Toaster` | Komponen yang harus ditempatkan di root layout untuk menampilkan toasts |

## Props Toaster

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `position` | `"top-left" \| "top-right" \| "bottom-left" \| "bottom-right" \| "top-center" \| "bottom-center"` | `"top-right"` | Posisi toast di layar |
| `theme` | `"light" \| "dark" \| "system"` | `"system"` | Tema toast |
| `richColors` | `boolean` | `false` | Menggunakan warna rich untuk toast |
| `closeButton` | `boolean` | `false` | Menampilkan tombol close |
| `duration` | `number` | `4000` | Durasi toast tampil (ms) |
| `expand` | `boolean` | `false` | Memperluas toast pada hover |

## Ikon Default

| Type | Ikon |
|------|------|
| `success` | `CircleCheckIcon` |
| `info` | `InfoIcon` |
| `warning` | `TriangleAlertIcon` |
| `error` | `OctagonXIcon` |
| `loading` | `Loader2Icon` (dengan animasi spin) |

## Contoh Penggunaan

### Setup Toaster di Root Layout
```tsx
// App.tsx atau RootLayout.tsx
import { Toaster } from "@/components/ui/sonner";

function App() {
  return (
    <div>
      <YourRoutes />
      <Toaster position="top-right" richColors closeButton />
    </div>
  );
}
```

### Basic Toasts
```tsx
import { toast } from "sonner";

// Success toast
toast.success("Operation completed successfully!");

// Error toast
toast.error("Something went wrong. Please try again.");

// Info toast
toast.info("New update available. Refresh to install.");

// Warning toast
toast.warning("Your session will expire in 5 minutes.");

// Loading toast
const promise = fetchData();
toast.promise(promise, {
  loading: "Loading data...",
  success: "Data loaded successfully!",
  error: "Failed to load data",
});
```

### Toast dengan Custom Description
```tsx
toast.success("User created", {
  description: "The user has been added to your organization.",
});

toast.error("Failed to save", {
  description: "Please check your internet connection and try again.",
});
```

### Toast dengan Action Button
```tsx
toast.success("Item added to cart", {
  action: {
    label: "Undo",
    onClick: () => {
      console.log("Undo action");
    },
  },
  duration: 5000,
});
```

### Toast dengan Custom Icon
```tsx
toast("Custom notification", {
  icon: <BellIcon className="h-5 w-5" />,
  description: "You have a new message",
});
```

### Promise Toast (Untuk Async Operations)
```tsx
// Menggunakan toast.promise untuk operasi async
const saveData = async () => {
  const response = await api.post("/data", payload);
  return response.data;
};

toast.promise(saveData(), {
  loading: "Saving data...",
  success: (data) => `Data saved successfully! ID: ${data.id}`,
  error: (err) => err.message || "Failed to save data",
});
```

### Toast dengan Custom Duration
```tsx
// Toast yang tahan lebih lama
toast.success("Important message", {
  duration: 10000, // 10 detik
});

// Toast yang tidak akan hilang otomatis
toast.warning("This message requires your attention", {
  duration: Infinity,
  closeButton: true,
});
```

### Toast dengan Posisi Berbeda
```tsx
// Top-right (default)
toast.success("Top-right notification");

// Top-left
toast.success("Top-left notification", {
  position: "top-left",
});

// Bottom-right
toast.success("Bottom-right notification", {
  position: "bottom-right",
});

// Bottom-left
toast.success("Bottom-left notification", {
  position: "bottom-left",
});

// Top-center
toast.success("Top-center notification", {
  position: "top-center",
});

// Bottom-center
toast.success("Bottom-center notification", {
  position: "bottom-center",
});
```

### Toast dalam Form Submit
```tsx
import { useForm } from "react-hook-form";
import { toast } from "sonner";

function LoginForm() {
  const { handleSubmit } = useForm();

  const onSubmit = async (data) => {
    try {
      await loginUser(data);
      toast.success("Logged in successfully!");
      // redirect
    } catch (error) {
      toast.error(error.message || "Invalid credentials");
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* form fields */}
      <Button type="submit">Login</Button>
    </form>
  );
}
```

### Toast untuk API Request dengan Loading State
```tsx
function DeleteButton({ id }) {
  const handleDelete = async () => {
    const promise = api.delete(`/items/${id}`);

    toast.promise(promise, {
      loading: "Deleting item...",
      success: () => {
        // Refetch data setelah sukses
        refetch();
        return "Item deleted successfully!";
      },
      error: "Failed to delete item",
    });
  };

  return (
    <Button variant="destructive" onClick={handleDelete}>
      Delete
    </Button>
  );
}
```

### Toast dengan HTML Content
```tsx
toast.success(
  <div>
    <strong>Welcome back!</strong>
    <p className="text-sm text-muted-foreground">You have 3 new messages.</p>
  </div>,
  {
    duration: 5000,
  }
);
```

### Dismiss Toast Programmatically
```tsx
const toastId = toast.loading("Processing...");

// Dismiss setelah beberapa saat
setTimeout(() => {
  toast.dismiss(toastId);
  toast.success("Process completed!");
}, 3000);
```

### Custom Toast dengan Styling
```tsx
toast.custom((t) => (
  <div className="bg-primary text-primary-foreground rounded-lg shadow-lg p-4">
    <div className="flex items-center gap-3">
      <CheckCircleIcon className="h-5 w-5" />
      <div>
        <p className="font-semibold">Custom Toast</p>
        <p className="text-sm opacity-90">This is a fully customizable toast</p>
      </div>
      <button
        onClick={() => toast.dismiss(t)}
        className="ml-auto"
      >
        <XIcon className="h-4 w-4" />
      </button>
    </div>
  </div>
));
```

## Kustomisasi

### Custom Toaster Configuration
```tsx
<Toaster
  position="top-center"
  richColors
  closeButton
  duration={3000}
  expand
  theme="light"
  className="toaster"
/>
```

### Global Styling via CSS
```css
/* global.css */
[data-sonner-toast] {
  font-family: var(--font-sans);
  border-radius: 12px;
}

[data-sonner-toast][data-type="success"] {
  background-color: #10b981;
  color: white;
}
```

## Best Practices

### 1. Gunakan untuk Feedback Non-Kritis
Toast cocok untuk feedback yang tidak memerlukan interaksi pengguna:
```tsx
// ✅ Baik: Feedback operasi yang sukses
toast.success("Item added to cart");

// ❌ Hindari: Konfirmasi yang memerlukan aksi
toast("Are you sure?") // Gunakan Dialog untuk ini
```

### 2. Deskripsi Singkat dan Jelas
```tsx
// ✅ Baik: Deskripsi jelas
toast.error("Invalid email format");

// ❌ Hindari: Deskripsi tidak jelas
toast.error("Error");
```

### 3. Gunakan Promise untuk Async Operations
```tsx
// ✅ Baik: Promise toast
toast.promise(saveData(), {
  loading: "Saving...",
  success: "Saved!",
  error: "Failed",
});

// ❌ Hindari: Manual loading state
toast.loading("Saving...");
try {
  await saveData();
  toast.dismiss();
  toast.success("Saved!");
} catch {
  toast.dismiss();
  toast.error("Failed");
}
```

### 4. Posisi yang Konsisten
Gunakan posisi toast yang sama di seluruh aplikasi untuk konsistensi UX.

### 5. Jangan Overuse
Terlalu banyak toast dapat mengganggu pengguna. Gunakan untuk notifikasi penting saja.

## Troubleshooting

### Toast Tidak Muncul
- Pastikan `<Toaster />` ditempatkan di root layout
- Periksa tidak ada error di console

### Toast Tidak Hilang
- Periksa duration setting
- Pastikan tidak ada kondisi yang mencegah dismiss

### Ikon Tidak Sesuai
- Ikon sudah dikonfigurasi di komponen Toaster
- Untuk custom ikon, gunakan prop `icon` pada toast

## Referensi

- [Sonner Documentation](https://sonner.emilkowal.ski/)
- [Sonner API Reference](https://sonner.emilkowal.ski/api)
- [Toast Design Guidelines](https://m3.material.io/components/snackbar)
