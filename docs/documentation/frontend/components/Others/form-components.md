---
title: "Form Components"
sidebar_label: "Form Components"
---

# Form Components

**Lokasi:** `components/custom/form-card.tsx`

## Deskripsi

Kumpulan komponen form dengan styling konsisten untuk membuat formulir yang rapi dan terstruktur. Komponen-komponen ini menyediakan building blocks untuk membangun form dengan layout grid, field validation, dan styling yang konsisten di seluruh aplikasi.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `FormCard` | Container utama form dengan border, shadow, dan title |
| `FormField` | Wrapper untuk field form dengan label dan error message |
| `FormInput` | Input text dengan styling konsisten |
| `FormSelect` | Dropdown select dengan styling konsisten |
| `FormTextarea` | Textarea dengan styling konsisten |
| `FormRow` | Grid layout untuk mengatur field dalam baris |

---

## FormCard

Container utama untuk form dengan border, shadow, dan header.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `children` | `React.ReactNode` | Ya | - | Konten form |
| `className` | `string` | Tidak | - | Custom CSS classes |
| `title` | `string` | Tidak | - | Judul card |
| `icon` | `React.ReactNode` | Tidak | - | Ikon untuk judul |

### Contoh Penggunaan
```tsx
<FormCard title="Informasi Pribadi">
  <FormRow cols={2}>
    <FormField label="Nama" required>
      <FormInput placeholder="Masukkan nama lengkap" />
    </FormField>
    <FormField label="Email">
      <FormInput type="email" placeholder="email@example.com" />
    </FormField>
  </FormRow>
</FormCard>
```

---

## FormField

Wrapper untuk field form dengan label, required indicator, dan error message.

### Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `children` | `React.ReactNode` | Ya | - | Komponen input (FormInput, FormSelect, dll) |
| `label` | `string` | Ya | - | Label field |
| `required` | `boolean` | Tidak | `false` | Menampilkan indikator required (*) |
| `error` | `string` | Tidak | - | Pesan error |
| `className` | `string` | Tidak | - | Custom CSS classes |

### Contoh Penggunaan
```tsx
<FormField label="Username" required error="Username is required">
  <FormInput placeholder="Enter username" />
</FormField>
```

---

## FormInput

Input text dengan styling konsisten.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `placeholder` | `string` | - | Placeholder text |
| `type` | `string` | `"text"` | Tipe input (text, email, password, number, dll) |
| `disabled` | `boolean` | `false` | Menonaktifkan input |
| `value` | `string` | - | Nilai input |
| `onChange` | `(e: ChangeEvent) => void` | - | Handler perubahan |
| `className` | `string` | - | Custom CSS classes |

### Contoh Penggunaan
```tsx
<FormInput 
  placeholder="Enter your name"
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

---

## FormSelect

Dropdown select dengan styling konsisten.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `placeholder` | `string` | `"Select"` | Placeholder text |
| `options` | `Array<{ value: string; label: string }>` | `[]` | Opsi select |
| `value` | `string` | - | Nilai yang dipilih |
| `onChange` | `(e: ChangeEvent) => void` | - | Handler perubahan |
| `disabled` | `boolean` | `false` | Menonaktifkan select |
| `className` | `string` | - | Custom CSS classes |

### Contoh Penggunaan
```tsx
<FormSelect
  placeholder="Pilih kategori"
  options={[
    { value: "electronics", label: "Electronics" },
    { value: "clothing", label: "Clothing" },
    { value: "books", label: "Books" },
  ]}
  value={category}
  onChange={(e) => setCategory(e.target.value)}
/>
```

---

## FormTextarea

Textarea dengan styling konsisten.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `placeholder` | `string` | - | Placeholder text |
| `rows` | `number` | `3` | Jumlah baris |
| `value` | `string` | - | Nilai textarea |
| `onChange` | `(e: ChangeEvent) => void` | - | Handler perubahan |
| `disabled` | `boolean` | `false` | Menonaktifkan textarea |
| `className` | `string` | - | Custom CSS classes |

### Contoh Penggunaan
```tsx
<FormTextarea
  placeholder="Enter description"
  rows={4}
  value={description}
  onChange={(e) => setDescription(e.target.value)}
/>
```

---

## FormRow

Grid layout untuk mengatur field dalam baris.

### Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `children` | `React.ReactNode` | - | Field-field yang akan diatur dalam grid |
| `cols` | `2 \| 3 \| 4` | `2` | Jumlah kolom grid (responsive: 1 kolom di mobile) |
| `className` | `string` | - | Custom CSS classes |

### Grid Cols
- `cols={2}`: 2 kolom di desktop, 1 kolom di mobile
- `cols={3}`: 3 kolom di desktop, 1 kolom di mobile
- `cols={4}`: 4 kolom di desktop, 2 kolom di tablet, 1 kolom di mobile

### Contoh Penggunaan
```tsx
<FormRow cols={2}>
  <FormField label="First Name" required>
    <FormInput placeholder="John" />
  </FormField>
  <FormField label="Last Name" required>
    <FormInput placeholder="Doe" />
  </FormField>
</FormRow>

<FormRow cols={3}>
  <FormField label="City">
    <FormInput placeholder="Jakarta" />
  </FormField>
  <FormField label="Province">
    <FormInput placeholder="DKI Jakarta" />
  </FormField>
  <FormField label="Postal Code">
    <FormInput placeholder="12345" />
  </FormField>
</FormRow>
```

---

## Contoh Lengkap

### Form Registrasi
```tsx
import { FormCard, FormField, FormInput, FormSelect, FormRow, FormTextarea } from "@/components/custom/form-card";

function RegistrationForm() {
  const [formData, setFormData] = useState({
    firstName: "",
    lastName: "",
    email: "",
    role: "",
    bio: "",
  });

  const [errors, setErrors] = useState({});

  const validate = () => {
    const newErrors = {};
    if (!formData.firstName) newErrors.firstName = "First name is required";
    if (!formData.lastName) newErrors.lastName = "Last name is required";
    if (!formData.email) newErrors.email = "Email is required";
    if (!formData.role) newErrors.role = "Role is required";
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (validate()) {
      console.log("Form submitted:", formData);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <FormCard title="User Registration" icon={<UserIcon className="h-4 w-4" />}>
        <FormRow cols={2}>
          <FormField label="First Name" required error={errors.firstName}>
            <FormInput
              placeholder="John"
              value={formData.firstName}
              onChange={(e) => setFormData({ ...formData, firstName: e.target.value })}
            />
          </FormField>
          <FormField label="Last Name" required error={errors.lastName}>
            <FormInput
              placeholder="Doe"
              value={formData.lastName}
              onChange={(e) => setFormData({ ...formData, lastName: e.target.value })}
            />
          </FormField>
        </FormRow>

        <FormField label="Email" required error={errors.email}>
          <FormInput
            type="email"
            placeholder="john.doe@example.com"
            value={formData.email}
            onChange={(e) => setFormData({ ...formData, email: e.target.value })}
          />
        </FormField>

        <FormField label="Role" required error={errors.role}>
          <FormSelect
            placeholder="Select role"
            options={[
              { value: "admin", label: "Administrator" },
              { value: "user", label: "Regular User" },
              { value: "guest", label: "Guest" },
            ]}
            value={formData.role}
            onChange={(e) => setFormData({ ...formData, role: e.target.value })}
          />
        </FormField>

        <FormField label="Bio">
          <FormTextarea
            placeholder="Tell us about yourself..."
            rows={3}
            value={formData.bio}
            onChange={(e) => setFormData({ ...formData, bio: e.target.value })}
          />
        </FormField>

        <div className="flex justify-end gap-2 pt-4">
          <Button type="button" variant="outline">Cancel</Button>
          <Button type="submit">Register</Button>
        </div>
      </FormCard>
    </form>
  );
}
```

### Form dengan React Hook Form
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email address"),
  role: z.string().min(1, "Role is required"),
});

function ProductForm() {
  const { register, handleSubmit, formState: { errors } } = useForm({
    resolver: zodResolver(schema),
  });

  return (
    <FormCard title="Product Information">
      <FormField label="Product Name" required error={errors.name?.message}>
        <FormInput placeholder="Enter product name" {...register("name")} />
      </FormField>

      <FormField label="Email" required error={errors.email?.message}>
        <FormInput type="email" placeholder="contact@example.com" {...register("email")} />
      </FormField>

      <FormField label="Category" required error={errors.role?.message}>
        <FormSelect
          options={[
            { value: "electronics", label: "Electronics" },
            { value: "clothing", label: "Clothing" },
          ]}
          {...register("role")}
        />
      </FormField>

      <div className="flex justify-end gap-2">
        <Button type="button" variant="outline">Cancel</Button>
        <Button type="submit" onClick={handleSubmit(onSubmit)}>Save</Button>
      </div>
    </FormCard>
  );
}
```

## Best Practices

### 1. Gunakan FormRow untuk Layout Grid
Gunakan `FormRow` untuk mengatur field dalam layout grid yang responsif:
```tsx
<FormRow cols={2}>
  <FormField label="First Name">...</FormField>
  <FormField label="Last Name">...</FormField>
</FormRow>
```

### 2. Validasi dengan Error Message
Gunakan prop `error` untuk menampilkan pesan error:
```tsx
<FormField label="Email" error={errors.email?.message}>
  <FormInput {...register("email")} />
</FormField>
```

### 3. Required Indicator
Gunakan `required` prop untuk menandai field wajib:
```tsx
<FormField label="Name" required>
  <FormInput />
</FormField>
```

### 4. Konsisten dengan Spacing
FormCard memiliki gap default `gap-5` antar field, konsisten di seluruh aplikasi.

## Referensi

- [React Hook Form Documentation](https://react-hook-form.com/)
- [Zod Documentation](https://zod.dev/)
- [Tailwind CSS Grid](https://tailwindcss.com/docs/grid-template-columns)
