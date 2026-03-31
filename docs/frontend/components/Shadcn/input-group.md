---
title: "Input Group"
sidebar_label: "Input Group"
---

# Input Group

**Lokasi:** `components/ui/input-group.tsx`  
**Dokumentasi Resmi:** - (Custom Component)

## Deskripsi

Komponen untuk mengelompokkan input dengan addon seperti ikon, tombol, atau teks. Input Group memudahkan pembuatan komponen input dengan elemen tambahan di kiri, kanan, atas, atau bawah.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `InputGroup` | Container utama input group |
| `InputGroupAddon` | Addon di kiri/kanan/atas/bawah input |
| `InputGroupButton` | Tombol dalam input group |
| `InputGroupText` | Teks dalam input group |
| `InputGroupInput` | Input utama dalam group |
| `InputGroupTextarea` | Textarea dalam group |

## Props

### InputGroup
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default" \| "noOutline"` | `"default"` | Varian input group |
| `size` | `"default" \| "sm" \| "xs"` | `"xs"` | Ukuran input group |

### InputGroupAddon
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `align` | `"inline-start" \| "inline-end" \| "block-start" \| "block-end"` | `"inline-start"` | Posisi addon |

### InputGroupButton
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `size` | `"xs" \| "sm" \| "icon-xs" \| "icon-sm"` | `"xs"` | Ukuran button |

## Ukuran

| Ukuran | Height | Penggunaan |
|--------|--------|------------|
| **xs** | `h-7` | Input group kecil |
| **sm** | `h-8` | Input group compact |
| **default** | `h-9` | Input group standar |

## Contoh Penggunaan

### Input Group dengan Ikon di Kiri
```tsx
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@/components/ui/input-group";

<InputGroup>
  <InputGroupAddon align="inline-start">
    <SearchIcon className="h-4 w-4" />
  </InputGroupAddon>
  <InputGroupInput placeholder="Search..." />
</InputGroup>
```

### Input Group dengan Ikon di Kanan
```tsx
<InputGroup>
  <InputGroupInput type="password" placeholder="Password" />
  <InputGroupAddon align="inline-end">
    <EyeIcon className="h-4 w-4 cursor-pointer" />
  </InputGroupAddon>
</InputGroup>
```

### Input Group dengan Ikon di Kiri dan Kanan
```tsx
<InputGroup>
  <InputGroupAddon align="inline-start">
    <MailIcon className="h-4 w-4" />
  </InputGroupAddon>
  <InputGroupInput type="email" placeholder="Email" />
  <InputGroupAddon align="inline-end">
    <CheckCircleIcon className="h-4 w-4 text-green-500" />
  </InputGroupAddon>
</InputGroup>
```

### Input Group dengan Tombol
```tsx
<InputGroup>
  <InputGroupInput placeholder="Enter code" />
  <InputGroupAddon align="inline-end">
    <InputGroupButton>Verify</InputGroupButton>
  </InputGroupAddon>
</InputGroup>
```

### Input Group dengan Tombol dan Ikon
```tsx
<InputGroup>
  <InputGroupInput placeholder="Search..." />
  <InputGroupAddon align="inline-end">
    <InputGroupButton variant="default">
      <SearchIcon className="h-3 w-3 mr-1" />
      Search
    </InputGroupButton>
  </InputGroupAddon>
</InputGroup>
```

### Input Group dengan Teks Addon
```tsx
<InputGroup>
  <InputGroupAddon align="inline-start">
    <InputGroupText>https://</InputGroupText>
  </InputGroupAddon>
  <InputGroupInput placeholder="yourdomain.com" />
</InputGroup>
```

### Input Group dengan Currency
```tsx
<InputGroup>
  <InputGroupAddon align="inline-start">
    <InputGroupText>$</InputGroupText>
  </InputGroupAddon>
  <InputGroupInput type="number" placeholder="0.00" />
  <InputGroupAddon align="inline-end">
    <InputGroupText>USD</InputGroupText>
  </InputGroupAddon>
</InputGroup>
```

### Input Group dengan Ukuran Berbeda
```tsx
<div className="space-y-2">
  <InputGroup size="xs">
    <InputGroupAddon align="inline-start">
      <SearchIcon className="h-3 w-3" />
    </InputGroupAddon>
    <InputGroupInput placeholder="Extra small" />
  </InputGroup>

  <InputGroup size="sm">
    <InputGroupAddon align="inline-start">
      <SearchIcon className="h-3.5 w-3.5" />
    </InputGroupAddon>
    <InputGroupInput placeholder="Small" />
  </InputGroup>

  <InputGroup size="default">
    <InputGroupAddon align="inline-start">
      <SearchIcon className="h-4 w-4" />
    </InputGroupAddon>
    <InputGroupInput placeholder="Default" />
  </InputGroup>
</div>
```

### Input Group dengan Variant No Outline
```tsx
<InputGroup variant="noOutline">
  <InputGroupAddon align="inline-start">
    <SearchIcon className="h-4 w-4" />
  </InputGroupAddon>
  <InputGroupInput placeholder="No border input" />
</InputGroup>
```

### Input Group dengan Textarea
```tsx
<InputGroup>
  <InputGroupAddon align="block-start">
    <UserIcon className="h-4 w-4" />
  </InputGroupAddon>
  <InputGroupTextarea placeholder="Write your comment..." rows={3} />
</InputGroup>
```

### Input Group untuk Pencarian dengan Clear Button
```tsx
function SearchInput() {
  const [value, setValue] = useState("");

  return (
    <InputGroup>
      <InputGroupAddon align="inline-start">
        <SearchIcon className="h-4 w-4" />
      </InputGroupAddon>
      <InputGroupInput 
        placeholder="Search..." 
        value={value}
        onChange={(e) => setValue(e.target.value)}
      />
      {value && (
        <InputGroupAddon align="inline-end">
          <InputGroupButton 
            variant="ghost" 
            size="icon-xs"
            onClick={() => setValue("")}
          >
            <XIcon className="h-3 w-3" />
          </InputGroupButton>
        </InputGroupAddon>
      )}
    </InputGroup>
  );
}
```

### Input Group dengan Dropdown
```tsx
<InputGroup>
  <InputGroupAddon align="inline-start">
    <Select defaultValue="https">
      <SelectTrigger className="w-24 border-0 shadow-none">
        <SelectValue />
      </SelectTrigger>
      <SelectContent>
        <SelectItem value="https">https://</SelectItem>
        <SelectItem value="http">http://</SelectItem>
        <SelectItem value="ftp">ftp://</SelectItem>
      </SelectContent>
    </Select>
  </InputGroupAddon>
  <InputGroupInput placeholder="yourdomain.com" />
</InputGroup>
```

### Input Group dengan Validation Icon
```tsx
function ValidatedInput() {
  const [value, setValue] = useState("");
  const isValid = value.length >= 3;

  return (
    <InputGroup>
      <InputGroupInput 
        placeholder="Username" 
        value={value}
        onChange={(e) => setValue(e.target.value)}
        className={value && !isValid ? "border-red-500" : ""}
      />
      <InputGroupAddon align="inline-end">
        {value && (
          isValid ? (
            <CheckCircleIcon className="h-4 w-4 text-green-500" />
          ) : (
            <XCircleIcon className="h-4 w-4 text-red-500" />
          )
        )}
      </InputGroupAddon>
    </InputGroup>
  );
}
```

### Input Group dengan Loading State
```tsx
<InputGroup>
  <InputGroupInput placeholder="Search..." />
  <InputGroupAddon align="inline-end">
    {isLoading ? (
      <Loader2Icon className="h-4 w-4 animate-spin" />
    ) : (
      <SearchIcon className="h-4 w-4" />
    )}
  </InputGroupAddon>
</InputGroup>
```

## Kustomisasi

### Custom Styling
```tsx
<InputGroup className="rounded-full bg-muted">
  <InputGroupAddon align="inline-start">
    <SearchIcon className="h-4 w-4" />
  </InputGroupAddon>
  <InputGroupInput className="bg-transparent" />
</InputGroup>
```

### Custom Border
```tsx
<InputGroup className="border-2 border-primary">
  <InputGroupInput placeholder="Custom border" />
</InputGroup>
```

## Best Practices

### 1. Gunakan untuk Input dengan Konteks
Input Group cocok untuk input yang memiliki konteks tambahan:
```tsx
// ✅ Baik: Input dengan konteks
<InputGroup>
  <InputGroupAddon>https://</InputGroupAddon>
  <InputGroupInput />
</InputGroup>

// ❌ Hindari: Input tanpa konteks
<InputGroup>
  <InputGroupAddon>🔍</InputGroupAddon>
  <InputGroupInput />
</InputGroup>
```

### 2. Konsisten dengan Ukuran
Gunakan ukuran yang konsisten untuk semua input dalam satu form:
```tsx
// Gunakan size yang sama untuk semua input group dalam satu form
<div className="space-y-4">
  <InputGroup size="sm">...</InputGroup>
  <InputGroup size="sm">...</InputGroup>
</div>
```

### 3. Aksesibilitas
Pastikan addon memiliki label yang sesuai untuk screen reader:
```tsx
<InputGroupAddon align="inline-start" aria-label="Search icon">
  <SearchIcon />
</InputGroupAddon>
```

## Troubleshooting

### Addon Tidak Sejajar
- Pastikan menggunakan align yang tepat (`inline-start` atau `inline-end` untuk horizontal)
- Periksa size prop konsisten

### Input Tidak Full Width
Input Group secara default full width. Untuk membatasi, tambahkan className:
```tsx
<InputGroup className="w-64">...</InputGroup>
```

## Referensi

- [Shadcn Input Documentation](https://ui.shadcn.com/docs/components/input)
- [Bootstrap Input Group](https://getbootstrap.com/docs/5.0/forms/input-group/)
- [Tailwind Forms](https://tailwindcss.com/docs/forms)
