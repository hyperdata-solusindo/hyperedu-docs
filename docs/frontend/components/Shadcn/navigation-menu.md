---
title: "Navigation Menu"
sidebar_label: "Navigation Menu"
---

# Navigation Menu

**Lokasi:** `components/ui/navigation-menu.tsx`  
**Dokumentasi Resmi:** [Shadcn Navigation Menu](https://ui.shadcn.com/docs/components/navigation-menu)

## Deskripsi

Komponen menu navigasi dengan dropdown, dibangun di atas Radix UI Navigation Menu. Navigation Menu digunakan untuk menampilkan navigasi utama aplikasi dengan dukungan submenu yang dapat muncul saat di-hover atau diklik.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `NavigationMenu` | Container utama navigation menu |
| `NavigationMenuList` | Daftar menu items |
| `NavigationMenuItem` | Item menu individual |
| `NavigationMenuTrigger` | Trigger untuk dropdown menu |
| `NavigationMenuContent` | Konten dropdown |
| `NavigationMenuLink` | Link dalam menu |
| `NavigationMenuIndicator` | Indikator visual untuk menu aktif |
| `NavigationMenuViewport` | Viewport untuk konten dropdown |

## Contoh Penggunaan

### Basic Navigation Menu
```tsx
import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
} from "@/components/ui/navigation-menu";

<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Products</NavigationMenuTrigger>
      <NavigationMenuContent>
        <ul className="grid w-48 gap-1 p-2">
          <li>
            <NavigationMenuLink href="/products/electronics">
              Electronics
            </NavigationMenuLink>
          </li>
          <li>
            <NavigationMenuLink href="/products/clothing">
              Clothing
            </NavigationMenuLink>
          </li>
          <li>
            <NavigationMenuLink href="/products/books">
              Books
            </NavigationMenuLink>
          </li>
        </ul>
      </NavigationMenuContent>
    </NavigationMenuItem>
    <NavigationMenuItem>
      <NavigationMenuLink href="/about">About</NavigationMenuLink>
    </NavigationMenuItem>
    <NavigationMenuItem>
      <NavigationMenuLink href="/contact">Contact</NavigationMenuLink>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

### Navigation Menu dengan Grid Layout
```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Resources</NavigationMenuTrigger>
      <NavigationMenuContent>
        <div className="grid w-[400px] gap-3 p-4 md:w-[500px] md:grid-cols-2">
          <div>
            <h4 className="mb-2 text-sm font-medium">Documentation</h4>
            <ul className="space-y-1">
              <li>
                <NavigationMenuLink href="/docs/getting-started">
                  Getting Started
                </NavigationMenuLink>
              </li>
              <li>
                <NavigationMenuLink href="/docs/api">
                  API Reference
                </NavigationMenuLink>
              </li>
            </ul>
          </div>
          <div>
            <h4 className="mb-2 text-sm font-medium">Community</h4>
            <ul className="space-y-1">
              <li>
                <NavigationMenuLink href="/community/discord">
                  Discord
                </NavigationMenuLink>
              </li>
              <li>
                <NavigationMenuLink href="/community/github">
                  GitHub
                </NavigationMenuLink>
              </li>
            </ul>
          </div>
        </div>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

### Navigation Menu dengan Card Style
```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Solutions</NavigationMenuTrigger>
      <NavigationMenuContent>
        <div className="w-[500px] p-4">
          <div className="grid gap-3 md:grid-cols-2">
            <NavigationMenuLink 
              href="/solutions/enterprise"
              className="block p-3 rounded-lg hover:bg-muted transition-colors"
            >
              <div className="font-medium">Enterprise</div>
              <p className="text-sm text-muted-foreground">
                For large organizations with complex needs
              </p>
            </NavigationMenuLink>
            <NavigationMenuLink 
              href="/solutions/startup"
              className="block p-3 rounded-lg hover:bg-muted transition-colors"
            >
              <div className="font-medium">Startup</div>
              <p className="text-sm text-muted-foreground">
                Perfect for growing businesses
              </p>
            </NavigationMenuLink>
            <NavigationMenuLink 
              href="/solutions/education"
              className="block p-3 rounded-lg hover:bg-muted transition-colors"
            >
              <div className="font-medium">Education</div>
              <p className="text-sm text-muted-foreground">
                Designed for schools and universities
              </p>
            </NavigationMenuLink>
          </div>
        </div>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

### Navigation Menu dengan Ikon
```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>
        <HomeIcon className="mr-2 h-4 w-4" />
        Home
      </NavigationMenuTrigger>
      <NavigationMenuContent>
        <ul className="w-48 p-2">
          <li>
            <NavigationMenuLink href="/dashboard" className="flex items-center gap-2">
              <LayoutDashboardIcon className="h-4 w-4" />
              Dashboard
            </NavigationMenuLink>
          </li>
          <li>
            <NavigationMenuLink href="/analytics" className="flex items-center gap-2">
              <TrendingUpIcon className="h-4 w-4" />
              Analytics
            </NavigationMenuLink>
          </li>
        </ul>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

### Navigation Menu dengan Separator
```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Account</NavigationMenuTrigger>
      <NavigationMenuContent>
        <ul className="w-48 p-2">
          <li>
            <NavigationMenuLink href="/profile">Profile</NavigationMenuLink>
          </li>
          <li>
            <NavigationMenuLink href="/settings">Settings</NavigationMenuLink>
          </li>
          <li className="h-px bg-border my-1" />
          <li>
            <NavigationMenuLink href="/logout" className="text-red-600">
              Logout
            </NavigationMenuLink>
          </li>
        </ul>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

### Responsive Navigation Menu (Mobile)
```tsx
function ResponsiveNavigation() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div className="relative">
      {/* Desktop Menu */}
      <div className="hidden md:block">
        <NavigationMenu>
          <NavigationMenuList>
            <NavigationMenuItem>
              <NavigationMenuLink href="/">Home</NavigationMenuLink>
            </NavigationMenuItem>
            <NavigationMenuItem>
              <NavigationMenuTrigger>Products</NavigationMenuTrigger>
              <NavigationMenuContent>
                {/* menu content */}
              </NavigationMenuContent>
            </NavigationMenuItem>
          </NavigationMenuList>
        </NavigationMenu>
      </div>

      {/* Mobile Menu */}
      <div className="md:hidden">
        <Button variant="ghost" size="icon" onClick={() => setIsOpen(!isOpen)}>
          <MenuIcon className="h-5 w-5" />
        </Button>
        {isOpen && (
          <div className="absolute top-full left-0 right-0 mt-2 bg-background border rounded-lg shadow-lg p-4">
            <div className="space-y-2">
              <a href="/" className="block py-2">Home</a>
              <a href="/products" className="block py-2">Products</a>
              <a href="/about" className="block py-2">About</a>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
```

### Navigation Menu dengan Active State
```tsx
function ActiveNavigation() {
  const pathname = usePathname();

  const isActive = (path: string) => pathname === path;

  return (
    <NavigationMenu>
      <NavigationMenuList>
        <NavigationMenuItem>
          <NavigationMenuLink 
            href="/"
            className={isActive("/") ? "bg-muted" : ""}
          >
            Home
          </NavigationMenuLink>
        </NavigationMenuItem>
        <NavigationMenuItem>
          <NavigationMenuLink 
            href="/about"
            className={isActive("/about") ? "bg-muted" : ""}
          >
            About
          </NavigationMenuLink>
        </NavigationMenuItem>
      </NavigationMenuList>
    </NavigationMenu>
  );
}
```

### Navigation Menu dengan Badge
```tsx
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>
        Notifications
        <Badge variant="destructive" className="ml-2 px-1.5 py-0 text-xs">
          3
        </Badge>
      </NavigationMenuTrigger>
      <NavigationMenuContent>
        <div className="w-80 p-2">
          <div className="font-medium px-2 py-1">Recent Notifications</div>
          <div className="space-y-1">
            <NavigationMenuLink href="/notifications/1" className="block px-2 py-1 text-sm">
              New message from John
            </NavigationMenuLink>
            <NavigationMenuLink href="/notifications/2" className="block px-2 py-1 text-sm">
              Your order has been shipped
            </NavigationMenuLink>
          </div>
        </div>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

## Kustomisasi

### Custom Styling
```tsx
<NavigationMenu className="bg-primary text-primary-foreground">
  <NavigationMenuList>
    {/* menu items */}
  </NavigationMenuList>
</NavigationMenu>
```

### Custom Trigger
```tsx
<NavigationMenuTrigger className="data-[state=open]:bg-accent">
  Custom Trigger
</NavigationMenuTrigger>
```

### Custom Content
```tsx
<NavigationMenuContent className="border-2 border-primary rounded-lg">
  {/* content */}
</NavigationMenuContent>
```

## Best Practices

### 1. Gunakan untuk Navigasi Utama
Navigation Menu ideal untuk navigasi utama aplikasi yang memiliki hierarki.

### 2. Responsive Design
Pastikan menu responsive untuk tampilan mobile:
```tsx
// Sembunyikan beberapa item di mobile
<div className="hidden md:flex">
  <NavigationMenu>...</NavigationMenu>
</div>
```

### 3. Accessibility
- Gunakan `aria-label` untuk menu
- Pastikan navigasi dengan keyboard berfungsi

### 4. Performance
Navigation Menu menggunakan viewport yang di-render di root untuk performa optimal.

## Troubleshooting

### Dropdown Tidak Muncul
- Pastikan `NavigationMenuContent` di-render dengan benar
- Periksa z-index tidak tertimpa

### Menu Tidak Responsif di Mobile
- Implementasikan mobile menu terpisah menggunakan Sheet atau Dropdown

## Referensi

- [Shadcn Navigation Menu Documentation](https://ui.shadcn.com/docs/components/navigation-menu)
- [Radix UI Navigation Menu](https://www.radix-ui.com/primitives/docs/components/navigation-menu)
- [WAI-ARIA Menu Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu/)
