---
title: "Scroll Area"
sidebar_label: "Scroll Area"
---

# Scroll Area

**Lokasi:** `components/ui/scroll-area.tsx`  
**Dokumentasi Resmi:** [Shadcn Scroll Area](https://ui.shadcn.com/docs/components/scroll-area)

## Deskripsi

Komponen area scroll dengan styling custom, dibangun di atas Radix UI Scroll Area. Scroll Area digunakan untuk membuat area scrollable dengan scrollbar yang dapat dikustomisasi, memberikan pengalaman scroll yang lebih baik dan konsisten di berbagai browser.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `ScrollArea` | Container utama dengan scroll functionality |
| `ScrollBar` | Scrollbar yang dapat dikustomisasi |

## Props

### ScrollArea
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `className` | `string` | - | Custom CSS classes |
| `children` | `React.ReactNode` | - | Konten yang akan di-scroll |

### ScrollBar
| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `orientation` | `"vertical" \| "horizontal"` | `"vertical"` | Orientasi scrollbar |

## Contoh Penggunaan

### Basic Scroll Area
```tsx
import { ScrollArea } from "@/components/ui/scroll-area";

<ScrollArea className="h-96 rounded-md border p-4">
  <div className="space-y-4">
    {Array.from({ length: 50 }).map((_, i) => (
      <p key={i} className="text-sm">
        Item {i + 1}: Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      </p>
    ))}
  </div>
</ScrollArea>
```

### Scroll Area dengan Fixed Height
```tsx
<ScrollArea className="h-[400px] w-full rounded-md border">
  <div className="p-4">
    <h4 className="mb-4 text-sm font-medium">Long Content</h4>
    {Array.from({ length: 30 }).map((_, i) => (
      <div key={i} className="mb-2 text-sm">
        Line {i + 1}: This is some scrollable content.
      </div>
    ))}
  </div>
</ScrollArea>
```

### Scroll Area Horizontal
```tsx
<ScrollArea className="w-full whitespace-nowrap rounded-md border">
  <div className="flex space-x-4 p-4">
    {Array.from({ length: 20 }).map((_, i) => (
      <div
        key={i}
        className="flex h-24 w-32 items-center justify-center rounded-md bg-muted"
      >
        Item {i + 1}
      </div>
    ))}
  </div>
  <ScrollBar orientation="horizontal" />
</ScrollArea>
```

### Scroll Area dengan Custom Scrollbar Styling
```tsx
<ScrollArea className="h-96 rounded-md border">
  <div className="p-4 space-y-4">
    {/* content */}
  </div>
  <ScrollBar className="w-2 bg-muted-foreground/20" />
</ScrollArea>
```

### Scroll Area dalam Dialog
```tsx
<Dialog>
  <DialogTrigger asChild>
    <Button>Open Terms</Button>
  </DialogTrigger>
  <DialogContent className="max-h-[80vh]">
    <DialogHeader>
      <DialogTitle>Terms and Conditions</DialogTitle>
      <DialogDescription>Please read the terms carefully</DialogDescription>
    </DialogHeader>
    <ScrollArea className="h-[400px] pr-4">
      <div className="space-y-4">
        <h4 className="font-semibold">1. Acceptance of Terms</h4>
        <p className="text-sm text-muted-foreground">
          By accessing and using this service, you accept and agree to be bound by the terms
          and provision of this agreement. In addition, when using this particular service,
          you shall be subject to any posted guidelines or rules applicable to such services.
        </p>
        <h4 className="font-semibold">2. Description of Service</h4>
        <p className="text-sm text-muted-foreground">
          HyperEdu provides users with access to a rich collection of educational resources,
          including various communications tools, forums, shopping services, and personalized
          content through its network of properties.
        </p>
        <h4 className="font-semibold">3. User Conduct</h4>
        <p className="text-sm text-muted-foreground">
          You understand that all information, data, text, software, music, sound, photographs,
          graphics, video, messages or other materials, whether publicly posted or privately
          transmitted, are the sole responsibility of the person from which such content originated.
        </p>
      </div>
    </ScrollArea>
    <DialogFooter>
      <Button variant="outline">Decline</Button>
      <Button>Accept</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

### Scroll Area dalam Sheet
```tsx
<Sheet>
  <SheetTrigger asChild>
    <Button>Open Notifications</Button>
  </SheetTrigger>
  <SheetContent className="w-[400px] sm:w-[540px]">
    <SheetHeader>
      <SheetTitle>Notifications</SheetTitle>
      <SheetDescription>Your recent notifications</SheetDescription>
    </SheetHeader>
    <ScrollArea className="h-[calc(100vh-120px)] mt-4">
      <div className="space-y-4 pr-4">
        {Array.from({ length: 50 }).map((_, i) => (
          <div key={i} className="flex items-start gap-4 rounded-lg border p-4">
            <div className="h-10 w-10 rounded-full bg-primary/10 flex items-center justify-center">
              <BellIcon className="h-5 w-5" />
            </div>
            <div className="flex-1 space-y-1">
              <p className="text-sm font-medium">Notification {i + 1}</p>
              <p className="text-xs text-muted-foreground">
                This is a sample notification message that might be longer.
              </p>
              <p className="text-xs text-muted-foreground">2 minutes ago</p>
            </div>
          </div>
        ))}
      </div>
    </ScrollArea>
    <SheetFooter>
      <Button variant="outline" className="w-full">Mark all as read</Button>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

### Scroll Area dengan Auto-scroll to Bottom
```tsx
function ChatScrollArea() {
  const scrollRef = useRef<HTMLDivElement>(null);

  const scrollToBottom = () => {
    if (scrollRef.current) {
      scrollRef.current.scrollTop = scrollRef.current.scrollHeight;
    }
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  return (
    <ScrollArea ref={scrollRef} className="h-[400px] rounded-md border">
      <div className="space-y-4 p-4">
        {messages.map((message, i) => (
          <div key={i} className="flex gap-3">
            <Avatar className="h-8 w-8">
              <AvatarFallback>{message.sender[0]}</AvatarFallback>
            </Avatar>
            <div className="space-y-1">
              <p className="text-sm font-medium">{message.sender}</p>
              <p className="text-sm text-muted-foreground">{message.content}</p>
            </div>
          </div>
        ))}
      </div>
    </ScrollArea>
  );
}
```

### Scroll Area dengan Gradient Fade
```tsx
<div className="relative">
  <ScrollArea className="h-96 rounded-md border">
    <div className="space-y-4 p-4">
      {/* long content */}
    </div>
  </ScrollArea>
  <div className="pointer-events-none absolute bottom-0 left-0 right-0 h-12 bg-gradient-to-t from-background to-transparent" />
</div>
```

### Scroll Area untuk List dengan Infinite Scroll
```tsx
function InfiniteScrollList() {
  const [items, setItems] = useState<number[]>(Array.from({ length: 20 }, (_, i) => i + 1));
  const [isLoading, setIsLoading] = useState(false);
  const scrollRef = useRef<HTMLDivElement>(null);

  const loadMore = () => {
    if (isLoading) return;
    setIsLoading(true);
    setTimeout(() => {
      setItems(prev => [...prev, ...Array.from({ length: 10 }, (_, i) => prev.length + i + 1)]);
      setIsLoading(false);
    }, 1000);
  };

  const handleScroll = (e: React.UIEvent<HTMLDivElement>) => {
    const { scrollTop, scrollHeight, clientHeight } = e.currentTarget;
    if (scrollHeight - scrollTop <= clientHeight + 100 && !isLoading) {
      loadMore();
    }
  };

  return (
    <ScrollArea className="h-96 rounded-md border" onScroll={handleScroll}>
      <div className="space-y-2 p-4">
        {items.map((item) => (
          <div key={item} className="rounded-lg border p-3">
            Item {item}
          </div>
        ))}
        {isLoading && (
          <div className="flex justify-center py-4">
            <Loader2Icon className="h-6 w-6 animate-spin" />
          </div>
        )}
      </div>
    </ScrollArea>
  );
}
```

## Kustomisasi

### Custom Scrollbar Width
```tsx
<ScrollArea className="h-96 [&_[data-slot=scroll-area-scrollbar]]:w-2">
  {/* content */}
</ScrollArea>
```

### Custom Scrollbar Color
```tsx
<ScrollArea className="h-96 [&_[data-slot=scroll-area-thumb]]:bg-primary">
  {/* content */}
</ScrollArea>
```

### Hide Scrollbar
```tsx
<ScrollArea className="h-96 [&_[data-slot=scroll-area-scrollbar]]:hidden">
  {/* content */}
</ScrollArea>
```

## Best Practices

### 1. Set Height yang Jelas
ScrollArea memerlukan height yang ditentukan untuk berfungsi:
```tsx
// ✅ Baik: Dengan height yang jelas
<ScrollArea className="h-96">...</ScrollArea>

// ❌ Hindari: Tanpa height
<ScrollArea>...</ScrollArea>
```

### 2. Gunakan untuk Konten Panjang
ScrollArea ideal untuk konten yang panjang seperti daftar, dokumen, atau chat.

### 3. Konsisten dengan Padding
Tambahkan padding pada konten untuk spacing yang baik:
```tsx
<ScrollArea>
  <div className="p-4">...</div>
</ScrollArea>
```

## Troubleshooting

### Scroll Tidak Berfungsi
- Pastikan container memiliki height yang ditentukan
- Periksa apakah konten melebihi height container

### Scrollbar Tidak Muncul
- Scrollbar hanya muncul saat konten melebihi container
- Gunakan `overflow-auto` jika perlu

## Referensi

- [Shadcn Scroll Area Documentation](https://ui.shadcn.com/docs/components/scroll-area)
- [Radix UI Scroll Area](https://www.radix-ui.com/primitives/docs/components/scroll-area)
- [MDN Scrollbar Styling](https://developer.mozilla.org/en-US/docs/Web/CSS/::-webkit-scrollbar)
