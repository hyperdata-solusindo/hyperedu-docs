---
title: "Collapsible"
sidebar_label: "Collapsible"
---

# Collapsible

**Lokasi:** `components/ui/collapsible.tsx`  
**Dokumentasi Resmi:** [Shadcn Collapsible](https://ui.shadcn.com/docs/components/collapsible)

## Deskripsi

Komponen untuk area yang dapat dilipat/dibuka, dibangun di atas Radix UI Collapsible. Collapsible digunakan untuk menyembunyikan konten yang tidak selalu perlu ditampilkan, seperti menu dropdown, filter panel, atau detail tambahan.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Collapsible` | Container utama collapsible |
| `CollapsibleTrigger` | Trigger untuk membuka/menutup collapsible |
| `CollapsibleContent` | Konten yang dapat dilipat |

## Props

### Collapsible
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `open` | `boolean` | - | Status collapsible (controlled) |
| `defaultOpen` | `boolean` | `false` | Status default (uncontrolled) |
| `onOpenChange` | `(open: boolean) => void` | - | Callback ketika status berubah |
| `disabled` | `boolean` | `false` | Menonaktifkan collapsible |

## Contoh Penggunaan

### Basic Collapsible
```tsx
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/collapsible";

<Collapsible>
  <CollapsibleTrigger asChild>
    <Button variant="ghost">
      Toggle Content
      <ChevronDownIcon className="ml-2 h-4 w-4" />
    </Button>
  </CollapsibleTrigger>
  <CollapsibleContent className="mt-2 p-4 border rounded-md">
    <p>This is the collapsible content that can be shown or hidden.</p>
  </CollapsibleContent>
</Collapsible>
```

### Controlled Collapsible
```tsx
function ControlledCollapsible() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <Collapsible open={isOpen} onOpenChange={setIsOpen}>
      <CollapsibleTrigger asChild>
        <Button variant="outline">
          {isOpen ? "Hide Details" : "Show Details"}
          {isOpen ? (
            <ChevronUpIcon className="ml-2 h-4 w-4" />
          ) : (
            <ChevronDownIcon className="ml-2 h-4 w-4" />
          )}
        </Button>
      </CollapsibleTrigger>
      <CollapsibleContent className="mt-4 space-y-2">
        <div className="p-4 bg-muted rounded-lg">
          <p className="text-sm">Detailed information here...</p>
        </div>
      </CollapsibleContent>
    </Collapsible>
  );
}
```

### Collapsible dengan Ikon Animasi
```tsx
<Collapsible>
  <CollapsibleTrigger asChild>
    <Button variant="ghost" className="gap-2">
      <ChevronRightIcon className="h-4 w-4 transition-transform group-data-[state=open]/collapsible:rotate-90" />
      <span>Toggle Section</span>
    </Button>
  </CollapsibleTrigger>
  <CollapsibleContent className="mt-2 pl-6">
    <div className="space-y-2">
      <p>Collapsible content with animated icon</p>
      <p>More content here...</p>
    </div>
  </CollapsibleContent>
</Collapsible>
```

### Collapsible dalam Sidebar (Menu)
```tsx
<SidebarMenu>
  <SidebarMenuItem>
    <Collapsible>
      <CollapsibleTrigger asChild>
        <SidebarMenuButton>
          <SettingsIcon className="h-4 w-4" />
          <span>Settings</span>
          <ChevronDownIcon className="ml-auto h-4 w-4" />
        </SidebarMenuButton>
      </CollapsibleTrigger>
      <CollapsibleContent>
        <SidebarMenuSub>
          <SidebarMenuSubButton asChild>
            <a href="/settings/profile">Profile</a>
          </SidebarMenuSubButton>
          <SidebarMenuSubButton asChild>
            <a href="/settings/account">Account</a>
          </SidebarMenuSubButton>
          <SidebarMenuSubButton asChild>
            <a href="/settings/security">Security</a>
          </SidebarMenuSubButton>
        </SidebarMenuSub>
      </CollapsibleContent>
    </Collapsible>
  </SidebarMenuItem>
</SidebarMenu>
```

### Collapsible dalam Card
```tsx
<Card>
  <CardHeader>
    <div className="flex items-center justify-between">
      <CardTitle>Order Summary</CardTitle>
      <Collapsible>
        <CollapsibleTrigger asChild>
          <Button variant="ghost" size="sm">
            Show Details
            <ChevronDownIcon className="ml-1 h-4 w-4" />
          </Button>
        </CollapsibleTrigger>
        <CollapsibleContent>
          <div className="mt-4 space-y-2">
            <div className="flex justify-between">
              <span>Subtotal</span>
              <span>$99.99</span>
            </div>
            <div className="flex justify-between">
              <span>Shipping</span>
              <span>$5.00</span>
            </div>
            <Separator />
            <div className="flex justify-between font-bold">
              <span>Total</span>
              <span>$104.99</span>
            </div>
          </div>
        </CollapsibleContent>
      </Collapsible>
    </div>
  </CardHeader>
  <CardContent>
    <p>Basic order information</p>
  </CardContent>
</Card>
```

### Nested Collapsible
```tsx
<Collapsible>
  <CollapsibleTrigger asChild>
    <Button variant="outline" className="w-full justify-between">
      <span>Level 1</span>
      <ChevronDownIcon className="h-4 w-4" />
    </Button>
  </CollapsibleTrigger>
  <CollapsibleContent className="mt-2 pl-4">
    <Collapsible>
      <CollapsibleTrigger asChild>
        <Button variant="ghost" className="w-full justify-between">
          <span>Level 2</span>
          <ChevronDownIcon className="h-4 w-4" />
        </Button>
      </CollapsibleTrigger>
      <CollapsibleContent className="mt-2 pl-4">
        <p className="p-2 bg-muted rounded">Nested content</p>
      </CollapsibleContent>
    </Collapsible>
  </CollapsibleContent>
</Collapsible>
```

### Collapsible dengan Multiple Items (FAQ)
```tsx
function FAQ() {
  const faqs = [
    { question: "What is HyperEdu?", answer: "HyperEdu is an educational platform..." },
    { question: "How to get started?", answer: "Sign up for an account and start learning..." },
    { question: "Is there a free trial?", answer: "Yes, we offer a 14-day free trial..." },
  ];

  return (
    <div className="space-y-4">
      {faqs.map((faq, index) => (
        <Collapsible key={index}>
          <CollapsibleTrigger asChild>
            <Button variant="outline" className="w-full justify-between">
              <span className="font-medium">{faq.question}</span>
              <ChevronDownIcon className="h-4 w-4" />
            </Button>
          </CollapsibleTrigger>
          <CollapsibleContent className="mt-2 p-4 bg-muted rounded-lg">
            <p className="text-sm text-muted-foreground">{faq.answer}</p>
          </CollapsibleContent>
        </Collapsible>
      ))}
    </div>
  );
}
```

### Collapsible dengan Filter Panel
```tsx
function FilterPanel() {
  const [filters, setFilters] = useState({
    category: false,
    price: false,
    rating: false,
  });

  const toggleFilter = (key: keyof typeof filters) => {
    setFilters(prev => ({ ...prev, [key]: !prev[key] }));
  };

  return (
    <Card>
      <CardHeader>
        <CardTitle>Filters</CardTitle>
      </CardHeader>
      <CardContent className="space-y-4">
        <Collapsible open={filters.category} onOpenChange={() => toggleFilter("category")}>
          <CollapsibleTrigger asChild>
            <Button variant="ghost" className="w-full justify-between p-0 h-auto">
              <span className="font-medium">Category</span>
              <ChevronDownIcon className="h-4 w-4 transition-transform" />
            </Button>
          </CollapsibleTrigger>
          <CollapsibleContent className="mt-2 space-y-2">
            <div className="flex items-center gap-2">
              <Checkbox id="electronics" />
              <Label htmlFor="electronics">Electronics</Label>
            </div>
            <div className="flex items-center gap-2">
              <Checkbox id="clothing" />
              <Label htmlFor="clothing">Clothing</Label>
            </div>
          </CollapsibleContent>
        </Collapsible>

        <Separator />

        <Collapsible open={filters.price} onOpenChange={() => toggleFilter("price")}>
          <CollapsibleTrigger asChild>
            <Button variant="ghost" className="w-full justify-between p-0 h-auto">
              <span className="font-medium">Price Range</span>
              <ChevronDownIcon className="h-4 w-4 transition-transform" />
            </Button>
          </CollapsibleTrigger>
          <CollapsibleContent className="mt-2">
            <div className="flex gap-2">
              <Input placeholder="Min" type="number" />
              <Input placeholder="Max" type="number" />
            </div>
          </CollapsibleContent>
        </Collapsible>
      </CardContent>
    </Card>
  );
}
```

### Collapsible dengan Loading State
```tsx
function AsyncCollapsible() {
  const [isOpen, setIsOpen] = useState(false);
  const [isLoading, setIsLoading] = useState(false);
  const [data, setData] = useState(null);

  const handleOpenChange = async (open: boolean) => {
    setIsOpen(open);
    if (open && !data) {
      setIsLoading(true);
      const result = await fetchData();
      setData(result);
      setIsLoading(false);
    }
  };

  return (
    <Collapsible open={isOpen} onOpenChange={handleOpenChange}>
      <CollapsibleTrigger asChild>
        <Button variant="outline">
          {isOpen ? "Hide" : "Show"} Details
          {isLoading && <Loader2Icon className="ml-2 h-4 w-4 animate-spin" />}
        </Button>
      </CollapsibleTrigger>
      <CollapsibleContent className="mt-4">
        {isLoading ? (
          <Skeleton className="h-32 w-full" />
        ) : data ? (
          <pre className="p-4 bg-muted rounded-lg">{JSON.stringify(data, null, 2)}</pre>
        ) : null}
      </CollapsibleContent>
    </Collapsible>
  );
}
```

## Kustomisasi

### Custom Animation
Collapsible memiliki animasi bawaan dari Radix UI. Untuk kustomisasi:
```tsx
<CollapsibleContent className="data-[state=open]:animate-slide-down data-[state=closed]:animate-slide-up">
  {/* content */}
</CollapsibleContent>
```

### Custom Styling
```tsx
<CollapsibleTrigger className="bg-primary text-primary-foreground hover:bg-primary/90">
  Custom Trigger
</CollapsibleTrigger>

<CollapsibleContent className="bg-muted rounded-lg p-4">
  Custom Content
</CollapsibleContent>
```

## Best Practices

### 1. Indikator Visual yang Jelas
Selalu berikan indikator bahwa konten dapat dilipat:
```tsx
// ✅ Baik: Ikon chevron menunjukkan arah
<CollapsibleTrigger>
  <span>Section</span>
  <ChevronDownIcon className="ml-auto" />
</CollapsibleTrigger>

// ❌ Hindari: Tanpa indikator
<CollapsibleTrigger>
  <span>Section</span>
</CollapsibleTrigger>
```

### 2. Animasi Halus
Gunakan transisi untuk pengalaman yang lebih baik:
```tsx
<CollapsibleContent className="transition-all data-[state=open]:animate-slide-down">
  {/* content */}
</CollapsibleContent>
```

### 3. Accessibility
Pastikan trigger dapat diakses dengan keyboard dan memiliki label yang sesuai.

## Troubleshooting

### Content Tidak Muncul Saat Dibuka
- Pastikan `CollapsibleContent` di-render dengan benar
- Periksa apakah ada CSS yang menyembunyikan content

### Animasi Tidak Halus
- Tambahkan transition pada CollapsibleContent
- Pastikan tidak ada CSS yang meng-override

## Referensi

- [Shadcn Collapsible Documentation](https://ui.shadcn.com/docs/components/collapsible)
- [Radix UI Collapsible](https://www.radix-ui.com/primitives/docs/components/collapsible)
- [MDN Details/Summary](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details)
