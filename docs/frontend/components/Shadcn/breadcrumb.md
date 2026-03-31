---
title: "Breadcrumb"
sidebar_label: "Breadcrumb"
---

# Breadcrumb

**Lokasi:** `components/ui/breadcrumb.tsx`  
**Dokumentasi Resmi:** [Shadcn Breadcrumb](https://ui.shadcn.com/docs/components/breadcrumb)

## Deskripsi

Komponen navigasi untuk menunjukkan lokasi pengguna dalam hierarki halaman. Breadcrumb membantu pengguna memahami posisi mereka dalam struktur aplikasi dan memudahkan navigasi ke level yang lebih tinggi tanpa harus menggunakan tombol back.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Breadcrumb` | Container utama untuk breadcrumb |
| `BreadcrumbList` | Container untuk daftar breadcrumb items |
| `BreadcrumbItem` | Item individual dalam breadcrumb |
| `BreadcrumbLink` | Link navigasi ke halaman sebelumnya |
| `BreadcrumbPage` | Halaman saat ini (bukan link, tidak dapat diklik) |
| `BreadcrumbSeparator` | Pemisah antar item (default: chevron right) |
| `BreadcrumbEllipsis` | Indikator item yang di-singkat |

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `asChild` | `boolean` | `false` | Render sebagai child component (untuk Link dari React Router) |
| `className` | `string` | - | Custom CSS classes |

## Contoh Penggunaan

### Basic Breadcrumb
```tsx
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/components/ui/breadcrumb";

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

### Breadcrumb dengan Custom Separator
```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator>
      <SlashIcon className="h-4 w-4" />
    </BreadcrumbSeparator>
    <BreadcrumbItem>
      <BreadcrumbLink href="/products">Products</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator>
      <SlashIcon className="h-4 w-4" />
    </BreadcrumbSeparator>
    <BreadcrumbItem>
      <BreadcrumbPage>Details</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

### Breadcrumb dengan Ellipsis (Untuk Menu yang Disembunyikan)
```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbEllipsis />
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbLink href="/products/electronics">Electronics</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Laptops</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

### Breadcrumb dengan React Router
```tsx
import { Link, useLocation } from 'react-router-dom';

function DynamicBreadcrumb() {
  const location = useLocation();
  const pathnames = location.pathname.split('/').filter(x => x);

  return (
    <Breadcrumb>
      <BreadcrumbList>
        <BreadcrumbItem>
          <BreadcrumbLink asChild>
            <Link to="/">Home</Link>
          </BreadcrumbLink>
        </BreadcrumbItem>
        {pathnames.map((name, index) => {
          const routeTo = `/${pathnames.slice(0, index + 1).join('/')}`;
          const isLast = index === pathnames.length - 1;
          
          return (
            <React.Fragment key={name}>
              <BreadcrumbSeparator />
              <BreadcrumbItem>
                {isLast ? (
                  <BreadcrumbPage>{name}</BreadcrumbPage>
                ) : (
                  <BreadcrumbLink asChild>
                    <Link to={routeTo}>{name}</Link>
                  </BreadcrumbLink>
                )}
              </BreadcrumbItem>
            </React.Fragment>
          );
        })}
      </BreadcrumbList>
    </Breadcrumb>
  );
}
```

### Breadcrumb dengan Ikon
```tsx
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/" className="flex items-center gap-1">
        <HomeIcon className="h-4 w-4" />
        <span>Home</span>
      </BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbLink href="/dashboard">Dashboard</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Analytics</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

### Breadcrumb dalam Layout Halaman
```tsx
function PageHeader() {
  return (
    <div className="space-y-2">
      <Breadcrumb>
        <BreadcrumbList>
          <BreadcrumbItem>
            <BreadcrumbLink href="/">Home</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbLink href="/products">Products</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbPage>Product Details</BreadcrumbPage>
          </BreadcrumbItem>
        </BreadcrumbList>
      </Breadcrumb>
      
      <div>
        <h1 className="text-2xl font-bold">Product Details</h1>
        <p className="text-muted-foreground">
          View and manage product information
        </p>
      </div>
    </div>
  );
}
```

### Breadcrumb dengan Dropdown Menu
```tsx
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";

<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <BreadcrumbLink className="cursor-pointer">
            Products
          </BreadcrumbLink>
        </DropdownMenuTrigger>
        <DropdownMenuContent align="start">
          <DropdownMenuItem>Electronics</DropdownMenuItem>
          <DropdownMenuItem>Clothing</DropdownMenuItem>
          <DropdownMenuItem>Books</DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Laptops</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

### Breadcrumb Responsive (Mobile)
```tsx
function ResponsiveBreadcrumb() {
  const [isMobile, setIsMobile] = useState(false);

  useEffect(() => {
    const checkMobile = () => setIsMobile(window.innerWidth < 768);
    checkMobile();
    window.addEventListener('resize', checkMobile);
    return () => window.removeEventListener('resize', checkMobile);
  }, []);

  if (isMobile) {
    return (
      <Breadcrumb>
        <BreadcrumbList>
          <BreadcrumbItem>
            <BreadcrumbLink href="/products">Back</BreadcrumbLink>
          </BreadcrumbItem>
        </BreadcrumbList>
      </Breadcrumb>
    );
  }

  return (
    <Breadcrumb>
      <BreadcrumbList>
        <BreadcrumbItem>
          <BreadcrumbLink href="/">Home</BreadcrumbLink>
        </BreadcrumbItem>
        <BreadcrumbSeparator />
        <BreadcrumbItem>
          <BreadcrumbLink href="/products">Products</BreadcrumbLink>
        </BreadcrumbItem>
        <BreadcrumbSeparator />
        <BreadcrumbItem>
          <BreadcrumbPage>Details</BreadcrumbPage>
        </BreadcrumbItem>
      </BreadcrumbList>
    </Breadcrumb>
  );
}
```

### Breadcrumb dengan Custom Styling
```tsx
<Breadcrumb className="bg-muted p-2 rounded-lg">
  <BreadcrumbList className="text-sm">
    <BreadcrumbItem>
      <BreadcrumbLink href="/" className="text-primary hover:underline">
        Home
      </BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator className="text-muted-foreground" />
    <BreadcrumbItem>
      <BreadcrumbLink href="/docs" className="text-primary hover:underline">
        Documentation
      </BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator className="text-muted-foreground" />
    <BreadcrumbItem>
      <BreadcrumbPage className="font-semibold">Components</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

## Kustomisasi

### Custom Separator Styling
```tsx
<BreadcrumbSeparator className="text-primary/50">
  <ChevronRightIcon className="h-4 w-4" />
</BreadcrumbSeparator>
```

### Custom Link Styling
```tsx
<BreadcrumbLink 
  href="/dashboard" 
  className="text-blue-600 hover:text-blue-800 transition-colors"
>
  Dashboard
</BreadcrumbLink>
```

### Active Page Styling
```tsx
<BreadcrumbPage className="text-gray-600 font-semibold">
  Current Page
</BreadcrumbPage>
```

## Best Practices

### 1. Gunakan untuk Hierarki yang Jelas
Breadcrumb hanya untuk halaman dengan struktur hierarkis yang jelas:
```tsx
// ✅ Baik: Hierarki jelas
Home > Products > Electronics > Laptops

// ❌ Hindari: Tidak ada hierarki
Home > Dashboard > Settings > Profile
```

### 2. Home Page Selalu Pertama
Selalu sertakan link ke home page sebagai item pertama breadcrumb.

### 3. Halaman Terakhir Bukan Link
Halaman saat ini (current page) tidak boleh berupa link, gunakan `BreadcrumbPage`.

### 4. Responsive Design
Pertimbangkan untuk menyembunyikan beberapa level pada mobile menggunakan ellipsis.

### 5. Aksesibilitas
Breadcrumb sudah memiliki atribut `aria-label` yang sesuai untuk aksesibilitas.

## Troubleshooting

### Breadcrumb Tidak Terlihat
- Pastikan breadcrumb ditempatkan di lokasi yang sesuai
- Periksa apakah ada CSS yang menyembunyikan breadcrumb

### Separator Tidak Muncul
- Pastikan `BreadcrumbSeparator` ditempatkan di antara `BreadcrumbItem`
- Periksa apakah ada CSS yang menimpa visibility

## Referensi

- [Shadcn Breadcrumb Documentation](https://ui.shadcn.com/docs/components/breadcrumb)
- [Radix UI Breadcrumb](https://www.radix-ui.com/primitives/docs/components/breadcrumb)
- [WAI-ARIA Breadcrumb Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/)
- [MDN Breadcrumb Navigation](https://developer.mozilla.org/en-US/docs/Web/CSS/Layout_cookbook/Breadcrumb_Navigation)
