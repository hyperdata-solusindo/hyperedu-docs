---
sidebar_position: 1
---
# Architecture

## Overview

Frontend HyperEdu adalah aplikasi halaman tunggal (SPA) modern yang menyediakan antarmuka pengguna intuitif untuk platform pendidikan. Ini mengikuti praktik terbaik React, menggunakan TypeScript untuk keamanan tipe, dan mengimplementasikan arsitektur komponen yang dapat diskalakan.

### Prinsip Utama

- **Modularitas**: Setiap fitur mandiri dengan routing, komponen, dan logika sendiri.
- **Pemisahan Tanggung Jawab**: UI, logika bisnis, pengambilan data, dan manajemen state dipisahkan dengan jelas.
- **Dapat Digunakan Ulang**: Komponen bersama, hooks, dan utilitas dipusatkan.
- **Skalabilitas**: Mudah menambahkan fitur baru tanpa memengaruhi yang sudah ada.

## Struktur Aplikasi

Aplikasi HyperEdu menggunakan pendekatan **Feature-Based Structure**. Hal ini bertujuan untuk meningkatkan _maintainability_ dan mempermudah isolasi kode.

```
src/
├── assets/             # Global assets (images, fonts, etc.)
├── components/         # Komponen SHARED (Button, Input, Modal) yang dipakai semua fitur
├── config/             # Konfigurasi global (axios instance, env variables)
├── features/           # PUSAT LOGIKA APLIKASI 🚀
│   ├── auth/           # Fitur Autentikasi
│   │   ├── api/        # Endpoint khusus login/register
│   │   ├── components/ # UI khusus auth (LoginForm, RegisterCard)
│   │   ├── hooks/      # useAuth, useSession
│   │   ├── types/      # User interface, AuthResponse
│   │   └── index.ts    # Public API untuk fitur ini (1)
│   ├── courses/        # Fitur Manajemen Kursus
│   │   ├── api/
│   │   ├── components/
│   │   ├── pages/      # Halaman khusus kursus
│   │   └── store/      # Slice Redux/Zustand khusus kursus
│   └── dashboard/      # Fitur Dashboard
├── hooks/              # Custom hooks global (useTheme, useWindowSize)
├── layouts/            # MainLayout, AuthLayout, AdminLayout
├── providers/          # AppProvider (QueryClient, Redux, ThemeProvider)
├── routes/             # Definisi routing (React Router)
├── stores/             # Global store (hanya untuk data yang benar-benar lintas fitur)
├── types/              # Global types/interfaces
└── utils/              # Helper functions (formatDate, currency)
```

:::note "Prinsip Utama"
 Setiap folder di dalam `src/features` harus bersifat mandiri. Jika fitur `courses` membutuhkan komponen dari `auth`, pastikan komponen tersebut diekspor melalui `features/auth/index.ts`.
:::

Setiap fitur mengikuti struktur yang konsisten:

```
features/[nama-fitur]/
├── api/ # Panggilan API dan logika pengambilan data
├── components/ # Komponen khusus fitur
├── hooks/ # Hooks khusus fitur
├── layouts/ # Layout khusus fitur
├── pages/ # Komponen halaman
├── slices/ # Slice Redux khusus fitur
├── stores/ # Konfigurasi store khusus fitur
├── types/ # Tipe khusus fitur
├── index.ts # Ekspor fitur
└── route.ts # Definisi rute
```

## Teknologi Utama

### Framework Frontend
- **React 19.2.0**: React modern dengan fitur konkurensi
- **TypeScript**: Pengembangan dengan tipe aman
- **Vite**: Alat build dan server pengembangan cepat
### Manajemen State
- **Redux Toolkit**: Redux yang disederhanakan dengan slice dan immer
- **React Redux**: Binding resmi React untuk Redux
### Pengambilan Data
- **TanStack React Query**: Sinkronisasi data yang kuat untuk React
- **Axios**: Klien HTTP untuk permintaan API
### UI dan Styling
- **Tailwind CSS**: Framework CSS utility-first
- **Radix UI**: Primitive UI tanpa gaya, dapat diakses
- **Lucide React**: Pustaka ikon yang indah
- **React Hook Form**: Form performan dengan validasi mudah
### Routing
- **React Router DOM**: Routing deklaratif untuk React
### Alat Pengembangan
- **ESLint**: Linting kode
- **Plop**: Generator kode untuk scaffolding konsisten

## Pola Arsitektur

### Arsitektur Berbasis Fitur
Aplikasi diorganisir berdasarkan fitur bisnis daripada lapisan teknis. Setiap fitur berisi semua kode terkait (komponen, logika, tipe, tes) dalam satu direktori.

**Manfaat:**
- Batas yang jelas antara fitur
- Mudah dipahami dan dipelihara
- Potensi deployment independen
- Pengurangan coupling antara fitur

### Komponen Container/Presentasional

Komponen dipisahkan menjadi:
- **Komponen Presentasional**: Komponen UI murni yang fokus pada rendering
- **Komponen Container**: Komponen yang menangani data dan logika

### Pola Custom Hooks

Logika bisnis diekstrak ke dalam custom hooks yang dapat digunakan ulang:
- Hooks API menggunakan TanStack Query
- Hooks form menggunakan React Hook Form
- Hooks state UI

### Slice Redux untuk Manajemen State

State aplikasi global dikelola melalui slice Redux:
- **authSlice**: State autentikasi
- **sidebarSlice**: State sidebar UI
- **themeSlice**: Preferensi tema

### Abstraksi Lapisan API

Panggilan API diabstraksikan melalui:
- Konfigurasi Axios di `config/axios.ts`
- Fungsi API khusus fitur
- TanStack Query untuk caching dan sinkronisasi

## Alur Data

1. **Interaksi Pengguna** → Komponen memicu aksi
2. **Dispatch Aksi** → Aksi Redux atau mutasi Query
3. **Update State** → Reducer Redux atau update cache Query
4. **Re-render UI** → Komponen mencerminkan state baru

## Generasi Kode dengan Plop

Proyek menggunakan Plop.js untuk scaffolding kode yang konsisten. Generator yang tersedia:
- `feature`: Generate modul fitur lengkap
- `page`: Generate halaman baru
- `datatable`: Generate komponen tabel data
- `form-hook`: Generate hooks form
- `crud-hook`: Generate operasi CRUD dengan TanStack Query

Untuk generate kode baru:

```bash
npm run generate
```

## Menambahkan Fitur Baru

1. Gunakan Plop untuk generate skeleton fitur:

```bash
npm run generate feature
```

2. Implementasikan logika fitur di folder yang dihasilkan
3. Tambahkan rute fitur ke `src/route.ts`
4. Ekspor fitur dari `index.ts`-nya

## Praktik Terbaik

### Pengembangan Komponen

- Gunakan komponen fungsional dengan hooks
- Lebih suka komposisi daripada inheritance
- Jaga komponen tetap kecil dan fokus
- Gunakan interface TypeScript untuk props

### Manajemen State

- Gunakan state lokal untuk data khusus komponen
- Gunakan Redux untuk state aplikasi global
- Gunakan TanStack Query untuk state server
- Hindari prop drilling dengan context jika sesuai
### Pengambilan Data

- Gunakan TanStack Query untuk semua panggilan API
- Implementasikan penanganan error yang tepat
- Gunakan update optimis untuk UX yang lebih baik
- Cache data dengan tepat

### Styling  
- Gunakan kelas utility Tailwind
- Ikuti token sistem desain
- Pastikan desain responsif
- Pertahankan standar aksesibilitas

## Pertimbangan Performa

- **Code Splitting**: Fitur dimuat secara lazy
- **Memoization**: Gunakan React.memo, useMemo, useCallback dengan tepat
- **Optimasi Query**: Konfigurasi TanStack Query untuk caching efisien
- **Analisis Bundle**: Monitor ukuran bundle dengan alat build
## Strategi Testing
  
_(Catatan: Detail implementasi testing akan tergantung pada persyaratan proyek)_
- Unit test untuk utilitas dan hooks
- Integration test untuk fitur
- E2E test untuk alur pengguna kritis
- Component testing dengan React Testing Library
## Deployment

Aplikasi dibangun dengan Vite dan dapat di-deploy ke layanan hosting statis apa pun. Proses build menghasilkan bundle yang dioptimalkan di direktori `dist/`.

```bash
npm run build
npm run preview
```