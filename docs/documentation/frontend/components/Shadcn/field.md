---
title: "Field"
sidebar_label: "Field"
---

# Field

**Lokasi:** `components/ui/field.tsx`  
**Dokumentasi Resmi:** - (Custom Component)

## Deskripsi

Komponen untuk membungkus form field dengan label, deskripsi, dan error message. Field menyediakan struktur yang konsisten untuk form elements dengan dukungan berbagai orientasi layout (vertical, horizontal, responsive).

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `FieldSet` | Container untuk group field |
| `FieldLegend` | Legend untuk fieldset |
| `FieldGroup` | Group fields dalam satu container |
| `Field` | Container utama field |
| `FieldContent` | Konten field (input, select, dll) |
| `FieldLabel` | Label untuk field |
| `FieldDescription` | Deskripsi field |
| `FieldError` | Pesan error |
| `FieldSeparator` | Separator antar field |
| `FieldTitle` | Title untuk field |

## Props

### Field
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `orientation` | `"vertical" \| "horizontal" \| "responsive"` | `"vertical"` | Orientasi layout field |

### FieldLabel
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `htmlFor` | `string` | - | ID dari input yang terkait |

## Contoh Penggunaan

### Basic Field (Vertical)
```tsx
import {
  Field,
  FieldContent,
  FieldDescription,
  FieldError,
  FieldLabel,
} from "@/components/ui/field";

<Field>
  <FieldLabel htmlFor="email">Email Address</FieldLabel>
  <FieldContent>
    <Input id="email" type="email" placeholder="you@example.com" />
    <FieldDescription>We'll never share your email.</FieldDescription>
    <FieldError>Email is required</FieldError>
  </FieldContent>
</Field>
```

### Field Horizontal
```tsx
<Field orientation="horizontal">
  <FieldLabel htmlFor="name" className="w-32">Full Name</FieldLabel>
  <FieldContent>
    <Input id="name" placeholder="John Doe" />
  </FieldContent>
</Field>
```

### Field Responsive
```tsx
<Field orientation="responsive">
  <FieldLabel htmlFor="address">Address</FieldLabel>
  <FieldContent>
    <Textarea id="address" placeholder="Enter your address" rows={3} />
  </FieldContent>
</Field>
```

### Field dengan Error
```tsx
<Field>
  <FieldLabel htmlFor="username">Username</FieldLabel>
  <FieldContent>
    <Input 
      id="username" 
      placeholder="username123" 
      className={errors.username ? "border-red-500" : ""}
    />
    <FieldError>Username is already taken</FieldError>
  </FieldContent>
</Field>
```

### FieldGroup untuk Multiple Fields
```tsx
<FieldGroup>
  <Field>
    <FieldLabel htmlFor="first-name">First Name</FieldLabel>
    <FieldContent>
      <Input id="first-name" placeholder="John" />
    </FieldContent>
  </Field>
  <Field>
    <FieldLabel htmlFor="last-name">Last Name</FieldLabel>
    <FieldContent>
      <Input id="last-name" placeholder="Doe" />
    </FieldContent>
  </Field>
</FieldGroup>
```

### FieldSet untuk Group Form
```tsx
<FieldSet>
  <FieldLegend>Personal Information</FieldLegend>
  <FieldGroup>
    <Field>
      <FieldLabel htmlFor="name">Full Name</FieldLabel>
      <FieldContent>
        <Input id="name" placeholder="John Doe" />
      </FieldContent>
    </Field>
    <Field>
      <FieldLabel htmlFor="email">Email</FieldLabel>
      <FieldContent>
        <Input id="email" type="email" placeholder="john@example.com" />
      </FieldContent>
    </Field>
  </FieldGroup>
</FieldSet>
```

### Field dengan Separator
```tsx
<div className="space-y-6">
  <FieldSet>
    <FieldLegend>Account Details</FieldLegend>
    <Field>
      <FieldLabel htmlFor="username">Username</FieldLabel>
      <FieldContent>
        <Input id="username" placeholder="username123" />
      </FieldContent>
    </Field>
    <FieldSeparator />
    <Field>
      <FieldLabel htmlFor="password">Password</FieldLabel>
      <FieldContent>
        <Input id="password" type="password" placeholder="••••••••" />
      </FieldContent>
    </Field>
  </FieldSet>
</div>
```

### Field dengan Title
```tsx
<Field>
  <FieldTitle>Notification Settings</FieldTitle>
  <FieldContent>
    <div className="space-y-2">
      <div className="flex items-center gap-2">
        <Checkbox id="email-notif" />
        <Label htmlFor="email-notif">Email notifications</Label>
      </div>
      <div className="flex items-center gap-2">
        <Checkbox id="push-notif" />
        <Label htmlFor="push-notif">Push notifications</Label>
      </div>
    </div>
  </FieldContent>
</Field>
```

### Field dengan Radio Group
```tsx
<Field>
  <FieldLabel>Payment Method</FieldLabel>
  <FieldContent>
    <div className="space-y-2">
      <div className="flex items-center gap-2">
        <RadioGroupItem value="credit-card" id="credit-card" />
        <Label htmlFor="credit-card">Credit Card</Label>
      </div>
      <div className="flex items-center gap-2">
        <RadioGroupItem value="paypal" id="paypal" />
        <Label htmlFor="paypal">PayPal</Label>
      </div>
      <div className="flex items-center gap-2">
        <RadioGroupItem value="bank-transfer" id="bank-transfer" />
        <Label htmlFor="bank-transfer">Bank Transfer</Label>
      </div>
    </div>
  </FieldContent>
</Field>
```

### Field dengan Switch
```tsx
<Field orientation="horizontal">
  <FieldLabel htmlFor="dark-mode">Dark Mode</FieldLabel>
  <FieldContent>
    <Switch id="dark-mode" />
  </FieldContent>
</Field>
```

### Field dengan Multiple Error Messages
```tsx
<Field>
  <FieldLabel htmlFor="password">Password</FieldLabel>
  <FieldContent>
    <Input id="password" type="password" />
    <FieldError 
      errors={[
        { message: "Password must be at least 8 characters" },
        { message: "Password must contain at least one number" }
      ]}
    />
  </FieldContent>
</Field>
```

### Field dalam Form (React Hook Form)
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  email: z.string().email("Invalid email address"),
  password: z.string().min(6, "Password must be at least 6 characters"),
});

function LoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm({
    resolver: zodResolver(schema),
  });

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <Field>
        <FieldLabel htmlFor="email">Email</FieldLabel>
        <FieldContent>
          <Input 
            id="email" 
            type="email" 
            {...register("email")}
            className={errors.email ? "border-red-500" : ""}
          />
          {errors.email && (
            <FieldError>{errors.email.message}</FieldError>
          )}
        </FieldContent>
      </Field>

      <Field>
        <FieldLabel htmlFor="password">Password</FieldLabel>
        <FieldContent>
          <Input 
            id="password" 
            type="password" 
            {...register("password")}
            className={errors.password ? "border-red-500" : ""}
          />
          {errors.password && (
            <FieldError>{errors.password.message}</FieldError>
          )}
        </FieldContent>
      </Field>

      <Button type="submit">Sign In</Button>
    </form>
  );
}
```

## Kustomisasi

### Custom Spacing
```tsx
<Field className="gap-4">
  <FieldLabel className="text-lg">Large Label</FieldLabel>
  <FieldContent>
    <Input />
  </FieldContent>
</Field>
```

### Custom Label Styling
```tsx
<FieldLabel className="text-primary font-bold">
  Required Field
  <span className="text-destructive ml-1">*</span>
</FieldLabel>
```

## Best Practices

### 1. Gunakan untuk Konsistensi Form
Field memastikan semua form memiliki struktur dan spacing yang konsisten.

### 2. Pilih Orientasi yang Tepat
- **vertical**: Untuk form standar (default)
- **horizontal**: Untuk form dengan label di samping input
- **responsive**: Untuk form yang perlu adaptasi ke mobile

### 3. Selalu Sertakan Label
Label membantu aksesibilitas dan user experience:
```tsx
// ✅ Baik
<FieldLabel htmlFor="email">Email</FieldLabel>

// ❌ Hindari: Tanpa label
<Input placeholder="Email" />
```

### 4. Error Handling yang Jelas
Gunakan `FieldError` untuk menampilkan pesan error yang informatif.

## Troubleshooting

### Field Tidak Sejajar
- Periksa orientasi yang digunakan
- Pastikan menggunakan `FieldGroup` untuk multiple fields

### Label dan Input Tidak Sejajar (Horizontal)
- Pastikan lebar label konsisten dengan `className="w-32"`

## Referensi

- [React Hook Form Documentation](https://react-hook-form.com/)
- [Zod Documentation](https://zod.dev/)
- [WAI-ARIA Form Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/form/)
