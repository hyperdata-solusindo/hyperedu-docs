---
title: "Section Components"
sidebar_label: "Section Components"
---

# Section Components

**Lokasi:** `components/custom/section.tsx`

## Deskripsi

Kumpulan komponen untuk layout halaman dengan struktur konsisten. Section Components menyediakan building blocks untuk membangun halaman dengan header, konten, dan area aksi yang terstruktur dengan baik.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `SectionPage` | Container utama untuk halaman dengan padding konsisten |
| `SectionPageHeader` | Header section dengan layout flex responsif |
| `SectionPageHeaderInfo` | Container untuk informasi judul dan deskripsi |
| `SectionPageHeaderTitle` | Judul utama halaman |
| `SectionPageHeaderDescription` | Deskripsi atau subtitle halaman |
| `SectionPageHeaderActions` | Container untuk tombol aksi |
| `SectionPageContent` | Container untuk konten utama halaman |

---

## SectionPage

Container utama untuk halaman dengan padding konsisten.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes |
| `children` | `React.ReactNode` | Tidak | - | Konten halaman |

### Contoh Penggunaan
```tsx
<SectionPage>
  <h1>Page Content</h1>
</SectionPage>
```

---

## SectionPageHeader

Header section dengan layout flex yang responsif (column pada mobile, row pada desktop).

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes |
| `children` | `React.ReactNode` | Tidak | - | Konten header |

### Contoh Penggunaan
```tsx
<SectionPageHeader>
  <SectionPageHeaderInfo>
    <SectionPageHeaderTitle>Dashboard</SectionPageHeaderTitle>
    <SectionPageHeaderDescription>Overview of your data</SectionPageHeaderDescription>
  </SectionPageHeaderInfo>
  <SectionPageHeaderActions>
    <Button>Export</Button>
  </SectionPageHeaderActions>
</SectionPageHeader>
```

---

## SectionPageHeaderInfo

Container untuk informasi judul dan deskripsi.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes |
| `children` | `React.ReactNode` | Tidak | - | Konten info |

---

## SectionPageHeaderTitle

Judul utama halaman dengan styling bold dan ukuran besar.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes |
| `children` | `React.ReactNode` | Tidak | - | Teks judul |

### Contoh Penggunaan
```tsx
<SectionPageHeaderTitle>User Management</SectionPageHeaderTitle>
```

---

## SectionPageHeaderDescription

Deskripsi atau subtitle halaman dengan styling lebih kecil dan warna muted.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes |
| `children` | `React.ReactNode` | Tidak | - | Teks deskripsi |

### Contoh Penggunaan
```tsx
<SectionPageHeaderDescription>
  Manage your users and their permissions
</SectionPageHeaderDescription>
```

---

## SectionPageHeaderActions

Container untuk tombol aksi yang berada di sebelah kanan header.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes |
| `children` | `React.ReactNode` | Tidak | - | Tombol aksi |

---

## SectionPageContent

Container untuk konten utama halaman.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `className` | `string` | Tidak | - | Custom CSS classes |
| `children` | `React.ReactNode` | Tidak | - | Konten utama |

---

## Contoh Penggunaan

### Basic Page Layout
```tsx
import {
  SectionPage,
  SectionPageContent,
  SectionPageHeader,
  SectionPageHeaderActions,
  SectionPageHeaderDescription,
  SectionPageHeaderInfo,
  SectionPageHeaderTitle,
} from "@/components/custom/section";

function DashboardPage() {
  return (
    <SectionPage>
      <SectionPageHeader>
        <SectionPageHeaderInfo>
          <SectionPageHeaderTitle>Dashboard</SectionPageHeaderTitle>
          <SectionPageHeaderDescription>
            Welcome back, John! Here's what's happening with your account.
          </SectionPageHeaderDescription>
        </SectionPageHeaderInfo>
        <SectionPageHeaderActions>
          <Button variant="outline">
            <DownloadIcon className="mr-2 h-4 w-4" />
            Export
          </Button>
          <Button>
            <RefreshCwIcon className="mr-2 h-4 w-4" />
            Refresh
          </Button>
        </SectionPageHeaderActions>
      </SectionPageHeader>
      
      <SectionPageContent>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
          {/* Stats cards */}
        </div>
        
        <div className="mt-6">
          {/* Charts and tables */}
        </div>
      </SectionPageContent>
    </SectionPage>
  );
}
```

### Dengan Breadcrumb
```tsx
function UsersPage() {
  return (
    <SectionPage>
      <div className="mb-4">
        <Breadcrumb>
          <BreadcrumbList>
            <BreadcrumbItem>
              <BreadcrumbLink href="/">Home</BreadcrumbLink>
            </BreadcrumbItem>
            <BreadcrumbSeparator />
            <BreadcrumbItem>
              <BreadcrumbPage>Users</BreadcrumbPage>
            </BreadcrumbItem>
          </BreadcrumbList>
        </Breadcrumb>
      </div>
      
      <SectionPageHeader>
        <SectionPageHeaderInfo>
          <SectionPageHeaderTitle>Users</SectionPageHeaderTitle>
          <SectionPageHeaderDescription>
            Manage user accounts and permissions
          </SectionPageHeaderDescription>
        </SectionPageHeaderInfo>
        <SectionPageHeaderActions>
          <Button>
            <PlusIcon className="mr-2 h-4 w-4" />
            Add User
          </Button>
        </SectionPageHeaderActions>
      </SectionPageHeader>
      
      <SectionPageContent>
        <UserTable />
      </SectionPageContent>
    </SectionPage>
  );
}
```

### Dengan Search dan Filter
```tsx
function ProductsPage() {
  const [search, setSearch] = useState("");
  const [filter, setFilter] = useState("all");

  return (
    <SectionPage>
      <SectionPageHeader>
        <SectionPageHeaderInfo>
          <SectionPageHeaderTitle>Products</SectionPageHeaderTitle>
          <SectionPageHeaderDescription>
            Manage your product catalog
          </SectionPageHeaderDescription>
        </SectionPageHeaderInfo>
        <SectionPageHeaderActions>
          <InputGroup className="w-64">
            <InputGroupAddon align="inline-start">
              <SearchIcon className="h-4 w-4" />
            </InputGroupAddon>
            <InputGroupInput
              placeholder="Search products..."
              value={search}
              onChange={(e) => setSearch(e.target.value)}
            />
          </InputGroup>
          
          <Select value={filter} onValueChange={setFilter}>
            <SelectTrigger className="w-32">
              <SelectValue placeholder="Filter" />
            </SelectTrigger>
            <SelectContent>
              <SelectItem value="all">All</SelectItem>
              <SelectItem value="active">Active</SelectItem>
              <SelectItem value="inactive">Inactive</SelectItem>
            </SelectContent>
          </Select>
          
          <Button>
            <PlusIcon className="mr-2 h-4 w-4" />
            Add Product
          </Button>
        </SectionPageHeaderActions>
      </SectionPageHeader>
      
      <SectionPageContent>
        <ProductTable search={search} filter={filter} />
      </SectionPageContent>
    </SectionPage>
  );
}
```

### Dengan Tabs
```tsx
function SettingsPage() {
  const [activeTab, setActiveTab] = useState("general");

  return (
    <SectionPage>
      <SectionPageHeader>
        <SectionPageHeaderInfo>
          <SectionPageHeaderTitle>Settings</SectionPageHeaderTitle>
          <SectionPageHeaderDescription>
            Configure your application settings
          </SectionPageHeaderDescription>
        </SectionPageHeaderInfo>
      </SectionPageHeader>
      
      <div className="mb-6">
        <Tabs value={activeTab} onValueChange={setActiveTab}>
          <TabsList>
            <TabsTrigger value="general">General</TabsTrigger>
            <TabsTrigger value="security">Security</TabsTrigger>
            <TabsTrigger value="notifications">Notifications</TabsTrigger>
          </TabsList>
        </Tabs>
      </div>
      
      <SectionPageContent>
        {activeTab === "general" && <GeneralSettings />}
        {activeTab === "security" && <SecuritySettings />}
        {activeTab === "notifications" && <NotificationSettings />}
      </SectionPageContent>
    </SectionPage>
  );
}
```

### Dengan Loading State
```tsx
function DataPage() {
  const { data, isLoading } = useQuery({ queryKey: ["data"], queryFn: fetchData });

  return (
    <SectionPage>
      <SectionPageHeader>
        <SectionPageHeaderInfo>
          <SectionPageHeaderTitle>Data Analysis</SectionPageHeaderTitle>
          <SectionPageHeaderDescription>
            View and analyze your data
          </SectionPageHeaderDescription>
        </SectionPageHeaderInfo>
        <SectionPageHeaderActions>
          <Button variant="outline" disabled={isLoading}>
            <DownloadIcon className="mr-2 h-4 w-4" />
            Export
          </Button>
        </SectionPageHeaderActions>
      </SectionPageHeader>
      
      <SectionPageContent>
        {isLoading ? (
          <div className="space-y-4">
            <Skeleton className="h-32 w-full" />
            <Skeleton className="h-64 w-full" />
          </div>
        ) : (
          <DataVisualization data={data} />
        )}
      </SectionPageContent>
    </SectionPage>
  );
}
```

### Custom Styling
```tsx
<SectionPage className="bg-muted/30">
  <SectionPageHeader className="border-b pb-4">
    <SectionPageHeaderInfo>
      <SectionPageHeaderTitle className="text-3xl">
        Custom Title
      </SectionPageHeaderTitle>
      <SectionPageHeaderDescription className="text-base">
        Custom description with larger text
      </SectionPageHeaderDescription>
    </SectionPageHeaderInfo>
    <SectionPageHeaderActions>
      <Button size="lg">Large Action</Button>
    </SectionPageHeaderActions>
  </SectionPageHeader>
  
  <SectionPageContent className="mt-8">
    {/* content */}
  </SectionPageContent>
</SectionPage>
```

## Best Practices

### 1. Gunakan Struktur Konsisten
Gunakan Section Components untuk semua halaman agar layout konsisten:
```tsx
<SectionPage>
  <SectionPageHeader>...</SectionPageHeader>
  <SectionPageContent>...</SectionPageContent>
</SectionPage>
```

### 2. Header yang Informatif
Pastikan header memberikan konteks yang jelas tentang halaman:
```tsx
<SectionPageHeaderTitle>User Management</SectionPageHeaderTitle>
<SectionPageHeaderDescription>
  Manage user accounts, roles, and permissions
</SectionPageHeaderDescription>
```

### 3. Actions yang Relevan
Tempatkan tombol aksi utama di SectionPageHeaderActions:
```tsx
<SectionPageHeaderActions>
  <Button>Primary Action</Button>
  <Button variant="outline">Secondary Action</Button>
</SectionPageHeaderActions>
```

### 4. Responsive Design
Section Components sudah responsif secara default dengan layout flex yang berubah di mobile.

## Referensi

- [Tailwind CSS Flexbox](https://tailwindcss.com/docs/flex)
- [Shadcn Layout Components](https://ui.shadcn.com/docs/components/layout)
- [Responsive Design Best Practices](https://web.dev/responsive-web-design-basics/)
