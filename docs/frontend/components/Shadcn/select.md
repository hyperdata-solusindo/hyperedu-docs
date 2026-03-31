---
title: "Select"
sidebar_label: "Select"
---

# Select

**Lokasi:** `components/ui/select.tsx`  
**Dokumentasi Resmi:** [Shadcn Select](https://ui.shadcn.com/docs/components/select)

## Deskripsi

Komponen select dropdown dengan grup dan scroll, dibangun di atas Radix UI Select. Select digunakan untuk memilih satu opsi dari daftar pilihan yang tersedia, cocok untuk form dengan opsi terbatas.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Select` | Container utama select |
| `SelectTrigger` | Trigger button yang menampilkan nilai yang dipilih |
| `SelectValue` | Menampilkan nilai yang dipilih |
| `SelectContent` | Konten dropdown |
| `SelectGroup` | Group items dalam dropdown |
| `SelectItem` | Item individual yang dapat dipilih |
| `SelectLabel` | Label untuk group |
| `SelectSeparator` | Separator antar items |
| `SelectScrollUpButton` | Tombol scroll ke atas |
| `SelectScrollDownButton` | Tombol scroll ke bawah |

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `value` | `string` | - | Nilai yang dipilih (controlled) |
| `defaultValue` | `string` | - | Nilai default (uncontrolled) |
| `onValueChange` | `(value: string) => void` | - | Callback ketika nilai berubah |
| `disabled` | `boolean` | `false` | Menonaktifkan select |
| `size` | `"xs" \| "sm" \| "default"` | `"default"` | Ukuran select trigger |

## Ukuran

| Ukuran | Height | Penggunaan |
|--------|--------|------------|
| **xs** | `h-7` | Select kecil di dalam tabel atau toolbar |
| **sm** | `h-8` | Select compact |
| **default** | `h-9` | Select standar |

## Contoh Penggunaan

### Basic Select
```tsx
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";

<Select>
  <SelectTrigger className="w-[180px]">
    <SelectValue placeholder="Select a fruit" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="apple">Apple</SelectItem>
    <SelectItem value="banana">Banana</SelectItem>
    <SelectItem value="orange">Orange</SelectItem>
    <SelectItem value="grape">Grape</SelectItem>
  </SelectContent>
</Select>
```

### Controlled Select
```tsx
function ControlledSelect() {
  const [value, setValue] = useState("");

  return (
    <Select value={value} onValueChange={setValue}>
      <SelectTrigger className="w-[180px]">
        <SelectValue placeholder="Select a fruit" />
      </SelectTrigger>
      <SelectContent>
        <SelectItem value="apple">Apple</SelectItem>
        <SelectItem value="banana">Banana</SelectItem>
        <SelectItem value="orange">Orange</SelectItem>
      </SelectContent>
    </Select>
  );
}
```

### Select dengan Group
```tsx
<Select>
  <SelectTrigger className="w-[200px]">
    <SelectValue placeholder="Select a country" />
  </SelectTrigger>
  <SelectContent>
    <SelectGroup>
      <SelectLabel>Asia</SelectLabel>
      <SelectItem value="indonesia">Indonesia</SelectItem>
      <SelectItem value="japan">Japan</SelectItem>
      <SelectItem value="korea">South Korea</SelectItem>
    </SelectGroup>
    <SelectGroup>
      <SelectLabel>Europe</SelectLabel>
      <SelectItem value="germany">Germany</SelectItem>
      <SelectItem value="france">France</SelectItem>
      <SelectItem value="italy">Italy</SelectItem>
    </SelectGroup>
  </SelectContent>
</Select>
```

### Select dengan Ukuran
```tsx
<div className="flex items-center gap-4">
  <Select>
    <SelectTrigger size="xs" className="w-[120px]">
      <SelectValue placeholder="XS" />
    </SelectTrigger>
    <SelectContent>
      <SelectItem value="1">Option 1</SelectItem>
    </SelectContent>
  </Select>

  <Select>
    <SelectTrigger size="sm" className="w-[120px]">
      <SelectValue placeholder="SM" />
    </SelectTrigger>
    <SelectContent>
      <SelectItem value="1">Option 1</SelectItem>
    </SelectContent>
  </Select>

  <Select>
    <SelectTrigger size="default" className="w-[120px]">
      <SelectValue placeholder="Default" />
    </SelectTrigger>
    <SelectContent>
      <SelectItem value="1">Option 1</SelectItem>
    </SelectContent>
  </Select>
</div>
```

### Select dengan Icon
```tsx
<Select>
  <SelectTrigger className="w-[200px]">
    <UserIcon className="mr-2 h-4 w-4" />
    <SelectValue placeholder="Select user" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="john">
      <div className="flex items-center gap-2">
        <UserIcon className="h-4 w-4" />
        <span>John Doe</span>
      </div>
    </SelectItem>
    <SelectItem value="jane">
      <div className="flex items-center gap-2">
        <UserIcon className="h-4 w-4" />
        <span>Jane Smith</span>
      </div>
    </SelectItem>
  </SelectContent>
</Select>
```

### Select dalam Form (React Hook Form)
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const formSchema = z.object({
  role: z.string().min(1, "Please select a role"),
});

function SelectForm() {
  const { register, handleSubmit, formState: { errors } } = useForm({
    resolver: zodResolver(formSchema),
  });

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div className="space-y-1.5">
        <Label htmlFor="role">Role</Label>
        <Select {...register("role")}>
          <SelectTrigger id="role" className="w-[200px]">
            <SelectValue placeholder="Select a role" />
          </SelectTrigger>
          <SelectContent>
            <SelectItem value="admin">Admin</SelectItem>
            <SelectItem value="user">User</SelectItem>
            <SelectItem value="guest">Guest</SelectItem>
          </SelectContent>
        </Select>
        {errors.role && (
          <p className="text-xs text-red-500">{errors.role.message}</p>
        )}
      </div>
      <Button type="submit">Submit</Button>
    </form>
  );
}
```

### Select dengan Controller (React Hook Form)
```tsx
import { useForm, Controller } from "react-hook-form";

function ControllerSelect() {
  const { control, handleSubmit } = useForm({
    defaultValues: { category: "" },
  });

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="category"
        control={control}
        rules={{ required: "Category is required" }}
        render={({ field, fieldState }) => (
          <div className="space-y-1.5">
            <Label>Category</Label>
            <Select
              value={field.value}
              onValueChange={field.onChange}
            >
              <SelectTrigger className="w-[200px]">
                <SelectValue placeholder="Select category" />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="electronics">Electronics</SelectItem>
                <SelectItem value="clothing">Clothing</SelectItem>
                <SelectItem value="books">Books</SelectItem>
              </SelectContent>
            </Select>
            {fieldState.error && (
              <p className="text-xs text-red-500">{fieldState.error.message}</p>
            )}
          </div>
        )}
      />
      <Button type="submit">Submit</Button>
    </form>
  );
}
```

### Select dengan Scroll (Banyak Opsi)
```tsx
<Select>
  <SelectTrigger className="w-[200px]">
    <SelectValue placeholder="Select a country" />
  </SelectTrigger>
  <SelectContent className="max-h-[300px]">
    {countries.map((country) => (
      <SelectItem key={country.code} value={country.code}>
        {country.name}
      </SelectItem>
    ))}
  </SelectContent>
</Select>
```

### Select Disabled
```tsx
<Select disabled>
  <SelectTrigger className="w-[180px]">
    <SelectValue placeholder="Disabled select" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="1">Option 1</SelectItem>
    <SelectItem value="2">Option 2</SelectItem>
  </SelectContent>
</Select>
```

### Select dengan Custom Trigger
```tsx
<Select>
  <SelectTrigger className="w-[200px] border-dashed">
    <SelectValue placeholder="Custom style" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="1">Option 1</SelectItem>
    <SelectItem value="2">Option 2</SelectItem>
  </SelectContent>
</Select>
```

## Kustomisasi

### Custom Styling Trigger
```tsx
<SelectTrigger className="rounded-full bg-muted border-none">
  <SelectValue placeholder="Custom trigger" />
</SelectTrigger>
```

### Custom Styling Content
```tsx
<SelectContent className="bg-primary text-primary-foreground">
  <SelectItem value="1">Custom background</SelectItem>
</SelectContent>
```

### Custom Icon
Modifikasi trigger untuk menggunakan ikon kustom:

```tsx
<SelectTrigger className="w-[200px]">
  <SelectValue placeholder="Select option" />
  <ChevronDownIcon className="ml-2 h-4 w-4 opacity-50" />
</SelectTrigger>
```

## Best Practices

### 1. Gunakan Placeholder yang Jelas
```tsx
// ✅ Baik
<SelectValue placeholder="Select a category" />

// ❌ Hindari
<SelectValue placeholder="Choose" />
```

### 2. Group Opsi yang Relevan
```tsx
// ✅ Baik: Group opsi yang terkait
<SelectGroup>
  <SelectLabel>Fruits</SelectLabel>
  <SelectItem value="apple">Apple</SelectItem>
  <SelectItem value="banana">Banana</SelectItem>
</SelectGroup>
<SelectGroup>
  <SelectLabel>Vegetables</SelectLabel>
  <SelectItem value="carrot">Carrot</SelectItem>
</SelectGroup>
```

### 3. Gunakan Ukuran yang Konsisten
```tsx
// Gunakan ukuran yang sama untuk select dalam satu form
<div className="space-y-4">
  <Select size="sm">...</Select>
  <Select size="sm">...</Select>
</div>
```

### 4. Handle Nilai Kosong
```tsx
// Gunakan placeholder untuk nilai kosong
<Select>
  <SelectTrigger>
    <SelectValue placeholder="Select an option" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="">None</SelectItem>
    <SelectItem value="1">Option 1</SelectItem>
  </SelectContent>
</Select>
```

## Troubleshooting

### Select Tidak Bisa Dibuka
- Periksa apakah ada `disabled` prop
- Pastikan tidak ada elemen yang menutupi trigger
- Cek z-index dari SelectContent

### Nilai Tidak Tersimpan
- Untuk controlled component, pastikan `onValueChange` diimplementasikan
- Pastikan `value` prop di-update dengan benar

### Dropdown Terpotong
- Gunakan `className` pada SelectContent untuk mengatur posisi
- Pastikan parent element memiliki overflow visible

## Referensi

- [Shadcn Select Documentation](https://ui.shadcn.com/docs/components/select)
- [Radix UI Select](https://www.radix-ui.com/primitives/docs/components/select)
- [MDN Select Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select)
