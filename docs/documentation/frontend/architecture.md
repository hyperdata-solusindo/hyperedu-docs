---
sidebar_position: 1
---
# Architecture

## Overview

Frontend HyperEdu adalah aplikasi halaman tunggal (SPA) modern yang menyediakan antarmuka pengguna intuitif untuk platform pendidikan. Ini mengikuti praktik terbaik React, menggunakan TypeScript untuk keamanan tipe, dan mengimplementasikan arsitektur komponen yang dapat diskalakan.

## Tumpukan Teknologi

- **Framework**: React 18+
- **Bahasa**: TypeScript 4.9+
- **Alat Build**: Vite
- **Manajemen State**: React State, Redux (widt Redux Toolkit), TanStack React-Query
- **Routing**: React Router 6
- **Styling**: Tailwind CSS
- **API Client**: Axios

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

Prinsip Utama

Setiap folder di dalam `src/features` harus bersifat mandiri. Jika fitur `courses` membutuhkan komponen dari `auth`, pastikan komponen tersebut diekspor melalui `features/auth/index.ts`.