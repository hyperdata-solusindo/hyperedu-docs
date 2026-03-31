---
title: "DatePicker"
sidebar_label: "DatePicker"
---

# DatePicker

**Lokasi:** `components/custom/date-picker.tsx`

## Deskripsi

Komponen date picker yang lebih kompleks dengan input teks dan kalender dropdown. DatePicker memungkinkan pengguna untuk memilih tanggal baik melalui input teks manual maupun melalui kalender interaktif.

## Fitur

- Input teks untuk manual entry tanggal
- Kalender dropdown untuk pemilihan visual
- Format tanggal otomatis (DD Month YYYY)
- Validasi input tanggal manual
- Keyboard navigation (Arrow Down untuk membuka kalender)
- Sinkronisasi antara input teks dan kalender
- Responsif dan accessible

## Komponen yang Digunakan

| Komponen | Deskripsi |
|----------|-----------|
| `Field` | Container untuk form field dengan label |
| `FieldLabel` | Label untuk date picker |
| `InputGroup` | Container untuk input dengan addon |
| `InputGroupInput` | Input teks untuk tanggal |
| `InputGroupAddon` | Container untuk ikon kalender |
| `InputGroupButton` | Tombol untuk membuka kalender |
| `Popover` | Popover untuk menampilkan kalender |
| `Calendar` | Komponen kalender untuk memilih tanggal |

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `value` | `Date` | Tidak | - | Nilai tanggal yang dipilih (controlled) |
| `onChange` | `(date?: Date) => void` | Tidak | - | Handler perubahan tanggal |
| `disabled` | `boolean` | Tidak | `false` | Menonaktifkan komponen |
| `placeholder` | `string` | Tidak | `"June 01, 2025"` | Placeholder input |
| `label` | `string` | Tidak | `"Date"` | Label untuk date picker |
| `required` | `boolean` | Tidak | `false` | Menampilkan indikator required |

## Fungsi Helper

### formatDate
```typescript
function formatDate(date: Date | undefined): string {
  if (!date) return "";
  return date.toLocaleDateString("en-US", {
    day: "2-digit",
    month: "long",
    year: "numeric",
  });
}
// Output: "01 June 2025"
```

### isValidDate
```typescript
function isValidDate(date: Date | undefined): boolean {
  if (!date) return false;
  return !isNaN(date.getTime());
}
```

## Contoh Penggunaan

### Basic Usage
```tsx
import { DatePickerInput } from "@/components/custom/date-picker";

function SubscriptionForm() {
  return (
    <div className="space-y-4">
      <DatePickerInput />
    </div>
  );
}
```

### Dengan Label Kustom
```tsx
<DatePickerInput label="Start Date" />
```

### Dengan Placeholder Kustom
```tsx
<DatePickerInput placeholder="Select your birthday" />
```

### Controlled Component
```tsx
function ControlledDatePicker() {
  const [date, setDate] = useState<Date | undefined>(new Date());

  return (
    <DatePickerInput
      value={date}
      onChange={setDate}
      label="Event Date"
    />
  );
}
```

### Dalam Form (React Hook Form)
```tsx
import { useForm, Controller } from "react-hook-form";

function EventForm() {
  const { control, handleSubmit } = useForm();

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="eventDate"
        control={control}
        rules={{ required: "Date is required" }}
        render={({ field, fieldState }) => (
          <div className="space-y-1.5">
            <DatePickerInput
              value={field.value}
              onChange={field.onChange}
              label="Event Date"
              required
            />
            {fieldState.error && (
              <p className="text-xs text-red-500">{fieldState.error.message}</p>
            )}
          </div>
        )}
      />
      <Button type="submit">Submit</Button>
    </form>
  );
}
```

### Dengan Disabled State
```tsx
<DatePickerInput disabled label="Disabled Date Picker" />
```

### Dengan Required Indicator
```tsx
<DatePickerInput required label="Birth Date" />
```

### Dalam Card
```tsx
<Card>
  <CardHeader>
    <CardTitle>Schedule Meeting</CardTitle>
    <CardDescription>Select a date for your meeting</CardDescription>
  </CardHeader>
  <CardContent>
    <DatePickerInput label="Meeting Date" />
  </CardContent>
  <CardFooter>
    <Button>Schedule</Button>
  </CardFooter>
</Card>
```

### Dalam Form dengan Validasi
```tsx
function BookingForm() {
  const [date, setDate] = useState<Date>();
  const [error, setError] = useState("");

  const validateDate = (selectedDate: Date | undefined) => {
    if (!selectedDate) {
      setError("Date is required");
      return false;
    }
    if (selectedDate < new Date()) {
      setError("Cannot select past date");
      return false;
    }
    setError("");
    return true;
  };

  const handleDateChange = (newDate: Date | undefined) => {
    setDate(newDate);
    validateDate(newDate);
  };

  return (
    <div className="space-y-2">
      <DatePickerInput
        value={date}
        onChange={handleDateChange}
        label="Booking Date"
        required
      />
      {error && (
        <p className="text-xs text-red-500">{error}</p>
      )}
    </div>
  );
}
```

### Dengan Minimum dan Maximum Date
```tsx
function DateRangePicker() {
  const [date, setDate] = useState<Date>();
  const minDate = new Date();
  const maxDate = new Date();
  maxDate.setMonth(maxDate.getMonth() + 3);

  const handleDateChange = (newDate: Date | undefined) => {
    if (newDate && newDate < minDate) {
      toast.error("Cannot select past date");
      return;
    }
    if (newDate && newDate > maxDate) {
      toast.error("Cannot select date beyond 3 months");
      return;
    }
    setDate(newDate);
  };

  return (
    <DatePickerInput
      value={date}
      onChange={handleDateChange}
      label="Select Date"
    />
  );
}
```

## Kustomisasi

### Custom Width
```tsx
<DatePickerInput className="w-64" />
```

### Custom Styling
```tsx
<DatePickerInput className="[&_input]:bg-muted [&_button]:bg-primary" />
```

## Best Practices

### 1. Gunakan untuk Input Tanggal
DatePicker cocok untuk semua jenis input tanggal:
```tsx
// ✅ Baik: Input tanggal
<DatePickerInput label="Birth Date" />

// ❌ Hindari: Input selain tanggal
```

### 2. Validasi Input Manual
Validasi input teks untuk memastikan format tanggal yang valid:
```tsx
const handleInputChange = (e) => {
  const date = new Date(e.target.value);
  if (isValidDate(date)) {
    setDate(date);
  }
};
```

### 3. Keyboard Navigation
Dukungan keyboard (Arrow Down) untuk membuka kalender:
```tsx
onKeyDown={(e) => {
  if (e.key === "ArrowDown") {
    e.preventDefault();
    setOpen(true);
  }
}}
```

### 4. Format yang Konsisten
Gunakan format tanggal yang konsisten di seluruh aplikasi:
```tsx
format: "DD Month YYYY" // 01 June 2025
```

## Troubleshooting

### Input Manual Tidak Valid
- Periksa fungsi `isValidDate` untuk validasi
- Input manual akan tetap menampilkan teks meskipun tidak valid

### Kalender Tidak Sinkron dengan Input
- Pastikan `month` state diupdate saat input berubah
- Gunakan `setMonth` saat input manual valid

### Popover Tidak Tertutup
- Kalender otomatis tertutup setelah memilih tanggal
- Popover bisa ditutup dengan mengklik di luar area

## Referensi

- [Shadcn Calendar Documentation](https://ui.shadcn.com/docs/components/calendar)
- [Shadcn Popover Documentation](https://ui.shadcn.com/docs/components/popover)
- [Shadcn Input Group Documentation](https://ui.shadcn.com/docs/components/input-group)
- [date-fns Documentation](https://date-fns.org/)
