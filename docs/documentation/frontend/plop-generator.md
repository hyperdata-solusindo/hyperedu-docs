---
sidebar_position: 4
---

# Code Generation with Plop

## Overview

HyperEdu menggunakan **Plop.js** untuk scaffolding kode yang konsisten dan efisien. Plop memungkinkan pengembang untuk menghasilkan struktur folder, komponen, hooks, dan file-file lainnya secara otomatis dengan template yang sudah ditentukan.

Generator ini dirancang untuk mempercepat pengembangan fitur baru dengan memastikan konsistensi struktur kode dan mengurangi boilerplate yang harus ditulis manual

---

## Menjalankan Generator

Untuk menjalankan generator, gunakan perintah berikut:

```bash
npm run generate
```

Setelah perintah dijalankan, Anda akan disajikan dengan daftar generator yang tersedia.

---

## Daftar Generator

### 1. Feature Generator (Basic)

**Perintah:** `feature`

**Deskripsi:** Membuat struktur fitur baru dengan arsitektur berbasis fitur (feature-based architecture).

**Prompt:**
- **Nama fitur:** Nama fitur baru (contoh: products, auth, user-profile)
- **Data fields:** Daftar field data (pisahkan dengan koma) untuk schema (contoh: id, name, email, role)

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── api/
│   └── get{pascalCase name}.ts
├── assets/
│   └── .gitkeep
├── components/
│   └── {dashCase name}-list.tsx
├── hooks/
│   └── use{pascalCase name}.ts
├── layouts/
│   └── {dashCase name}-layout.tsx
├── pages/
│   └── {dashCase name}.tsx
├── slices/
│   └── slice{pascalCase name}.tsx
├── stores/
│   └── store{pascalCase name}.tsx
├── types/
│   └── schema.ts
├── index.ts
└── route.ts
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "feature"
# Nama fitur: products
# Data fields: id, name, price, category
```

---

### 2. Feature + Datatable Generator

**Perintah:** `feature:datatable`

**Deskripsi:** Membuat struktur fitur baru dengan komponen datatable menggunakan TanStack Table.

**Prompt:**
- **Nama Component/Table:** Nama tabel (contoh: user)
- **Target Directory:** Direktori target (default: `src/features/{name}`)
- **Columns:** Daftar kolom (pisahkan dengan koma) (contoh: id, name, email, role)

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── components/
│   └── {dashCase name}-table.tsx    # Komponen datatable
└── ... (struktur fitur lainnya)
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "feature:datatable"
# Nama: user
# Columns: id, name, email, role
```

---

### 3. Feature + Form Hook Generator

**Perintah:** `feature:form-hook`

**Deskripsi:** Membuat struktur fitur baru dengan React Hook Form untuk manajemen form.

**Prompt:**
- **Nama Form:** Nama form (contoh: login, user, product)
- **Target Directory:** Direktori target (default: `src/features/{name}`)
- **Field Input:** Daftar field input (pisahkan dengan koma) (contoh: email, password, full name)

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── hooks/
│   └── use{pascalCase name}Form.ts      # Custom hook untuk form
├── components/
│   └── {dashCase name}-form.tsx         # Komponen form UI
└── ... (struktur fitur lainnya)
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "feature:form-hook"
# Nama: user
# Fields: email, password, full_name
```

---

### 4. Feature + Page Generator

**Perintah:** `feature:page`

**Deskripsi:** Membuat halaman baru dengan pilihan tipe halaman.

**Prompt:**
- **Nama Entity:** Nama entity (contoh: product, user)
- **Jenis Halaman:** Pilihan (Blank, Create, Edit)
- **Target Directory:** Direktori target (default: `src/features/{name}`)

**Struktur yang Dihasilkan:**
- **Blank:** `{dashCase name}.tsx`
- **Create:** `{dashCase name}-create.tsx`
- **Edit:** `{dashCase name}-edit.tsx`

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "feature:page"
# Nama: product
# Jenis: Create
```

---

### 5. Feature + Async Combobox Generator

**Perintah:** `feature:async-combobox`

**Deskripsi:** Membuat custom hook dan component untuk async combobox/select.

**Prompt:**
- **Nama Entity:** Nama entity (contoh: Category, Product, User)
- **Directory:** Direktori target (default: `src/features/{name}`)

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── hooks/
│   └── use{pascalCase name}Select.ts
└── components/
    └── {dashCase name}-select.tsx
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "feature:async-combobox"
# Nama: Category
```

---

### 6. TanStack + Feature Generator

**Perintah:** `TanStack + feature`

**Deskripsi:** Membuat struktur fitur lengkap dengan integrasi TanStack Query, termasuk konfigurasi form hook, datatable, dan async select secara interaktif.

**Prompt:**
- **Nama fitur:** Nama fitur baru (contoh: products, auth, user-profile)
- **Data fields:** Daftar field data (pisahkan dengan koma) (contoh: id, name, email, role)
- **Include Datatable?:** Apakah ingin menyertakan datatable
- **Include Custom Form Hook?:** Apakah ingin menyertakan form hook
  - Jika ya, pilih field dan tentukan tipe input untuk setiap field:
    - Input Text
    - Textarea
    - Select Dropdown
    - Checkbox/Switch
- **Include Async Select Hook?:** Apakah ingin menyertakan async select
- **Include Edit/Create Page?:** Apakah ingin menyertakan halaman edit/create
  - Jika ya, pilih jenis halaman (Create, Edit)

**Struktur yang Dihasilkan:**
```
src/features/{model}/
├── api/
│   └── get{pascalCase name}.ts
├── assets/
│   └── .gitkeep
├── components/
│   ├── {dashCase name}-list.tsx
│   ├── {dashCase name}-form.tsx (jika useFormHook=true)
│   └── {dashCase name}-table.tsx (jika useDatatable=true)
├── hooks/
│   ├── use{pascalCase name}.ts
│   ├── use{pascalCase name}Form.ts (jika useFormHook=true)
│   └── use{pascalCase name}Select.ts (jika useAsyncSelect=true)
├── layouts/
│   └── {dashCase name}-layout.tsx
├── pages/
│   ├── {dashCase name}.tsx
│   ├── {dashCase name}-create.tsx (jika useFormPage dan Create dipilih)
│   └── {dashCase name}-edit.tsx (jika useFormPage dan Edit dipilih)
├── slices/
│   └── slice{pascalCase name}.tsx
├── stores/
│   └── store{pascalCase name}.tsx
├── types/
│   ├── schema.ts
│   └── query-keys.ts
├── index.ts
└── route.ts
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "TanStack + feature"
# Nama: products
# Data: id, name, price, description, category
# Include Datatable? Yes
# Include Custom Form Hook? Yes
#   - name: input text
#   - price: input text
#   - description: textarea
#   - category: select
# Include Async Select Hook? Yes
# Include Edit/Create Page? Yes (pilih Create dan Edit)
```

---

### 7. TanStack + Form Hook Generator

**Perintah:** `TanStack + feature:form-hook`

**Deskripsi:** Membuat custom hook dan komponen form dengan integrasi TanStack Query.

**Prompt:**
- **Nama Form:** Nama form (contoh: login, user, product)
- **Target Directory:** Direktori target (default: `src/features/{name}`)
- **Field Input:** Daftar field input (pisahkan dengan koma)
- **Konfigurasi Tipe Field:** Untuk setiap field, tentukan tipe input:
  - Input Text
  - Textarea
  - Select Dropdown
  - Checkbox/Switch

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── hooks/
│   └── use{pascalCase name}Form.ts
├── components/
│   └── {dashCase name}-form.tsx
└── types/
    └── schema.ts (diperbarui dengan zod schema)
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "TanStack + feature:form-hook"
# Nama: product
# Fields: name, description, price, is_active
#   - name: Input Text
#   - description: Textarea
#   - price: Input Text
#   - is_active: Checkbox/Switch
```

---

### 8. TanStack + Datatable Generator

**Perintah:** `TanStack + feature:datatable`

**Deskripsi:** Membuat komponen datatable dengan integrasi TanStack Query.

**Prompt:**
- **Nama Component/Table:** Nama tabel (contoh: user)
- **Target Directory:** Direktori target (default: `src/features/{name}`)
- **Columns:** Daftar kolom (pisahkan dengan koma) (contoh: id, name, email, role)

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── components/
│   └── {dashCase name}-table.tsx
└── types/
    └── schema.ts (diperbarui dengan datatable schema)
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "TanStack + feature:datatable"
# Nama: user
# Columns: id, name, email, role, status
```

---

### 9. TanStack + Page Generator

**Perintah:** `TanStack + feature:page`

**Deskripsi:** Membuat halaman baru dengan integrasi TanStack Query.

**Prompt:**
- **Nama Entity:** Nama entity (contoh: product, user)
- **Jenis Halaman:** Pilihan (Blank, Create, Edit)
- **Target Directory:** Direktori target (default: `src/features/{name}`)
- **Apakah ingin menyertakan konfigurasi Datatable?** (hanya untuk Blank page)

**Struktur yang Dihasilkan:**
- **Blank:** `{dashCase name}.tsx` (dengan atau tanpa datatable)
- **Create:** `{dashCase name}-create.tsx`
- **Edit:** `{dashCase name}-edit.tsx`

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "TanStack + feature:page"
# Nama: product
# Jenis: Edit
```

---

### 10. TanStack + Async Select Generator

**Perintah:** `TanStack + feature:asyncselect`

**Deskripsi:** Membuat custom hook untuk async select dengan integrasi TanStack Query.

**Prompt:**
- **Nama Entity:** Nama entity (contoh: Category, Product, User)
- **Directory:** Direktori target (default: `src/features/{name}`)

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
└── hooks/
    └── use{pascalCase name}Select.ts
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "TanStack + feature:asyncselect"
# Nama: Category
```

---

### 11. TanStack + Form Dialog Generator

**Perintah:** `TanStack + feature:form-dialog`

**Deskripsi:** Membuat form dialog dengan React Hook Form dan integrasi TanStack Query.

**Prompt:**
- **Nama Form:** Nama form (contoh: login, user, product)
- **Target Directory:** Direktori target (default: `src/features/{name}`)
- **Field Input:** Daftar field input (pisahkan dengan koma)
- **Konfigurasi Tipe Field:** Untuk setiap field, tentukan tipe input

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── hooks/
│   └── use{pascalCase name}Form.ts
├── components/
│   └── {dashCase name}-form.tsx
└── types/
    └── schema.ts (diperbarui dengan zod schema)
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "TanStack + feature:form-dialog"
# Nama: product
# Fields: name, price, category
```

---

### 12. TanStack + CRUD Hook Generator

**Perintah:** `TanStack + feature:crud-hook`

**Deskripsi:** Membuat custom hook lengkap untuk operasi CRUD dengan TanStack Query.

**Prompt:**
- **Nama Entity:** Nama entity (contoh: Category, Product, User)
- **Directory:** Direktori target (default: `src/features/{name}`)

**Struktur yang Dihasilkan:**
```
src/features/{nama-fitur}/
├── api/
│   └── get{pascalCase name}.ts
├── hooks/
│   └── use{pascalCase name}.ts
└── types/
    └── schema.ts (diperbarui dengan data schema, form schema, dan query keys)
```

**Contoh Penggunaan:**
```bash
npm run generate
# Pilih "TanStack + feature:crud-hook"
# Nama: Category
```

---

## Custom Helpers

Plop dilengkapi dengan helper functions untuk mempermudah templating:

| Helper | Deskripsi | Contoh |
|--------|-----------|--------|
| `eq` | Membandingkan dua nilai | `{{#eq value "active"}}` |
| `split` | Memisahkan string menjadi array | `{{split data ","}}` |
| `removeHyphen` | Menghapus karakter hyphen | `{{removeHyphen "user-profile"}}` → `userprofile` |
| `ucfirst` | Mengubah huruf pertama menjadi kapital | `{{ucfirst "hello"}}` → `Hello` |
| `extractFileName` | Mengekstrak nama file dari path | `{{extractFileName "src/features/user"}}` → `user` |

**Contoh Penggunaan Helper dalam Template:**
```handlebars
{{#each fields}}
  {{#if isInput}}
    <FormInput name="{{name}}" label="{{ucfirst name}}" />
  {{/if}}
  {{#if isTextarea}}
    <FormTextarea name="{{name}}" label="{{ucfirst name}}" />
  {{/if}}
{{/each}}
```

---

## Custom Action: appendIfNotExists

Generator juga menyediakan action khusus `appendIfNotExists` yang berguna untuk menambahkan konten ke file yang sudah ada tanpa menduplikasi.

**Fungsi:** Mengecek apakah suatu string/pattern sudah ada dalam file, jika belum maka akan ditambahkan.

**Contoh Penggunaan dalam Generator:**
```javascript
{
  type: "appendIfNotExists",
  path: "{{directory}}/types/schema.ts",
  templateFile: "plop-templates/tanstack-crudhook/schema-data.hbs",
  checkFor: "export const {{ camelCase name }}DataSchema",
  skipIfExists: true,
}
```

---

## Template Files Structure

Semua template disimpan dalam folder `plop-templates/` dengan struktur berikut:

```
plop-templates/
├── async-combobox/
│   ├── hook.hbs
│   └── component.hbs
├── datatable/
│   └── component.hbs
├── feature/
│   ├── api/
│   │   └── get.hbs
│   ├── components/
│   │   └── list.hbs
│   ├── hooks/
│   │   └── use.hbs
│   ├── layouts/
│   │   └── layout.hbs
│   ├── pages/
│   │   └── page.hbs
│   ├── slices/
│   │   └── slice.hbs
│   ├── stores/
│   │   └── store.hbs
│   ├── types/
│   │   └── schema.hbs
│   ├── index.hbs
│   └── route.hbs
├── form/
│   ├── hook.hbs
│   └── component.hbs
├── page/
│   ├── component.hbs
│   └── component-blank.hbs
├── tanstack-asyncselect/
│   └── hook.hbs
├── tanstack-crudhook/
│   ├── api.hbs
│   ├── hook.hbs
│   ├── schema-data.hbs
│   ├── schema-datatable.hbs
│   ├── schema-form.hbs
│   └── query-keys.hbs
├── tanstack-datatable/
│   ├── component.hbs
│   └── schema.hbs
├── tanstack-feature/
│   ├── api/
│   │   └── get.hbs
│   ├── components/
│   │   └── list.hbs
│   ├── hooks/
│   │   └── use.hbs
│   ├── layouts/
│   │   └── layout.hbs
│   ├── pages/
│   │   └── page.hbs
│   ├── slices/
│   │   └── slice.hbs
│   ├── stores/
│   │   └── store.hbs
│   ├── types/
│   │   ├── schema.hbs
│   │   └── query-keys.hbs
│   ├── index.hbs
│   └── route.hbs
├── tanstack-formdialog/
│   ├── hook.hbs
│   ├── component.hbs
│   ├── schema-data.hbs
│   └── schema-form.hbs
├── tanstack-formhook/
│   ├── hook.hbs
│   └── component.hbs
└── tanstack-page/
    ├── component.hbs
    └── component-blank.hbs
```

---

## Praktik Terbaik

### 1. Gunakan Generator untuk Semua Fitur Baru
Selalu gunakan generator untuk membuat fitur baru agar struktur kode tetap konsisten di seluruh proyek.

### 2. Pilih Template yang Sesuai
- Gunakan **Basic Feature** untuk fitur sederhana tanpa integrasi API yang kompleks
- Gunakan **TanStack + Feature** untuk fitur yang membutuhkan pengelolaan data server
- Gunakan **CRUD Hook** untuk entitas yang memerlukan operasi create, read, update, delete

### 3. Modifikasi Hasil Generator
Hasil generator adalah titik awal yang baik, tetapi jangan ragu untuk memodifikasi:
- Tambahkan validasi tambahan di schema Zod
- Sesuaikan styling komponen
- Tambahkan logika bisnis spesifik

### 4. Jangan Menghapus File .gitkeep
File `.gitkeep` digunakan untuk menjaga agar folder kosong tetap ter-commit ke repository.

### 5. Periksa Hasil Generator Sebelum Commit
Selalu periksa file yang dihasilkan untuk memastikan tidak ada konflik dengan kode yang sudah ada.

### 6. Update Routing Secara Manual
Setelah membuat fitur baru, pastikan untuk menambahkan route ke file routing utama (`src/route.ts`).

### 7. Gunakan Helper Functions
Manfaatkan helper functions yang tersedia untuk mempermudah logika dalam template.

---

## Troubleshooting

### Generator Tidak Berjalan
Pastikan Anda telah menginstal semua dependensi:
```bash
npm install
```

### File Tidak Terbuat
Periksa izin folder dan pastikan path yang dituju valid.

### Template Error
Pastikan template file (.hbs) berada di folder yang benar dengan nama yang sesuai.

### Duplicate Code
Gunakan `skipIfExists: true` untuk mencegah penimpaan file yang sudah ada.

---

## Menambahkan Generator Baru

Untuk menambahkan generator baru:

1. Buat file generator baru di folder `plop-config/`
2. Import generator di `plopfile.js`
3. Tambahkan ke daftar generator dengan `plop.setGenerator()`

**Template Generator Baru:**
```javascript
// plop-config/my-generator.js
export default {
  description: "Deskripsi generator",
  
  prompts: [
    {
      type: "input",
      name: "name",
      message: "Pertanyaan?",
    },
  ],
  
  actions: [
    {
      type: "add",
      path: "src/features/{{dashCase name}}/my-file.tsx",
      templateFile: "plop-templates/my-template.hbs",
    },
  ],
};
```

**Registrasi di plopfile.js:**
```javascript
import myGenerator from "./plop-config/my-generator.js";

export default function (plop) {
  // ... existing code
  plop.setGenerator("my-generator", myGenerator);
}
```