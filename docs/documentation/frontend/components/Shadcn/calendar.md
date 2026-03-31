---
title: "Calendar"
sidebar_label: "Calendar"
---

# Calendar

**Lokasi:** `components/ui/calendar.tsx`  
**Dokumentasi Resmi:** [Shadcn Calendar](https://ui.shadcn.com/docs/components/calendar)

## Deskripsi

Komponen kalender untuk memilih tanggal, built on top of react-day-picker. Calendar digunakan untuk input tanggal dengan tampilan kalender interaktif, mendukung single date selection, range selection, dan dropdown untuk memilih bulan/tahun.

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `Calendar` | Komponen kalender utama dengan berbagai mode selection |
| `CalendarDayButton` | Tombol untuk setiap hari dalam kalender (internal) |

## Props

| Prop | Tipe | Default | Deskripsi |
|------|------|---------|-----------|
| `mode` | `"single" \| "range" \| "multiple"` | `"single"` | Mode selection kalender |
| `selected` | `Date \| Date[] \| { from: Date; to: Date }` | - | Tanggal yang dipilih |
| `onSelect` | `(date: Date \| undefined) => void` | - | Callback ketika tanggal dipilih |
| `defaultMonth` | `Date` | - | Bulan default yang ditampilkan |
| `captionLayout` | `"label" \| "dropdown" \| "dropdown-buttons"` | `"label"` | Layout caption (label atau dropdown) |
| `buttonVariant` | `ButtonProps["variant"]` | `"ghost"` | Varian button untuk navigasi |
| `disabled` | `Date[] \| { before: Date; after: Date }` | - | Tanggal yang dinonaktifkan |
| `showOutsideDays` | `boolean` | `true` | Menampilkan hari dari bulan lain |
| `className` | `string` | - | Custom CSS classes |

## Contoh Penggunaan

### Basic Calendar (Single Date)
```tsx
import { Calendar } from "@/components/ui/calendar";
import { useState } from "react";

function DatePicker() {
  const [date, setDate] = useState<Date | undefined>(new Date());

  return (
    <Calendar
      mode="single"
      selected={date}
      onSelect={setDate}
      className="rounded-md border"
    />
  );
}
```

### Calendar dengan Dropdown untuk Bulan/Tahun
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  captionLayout="dropdown"
  className="rounded-md border"
/>
```

### Calendar dengan Range Selection
```tsx
function DateRangePicker() {
  const [range, setRange] = useState<{ from: Date; to: Date } | undefined>();

  return (
    <Calendar
      mode="range"
      selected={range}
      onSelect={setRange}
      className="rounded-md border"
    />
  );
}
```

### Calendar dengan Multiple Selection
```tsx
function MultipleDatePicker() {
  const [dates, setDates] = useState<Date[] | undefined>([]);

  return (
    <Calendar
      mode="multiple"
      selected={dates}
      onSelect={setDates}
      className="rounded-md border"
    />
  );
}
```

### Calendar dengan Disabled Dates
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  disabled={[
    new Date(2024, 0, 1),
    new Date(2024, 0, 15),
    { before: new Date(2024, 0, 10) },
    { after: new Date(2024, 0, 20) },
  ]}
  className="rounded-md border"
/>
```

### Calendar dengan Min/Max Date
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  disabled={{ before: new Date(2024, 0, 1), after: new Date(2024, 11, 31) }}
  className="rounded-md border"
/>
```

### Calendar dengan Default Month
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  defaultMonth={new Date(2024, 5)} // June 2024
  className="rounded-md border"
/>
```

### Calendar dalam Popover (Date Picker)
```tsx
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover";
import { Button } from "@/components/ui/button";
import { CalendarIcon } from "lucide-react";
import { format } from "date-fns";

function DatePickerWithPopover() {
  const [date, setDate] = useState<Date>();

  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button variant="outline" className="w-48 justify-start">
          <CalendarIcon className="mr-2 h-4 w-4" />
          {date ? format(date, "PPP") : "Pick a date"}
        </Button>
      </PopoverTrigger>
      <PopoverContent className="w-auto p-0">
        <Calendar
          mode="single"
          selected={date}
          onSelect={setDate}
          initialFocus
        />
      </PopoverContent>
    </Popover>
  );
}
```

### Calendar dalam Form (React Hook Form)
```tsx
import { useForm, Controller } from "react-hook-form";
import { format } from "date-fns";

function DateForm() {
  const { control, handleSubmit } = useForm();

  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div className="space-y-1.5">
        <Label>Birth Date</Label>
        <Controller
          name="birthDate"
          control={control}
          rules={{ required: "Birth date is required" }}
          render={({ field, fieldState }) => (
            <>
              <Popover>
                <PopoverTrigger asChild>
                  <Button variant="outline" className="w-full justify-start">
                    <CalendarIcon className="mr-2 h-4 w-4" />
                    {field.value ? format(field.value, "PPP") : "Select date"}
                  </Button>
                </PopoverTrigger>
                <PopoverContent className="w-auto p-0">
                  <Calendar
                    mode="single"
                    selected={field.value}
                    onSelect={field.onChange}
                    initialFocus
                  />
                </PopoverContent>
              </Popover>
              {fieldState.error && (
                <p className="text-xs text-red-500">{fieldState.error.message}</p>
              )}
            </>
          )}
        />
      </div>
      <Button type="submit">Submit</Button>
    </form>
  );
}
```

### Calendar dengan Today Highlight
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  today={new Date()}
  className="rounded-md border"
/>
```

### Calendar dengan Custom Styling
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  className="rounded-lg border-2 border-primary"
  classNames={{
    day_selected: "bg-primary text-primary-foreground",
    day_today: "bg-accent text-accent-foreground",
  }}
/>
```

### Calendar dengan Weekday Start (Monday)
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  weekStartsOn={1} // Monday
  className="rounded-md border"
/>
```

### Calendar dengan Italian/Indonesian Locale
```tsx
import { id } from "date-fns/locale";

<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  locale={id}
  className="rounded-md border"
/>
```

### Calendar dengan Custom Day Render
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  components={{
    Day: ({ date, ...props }) => {
      const isWeekend = date.getDay() === 0 || date.getDay() === 6;
      return (
        <div
          {...props}
          className={cn(
            "h-9 w-9 p-0 font-normal",
            isWeekend && "text-red-500",
            props.className
          )}
        >
          {date.getDate()}
        </div>
      );
    },
  }}
/>
```

## Kustomisasi

### Custom Button Varian
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  buttonVariant="outline"
/>
```

### Custom Caption Layout
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  captionLayout="dropdown-buttons"
/>
```

### Hide Outside Days
```tsx
<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  showOutsideDays={false}
/>
```

## Best Practices

### 1. Gunakan Popover untuk Form
Untuk form, kombinasikan Calendar dengan Popover untuk pengalaman yang lebih baik:
```tsx
<Popover>
  <PopoverTrigger asChild>
    <Button variant="outline">
      {date ? format(date, "PPP") : "Pick a date"}
    </Button>
  </PopoverTrigger>
  <PopoverContent className="w-auto p-0">
    <Calendar mode="single" selected={date} onSelect={setDate} />
  </PopoverContent>
</Popover>
```

### 2. Format Tanggal dengan date-fns
Gunakan library date-fns untuk format tanggal yang konsisten:
```tsx
import { format } from "date-fns";
format(date, "dd/MM/yyyy") // 31/12/2024
format(date, "PPP") // December 31st, 2024
```

### 3. Validasi Range
Untuk range selection, validasi bahwa tanggal akhir tidak lebih kecil dari tanggal awal.

### 4. Aksesibilitas
Calendar sudah memiliki keyboard navigation yang baik (arrow keys, enter, space).

## Troubleshooting

### Calendar Tidak Menampilkan Tanggal
- Pastikan `selected` prop di-set dengan benar
- Periksa apakah `mode` sesuai dengan tipe `selected`

### Dropdown Bulan/Tahun Tidak Muncul
- Gunakan `captionLayout="dropdown"` atau `captionLayout="dropdown-buttons"`

### Range Selection Tidak Berfungsi
- Pastikan `mode="range"` dan `selected` memiliki tipe `{ from: Date; to: Date }`

## Referensi

- [Shadcn Calendar Documentation](https://ui.shadcn.com/docs/components/calendar)
- [React Day Picker Documentation](https://react-day-picker.js.org/)
- [date-fns Documentation](https://date-fns.org/)
- [MDN Date Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
