---
title: "LoadingScreen"
sidebar_label: "LoadingScreen"
---

# LoadingScreen

**Lokasi:** `components/custom/loading-screen.tsx`

## Deskripsi

Fullscreen loading overlay dengan animasi yang digunakan untuk menampilkan indikator loading di seluruh halaman. LoadingScreen cocok digunakan untuk proses loading awal aplikasi, transisi antar halaman, atau operasi async yang memerlukan waktu lama.

## Fitur

- Fullscreen overlay dengan efek blur
- Animasi spinner yang halus
- Animasi pulse pada teks
- Transisi fade in/out yang smooth
- Background gradient dengan warna brand
- Pointer events none saat tidak aktif

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `show` | `boolean` | Ya | - | Status tampilan loading screen |

## Contoh Penggunaan

### Basic Usage
```tsx
import { LoadingScreen } from "@/components/custom/loading-screen";

function App() {
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    // Simulate loading
    setTimeout(() => setIsLoading(false), 2000);
  }, []);

  return (
    <>
      <LoadingScreen show={isLoading} />
      <div className="p-4">
        <h1>Main Content</h1>
      </div>
    </>
  );
}
```

### Untuk Loading Data Awal
```tsx
function DataPage() {
  const { data, isLoading } = useQuery({
    queryKey: ["data"],
    queryFn: fetchData,
  });

  return (
    <>
      <LoadingScreen show={isLoading} />
      {data && <DataDisplay data={data} />}
    </>
  );
}
```

### Untuk Navigasi Antar Halaman
```tsx
import { useNavigation } from "react-router-dom";

function Layout() {
  const navigation = useNavigation();
  const isLoading = navigation.state === "loading";

  return (
    <>
      <LoadingScreen show={isLoading} />
      <Header />
      <Outlet />
      <Footer />
    </>
  );
}
```

### Untuk Operasi Async (Submit Form)
```tsx
function SubmitForm() {
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = async (e) => {
    e.preventDefault();
    setIsSubmitting(true);
    try {
      await api.post("/data", formData);
      toast.success("Data saved");
    } catch (error) {
      toast.error("Failed to save");
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <>
      <LoadingScreen show={isSubmitting} />
      <form onSubmit={handleSubmit}>
        {/* form fields */}
        <Button type="submit" disabled={isSubmitting}>
          Submit
        </Button>
      </form>
    </>
  );
}
```

### Untuk Loading dengan Timeout
```tsx
function AppWithTimeout() {
  const [isLoading, setIsLoading] = useState(true);
  const [hasError, setHasError] = useState(false);

  useEffect(() => {
    const timer = setTimeout(() => {
      if (isLoading) {
        setIsLoading(false);
        setHasError(true);
        toast.error("Loading timeout");
      }
    }, 10000);

    return () => clearTimeout(timer);
  }, [isLoading]);

  // Simulate loading
  useEffect(() => {
    fetchData()
      .then(() => setIsLoading(false))
      .catch(() => setHasError(true));
  }, []);

  if (hasError) {
    return <ErrorFallback />;
  }

  return (
    <>
      <LoadingScreen show={isLoading} />
      <MainContent />
    </>
  );
}
```

### Dalam Router dengan Suspense
```tsx
import { Suspense } from "react";

function App() {
  const [isLoading, setIsLoading] = useState(false);

  // Gunakan dengan React Router data loading
  const router = createBrowserRouter([
    {
      path: "/",
      element: <Root />,
      loader: async () => {
        setIsLoading(true);
        const data = await fetchData();
        setIsLoading(false);
        return data;
      },
    },
  ]);

  return (
    <>
      <LoadingScreen show={isLoading} />
      <RouterProvider router={router} />
    </>
  );
}
```

### Dengan Multiple Loading States
```tsx
function Dashboard() {
  const [userLoading, setUserLoading] = useState(true);
  const [statsLoading, setStatsLoading] = useState(true);
  const [chartLoading, setChartLoading] = useState(true);

  const isLoading = userLoading || statsLoading || chartLoading;

  useEffect(() => {
    Promise.all([
      fetchUser().then(() => setUserLoading(false)),
      fetchStats().then(() => setStatsLoading(false)),
      fetchChart().then(() => setChartLoading(false)),
    ]);
  }, []);

  return (
    <>
      <LoadingScreen show={isLoading} />
      {!isLoading && <DashboardContent />}
    </>
  );
}
```

### Dengan Minimal Duration (Mencegah Flicker)
```tsx
function AppWithMinDuration() {
  const [isLoading, setIsLoading] = useState(true);
  const [showLoader, setShowLoader] = useState(true);

  useEffect(() => {
    const startTime = Date.now();

    fetchData().finally(() => {
      const elapsed = Date.now() - startTime;
      const remaining = Math.max(0, 500 - elapsed); // Minimal 500ms

      setTimeout(() => {
        setIsLoading(false);
        // Delay hide loader untuk smooth transition
        setTimeout(() => setShowLoader(false), 300);
      }, remaining);
    });
  }, []);

  return <LoadingScreen show={showLoader} />;
}
```

## Kustomisasi

### Custom Colors
Untuk mengubah warna loading screen, modifikasi file:
```css
/* Modify gradient colors */
.bg-linear-to-b.from-(--color-brand-start) 
.via-(--color-brand-mid) 
.to-(--color-brand-end)
```

### Custom Animation Speed
```tsx
// Modifikasi animasi spinner
<div className="animate-spin rounded-full h-15 w-15 border-b-2 border-white mx-auto mb-4"></div>

// Modifikasi pulse animation
<p className="text-white text-sm font-medium animate-pulse duration-1000">
  Please wait ...
</p>
```

## Best Practices

### 1. Gunakan untuk Loading yang Signifikan
LoadingScreen cocok untuk loading yang mempengaruhi seluruh halaman:
```tsx
// ✅ Baik: Loading seluruh halaman
<LoadingScreen show={isPageLoading} />

// ❌ Hindari: Loading komponen kecil
// Gunakan Skeleton untuk komponen lokal
```

### 2. Minimal Duration
Gunakan minimal duration untuk mencegah flicker:
```tsx
const MIN_LOADING_TIME = 500;
const startTime = Date.now();

await fetchData();
const elapsed = Date.now() - startTime;
const remaining = Math.max(0, MIN_LOADING_TIME - elapsed);
setTimeout(() => setIsLoading(false), remaining);
```

### 3. Handle Error Timeout
Tambahkan timeout untuk mencegah loading tak terbatas:
```tsx
setTimeout(() => {
  if (isLoading) {
    setIsLoading(false);
    toast.error("Loading timeout");
  }
}, 10000);
```

### 4. Transisi Halus
LoadingScreen sudah memiliki transisi fade, gunakan dengan tepat.

## Troubleshooting

### Loading Screen Tidak Hilang
- Pastikan `show` prop di-set ke `false` setelah loading selesai
- Periksa apakah ada error yang menghentikan proses

### Loading Screen Tidak Muncul
- Pastikan `show` prop di-set ke `true`
- Periksa apakah komponen di-render dengan benar

### Animasi Tidak Berjalan
- Pastikan Tailwind CSS dikonfigurasi dengan benar
- Periksa apakah ada CSS yang menimpa animation

## Referensi

- [Tailwind CSS Animation](https://tailwindcss.com/docs/animation)
- [React Suspense Documentation](https://react.dev/reference/react/Suspense)
- [React Router Data Loading](https://reactrouter.com/en/main/start/tutorial#loading-data)
