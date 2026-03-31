---
title: "Sidebar"
sidebar_label: "Sidebar"
---

# Sidebar

**Lokasi:** `components/ui/sidebar.tsx`  
**Dokumentasi Resmi:** [Shadcn Sidebar](https://ui.shadcn.com/docs/components/sidebar)

## Deskripsi

Komponen sidebar lengkap dengan support collapsible, responsive, dan mobile view. Sidebar digunakan untuk navigasi utama aplikasi dengan menu yang dapat dilipat untuk menghemat ruang layar, serta mendukung tampilan mobile yang diimplementasikan sebagai sheet.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `SidebarProvider` | Provider untuk state sidebar (open/close, mobile, dll) |
| `Sidebar` | Container utama sidebar |
| `SidebarTrigger` | Tombol untuk toggle sidebar |
| `SidebarHeader` | Header section sidebar |
| `SidebarContent` | Konten scrollable sidebar |
| `SidebarFooter` | Footer section sidebar |
| `SidebarMenu` | Container menu |
| `SidebarMenuItem` | Item menu |
| `SidebarMenuButton` | Tombol menu |
| `SidebarMenuSub` | Submenu container |
| `SidebarMenuSubButton` | Tombol submenu |
| `SidebarRail` | Rail untuk toggle sidebar (hover area) |
| `SidebarInset` | Container konten utama yang mengikuti sidebar |
| `SidebarGroup` | Group menu items |
| `SidebarGroupLabel` | Label untuk group |
| `SidebarGroupContent` | Konten group |
| `SidebarSeparator` | Separator antar section |

## Hooks

| Hook | Deskripsi |
|------|-----------|
| `useSidebar` | Hook untuk mengakses state sidebar (open, toggle, isMobile, dll) |

## Props

### SidebarProvider
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `defaultOpen` | `boolean` | `true` | Status awal sidebar (uncontrolled) |
| `open` | `boolean` | - | Status sidebar (controlled) |
| `onOpenChange` | `(open: boolean) => void` | - | Callback ketika sidebar dibuka/ditutup |

### Sidebar
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `side` | `"left" \| "right"` | `"left"` | Posisi sidebar |
| `variant` | `"sidebar" \| "floating" \| "inset"` | `"sidebar"` | Varian visual sidebar |
| `collapsible` | `"offcanvas" \| "icon" \| "none"` | `"offcanvas"` | Mode collapse sidebar |

## Varian Sidebar

| Varian | Deskripsi |
|--------|-----------|
| **sidebar** | Sidebar standar dengan background solid |
| **floating** | Sidebar mengambang dengan shadow dan rounded |
| **inset** | Sidebar berada di dalam konten utama dengan margin |

## Mode Collapsible

| Mode | Deskripsi |
|------|-----------|
| **offcanvas** | Sidebar disembunyikan sepenuhnya, muncul saat di-toggle |
| **icon** | Sidebar collapse menjadi icon-only |
| **none** | Sidebar tidak dapat di-collapse |

## Contoh Penggunaan

### Basic Sidebar Setup
```tsx
import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarHeader,
  SidebarMenu,
  SidebarMenuButton,
  SidebarMenuItem,
  SidebarProvider,
  SidebarTrigger,
  SidebarInset,
} from "@/components/ui/sidebar";

function App() {
  return (
    <SidebarProvider defaultOpen={true}>
      <Sidebar>
        <SidebarHeader>
          <div className="px-4 py-2 font-bold">Logo</div>
        </SidebarHeader>
        <SidebarContent>
          <SidebarMenu>
            <SidebarMenuItem>
              <SidebarMenuButton asChild>
                <a href="/dashboard">
                  <HomeIcon className="h-4 w-4" />
                  <span>Dashboard</span>
                </a>
              </SidebarMenuButton>
            </SidebarMenuItem>
            <SidebarMenuItem>
              <SidebarMenuButton asChild>
                <a href="/users">
                  <UsersIcon className="h-4 w-4" />
                  <span>Users</span>
                </a>
              </SidebarMenuButton>
            </SidebarMenuItem>
            <SidebarMenuItem>
              <SidebarMenuButton asChild>
                <a href="/settings">
                  <SettingsIcon className="h-4 w-4" />
                  <span>Settings</span>
                </a>
              </SidebarMenuButton>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarContent>
        <SidebarFooter>
          <div className="px-4 py-2 text-sm text-muted-foreground">
            Logged in as John Doe
          </div>
        </SidebarFooter>
      </Sidebar>
      <SidebarInset>
        <header className="flex h-12 items-center gap-2 border-b px-4">
          <SidebarTrigger />
          <h1 className="font-semibold">Page Title</h1>
        </header>
        <main className="p-4">
          <p>Main content goes here</p>
        </main>
      </SidebarInset>
    </SidebarProvider>
  );
}
```

### Sidebar dengan Menu Submenu
```tsx
<SidebarMenu>
  <SidebarMenuItem>
    <SidebarMenuButton>
      <UsersIcon className="h-4 w-4" />
      <span>Users</span>
    </SidebarMenuButton>
  </SidebarMenuItem>
  <SidebarMenuItem>
    <SidebarMenuButton>
      <SettingsIcon className="h-4 w-4" />
      <span>Settings</span>
    </SidebarMenuButton>
    <SidebarMenuSub>
      <SidebarMenuSubButton asChild>
        <a href="/settings/profile">Profile</a>
      </SidebarMenuSubButton>
      <SidebarMenuSubButton asChild>
        <a href="/settings/account">Account</a>
      </SidebarMenuSubButton>
      <SidebarMenuSubButton asChild>
        <a href="/settings/security">Security</a>
      </SidebarMenuSubButton>
    </SidebarMenuSub>
  </SidebarMenuItem>
</SidebarMenu>
```

### Sidebar dengan Group Menu
```tsx
<SidebarGroup>
  <SidebarGroupLabel>Main Navigation</SidebarGroupLabel>
  <SidebarGroupContent>
    <SidebarMenu>
      <SidebarMenuItem>
        <SidebarMenuButton asChild>
          <a href="/dashboard">Dashboard</a>
        </SidebarMenuButton>
      </SidebarMenuItem>
      <SidebarMenuItem>
        <SidebarMenuButton asChild>
          <a href="/analytics">Analytics</a>
        </SidebarMenuButton>
      </SidebarMenuItem>
    </SidebarMenu>
  </SidebarGroupContent>
</SidebarGroup>

<SidebarGroup>
  <SidebarGroupLabel>Management</SidebarGroupLabel>
  <SidebarGroupContent>
    <SidebarMenu>
      <SidebarMenuItem>
        <SidebarMenuButton asChild>
          <a href="/users">Users</a>
        </SidebarMenuButton>
      </SidebarMenuItem>
      <SidebarMenuItem>
        <SidebarMenuButton asChild>
          <a href="/roles">Roles</a>
        </SidebarMenuButton>
      </SidebarMenuItem>
    </SidebarMenu>
  </SidebarGroupContent>
</SidebarGroup>
```

### Sidebar dengan Active State
```tsx
function SidebarWithActiveState() {
  const location = useLocation();

  const isActive = (path: string) => location.pathname === path;

  return (
    <SidebarMenu>
      <SidebarMenuItem>
        <SidebarMenuButton asChild isActive={isActive("/dashboard")}>
          <a href="/dashboard">Dashboard</a>
        </SidebarMenuButton>
      </SidebarMenuItem>
      <SidebarMenuItem>
        <SidebarMenuButton asChild isActive={isActive("/users")}>
          <a href="/users">Users</a>
        </SidebarMenuButton>
      </SidebarMenuItem>
    </SidebarMenu>
  );
}
```

### Sidebar dengan Tooltip (Collapsed Mode)
```tsx
<SidebarMenu>
  <SidebarMenuItem>
    <SidebarMenuButton asChild tooltip="Dashboard">
      <a href="/dashboard">
        <HomeIcon className="h-4 w-4" />
        <span>Dashboard</span>
      </a>
    </SidebarMenuButton>
  </SidebarMenuItem>
</SidebarMenu>
```

### Sidebar dengan Custom Ikon
```tsx
<SidebarMenuButton asChild>
  <a href="/dashboard">
    <LazyIcon name="Home" size={16} />
    <span>Dashboard</span>
  </a>
</SidebarMenuButton>
```

### Sidebar dengan Variant Floating
```tsx
<SidebarProvider>
  <Sidebar variant="floating" collapsible="icon">
    <SidebarContent>
      {/* menu items */}
    </SidebarContent>
  </Sidebar>
  <SidebarInset>
    {/* main content */}
  </SidebarInset>
</SidebarProvider>
```

### Sidebar dengan Variant Inset
```tsx
<SidebarProvider>
  <Sidebar variant="inset" collapsible="icon">
    <SidebarContent>
      {/* menu items */}
    </SidebarContent>
  </Sidebar>
  <SidebarInset>
    {/* main content with inset spacing */}
  </SidebarInset>
</SidebarProvider>
```

### Sidebar dengan Collapsible Mode Icon
```tsx
<SidebarProvider defaultOpen={true}>
  <Sidebar collapsible="icon">
    <SidebarHeader>
      <div className="px-4 py-2 font-bold">Logo</div>
    </SidebarHeader>
    <SidebarContent>
      {/* menu items - akan collapse menjadi icon only */}
    </SidebarContent>
  </Sidebar>
  <SidebarInset>
    <SidebarTrigger />
    {/* main content */}
  </SidebarInset>
</SidebarProvider>
```

### Sidebar dengan Collapsible Mode Offcanvas
```tsx
<SidebarProvider defaultOpen={false}>
  <Sidebar collapsible="offcanvas">
    <SidebarContent>
      {/* menu items - akan disembunyikan sepenuhnya */}
    </SidebarContent>
  </Sidebar>
  <SidebarInset>
    <SidebarTrigger />
    {/* main content */}
  </SidebarInset>
</SidebarProvider>
```

### Sidebar dengan Avatar di Footer
```tsx
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

<SidebarFooter>
  <div className="flex items-center gap-3 px-4 py-2">
    <Avatar className="h-8 w-8">
      <AvatarImage src="/avatar.jpg" />
      <AvatarFallback>JD</AvatarFallback>
    </Avatar>
    <div className="flex-1">
      <p className="text-sm font-medium">John Doe</p>
      <p className="text-xs text-muted-foreground">john@example.com</p>
    </div>
  </div>
</SidebarFooter>
```

### Sidebar dengan Badge Notifikasi
```tsx
<SidebarMenuItem>
  <SidebarMenuButton asChild>
    <a href="/notifications">
      <BellIcon className="h-4 w-4" />
      <span>Notifications</span>
      <SidebarMenuBadge>3</SidebarMenuBadge>
    </a>
  </SidebarMenuButton>
</SidebarMenuItem>
```

### Sidebar dengan Skeleton Loading
```tsx
<SidebarMenu>
  {Array.from({ length: 3 }).map((_, i) => (
    <SidebarMenuItem key={i}>
      <SidebarMenuSkeleton showIcon />
    </SidebarMenuItem>
  ))}
</SidebarMenu>
```

## Hooks Usage

### Menggunakan useSidebar
```tsx
import { useSidebar } from "@/components/ui/sidebar";

function CustomSidebarTrigger() {
  const { toggleSidebar, open, isMobile, state } = useSidebar();

  return (
    <div>
      <button onClick={toggleSidebar}>
        {open ? "Close" : "Open"} Sidebar
      </button>
      <p>State: {state}</p>
      <p>Mobile: {isMobile ? "Yes" : "No"}</p>
    </div>
  );
}
```

## Kustomisasi

### Custom Width
```tsx
<SidebarProvider
  style={
    {
      "--sidebar-width": "280px",
      "--sidebar-width-icon": "4rem",
    } as React.CSSProperties
  }
>
  <Sidebar>...</Sidebar>
</SidebarProvider>
```

### Custom Styling
```tsx
<Sidebar className="bg-primary text-primary-foreground">
  <SidebarContent className="[&::-webkit-scrollbar]:w-1">
    {/* content */}
  </SidebarContent>
</Sidebar>
```

## Best Practices

### 1. Gunakan SidebarProvider di Root Layout
```tsx
// Root layout
<SidebarProvider>
  <App />
</SidebarProvider>
```

### 2. Kombinasikan dengan SidebarInset untuk Konten Utama
```tsx
<SidebarProvider>
  <Sidebar>...</Sidebar>
  <SidebarInset>
    <header>...</header>
    <main>...</main>
  </SidebarInset>
</SidebarProvider>
```

### 3. Gunakan Tooltip untuk Collapsed Mode
```tsx
<SidebarMenuButton tooltip="Dashboard">
  <HomeIcon />
  <span>Dashboard</span>
</SidebarMenuButton>
```

### 4. Responsive Design
Sidebar secara otomatis berubah menjadi sheet pada mobile view.

## Troubleshooting

### Sidebar Tidak Muncul
- Pastikan komponen dibungkus dengan `SidebarProvider`
- Periksa `collapsible` mode tidak `"none"` jika ingin bisa di-toggle

### SidebarInset Tidak Menyesuaikan
- Pastikan `SidebarInset` berada di luar `Sidebar` component

### Tooltip Tidak Muncul di Collapsed Mode
- Pastikan prop `tooltip` diisi pada `SidebarMenuButton`

## Referensi

- [Shadcn Sidebar Documentation](https://ui.shadcn.com/docs/components/sidebar)
- [Radix UI Collapsible](https://www.radix-ui.com/primitives/docs/components/collapsible)
