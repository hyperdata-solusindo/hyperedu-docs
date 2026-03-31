---
title: "Label"
sidebar_label: "Label"
---

# Label

**Lokasi:** `components/ui/label.tsx`  
**Dokumentasi Resmi:** [Shadcn Label](https://ui.shadcn.com/docs/components/label)

## Deskripsi

Komponen label untuk form fields, dibangun di atas Radix UI Label. Label digunakan untuk memberikan nama atau deskripsi pada form control seperti input, select, atau textarea, meningkatkan aksesibilitas dan user experience.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `htmlFor` | `string` | - | ID dari form control yang terkait |
| `disabled` | `boolean` | `false` | Menandakan bahwa form control terkait disabled |
| `className` | `string` | - | Custom CSS classes tambahan |

## Contoh Penggunaan

### Basic Label
```tsx
import { Label } from "@/components/ui/label";
import { Input } from "@/components/ui/input";

<div className="space-y-1.5">
  <Label htmlFor="name">Full Name</Label>
  <Input id="name" placeholder="John Doe" />
</div>
```

### Label dengan Required Indicator
```tsx
<Label htmlFor="email" className="flex items-center gap-1">
  Email Address
  <span className="text-destructive">*</span>
</Label>
<Input id="email" type="email" placeholder="you@example.com" required />
```

### Label dengan Description
```tsx
<div className="space-y-1.5">
  <Label htmlFor="username">Username</Label>
  <Input id="username" placeholder="username123" />
  <p className="text-xs text-muted-foreground">
    This will be your unique identifier
  </p>
</div>
```

### Label Disabled
```tsx
<div className="space-y-1.5">
  <Label htmlFor="disabled-field" className="text-muted-foreground">
    Disabled Field
  </Label>
  <Input id="disabled-field" disabled placeholder="Cannot edit" />
</div>
```

### Label dengan Error State
```tsx
<div className="space-y-1.5">
  <Label htmlFor="password" className="text-destructive">
    Password
  </Label>
  <Input 
    id="password" 
    type="password" 
    className="border-destructive focus-visible:ring-destructive/20"
  />
  <p className="text-xs text-destructive">Password must be at least 8 characters</p>
</div>
```

### Label dalam Form (React Hook Form)
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  email: z.string().email("Invalid email address"),
  password: z.string().min(6, "Password must be at least 6 characters"),
});

function LoginForm() {
  const { register, formState: { errors } } = useForm({
    resolver: zodResolver(schema),
  });

  return (
    <div className="space-y-4">
      <div className="space-y-1.5">
        <Label 
          htmlFor="email" 
          className={errors.email ? "text-destructive" : ""}
        >
          Email
        </Label>
        <Input 
          id="email" 
          type="email" 
          {...register("email")}
          className={errors.email ? "border-destructive" : ""}
        />
        {errors.email && (
          <p className="text-xs text-destructive">{errors.email.message}</p>
        )}
      </div>

      <div className="space-y-1.5">
        <Label 
          htmlFor="password" 
          className={errors.password ? "text-destructive" : ""}
        >
          Password
        </Label>
        <Input 
          id="password" 
          type="password" 
          {...register("password")}
          className={errors.password ? "border-destructive" : ""}
        />
        {errors.password && (
          <p className="text-xs text-destructive">{errors.password.message}</p>
        )}
      </div>
    </div>
  );
}
```

### Label dengan Checkbox
```tsx
<div className="flex items-center gap-2">
  <Checkbox id="terms" />
  <Label htmlFor="terms" className="text-sm">
    I agree to the{" "}
    <a href="/terms" className="text-primary underline">
      Terms of Service
    </a>
  </Label>
</div>
```

### Label dengan Radio Group
```tsx
<fieldset className="space-y-3">
  <Label as="legend" className="text-sm font-medium">
    Payment Method
  </Label>
  <div className="space-y-2">
    <div className="flex items-center gap-2">
      <RadioGroupItem value="credit-card" id="credit-card" />
      <Label htmlFor="credit-card">Credit Card</Label>
    </div>
    <div className="flex items-center gap-2">
      <RadioGroupItem value="paypal" id="paypal" />
      <Label htmlFor="paypal">PayPal</Label>
    </div>
  </div>
</fieldset>
```

### Label dengan Switch
```tsx
<div className="flex items-center justify-between">
  <Label htmlFor="notifications" className="cursor-pointer">
    Email Notifications
  </Label>
  <Switch id="notifications" />
</div>
```

### Label dengan Tooltip
```tsx
<div className="flex items-center gap-1">
  <Label htmlFor="api-key">API Key</Label>
  <Tooltip>
    <TooltipTrigger asChild>
      <HelpCircleIcon className="h-4 w-4 text-muted-foreground cursor-help" />
    </TooltipTrigger>
    <TooltipContent>
      <p>Your API key is used for authentication</p>
    </TooltipContent>
  </Tooltip>
</div>
<Input id="api-key" placeholder="Enter your API key" />
```

### Label dalam Card
```tsx
<Card>
  <CardHeader>
    <CardTitle>Profile Settings</CardTitle>
    <CardDescription>Update your personal information</CardDescription>
  </CardHeader>
  <CardContent className="space-y-4">
    <div className="space-y-1.5">
      <Label htmlFor="name">Full Name</Label>
      <Input id="name" defaultValue="John Doe" />
    </div>
    <div className="space-y-1.5">
      <Label htmlFor="email">Email</Label>
      <Input id="email" type="email" defaultValue="john@example.com" />
    </div>
    <div className="space-y-1.5">
      <Label htmlFor="bio">Bio</Label>
      <Textarea id="bio" placeholder="Tell us about yourself" rows={3} />
    </div>
  </CardContent>
  <CardFooter>
    <Button>Save Changes</Button>
  </CardFooter>
</Card>
```

### Label dengan Optional Indicator
```tsx
<Label htmlFor="company" className="flex items-center gap-1">
  Company
  <span className="text-xs text-muted-foreground font-normal">(Optional)</span>
</Label>
<Input id="company" placeholder="Your company name" />
```

### Label dengan Icon
```tsx
<Label htmlFor="search" className="sr-only">Search</Label>
<div className="relative">
  <SearchIcon className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-muted-foreground" />
  <Input id="search" className="pl-9" placeholder="Search..." />
</div>
```

## Kustomisasi

### Custom Styling
```tsx
<Label className="text-primary font-bold text-lg">
  Custom Label
</Label>
```

### Required Field Styling
```tsx
<Label className="after:content-['*'] after:ml-0.5 after:text-destructive">
  Required Field
</Label>
```

## Best Practices

### 1. Selalu Gunakan htmlFor
```tsx
// ✅ Baik: Terkait dengan input
<Label htmlFor="email">Email</Label>
<Input id="email" />

// ❌ Hindari: Tidak terkait
<Label>Email</Label>
<Input />
```

### 2. Gunakan untuk Form Control
Label sebaiknya digunakan untuk form control (input, select, textarea, checkbox, radio).

### 3. Indikator Required
Berikan indikator visual untuk field yang wajib diisi:
```tsx
<Label className="flex items-center gap-1">
  Email
  <span className="text-destructive text-xs">*</span>
</Label>
```

### 4. Aksesibilitas
- Label membantu screen reader untuk mengidentifikasi form control
- Pastikan `htmlFor` sesuai dengan `id` input

### 5. Konsistensi Spacing
Gunakan spacing yang konsisten antara label dan form control:
```tsx
<div className="space-y-1.5">
  <Label>Field Label</Label>
  <Input />
</div>
```

## Troubleshooting

### Klik Label Tidak Memfokuskan Input
- Pastikan `htmlFor` pada label sama dengan `id` pada input
- Periksa apakah ada JavaScript yang mengganggu

### Label Tidak Terlihat
- Periksa apakah ada CSS yang menyembunyikan label
- Pastikan tidak menggunakan `sr-only` jika perlu terlihat

## Referensi

- [Shadcn Label Documentation](https://ui.shadcn.com/docs/components/label)
- [Radix UI Label](https://www.radix-ui.com/primitives/docs/components/label)
- [MDN Label Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/label)
- [WCAG Label Guidelines](https://www.w3.org/WAI/WCAG21/Understanding/labels-or-instructions.html)
