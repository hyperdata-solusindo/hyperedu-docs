---
title: "ProtectedAccess"
sidebar_label: "ProtectedAccess"
---

# ProtectedAccess

**Lokasi:** `components/custom/protected-access.tsx`

## Deskripsi

Komponen untuk mengontrol akses berdasarkan fitur dan izin pengguna. ProtectedAccess digunakan untuk melindungi komponen atau halaman agar hanya dapat diakses oleh pengguna yang memiliki izin tertentu berdasarkan fitur yang diaktifkan.

## Fitur

- Kontrol akses berbasis fitur (feature-based)
- Dukungan untuk halaman penuh atau komponen individual
- Integrasi dengan Redux untuk data menu dan fitur
- Loading state untuk menunggu data menu
- Redirect ke halaman unauthorized jika tidak memiliki akses

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `route` | `string` | Ya | - | Route yang diakses (digunakan untuk mencari menu) |
| `feature` | `string` | Ya | - | Kode fitur yang diperlukan (contoh: "view", "create", "edit", "delete") |
| `isPage` | `boolean` | Tidak | `false` | Apakah ini halaman (menampilkan unauthorized page jika true) |
| `children` | `React.ReactNode` | Tidak | - | Konten yang dilindungi |

## Contoh Penggunaan

### Melindungi Komponen
```tsx
import { ProtectedAccess } from "@/components/custom/protected-access";

function UserList() {
  return (
    <ProtectedAccess route="/users" feature="view">
      <UserTable />
    </ProtectedAccess>
  );
}
```

### Melindungi Halaman (Menampilkan Unauthorized Page)
```tsx
import { ProtectedAccess } from "@/components/custom/protected-access";

function AdminPage() {
  return (
    <ProtectedAccess route="/admin" feature="view" isPage>
      <AdminDashboard />
    </ProtectedAccess>
  );
}
```

### Dengan Multiple Fitur
```tsx
function UserManagement() {
  return (
    <div>
      <ProtectedAccess route="/users" feature="view">
        <UserTable />
      </ProtectedAccess>
      
      <div className="flex gap-2 mt-4">
        <ProtectedAccess route="/users" feature="create">
          <Button>Add User</Button>
        </ProtectedAccess>
        
        <ProtectedAccess route="/users" feature="edit">
          <Button variant="outline">Edit</Button>
        </ProtectedAccess>
        
        <ProtectedAccess route="/users" feature="delete">
          <Button variant="destructive">Delete</Button>
        </ProtectedAccess>
      </div>
    </div>
  );
}
```

### Dalam Route Definition
```tsx
import { ProtectedAccess } from "@/components/custom/protected-access";

const router = createBrowserRouter([
  {
    path: "/users",
    element: (
      <ProtectedAccess route="/users" feature="view" isPage>
        <UserPage />
      </ProtectedAccess>
    ),
  },
  {
    path: "/users/create",
    element: (
      <ProtectedAccess route="/users" feature="create" isPage>
        <UserCreatePage />
      </ProtectedAccess>
    ),
  },
  {
    path: "/users/edit/:id",
    element: (
      <ProtectedAccess route="/users" feature="edit" isPage>
        <UserEditPage />
      </ProtectedAccess>
    ),
  },
]);
```

### Dalam Layout
```tsx
function DashboardLayout() {
  return (
    <div className="flex">
      <Sidebar />
      <main className="flex-1">
        <ProtectedAccess route="/dashboard" feature="view">
          <Outlet />
        </ProtectedAccess>
      </main>
    </div>
  );
}
```

### Dengan Conditional Rendering
```tsx
function ActionButtons({ user }) {
  return (
    <div className="flex gap-2">
      <ProtectedAccess route="/users" feature="view">
        <Button variant="ghost" size="sm">View</Button>
      </ProtectedAccess>
      
      <ProtectedAccess route="/users" feature="edit">
        <Button variant="outline" size="sm">Edit</Button>
      </ProtectedAccess>
      
      <ProtectedAccess route="/users" feature="delete">
        <Button variant="destructive" size="sm">Delete</Button>
      </ProtectedAccess>
    </div>
  );
}
```

### Dalam Tabel
```tsx
function UserTable({ users }) {
  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead>Name</TableHead>
          <TableHead>Email</TableHead>
          <TableHead>Actions</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {users.map((user) => (
          <TableRow key={user.id}>
            <TableCell>{user.name}</TableCell>
            <TableCell>{user.email}</TableCell>
            <TableCell>
              <div className="flex gap-2">
                <ProtectedAccess route="/users" feature="edit">
                  <Button variant="ghost" size="icon">
                    <EditIcon className="h-4 w-4" />
                  </Button>
                </ProtectedAccess>
                
                <ProtectedAccess route="/users" feature="delete">
                  <Button variant="ghost" size="icon">
                    <TrashIcon className="h-4 w-4" />
                  </Button>
                </ProtectedAccess>
              </div>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

### Dengan Custom Unauthorized Page
```tsx
// default: menampilkan <></> untuk komponen, atau <h1>Unauthorized Page</h1> untuk halaman
<ProtectedAccess route="/admin" feature="view" isPage>
  <AdminPanel />
</ProtectedAccess>

// Untuk komponen, jika tidak memiliki akses, children tidak di-render
<ProtectedAccess route="/admin" feature="view">
  <SecretComponent />
</ProtectedAccess>
```

## Cara Kerja

1. Komponen mengambil data menu dari Redux state (`state.sidebar.menus`)
2. Mencari menu dengan `link` yang sesuai dengan prop `route`
3. Memeriksa apakah menu memiliki feature dengan `code` sesuai prop `feature` dan `enabled: true`
4. Jika memiliki akses, render `children`
5. Jika tidak memiliki akses:
   - Untuk komponen (`isPage: false`): render `null`
   - Untuk halaman (`isPage: true`): render halaman unauthorized

## Best Practices

### 1. Definisikan Fitur dengan Jelas
Gunakan kode fitur yang konsisten di seluruh aplikasi:
```typescript
const FEATURES = {
  VIEW: "view",
  CREATE: "create",
  EDIT: "edit",
  DELETE: "delete",
  EXPORT: "export",
  IMPORT: "import",
} as const;
```

### 2. Gunakan untuk Setiap Aksi
Lindungi setiap aksi yang memerlukan izin:
```tsx
<ProtectedAccess route="/users" feature="delete">
  <Button onClick={handleDelete}>Delete</Button>
</ProtectedAccess>
```

### 3. Jangan Hardcode Route
Gunakan konstanta untuk route:
```tsx
const ROUTES = {
  USERS: "/users",
  USERS_CREATE: "/users/create",
  USERS_EDIT: "/users/edit/:id",
};

<ProtectedAccess route={ROUTES.USERS} feature="view">
  <UserList />
</ProtectedAccess>
```

### 4. Loading State
ProtectedAccess menampilkan spinner saat data menu masih dimuat:
```tsx
// Saat isLoading true, menampilkan spinner
{isLoading && <Loader2 className="w-4 h-4 animate-spin" />}
```

## Troubleshooting

### Komponen Tetap Terlihat Tanpa Akses
- Periksa apakah fitur di menu memiliki `enabled: true`
- Pastikan `feature` code sesuai dengan yang ada di menu

### Loading Terus Menerus
- Periksa apakah data menu sudah di-fetch dengan benar
- Pastikan Redux state `isLoadSidebar` di-set dengan benar

### Route Tidak Ditemukan
- Pastikan menu dengan `link` yang sesuai ada dalam data menu
- Periksa apakah route yang digunakan sama persis dengan link menu

## Referensi

- [Redux Toolkit Documentation](https://redux-toolkit.js.org/)
- [React Router Documentation](https://reactrouter.com/)
- [Role-Based Access Control (RBAC) Pattern](https://en.wikipedia.org/wiki/Role-based_access_control)
