---
title: "Textarea"
sidebar_label: "Textarea"
---

# Textarea

**Lokasi:** `components/ui/textarea.tsx`  
**Dokumentasi Resmi:** [Shadcn Textarea](https://ui.shadcn.com/docs/components/textarea)

## Deskripsi

Komponen textarea dengan styling konsisten untuk input teks multiline. Textarea digunakan untuk mengumpulkan input teks yang lebih panjang seperti deskripsi, komentar, atau catatan.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `variant` | `"default"` | `"default"` | Varian textarea (saat ini hanya default) |
| `size` | `"default" \| "sm" \| "xs"` | `"default"` | Ukuran textarea |
| `placeholder` | `string` | - | Placeholder text |
| `disabled` | `boolean` | `false` | Menonaktifkan textarea |
| `rows` | `number` | `4` | Jumlah baris default |
| `className` | `string` | - | Custom CSS classes tambahan |

## Ukuran

| Ukuran | Min Height | Font Size | Padding | Penggunaan |
|--------|------------|-----------|---------|------------|
| **xs** | `h-16` | `12px` | `px-3 py-2` | Textarea kecil untuk input singkat |
| **sm** | `h-16` | `12px` | `px-3 py-2` | Textarea compact |
| **default** | `h-16` | `12px` | `px-3 py-2` | Textarea standar |

## Contoh Penggunaan

### Basic Textarea
```tsx
import { Textarea } from "@/components/ui/textarea";

<Textarea 
  placeholder="Type your message here." 
  rows={4}
/>
```

### Textarea dengan Ukuran
```tsx
<div className="space-y-4">
  <Textarea size="xs" placeholder="Extra small textarea" rows={2} />
  <Textarea size="sm" placeholder="Small textarea" rows={3} />
  <Textarea size="default" placeholder="Default textarea" rows={4} />
</div>
```

### Textarea dengan Label
```tsx
<div className="space-y-1.5">
  <Label htmlFor="description">Description</Label>
  <Textarea 
    id="description" 
    placeholder="Enter product description..." 
    rows={4}
  />
  <p className="text-xs text-muted-foreground">
    Max 500 characters
  </p>
</div>
```

### Textarea dengan Counter Karakter
```tsx
function CharacterCounter() {
  const [value, setValue] = useState("");
  const maxLength = 500;

  return (
    <div className="space-y-1.5">
      <Label htmlFor="bio">Bio</Label>
      <Textarea
        id="bio"
        placeholder="Tell us about yourself..."
        rows={4}
        value={value}
        onChange={(e) => setValue(e.target.value)}
        maxLength={maxLength}
      />
      <p className="text-xs text-muted-foreground text-right">
        {value.length}/{maxLength} characters
      </p>
    </div>
  );
}
```

### Textarea dengan Error State
```tsx
<Textarea 
  className="border-red-500 focus-visible:ring-red-500/20" 
  placeholder="Error state"
/>
```

### Textarea dalam Form (React Hook Form)
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const formSchema = z.object({
  description: z.string().min(10, "Description must be at least 10 characters"),
});

function ProductForm() {
  const { register, handleSubmit, formState: { errors } } = useForm({
    resolver: zodResolver(formSchema),
  });

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div className="space-y-1.5">
        <Label htmlFor="description">Description</Label>
        <Textarea
          id="description"
          placeholder="Enter product description..."
          rows={4}
          {...register("description")}
          className={errors.description ? "border-red-500" : ""}
        />
        {errors.description && (
          <p className="text-xs text-red-500">{errors.description.message}</p>
        )}
      </div>
      <Button type="submit">Submit</Button>
    </form>
  );
}
```

### Textarea dengan Auto-resize (Menggunakan CSS)
```tsx
// Auto-resize menggunakan CSS field-sizing
<Textarea 
  placeholder="Type here... (auto-resizing)"
  className="field-sizing-content min-h-16"
/>
```

### Textarea dengan Disabled State
```tsx
<Textarea 
  disabled 
  placeholder="Disabled textarea"
  value="This textarea is disabled"
/>
```

### Textarea dengan Readonly State
```tsx
<Textarea 
  readOnly 
  placeholder="Readonly textarea"
  value="This content cannot be edited"
/>
```

### Textarea dalam Card
```tsx
<Card>
  <CardHeader>
    <CardTitle>Add Comment</CardTitle>
    <CardDescription>Share your thoughts about this product</CardDescription>
  </CardHeader>
  <CardContent>
    <Textarea 
      placeholder="Write your comment here..." 
      rows={4}
    />
  </CardContent>
  <CardFooter className="flex justify-end">
    <Button>Post Comment</Button>
  </CardFooter>
</Card>
```

### Textarea dengan Tombol Aksi
```tsx
<div className="space-y-2">
  <Textarea 
    placeholder="Write your feedback..."
    rows={3}
  />
  <div className="flex justify-end gap-2">
    <Button variant="outline" size="sm">Cancel</Button>
    <Button size="sm">Submit</Button>
  </div>
</div>
```

### Textarea dalam Modal
```tsx
<Dialog>
  <DialogTrigger asChild>
    <Button>Write Review</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Write a Review</DialogTitle>
      <DialogDescription>
        Share your experience with this product
      </DialogDescription>
    </DialogHeader>
    <div className="space-y-4 py-4">
      <Textarea 
        placeholder="What did you like or dislike?" 
        rows={5}
      />
    </div>
    <DialogFooter>
      <Button variant="outline">Cancel</Button>
      <Button>Submit Review</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

### Textarea untuk Kode
```tsx
<Textarea 
  placeholder="Paste your code here..."
  className="font-mono text-xs"
  rows={8}
/>
```

### Textarea dengan Format JSON Preview
```tsx
function JsonTextarea() {
  const [jsonInput, setJsonInput] = useState("");
  const [isValid, setIsValid] = useState(true);

  const handleChange = (e: React.ChangeEvent<HTMLTextAreaElement>) => {
    const value = e.target.value;
    setJsonInput(value);
    try {
      JSON.parse(value);
      setIsValid(true);
    } catch {
      setIsValid(false);
    }
  };

  return (
    <div className="space-y-1.5">
      <Label>JSON Input</Label>
      <Textarea
        placeholder='{"key": "value"}'
        value={jsonInput}
        onChange={handleChange}
        rows={6}
        className={!isValid ? "border-red-500" : ""}
      />
      {!isValid && (
        <p className="text-xs text-red-500">Invalid JSON format</p>
      )}
    </div>
  );
}
```

## Kustomisasi

### Custom Styling
```tsx
<Textarea className="bg-muted border-none focus-visible:ring-0" />

<Textarea className="rounded-none border-2 border-primary" />

<Textarea className="font-serif text-base" />
```

### Custom Variant
Untuk menambahkan varian baru, modifikasi `textAreaVariants` di `textarea.tsx`:

```typescript
const textAreaVariants = cva(
  cn(
    "min-h-16 w-full rounded-md px-3 py-2 shadow-xs transition-[color,box-shadow] outline-none",
    "focus-visible:ring-[3px] disabled:cursor-not-allowed disabled:opacity-50"
  ),
  {
    variants: {
      variant: {
        default: cn(
          "bg-transparent border border-input",
          "placeholder:text-muted-foreground",
          "focus-visible:border-primary focus-visible:ring-primary/20"
        ),
        filled: cn(
          "bg-muted border-transparent",
          "focus-visible:bg-background focus-visible:border-primary"
        ),
      },
      size: {
        // ... existing sizes
      },
    },
  }
);
```

## Best Practices

### 1. Gunakan Label yang Jelas
```tsx
// ✅ Baik
<Label htmlFor="description">Product Description</Label>
<Textarea id="description" placeholder="Describe your product..." />

// ❌ Hindari: Tanpa label
<Textarea placeholder="Description" />
```

### 2. Tampilkan Batasan Karakter
Untuk textarea dengan batasan panjang, tampilkan counter:
```tsx
<div className="space-y-1.5">
  <Textarea maxLength={500} />
  <p className="text-xs text-muted-foreground text-right">
    {value.length}/500 characters
  </p>
</div>
```

### 3. Gunakan Rows yang Tepat
Sesuaikan jumlah rows dengan konten yang diharapkan:
```tsx
<Textarea rows={3} />  // Untuk komentar singkat
<Textarea rows={6} />  // Untuk deskripsi produk
<Textarea rows={10} /> // Untuk konten panjang seperti artikel
```

### 4. Validasi Real-time
Untuk form penting, lakukan validasi real-time:
```tsx
<Textarea 
  {...register("bio", {
    minLength: { value: 10, message: "Minimum 10 characters" },
    maxLength: { value: 200, message: "Maximum 200 characters" }
  })}
/>
```

## Troubleshooting

### Textarea Tidak Bisa Di-resize
Textarea secara default dapat di-resize. Untuk mencegah:
```tsx
<Textarea className="resize-none" />
```

### Auto-resize Tidak Bekerja
Gunakan CSS `field-sizing-content` untuk auto-resize:
```tsx
<Textarea className="field-sizing-content min-h-16" />
```

### Scrollbar Muncul Tidak Perlu
Gunakan `overflow-hidden` jika diperlukan:
```tsx
<Textarea className="overflow-hidden resize-none" />
```

## Referensi

- [Shadcn Textarea Documentation](https://ui.shadcn.com/docs/components/textarea)
- [MDN Textarea Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/textarea)
- [CSS field-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/field-sizing)
