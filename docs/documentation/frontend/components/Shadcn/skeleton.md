---
title: "Skeleton"
sidebar_label: "Skeleton"
---

# Skeleton

**Lokasi:** `components/ui/skeleton.tsx`  
**Dokumentasi Resmi:** [Shadcn Skeleton](https://ui.shadcn.com/docs/components/skeleton)

## Deskripsi

Komponen placeholder untuk loading state dengan animasi pulse. Skeleton digunakan untuk memberikan indikasi visual bahwa konten sedang dimuat, meningkatkan perceived performance dan mengurangi layout shift.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `className` | `string` | - | Custom CSS classes untuk mengatur dimensi dan styling |
| `children` | `React.ReactNode` | - | Konten yang akan ditampilkan (jarang digunakan) |

## Contoh Penggunaan

### Basic Skeleton
```tsx
import { Skeleton } from "@/components/ui/skeleton";

<div className="space-y-2">
  <Skeleton className="h-4 w-32" />
  <Skeleton className="h-4 w-48" />
  <Skeleton className="h-4 w-64" />
</div>
```

### Skeleton untuk Avatar
```tsx
<div className="flex items-center gap-4">
  <Skeleton className="h-12 w-12 rounded-full" />
  <div className="space-y-2">
    <Skeleton className="h-4 w-32" />
    <Skeleton className="h-3 w-24" />
  </div>
</div>
```

### Skeleton untuk Card
```tsx
<Card className="w-80">
  <CardHeader>
    <Skeleton className="h-6 w-3/4 mb-2" />
    <Skeleton className="h-4 w-full" />
  </CardHeader>
  <CardContent className="space-y-4">
    <Skeleton className="h-20 w-full rounded-lg" />
    <div className="flex gap-2">
      <Skeleton className="h-8 w-16" />
      <Skeleton className="h-8 w-16" />
    </div>
  </CardContent>
  <CardFooter>
    <Skeleton className="h-10 w-full" />
  </CardFooter>
</Card>
```

### Skeleton untuk Table
```tsx
<Table>
  <TableHeader>
    <TableRow>
      <TableHead>Name</TableHead>
      <TableHead>Email</TableHead>
      <TableHead>Status</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {Array.from({ length: 5 }).map((_, i) => (
      <TableRow key={i}>
        <TableCell><Skeleton className="h-4 w-32" /></TableCell>
        <TableCell><Skeleton className="h-4 w-48" /></TableCell>
        <TableCell><Skeleton className="h-5 w-16 rounded-full" /></TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### Skeleton untuk Form
```tsx
<div className="space-y-4">
  <div className="space-y-2">
    <Skeleton className="h-4 w-24" />
    <Skeleton className="h-10 w-full rounded-md" />
  </div>
  <div className="space-y-2">
    <Skeleton className="h-4 w-24" />
    <Skeleton className="h-10 w-full rounded-md" />
  </div>
  <div className="space-y-2">
    <Skeleton className="h-4 w-24" />
    <Skeleton className="h-20 w-full rounded-md" />
  </div>
  <Skeleton className="h-10 w-32 rounded-md" />
</div>
```

### Skeleton untuk Dashboard Stats
```tsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
  {Array.from({ length: 4 }).map((_, i) => (
    <Card key={i}>
      <CardHeader className="pb-2">
        <Skeleton className="h-4 w-24" />
      </CardHeader>
      <CardContent>
        <Skeleton className="h-8 w-16 mb-2" />
        <Skeleton className="h-3 w-32" />
      </CardContent>
    </Card>
  ))}
</div>
```

### Skeleton untuk Chart
```tsx
<Card>
  <CardHeader>
    <Skeleton className="h-6 w-32 mb-2" />
    <Skeleton className="h-4 w-48" />
  </CardHeader>
  <CardContent>
    <Skeleton className="h-64 w-full rounded-lg" />
  </CardContent>
</Card>
```

### Skeleton untuk List Item
```tsx
<div className="space-y-4">
  {Array.from({ length: 3 }).map((_, i) => (
    <div key={i} className="flex items-center gap-4 p-4 border rounded-lg">
      <Skeleton className="h-12 w-12 rounded-full" />
      <div className="flex-1 space-y-2">
        <Skeleton className="h-4 w-48" />
        <Skeleton className="h-3 w-32" />
      </div>
      <Skeleton className="h-8 w-20 rounded-md" />
    </div>
  ))}
</div>
```

### Skeleton untuk Chat Message
```tsx
<div className="space-y-4">
  <div className="flex gap-3">
    <Skeleton className="h-8 w-8 rounded-full" />
    <div className="space-y-2">
      <Skeleton className="h-4 w-48" />
      <Skeleton className="h-16 w-64 rounded-lg" />
    </div>
  </div>
  <div className="flex justify-end gap-3">
    <div className="space-y-2">
      <Skeleton className="h-4 w-32" />
      <Skeleton className="h-12 w-48 rounded-lg" />
    </div>
    <Skeleton className="h-8 w-8 rounded-full" />
  </div>
</div>
```

### Skeleton dengan Random Width
```tsx
function RandomWidthSkeleton() {
  const widths = ["w-32", "w-48", "w-64", "w-56", "w-40", "w-72"];

  return (
    <div className="space-y-2">
      {Array.from({ length: 5 }).map((_, i) => {
        const randomWidth = widths[Math.floor(Math.random() * widths.length)];
        return <Skeleton key={i} className={`h-4 ${randomWidth}`} />;
      })}
    </div>
  );
}
```

### Skeleton dengan Aspect Ratio
```tsx
<div className="grid grid-cols-3 gap-4">
  {Array.from({ length: 6 }).map((_, i) => (
    <div key={i} className="space-y-2">
      <Skeleton className="aspect-video rounded-lg" />
      <Skeleton className="h-4 w-3/4" />
      <Skeleton className="h-3 w-1/2" />
    </div>
  ))}
</div>
```

### Conditional Rendering dengan Skeleton
```tsx
function UserProfile({ user, isLoading }) {
  if (isLoading) {
    return (
      <Card className="w-96">
        <CardHeader>
          <div className="flex items-center gap-4">
            <Skeleton className="h-16 w-16 rounded-full" />
            <div className="space-y-2">
              <Skeleton className="h-5 w-32" />
              <Skeleton className="h-4 w-48" />
            </div>
          </div>
        </CardHeader>
        <CardContent className="space-y-3">
          <Skeleton className="h-4 w-full" />
          <Skeleton className="h-4 w-3/4" />
        </CardContent>
      </Card>
    );
  }

  return (
    <Card className="w-96">
      <CardHeader>
        <div className="flex items-center gap-4">
          <Avatar className="h-16 w-16">
            <AvatarImage src={user.avatar} />
            <AvatarFallback>{user.name[0]}</AvatarFallback>
          </Avatar>
          <div>
            <CardTitle>{user.name}</CardTitle>
            <CardDescription>{user.email}</CardDescription>
          </div>
        </div>
      </CardHeader>
      <CardContent>
        <p>{user.bio}</p>
      </CardContent>
    </Card>
  );
}
```

## Kustomisasi

### Custom Animation Speed
```tsx
<Skeleton className="h-4 w-32 animate-pulse duration-1000" />
```

### Custom Background Color
```tsx
<Skeleton className="h-4 w-32 bg-blue-100" />
```

### Custom Border Radius
```tsx
<Skeleton className="h-4 w-32 rounded-full" />
<Skeleton className="h-32 w-32 rounded-none" />
```

## Best Practices

### 1. Gunakan Skeleton untuk Semua Loading State
```tsx
// ✅ Baik: Skeleton untuk loading
{isLoading ? <Skeleton className="h-4 w-32" /> : <span>{data}</span>}

// ❌ Hindari: Teks "Loading..."
{isLoading ? <p>Loading...</p> : <span>{data}</span>}
```

### 2. Pertahankan Layout yang Sama
Skeleton harus memiliki dimensi yang sama dengan konten asli untuk mencegah layout shift:
```tsx
// ✅ Baik: Dimensi sama dengan konten
<Skeleton className="h-10 w-full rounded-md" />
<Input className="h-10 w-full rounded-md" />

// ❌ Hindari: Dimensi berbeda
<Skeleton className="h-8 w-full" />
<Input className="h-10 w-full" />
```

### 3. Gunakan Skeleton untuk Group Item
```tsx
// Untuk list, gunakan jumlah skeleton yang sama dengan konten yang akan ditampilkan
{isLoading
  ? Array.from({ length: 5 }).map((_, i) => (
      <Skeleton key={i} className="h-12 w-full" />
    ))
  : data.map((item) => <Item key={item.id} {...item} />)
}
```

### 4. Kombinasikan dengan Card
Skeleton bekerja sangat baik dengan Card component untuk loading state dashboard.

## Troubleshooting

### Skeleton Tidak Beranimasi
- Pastikan tidak ada CSS yang menimpa animation
- Animasi pulse adalah default, tidak perlu props tambahan

### Layout Shift
- Pastikan skeleton memiliki dimensi yang sama dengan konten asli
- Gunakan `aspect-*` classes untuk konten dengan ratio tertentu

## Referensi

- [Shadcn Skeleton Documentation](https://ui.shadcn.com/docs/components/skeleton)
- [Tailwind Animation](https://tailwindcss.com/docs/animation)
