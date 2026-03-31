---
title: "IconCombobox"
sidebar_label: "IconCombobox"
---

# IconCombobox

**Lokasi:** `components/custom/icon-combobox.tsx`

## Deskripsi

Combobox untuk memilih ikon dari library Lucide React. IconCombobox menyediakan antarmuka yang mudah untuk memilih ikon dari koleksi Lucide yang lengkap, dengan fitur pencarian real-time dan infinite scroll untuk memuat lebih banyak ikon.

## Fitur

- Pencarian ikon secara real-time
- Infinite scroll untuk memuat lebih banyak ikon
- Preview ikon dalam dropdown
- Fokus otomatis pada input pencarian saat dropdown terbuka
- Menampilkan nama ikon beserta preview

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `defaultValue` | `any` | Tidak | - | Nilai default ikon (nama ikon) |
| `onChange` | `(value: any) => void` | Tidak | - | Handler perubahan ikon |

## Contoh Penggunaan

### Basic Usage
```tsx
import { IconCombobox } from "@/components/custom/icon-combobox";

function IconPicker() {
  const [selectedIcon, setSelectedIcon] = useState("Home");

  return (
    <div className="space-y-4">
      <div className="flex items-center gap-4">
        <div className="p-2 border rounded-lg">
          {selectedIcon && <LazyIcon name={selectedIcon} size={24} />}
        </div>
        <IconCombobox
          defaultValue={selectedIcon}
          onChange={setSelectedIcon}
        />
      </div>
      <p className="text-sm text-muted-foreground">
        Selected icon: {selectedIcon}
      </p>
    </div>
  );
}
```

### Dalam Form
```tsx
import { useForm, Controller } from "react-hook-form";

function MenuForm() {
  const { control, handleSubmit } = useForm({
    defaultValues: {
      name: "",
      icon: "Circle",
    },
  });

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div className="space-y-1.5">
        <Label>Menu Name</Label>
        <Input {...register("name")} />
      </div>

      <div className="space-y-1.5">
        <Label>Icon</Label>
        <Controller
          name="icon"
          control={control}
          render={({ field }) => (
            <IconCombobox
              defaultValue={field.value}
              onChange={field.onChange}
            />
          )}
        />
      </div>

      <Button type="submit">Save Menu</Button>
    </form>
  );
}
```

### Dengan Preview
```tsx
function IconPickerWithPreview() {
  const [selectedIcon, setSelectedIcon] = useState("Settings");

  return (
    <div className="space-y-4">
      <div className="flex items-center gap-6 p-4 border rounded-lg bg-muted/30">
        <div className="flex flex-col items-center gap-2">
          <div className="w-12 h-12 rounded-lg bg-primary/10 flex items-center justify-center">
            <LazyIcon name={selectedIcon} size={24} className="text-primary" />
          </div>
          <span className="text-xs text-muted-foreground">Preview</span>
        </div>
        
        <div className="flex-1">
          <Label>Select Icon</Label>
          <IconCombobox
            defaultValue={selectedIcon}
            onChange={setSelectedIcon}
          />
        </div>
      </div>
      
      <div className="text-sm text-muted-foreground">
        Selected: <code className="bg-muted px-1 py-0.5 rounded">{selectedIcon}</code>
      </div>
    </div>
  );
}
```

### Dalam Sidebar Menu Editor
```tsx
function MenuItemEditor() {
  const [menuItems, setMenuItems] = useState([
    { id: 1, name: "Dashboard", icon: "LayoutDashboard" },
    { id: 2, name: "Users", icon: "Users" },
    { id: 3, name: "Settings", icon: "Settings" },
  ]);

  const updateIcon = (id: number, icon: string) => {
    setMenuItems(prev =>
      prev.map(item =>
        item.id === id ? { ...item, icon } : item
      )
    );
  };

  return (
    <div className="space-y-4">
      {menuItems.map((item) => (
        <div key={item.id} className="flex items-center gap-4 p-3 border rounded-lg">
          <div className="w-8 h-8 bg-primary/10 rounded-lg flex items-center justify-center">
            <LazyIcon name={item.icon} size={16} />
          </div>
          <div className="flex-1">
            <p className="font-medium">{item.name}</p>
          </div>
          <IconCombobox
            defaultValue={item.icon}
            onChange={(icon) => updateIcon(item.id, icon)}
          />
        </div>
      ))}
    </div>
  );
}
```

## Kustomisasi

### Custom Dropdown Width
```tsx
<IconCombobox className="w-80" />
```

### Custom Button Variant
Button styling dapat dimodifikasi melalui className:
```tsx
<IconCombobox className="[&_[role=combobox]]:border-primary" />
```

## Best Practices

### 1. Gunakan untuk Pemilihan Ikon
IconCombobox ideal untuk memilih ikon dari koleksi Lucide:
```tsx
// ✅ Baik: Memilih ikon untuk menu, button, dll
<IconCombobox onChange={setIcon} />

// ❌ Hindari: Untuk pemilihan selain ikon
```

### 2. Preview Sebelum Memilih
Tampilkan preview ikon yang dipilih untuk memberikan feedback visual:
```tsx
<div className="flex items-center gap-2">
  <LazyIcon name={selectedIcon} size={20} />
  <IconCombobox value={selectedIcon} onChange={setSelectedIcon} />
</div>
```

### 3. Default Value
Selalu berikan defaultValue untuk state awal:
```tsx
<IconCombobox defaultValue="Circle" onChange={setIcon} />
```

## Troubleshooting

### Ikon Tidak Muncul
- Pastikan nama ikon sesuai dengan nama di Lucide (PascalCase)
- Periksa apakah LazyIcon sudah di-import dengan benar

### Pencarian Tidak Berfungsi
- IconCombobox melakukan filter client-side pada semua ikon Lucide
- Pastikan tidak ada error di console

### Dropdown Tidak Terbuka
- Periksa apakah ada CSS yang menimpa visibility
- Pastikan komponen Combobox berfungsi dengan benar

## Referensi

- [Lucide Icons](https://lucide.dev/icons/)
- [Shadcn Combobox Documentation](https://ui.shadcn.com/docs/components/combobox)
- [LazyIcon Component](./lazy-icon.md)
