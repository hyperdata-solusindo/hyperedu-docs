---
title: "AppSidebar"
sidebar_label: "AppSidebar"
---

# AppSidebar

**Lokasi:** `components/custom/app-sidebar.tsx`

## Deskripsi

Komponen navigasi sidebar utama yang menampilkan struktur menu aplikasi dengan bagian yang dapat dilipat (collapsible sections), pelacakan status aktif, dan informasi pengguna. Sidebar ini terintegrasi dengan Redux untuk state management dan mendukung mode collapse untuk tampilan yang lebih ringkas.

## Fitur

- Struktur menu bertingkat dengan ikon
- Indikasi visual menu aktif dengan border kiri
- Dukungan menu bersarang (unlimited depth)
- Tampilan profil pengguna dengan role dan waktu saat ini
- State loading dengan skeleton
- Desain responsif dengan mode collapse
- Animasi smooth untuk transisi
- Dukungan tooltip pada mode collapsed

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `items` | `MenuItem[]` | Tidak | `[]` | Array item menu yang akan ditampilkan |
| `activeMenus` | `string[]` | Tidak | `[]` | Array link menu yang aktif |
| `isLoading` | `boolean` | Tidak | `false` | Status loading sidebar |
| `isLogOuting` | `boolean` | Tidak | `false` | Status proses logout |
| `onClickLogout` | `() => void` | Tidak | - | Handler klik logout |
| `onNavigateHandler` | `(link: string, deepLevel: number) => void` | Ya | - | Fungsi navigasi |

## Type Definitions

### MenuItem
```typescript
interface MenuItem {
  name: string;
  link: string;
  icon?: string;
  parent: string | null;
  sequence?: number;
  features?: FeatureItem[];
}

interface FeatureItem {
  code: string;
  enabled: boolean;
}
```

## Contoh Penggunaan

### Basic Usage
```tsx
import { AppSidebar } from "@/components/custom/app-sidebar";
import { useAppSelector } from "@/hooks";

function App() {
  const { menuItems } = useAppSelector((state) => state.sidebar);
  const { activeMenus } = useAppSelector((state) => state.navigation);

  const handleNavigate = (link: string, deepLevel: number) => {
    // Handle navigation logic
    console.log(`Navigate to ${link} at level ${deepLevel}`);
  };

  return (
    <AppSidebar
      items={menuItems}
      activeMenus={activeMenus}
      onNavigateHandler={handleNavigate}
    />
  );
}
```

### Dengan Loading State
```tsx
<AppSidebar
  items={[]}
  activeMenus={[]}
  isLoading={true}
  onNavigateHandler={handleNavigate}
/>
```

### Dengan Logout Handler
```tsx
<AppSidebar
  items={menuItems}
  activeMenus={activeMenus}
  onNavigateHandler={handleNavigate}
  onClickLogout={handleLogout}
  isLogOuting={isLoggingOut}
/>
```

## Struktur Menu

### Format Menu Items
```typescript
const menuItems: MenuItem[] = [
  // Level 1 - Dashboard
  {
    name: "Dashboard",
    link: "/dashboard",
    icon: "LayoutDashboard",
    parent: null,
    sequence: 1,
    features: [{ code: "view", enabled: true }]
  },
  // Level 1 - Users dengan submenu
  {
    name: "User Management",
    link: "/users",
    icon: "Users",
    parent: null,
    sequence: 2,
    features: [{ code: "view", enabled: true }]
  },
  // Level 2 - Users List
  {
    name: "All Users",
    link: "/users/list",
    icon: "User",
    parent: "/users",
    sequence: 1,
    features: [{ code: "view", enabled: true }]
  },
  // Level 2 - Roles
  {
    name: "Roles",
    link: "/users/roles",
    icon: "Shield",
    parent: "/users",
    sequence: 2,
    features: [{ code: "view", enabled: true }]
  },
  // Level 3 - Permission (nested deeper)
  {
    name: "Permissions",
    link: "/users/roles/permissions",
    icon: "Key",
    parent: "/users/roles",
    sequence: 1,
    features: [{ code: "view", enabled: true }]
  }
];
```

### Menu dengan Feature Access Control
```typescript
const menuItems: MenuItem[] = [
  {
    name: "Admin Panel",
    link: "/admin",
    icon: "Shield",
    parent: null,
    sequence: 1,
    features: [
      { code: "view", enabled: true },      // View menu
      { code: "create", enabled: false },   // Cannot create
      { code: "edit", enabled: true },      // Can edit
      { code: "delete", enabled: false }    // Cannot delete
    ]
  }
];
```

## Fitur-Fitur

### 1. Menu Collapsible
Sidebar mendukung menu bertingkat yang dapat dilipat:
- Menu dengan children akan menampilkan ikon chevron
- Klik pada menu utama untuk membuka/menutup submenu
- State terbuka/tutup dipertahankan berdasarkan menu aktif

### 2. Active Menu Highlight
Menu yang aktif akan memiliki:
- Background accent color
- Border kiri berwarna primary
- Ikon dan teks berwarna accent

### 3. Collapse Mode
Saat sidebar dalam mode collapsed (icon only):
- Menampilkan hanya ikon menu
- Tooltip muncul saat hover untuk menampilkan nama menu
- Profil pengguna disembunyikan

### 4. Loading State
Menampilkan skeleton loading saat data menu sedang dimuat:
- 3 skeleton items dengan random width
- Animasi pulse

### 5. User Profile Section
Menampilkan informasi pengguna:
- Nama lengkap
- Role (dengan format uppercase)
- Waktu saat ini (real-time)

## Kustomisasi

### Custom Icon
Gunakan `LazyIcon` untuk memuat ikon secara dinamis:
```tsx
<LazyIcon
  name={item.icon ?? "Circle"}
  size={14}
  color={isActive ? "#fff" : "#C7D2FE"}
/>
```

### Custom Styling untuk Active Menu
```tsx
// Modifikasi className di SidebarMenuButton
isActive
  ? "bg-sidebar-accent border-l-5 border-sidebar-primary/75"
  : "text-sidebar-foreground/60"
```

### Custom Tooltip untuk Collapsed Mode
```tsx
<SidebarMenuButton
  tooltip={item.name}
  // ... props lainnya
>
```

## Best Practices

### 1. Menu Structure
- Gunakan `parent: null` untuk menu level 1
- Gunakan `link` sebagai parent reference untuk child menu
- Atur `sequence` untuk mengontrol urutan menu

### 2. Feature Access Control
Gunakan array `features` untuk mengontrol akses menu:
```tsx
menu.features?.filter((feature: FeatureItem) => 
  feature.code === "view" && feature.enabled
).shift()
```

### 3. Navigation Handler
Implementasikan navigation handler untuk update active menu:
```tsx
const handleNavigate = (link: string, deepLevel: number) => {
  // Update active menu di Redux
  dispatch(setActive({ route: link, deepLevel }));
  // Navigate menggunakan React Router
  navigate(link);
};
```

### 4. Performance
- Gunakan `useMemo` untuk filter menu jika perlu
- Lazy load ikon dengan `LazyIcon`

## Troubleshooting

### Menu Tidak Muncul
- Periksa apakah `items` array memiliki data
- Pastikan setiap menu memiliki `features` dengan code "view" dan enabled true

### Ikon Tidak Tampil
- Pastikan nama ikon sesuai dengan Lucide icon names
- Gunakan `LazyIcon` untuk memastikan ikon dimuat dengan benar

### Active Menu Tidak Terhighlight
- Pastikan `activeMenus` array berisi link yang sesuai dengan depth level
- Periksa nilai `deepLevel` pada handler navigasi

### Collapse Mode Tidak Berfungsi
- Pastikan menggunakan `useSidebar` hook untuk mengakses state sidebar
- Sidebar harus dibungkus dengan `SidebarProvider`

## Referensi

- [Shadcn Sidebar Documentation](https://ui.shadcn.com/docs/components/sidebar)
- [Lucide Icons](https://lucide.dev/icons/)
- [Redux Toolkit](https://redux-toolkit.js.org/)
