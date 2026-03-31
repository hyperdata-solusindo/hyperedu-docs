---
title: "Checkbox"
sidebar_label: "Checkbox"
---

# Checkbox

**Lokasi:** `components/ui/checkbox.tsx`  
**Dokumentasi Resmi:** [Shadcn Checkbox](https://ui.shadcn.com/docs/components/checkbox)

## Deskripsi

Komponen checkbox dengan styling konsisten, dibangun di atas Radix UI Checkbox. Checkbox digunakan untuk memilih satu atau beberapa opsi dari daftar atau untuk mengkonfirmasi persetujuan seperti terms and conditions.

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `checked` | `boolean \| "indeterminate"` | `false` | Status checkbox (checked, unchecked, atau indeterminate) |
| `disabled` | `boolean` | `false` | Menonaktifkan checkbox |
| `onCheckedChange` | `(checked: boolean \| "indeterminate") => void` | - | Callback ketika status checkbox berubah |
| `className` | `string` | - | Custom CSS classes tambahan |

## State Checkbox

| State | Deskripsi | Visual |
|-------|-----------|--------|
| **Unchecked** | Checkbox tidak dipilih | Kotak kosong dengan border |
| **Checked** | Checkbox dipilih | Kotak dengan centang dan background primary |
| **Indeterminate** | State antara checked dan unchecked | Kotak dengan garis horizontal |

## Contoh Penggunaan

### Basic Checkbox
```tsx
<div className="flex items-center gap-2">
  <Checkbox id="terms" />
  <Label htmlFor="terms">Accept terms and conditions</Label>
</div>
```

### Checkbox dengan State
```tsx
function CheckboxDemo() {
  const [checked, setChecked] = useState(false);

  return (
    <div className="flex items-center gap-2">
      <Checkbox 
        id="notify" 
        checked={checked}
        onCheckedChange={setChecked}
      />
      <Label htmlFor="notify">Receive email notifications</Label>
      <p className="text-sm text-muted-foreground">
        {checked ? "Notifications enabled" : "Notifications disabled"}
      </p>
    </div>
  );
}
```

### Disabled Checkbox
```tsx
<div className="flex flex-col gap-2">
  <div className="flex items-center gap-2">
    <Checkbox id="disabled" disabled />
    <Label htmlFor="disabled" className="text-muted-foreground">
      Disabled unchecked
    </Label>
  </div>
  <div className="flex items-center gap-2">
    <Checkbox id="disabled-checked" disabled checked />
    <Label htmlFor="disabled-checked" className="text-muted-foreground">
      Disabled checked
    </Label>
  </div>
</div>
```

### Indeterminate State
```tsx
function IndeterminateCheckbox() {
  const [items, setItems] = useState([
    { id: 1, label: "Item 1", checked: false },
    { id: 2, label: "Item 2", checked: false },
    { id: 3, label: "Item 3", checked: false },
  ]);

  const allChecked = items.every(item => item.checked);
  const someChecked = items.some(item => item.checked) && !allChecked;

  const handleSelectAll = (checked: boolean | "indeterminate") => {
    setItems(items.map(item => ({ ...item, checked: checked === true })));
  };

  return (
    <div className="space-y-4">
      <div className="flex items-center gap-2 border-b pb-2">
        <Checkbox 
          checked={allChecked ? true : someChecked ? "indeterminate" : false}
          onCheckedChange={handleSelectAll}
        />
        <Label className="font-semibold">Select All</Label>
      </div>
      
      {items.map((item) => (
        <div key={item.id} className="flex items-center gap-2 pl-6">
          <Checkbox 
            checked={item.checked}
            onCheckedChange={(checked) => {
              setItems(items.map(i => 
                i.id === item.id ? { ...i, checked: checked === true } : i
              ));
            }}
          />
          <Label>{item.label}</Label>
        </div>
      ))}
    </div>
  );
}
```

### Checkbox Group
```tsx
function CheckboxGroup() {
  const [selected, setSelected] = useState<string[]>([]);

  const options = [
    { id: "react", label: "React" },
    { id: "vue", label: "Vue" },
    { id: "angular", label: "Angular" },
    { id: "svelte", label: "Svelte" },
  ];

  const handleCheck = (id: string, checked: boolean | "indeterminate") => {
    if (checked === true) {
      setSelected([...selected, id]);
    } else {
      setSelected(selected.filter(item => item !== id));
    }
  };

  return (
    <div className="space-y-3">
      <p className="text-sm font-medium">Select your favorite frameworks:</p>
      {options.map((option) => (
        <div key={option.id} className="flex items-center gap-2">
          <Checkbox 
            id={option.id}
            checked={selected.includes(option.id)}
            onCheckedChange={(checked) => handleCheck(option.id, checked)}
          />
          <Label htmlFor={option.id}>{option.label}</Label>
        </div>
      ))}
      <p className="text-sm text-muted-foreground mt-2">
        Selected: {selected.length} / {options.length}
      </p>
    </div>
  );
}
```

### Checkbox dalam Form (React Hook Form)
```tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const formSchema = z.object({
  newsletter: z.boolean().default(false),
  terms: z.boolean().refine(val => val === true, {
    message: "You must accept the terms",
  }),
});

function CheckboxForm() {
  const { register, handleSubmit, formState: { errors } } = useForm({
    resolver: zodResolver(formSchema),
    defaultValues: {
      newsletter: false,
      terms: false,
    },
  });

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div className="flex items-center gap-2">
        <Checkbox 
          id="newsletter" 
          {...register("newsletter")}
        />
        <Label htmlFor="newsletter">Subscribe to newsletter</Label>
      </div>

      <div className="flex items-center gap-2">
        <Checkbox 
          id="terms" 
          {...register("terms")}
        />
        <Label htmlFor="terms">I accept the terms and conditions</Label>
      </div>
      
      {errors.terms && (
        <p className="text-sm text-red-500">{errors.terms.message}</p>
      )}

      <Button type="submit">Submit</Button>
    </form>
  );
}
```

### Checkbox dalam Tabel (Row Selection)
```tsx
import { useReactTable, getCoreRowModel } from "@tanstack/react-table";

function DataTableWithCheckbox() {
  const [rowSelection, setRowSelection] = useState({});

  const columns = [
    {
      id: "select",
      header: ({ table }) => (
        <Checkbox
          checked={
            table.getIsAllPageRowsSelected() ||
            (table.getIsSomePageRowsSelected() && "indeterminate")
          }
          onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
        />
      ),
      cell: ({ row }) => (
        <Checkbox
          checked={row.getIsSelected()}
          onCheckedChange={(value) => row.toggleSelected(!!value)}
        />
      ),
    },
    {
      accessorKey: "name",
      header: "Name",
    },
    {
      accessorKey: "email",
      header: "Email",
    },
  ];

  const table = useReactTable({
    data,
    columns,
    state: { rowSelection },
    onRowSelectionChange: setRowSelection,
    getCoreRowModel: getCoreRowModel(),
  });

  return (
    <div>
      <Table>
        <TableHeader>
          {table.getHeaderGroups().map((headerGroup) => (
            <TableRow key={headerGroup.id}>
              {headerGroup.headers.map((header) => (
                <TableHead key={header.id}>
                  {flexRender(header.column.columnDef.header, header.getContext())}
                </TableHead>
              ))}
            </TableRow>
          ))}
        </TableHeader>
        <TableBody>
          {table.getRowModel().rows.map((row) => (
            <TableRow key={row.id}>
              {row.getVisibleCells().map((cell) => (
                <TableCell key={cell.id}>
                  {flexRender(cell.column.columnDef.cell, cell.getContext())}
                </TableCell>
              ))}
            </TableRow>
          ))}
        </TableBody>
      </Table>
      
      <div className="mt-4 text-sm text-muted-foreground">
        {Object.keys(rowSelection).length} row(s) selected
      </div>
    </div>
  );
}
```

### Checkbox dengan Card
```tsx
<Card className="w-96">
  <CardHeader>
    <CardTitle>Settings</CardTitle>
    <CardDescription>Configure your notification preferences</CardDescription>
  </CardHeader>
  <CardContent className="space-y-4">
    <div className="flex items-start gap-3">
      <Checkbox id="email" className="mt-0.5" />
      <div>
        <Label htmlFor="email" className="font-medium">Email Notifications</Label>
        <p className="text-sm text-muted-foreground">
          Receive updates about your account via email
        </p>
      </div>
    </div>
    
    <div className="flex items-start gap-3">
      <Checkbox id="push" className="mt-0.5" />
      <div>
        <Label htmlFor="push" className="font-medium">Push Notifications</Label>
        <p className="text-sm text-muted-foreground">
          Get real-time alerts on your device
        </p>
      </div>
    </div>
    
    <div className="flex items-start gap-3">
      <Checkbox id="sms" className="mt-0.5" />
      <div>
        <Label htmlFor="sms" className="font-medium">SMS Alerts</Label>
        <p className="text-sm text-muted-foreground">
          Important updates sent to your phone
        </p>
      </div>
    </div>
  </CardContent>
  <CardFooter>
    <Button className="w-full">Save Preferences</Button>
  </CardFooter>
</Card>
```

## Kustomisasi

### Custom Styling
```tsx
<Checkbox className="border-2 border-primary data-[state=checked]:bg-primary-600" />

<Checkbox className="rounded-full" />

<Checkbox className="h-5 w-5 border-2" />
```

### Custom Icon
Untuk mengganti ikon centang, modifikasi file checkbox.tsx:

```typescript
<CheckboxPrimitive.Indicator className="grid place-content-center text-current">
  <CheckIcon className="size-3.5" />
  {/* Ganti dengan ikon lain */}
  <StarIcon className="size-3.5" />
</CheckboxPrimitive.Indicator>
```

## Best Practices

### 1. Label yang Jelas
Selalu gunakan label yang jelas dan informatif:
```tsx
// ✅ Baik
<Checkbox id="terms" />
<Label htmlFor="terms">I agree to the Terms of Service and Privacy Policy</Label>

// ❌ Hindari: Label tidak jelas
<Checkbox id="terms" />
<Label htmlFor="terms">Agree</Label>
```

### 2. Klik pada Label
Gunakan `htmlFor` agar label dapat mengklik checkbox:
```tsx
<Checkbox id="option" />
<Label htmlFor="option">Option label</Label>
```

### 3. Group Checkbox
Untuk multiple checkbox, gunakan grup yang jelas:
```tsx
<fieldset className="space-y-2">
  <legend className="text-sm font-medium mb-2">Select options</legend>
  <div className="flex items-center gap-2">
    <Checkbox id="option1" />
    <Label htmlFor="option1">Option 1</Label>
  </div>
  <div className="flex items-center gap-2">
    <Checkbox id="option2" />
    <Label htmlFor="option2">Option 2</Label>
  </div>
</fieldset>
```

### 4. Indeterminate State
Gunakan indeterminate state untuk partial selection:
```tsx
// Ketika sebagian item dalam grup dipilih
const isIndeterminate = someSelected && !allSelected;
<Checkbox checked={allSelected ? true : isIndeterminate ? "indeterminate" : false} />
```

## Troubleshooting

### Checkbox Tidak Bisa Diklik
- Periksa apakah ada `disabled` prop
- Pastikan tidak ada elemen yang menutupi checkbox
- Cek apakah `onCheckedChange` berfungsi dengan benar

### Indeterminate State Tidak Terlihat
- Pastikan menggunakan nilai `"indeterminate"` bukan `true` atau `false`
- Indeterminate hanya visual, nilai tetap dianggap unchecked

```tsx
// ✅ Benar
<Checkbox checked={someChecked ? "indeterminate" : false} />

// ❌ Salah
<Checkbox checked="indeterminate" />
```

### Checkbox Tidak Centang
- Pastikan Anda menggunakan `checked` prop untuk controlled component
- Pastikan `onCheckedChange` mengupdate state dengan benar

## Referensi

- [Shadcn Checkbox Documentation](https://ui.shadcn.com/docs/components/checkbox)
- [Radix UI Checkbox](https://www.radix-ui.com/primitives/docs/components/checkbox)
- [MDN Checkbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox)
