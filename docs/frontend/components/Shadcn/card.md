---
title: "Card"
sidebar_label: "Card"
---

# Card

**Lokasi:** `components/ui/card.tsx`  
**Dokumentasi Resmi:** [Shadcn Card](https://ui.shadcn.com/docs/components/card)

## Deskripsi

Komponen container dengan border, shadow, dan struktur yang konsisten. Card digunakan untuk mengelompokkan konten terkait dalam satu wadah yang terdefinisi dengan baik, seperti informasi produk, profil pengguna, atau ringkasan data.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Card` | Container utama card dengan border, rounded, dan background putih |
| `CardHeader` | Header section, biasanya berisi judul dan deskripsi |
| `CardTitle` | Judul card dengan styling bold dan uppercase |
| `CardDescription` | Deskripsi atau subjudul dengan warna lebih redup |
| `CardAction` | Area untuk aksi (tombol) yang diposisikan di kanan header |
| `CardContent` | Konten utama card |
| `CardFooter` | Footer section, biasanya berisi tombol aksi |

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `className` | `string` | - | Custom CSS classes untuk masing-masing komponen |

## Contoh Penggunaan

### Basic Card
```tsx
<Card>
  <CardHeader>
    <CardTitle>User Profile</CardTitle>
    <CardDescription>Manage your account settings and preferences</CardDescription>
  </CardHeader>
  <CardContent>
    <p className="text-sm text-muted-foreground">
      Your profile information will appear here. You can edit your details by clicking the edit button.
    </p>
  </CardContent>
  <CardFooter>
    <Button>Edit Profile</Button>
  </CardFooter>
</Card>
```

### Card dengan Action Button
```tsx
<Card>
  <CardHeader>
    <CardTitle>Notifications</CardTitle>
    <CardDescription>Configure your notification preferences</CardDescription>
    <CardAction>
      <Button variant="ghost" size="icon">
        <SettingsIcon className="h-4 w-4" />
      </Button>
    </CardAction>
  </CardHeader>
  <CardContent>
    <div className="space-y-2">
      <div className="flex items-center justify-between">
        <span className="text-sm">Email notifications</span>
        <Switch />
      </div>
      <div className="flex items-center justify-between">
        <span className="text-sm">Push notifications</span>
        <Switch />
      </div>
    </div>
  </CardContent>
</Card>
```

### Card dengan Image
```tsx
<Card className="overflow-hidden">
  <img 
    src="/product-image.jpg" 
    alt="Product" 
    className="h-48 w-full object-cover"
  />
  <CardHeader>
    <CardTitle>Premium Headphones</CardTitle>
    <CardDescription>High-quality wireless headphones</CardDescription>
  </CardHeader>
  <CardContent>
    <div className="flex items-center justify-between">
      <span className="text-2xl font-bold">$199.99</span>
      <Badge variant="success">In Stock</Badge>
    </div>
  </CardContent>
  <CardFooter>
    <Button className="w-full">Add to Cart</Button>
  </CardFooter>
</Card>
```

### Card Grid Layout
```tsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  {products.map((product) => (
    <Card key={product.id} className="hover:shadow-lg transition-shadow">
      <CardHeader>
        <CardTitle>{product.name}</CardTitle>
        <CardDescription>{product.category}</CardDescription>
      </CardHeader>
      <CardContent>
        <p className="text-2xl font-bold text-primary">${product.price}</p>
        <p className="text-sm text-muted-foreground mt-2">{product.description}</p>
      </CardContent>
      <CardFooter className="flex gap-2">
        <Button variant="outline" size="sm">Details</Button>
        <Button size="sm">Buy Now</Button>
      </CardFooter>
    </Card>
  ))}
</div>
```

### Card dengan Statistik
```tsx
<Card>
  <CardHeader className="pb-2">
    <CardTitle className="text-sm font-medium text-muted-foreground">
      Total Revenue
    </CardTitle>
  </CardHeader>
  <CardContent>
    <div className="text-3xl font-bold">$45,231.89</div>
    <div className="flex items-center gap-1 mt-2">
      <TrendingUpIcon className="h-4 w-4 text-green-500" />
      <span className="text-sm text-green-500">+20.1%</span>
      <span className="text-sm text-muted-foreground">from last month</span>
    </div>
  </CardContent>
  <CardFooter>
    <Button variant="ghost" size="sm" className="w-full">
      View Details
      <ArrowRightIcon className="ml-2 h-4 w-4" />
    </Button>
  </CardFooter>
</Card>
```

### Card dengan List Items
```tsx
<Card>
  <CardHeader>
    <CardTitle>Recent Orders</CardTitle>
    <CardDescription>Your last 5 orders</CardDescription>
  </CardHeader>
  <CardContent className="p-0">
    <div className="divide-y">
      {orders.map((order) => (
        <div key={order.id} className="flex items-center justify-between p-4 hover:bg-muted/50">
          <div>
            <p className="font-medium">{order.product}</p>
            <p className="text-sm text-muted-foreground">{order.date}</p>
          </div>
          <div className="text-right">
            <p className="font-medium">${order.amount}</p>
            <Badge variant={order.status === 'completed' ? 'success' : 'warning'}>
              {order.status}
            </Badge>
          </div>
        </div>
      ))}
    </div>
  </CardContent>
  <CardFooter>
    <Button variant="outline" className="w-full">View All Orders</Button>
  </CardFooter>
</Card>
```

### Card dengan Form
```tsx
<Card>
  <CardHeader>
    <CardTitle>Change Password</CardTitle>
    <CardDescription>Update your password to keep your account secure</CardDescription>
  </CardHeader>
  <CardContent>
    <form className="space-y-4">
      <div className="space-y-1.5">
        <Label htmlFor="current-password">Current Password</Label>
        <Input id="current-password" type="password" placeholder="••••••••" />
      </div>
      <div className="space-y-1.5">
        <Label htmlFor="new-password">New Password</Label>
        <Input id="new-password" type="password" placeholder="••••••••" />
      </div>
      <div className="space-y-1.5">
        <Label htmlFor="confirm-password">Confirm Password</Label>
        <Input id="confirm-password" type="password" placeholder="••••••••" />
      </div>
    </form>
  </CardContent>
  <CardFooter className="flex justify-end gap-2">
    <Button variant="outline">Cancel</Button>
    <Button>Update Password</Button>
  </CardFooter>
</Card>
```

### Card dengan Hover Effect
```tsx
<Card className="group cursor-pointer hover:shadow-xl transition-all duration-200">
  <CardHeader>
    <div className="flex items-center gap-3">
      <div className="p-2 bg-primary/10 rounded-lg group-hover:bg-primary/20 transition-colors">
        <FileTextIcon className="h-6 w-6 text-primary" />
      </div>
      <CardTitle>Document Title</CardTitle>
    </div>
    <CardDescription>Click to view document details</CardDescription>
  </CardHeader>
  <CardContent>
    <p className="text-sm text-muted-foreground">
      This document contains important information about your account.
    </p>
  </CardContent>
  <CardFooter className="text-sm text-muted-foreground group-hover:text-primary transition-colors">
    View Details →
  </CardFooter>
</Card>
```

## Kustomisasi

### Variant Card
```tsx
// Card dengan border yang berbeda
<Card className="border-l-4 border-l-primary">
  <CardHeader>
    <CardTitle>Important Announcement</CardTitle>
  </CardHeader>
  <CardContent>
    <p>This is an important message that requires attention.</p>
  </CardContent>
</Card>

// Card dengan shadow yang lebih besar
<Card className="shadow-lg hover:shadow-xl transition-shadow">
  <CardContent className="p-6">
    <p>Card with enhanced shadow</p>
  </CardContent>
</Card>

// Card tanpa border
<Card className="border-0 shadow-none bg-transparent">
  <CardContent>
    <p>Card without border and shadow</p>
  </CardContent>
</Card>
```

### Custom Spacing
```tsx
// Mengubah padding
<Card className="p-0">
  <CardHeader className="p-4 pb-0">
    <CardTitle>No Padding Card</CardTitle>
  </CardHeader>
  <CardContent className="p-4">
    <p>Content with custom padding</p>
  </CardContent>
</Card>

// Mengubah gap antar section
<Card className="[&>div]:gap-1">
  <CardHeader>
    <CardTitle>Tight Spacing</CardTitle>
  </CardHeader>
  <CardContent>
    <p>Content with reduced gap</p>
  </CardContent>
</Card>
```

## Best Practices

### 1. Konsistensi Struktur
Gunakan struktur Card yang konsisten di seluruh aplikasi:
```tsx
// ✅ Baik: Struktur yang konsisten
<Card>
  <CardHeader>
    <CardTitle>Title</CardTitle>
    <CardDescription>Description</CardDescription>
  </CardHeader>
  <CardContent>
    {/* Content */}
  </CardContent>
  <CardFooter>
    {/* Actions */}
  </CardFooter>
</Card>

// ❌ Hindari: Mengacak struktur
<Card>
  <CardContent>Content</CardContent>
  <CardHeader>Title</CardHeader>
</Card>
```

### 2. Penggunaan CardAction
Gunakan `CardAction` untuk tombol atau ikon yang berada di pojok kanan header:
```tsx
<CardHeader>
  <CardTitle>Title</CardTitle>
  <CardDescription>Description</CardDescription>
  <CardAction>
    <Button variant="ghost" size="icon">
      <MoreVerticalIcon className="h-4 w-4" />
    </Button>
  </CardAction>
</CardHeader>
```

### 3. Responsive Design
Card secara default responsive, tetapi bisa disesuaikan:
```tsx
<Card className="w-full md:w-96">
  <CardContent>
    <p>Fixed width on desktop, full width on mobile</p>
  </CardContent>
</Card>
```

### 4. Loading State
Tampilkan skeleton saat data sedang dimuat:
```tsx
{isLoading ? (
  <Card>
    <CardHeader>
      <Skeleton className="h-6 w-32" />
      <Skeleton className="h-4 w-48" />
    </CardHeader>
    <CardContent>
      <Skeleton className="h-20 w-full" />
    </CardContent>
  </Card>
) : (
  <Card>
    {/* Actual content */}
  </Card>
)}
```

## Troubleshooting

### Card Tidak Memiliki Border
- Periksa apakah ada CSS yang menimpa border
- Pastikan tidak menggunakan `border-0` di className

### Konten Card Meluber
- Gunakan `overflow-hidden` pada Card jika ada konten yang meluber
- Pastikan gambar memiliki `object-cover` jika perlu

```tsx
<Card className="overflow-hidden">
  <img className="w-full h-48 object-cover" />
</Card>
```

### CardAction Tidak Sejajar
CardAction secara otomatis diposisikan di kanan header. Jika tidak sejajar, periksa apakah ada elemen lain yang mempengaruhi layout.

## Referensi

- [Shadcn Card Documentation](https://ui.shadcn.com/docs/components/card)
- [Radix UI Card](https://www.radix-ui.com/primitives/docs/components/card)
