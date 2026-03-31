---
title: "Input"
sidebar_label: "Input"
---

# Input

**Lokasi:** `components/ui/input.tsx`  
**Dokumentasi Resmi:** [Shadcn Input](https://ui.shadcn.com/docs/components/input)

## Deskripsi

Komponen input text dengan berbagai ukuran dan varian. Input adalah komponen fundamental untuk mengumpulkan data dari pengguna, digunakan dalam form, pencarian, dan berbagai interaksi lainnya.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default"` | `"default"` | Varian input (saat ini hanya default) |
| `inputSize` | `"default" \| "sm" \| "xs"` | `"default"` | Ukuran input |
| `type` | `string` | `"text"` | Tipe input (text, email, password, number, dll) |
| `placeholder` | `string` | - | Placeholder text |
| `disabled` | `boolean` | `false` | Menonaktifkan input |
| `className` | `string` | - | Custom CSS classes tambahan |

## Ukuran

| Ukuran | Height | Font Size | Padding | Penggunaan |
|--------|--------|-----------|---------|------------|
| **xs** | `h-7` | `12px` | `px-3 py-1` | Input kecil di dalam tabel atau toolbar padat |
| **sm** | `h-8` | `12px` | `px-3 py-1` | Input compact untuk form yang padat |
| **default** | `h-9` | `12px` | `px-3 py-1` | Input standar untuk kebanyakan kasus |

## Contoh Penggunaan

### Basic Input
```tsx
<div className="space-y-4">
  <Input placeholder="Enter your name" />
  <Input type="email" placeholder="Email address" />
  <Input type="password" placeholder="Password" />
  <Input type="number" placeholder="Age" />
</div>
```

### Ukuran Input
```tsx
<div className="space-y-2">
  <Input inputSize="xs" placeholder="Extra Small" />
  <Input inputSize="sm" placeholder="Small" />
  <Input inputSize="default" placeholder="Default" />
</div>
```

### Input dengan State
```tsx
<div className="space-y-4">
  {/* Disabled */}
  <Input disabled placeholder="Disabled input" value="Cannot edit" />
  
  {/* Read Only */}
  <Input readOnly value="Read-only value" placeholder="Read only" />
  
  {/* Error State - menggunakan className custom */}
  <Input className="border-red-500 focus-visible:ring-red-500/20" placeholder="Error input" />
</div>
```

### Input dengan Label
```tsx
<div className="space-y-1.5">
  <Label htmlFor="name">Full Name</Label>
  <Input id="name" placeholder="John Doe" />
  <p className="text-xs text-muted-foreground">Enter your full name as on ID card</p>
</div>
```

### Input dengan Icon (Menggunakan Input Group)
```tsx
import { InputGroup, InputGroupAddon, InputGroupInput } from "./input-group";

<InputGroup>
  <InputGroupAddon align="inline-start">
    <MailIcon className="h-4 w-4" />
  </InputGroupAddon>
  <InputGroupInput placeholder="Email address" />
</InputGroup>

<InputGroup>
  <InputGroupInput type="password" placeholder="Password" />
  <InputGroupAddon align="inline-end">
    <EyeIcon className="h-4 w-4 cursor-pointer" />
  </InputGroupAddon>
</InputGroup>
```

### Input dalam Form
```tsx
import { useForm } from "react-hook-form";

function LoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm();

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div className="space-y-1.5">
        <Label htmlFor="email">Email</Label>
        <Input
          id="email"
          type="email"
          placeholder="you@example.com"
          {...register("email", { required: "Email is required" })}
          className={errors.email ? "border-red-500" : ""}
        />
        {errors.email && (
          <p className="text-xs text-red-500">{errors.email.message}</p>
        )}
      </div>
      
      <div className="space-y-1.5">
        <Label htmlFor="password">Password</Label>
        <Input
          id="password"
          type="password"
          placeholder="••••••••"
          {...register("password", { required: "Password is required" })}
          className={errors.password ? "border-red-500" : ""}
        />
        {errors.password && (
          <p className="text-xs text-red-500">{errors.password.message}</p>
        )}
      </div>
      
      <Button type="submit" className="w-full">Sign In</Button>
    </form>
  );
}
```

### Input untuk Pencarian
```tsx
<div className="relative">
  <SearchIcon className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-muted-foreground" />
  <Input 
    className="pl-9" 
    placeholder="Search..." 
    value={search}
    onChange={(e) => setSearch(e.target.value)}
  />
</div>
```

### Input dengan Tombol Clear
```tsx
<div className="relative">
  <Input 
    value={value}
    onChange={(e) => setValue(e.target.value)}
    placeholder="Type something..."
  />
  {value && (
    <button
      onClick={() => setValue("")}
      className="absolute right-3 top-1/2 -translate-y-1/2"
    >
      <XIcon className="h-4 w-4 text-muted-foreground hover:text-foreground" />
    </button>
  )}
</div>
```

## Kustomisasi

### Custom Styling
```tsx
<Input className="rounded-full border-2 border-primary/50 focus-visible:border-primary" />

<Input className="bg-gray-50 border-gray-200 focus-visible:bg-white" />

<Input className="text-center font-mono text-sm" placeholder="Enter code" />
```

### Custom Variant
Untuk menambahkan varian baru, modifikasi `inputVariants` di `input.tsx`:

```typescript
const inputVariants = cva(
  cn(
    "w-full min-w-0 rounded-[12px] px-3 py-1 shadow-xs transition-[color,box-shadow] outline-none",
    "focus-visible:border-ring focus-visible:ring-[3px]",
    "aria-invalid:ring-destructive/20 aria-invalid:border-destructive"
  ),
  {
    variants: {
      variant: {
        default: cn(
          "bg-transparent text-inherit border border-input",
          "placeholder:text-muted placeholder:font-semibold",
          "focus-visible:border-primary focus-visible:ring-primary/20"
        ),
        filled: cn(
          "bg-muted border-transparent focus-visible:bg-background",
          "placeholder:text-muted-foreground/70"
        ),
        flushed: cn(
          "border-x-0 border-t-0 border-b rounded-none px-0",
          "focus-visible:ring-0 focus-visible:border-primary"
        ),
      },
      inputSize: {
        // ... existing sizes
      },
    },
  }
);
```

## Best Practices

### 1. Gunakan Label yang Jelas
Selalu sertakan label untuk setiap input agar user tahu informasi apa yang harus diisi:
```tsx
// ✅ Baik
<div className="space-y-1.5">
  <Label htmlFor="email">Email Address</Label>
  <Input id="email" type="email" placeholder="you@example.com" />
</div>

// ❌ Hindari: Tanpa label
<Input placeholder="Email" />
```

### 2. Placeholder Bukan Pengganti Label
Placeholder hanya untuk contoh format, bukan untuk informasi penting:
```tsx
// ✅ Baik: Placeholder sebagai contoh
<Input placeholder="name@example.com" />

// ❌ Hindari: Placeholder menggantikan label
<Input placeholder="Enter your email address here" />
```

### 3. Validasi dan Error State
Tampilkan error dengan jelas dan berikan pesan yang membantu:
```tsx
{errors.email && (
  <p className="text-xs text-red-500 flex items-center gap-1 mt-1">
    <AlertCircleIcon className="h-3 w-3" />
    {errors.email.message}
  </p>
)}
```

### 4. Gunakan Type yang Tepat
Manfaatkan berbagai tipe input untuk pengalaman yang lebih baik:
```tsx
<Input type="email" />      // Keyboard dengan @ pada mobile
<Input type="tel" />        // Keyboard numeric pada mobile
<Input type="number" />     // Stepper controls
<Input type="password" />   // Masks input
<Input type="search" />     // Clear button di beberapa browser
```

### 5. Ukuran yang Konsisten
Gunakan ukuran yang konsisten di seluruh form:
```tsx
// Semua input dalam satu form menggunakan ukuran yang sama
<div className="space-y-4">
  <Input inputSize="sm" placeholder="Small input" />
  <Input inputSize="sm" placeholder="Another small input" />
</div>
```

## Troubleshooting

### Input Tidak Bisa Diisi
- Periksa apakah ada `disabled` prop
- Pastikan tidak ada `readOnly` prop
- Cek apakah ada event handler yang mencegah perubahan

### Nilai Input Tidak Berubah
- Pastikan Anda menggunakan `value` dan `onChange` dengan benar untuk controlled component
- Cek apakah state di-update dengan benar

```tsx
// ✅ Controlled component
const [value, setValue] = useState("");
<Input value={value} onChange={(e) => setValue(e.target.value)} />

// ✅ Uncontrolled component (default value)
<Input defaultValue="Initial value" />
```

### Styling Tidak Sesuai
- Pastikan tidak ada CSS yang menimpa styling default
- Gunakan `className` untuk override, bukan mengubah file komponen

## Referensi

- [Shadcn Input Documentation](https://ui.shadcn.com/docs/components/input)
- [MDN Input Types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#input_types)
- [React Hook Form - Input](https://react-hook-form.com/docs/useform/register)
