---
title: Form
sidebar_label: Form
sidebar_position: 2
---
# Pembuatan Custom Hook Form

## Deskripsi

Custom hook form adalah solusi efektif untuk mengelola state form dalam aplikasi React. Dengan memanfaatkan **React Hook Form** sebagai library utama untuk state management form, custom hook ini menyediakan performa tinggi, validasi otomatis, dan kemudahan integrasi dengan komponen UI. Custom hook form dirancang untuk mengurangi boilerplate code, meningkatkan reusability, dan memisahkan logika form dari komponen UI. Ini memungkinkan developer untuk fokus pada logika bisnis sambil memastikan form handling yang konsisten dan type-safe.

Referensi utama: [React Hook Form Documentation](https://react-hook-form.com/) untuk pemahaman mendalam tentang state management form di React.

## Parameter

Custom hook form biasanya menerima satu parameter opsional:

|Nama|Tipe|Keterangan|
|---|---|---|
|`defaultValue`|`T \| null`|Nilai default untuk menginisialisasi form. Jika tidak diberikan, form akan menggunakan nilai kosong sebagai default.|

Di mana `T` adalah tipe data untuk form (misalnya, interface atau schema data).

## Return Value

Hook mengembalikan objek dengan properti berikut:

|Nama|Tipe|Keterangan/Kegunaan|
|---|---|---|
|`form`|`UseFormReturn<TFormSchema>`|Instance React Hook Form yang berisi semua method dan state form (register, handleSubmit, watch, dll.).|
|`selects`|`Record<string, any>`|Data untuk select fields yang sudah di-bind dengan form (jika ada).|
|`setValues`|`(data: TDataSchema) => void`|Method untuk mengatur nilai form berdasarkan data yang diberikan, termasuk handling select fields.|
|`setErrors`|`(errors: any) => void`|Method untuk mengatur error validasi pada form fields.|
|`reset`|`() => void`|Method untuk mereset form ke nilai awal.|
|`registerSelect`|`(name: string, select: any) => void`|Method untuk mendaftarkan select field ke form (jika ada).|

## Struktur Custom Hook Form

Proses pembuatan custom hook form dilakukan langkah demi langkah sebagai berikut:

### Langkah 1: Import Dependencies

Langkah pertama adalah mengimpor semua dependencies yang diperlukan, termasuk React hooks, React Hook Form, validasi schema, dan custom hooks lainnya jika diperlukan.

```typescript
import { useCallback } from "react";
import { useForm, type UseFormReturn } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import type { UseHookForm } from "@/types";

// Import schema dan tipe sesuai kebutuhan
import { formSchema } from "../types/schema";
import type { DataSchema, FormSchema } from "../types/schema";

// Import custom hooks jika ada select fields
import { useBindSelect } from "@/hooks/useBindSelect";
import { useSelectHook } from "./useSelectHook";
import { createErrorHandler } from "@/utils";
```

Penjelasan: Import ini mencakup hook React, React Hook Form untuk form management, Zod untuk validasi, dan utility lainnya.

### Langkah 2: Definisi Interface

Mendefinisikan interface untuk return value hook agar type-safe.

```typescript
export interface UseCustomForm extends UseHookForm<FormSchema, DataSchema> {
  form: UseFormReturn<FormSchema>;
}
```

Penjelasan: Interface ini extends `UseHookForm` dan menambahkan properti `form` yang merupakan instance React Hook Form.

### Langkah 3: Inisialisasi Hook dan Nilai Default

Memulai hook dengan parameter dan menginisialisasi nilai default.

```typescript
export const useCustomForm = (defaultValue?: DataSchema | null): UseCustomForm => {
  const initValue = {
    field1: defaultValue?.field1 || "",
    field2: defaultValue?.field2 || "",
    // ... inisialisasi field lainnya
  };
```

Penjelasan: Menggunakan parameter `defaultValue` untuk mengatur nilai awal form, atau fallback ke nilai kosong.

### Langkah 4: Setup React Hook Form

Mengkonfigurasi React Hook Form dengan resolver dan nilai default.

```typescript
const form = useForm<FormSchema>({
  resolver: zodResolver(formSchema),
  values: initValue,
  defaultValues: initValue,
});
```

Penjelasan: `useForm` diinisialisasi dengan resolver untuk validasi otomatis berdasarkan schema, dan nilai awal.

### Langkah 5: Binding Select Fields (Opsional)

Jika ada select fields, gunakan custom hook untuk binding.

```typescript
const formSelect = useBindSelect({
  form: form,
  selects: {
    selectField: useSelectHook(),
    // ... select fields lainnya
  },
});
```

Penjelasan: Select fields di-bind ke form menggunakan hook binding untuk integrasi yang mulus.

### Langkah 6: Implementasi Helper Methods

Membuat method untuk manipulasi form seperti setValues, setErrors, dan reset.

```typescript
const setValues = (data: DataSchema) => {
  form.setValue("field1", data.field1 || "");
  form.setValue("field2", data.field2 || "");
  // ... set nilai lainnya
  // Jika ada select: formSelect.setValue("selectField", data.selectValue);
};

const setErrors = useCallback((errors: any) => createErrorHandler(form)(errors), [form]);

const reset = useCallback(() => {
  form.reset(initValue);
}, [form, initValue]);
```

Penjelasan: `setValues` mengatur nilai form dari data. `setErrors` menggunakan error handler. `reset` mengembalikan form ke nilai awal.

### Langkah 7: Return Object

Mengembalikan objek dengan semua properti dan method.

```typescript
  return {
    selects: formSelect?.data || {},
    form,
    setValues,
    setErrors,
    reset,
    registerSelect: formSelect?.register,
  };
};
```

Penjelasan: Return value mencakup semua yang diperlukan untuk menggunakan hook ini di komponen.

## Penulisan Lengkap

Berikut adalah kode lengkap contoh custom hook form dengan komentar langkah-langkah:

```typescript
import { useCallback } from "react"; // 1. Import React hooks
import { useForm, type UseFormReturn } from "react-hook-form"; // 1. Import React Hook Form
import { zodResolver } from "@hookform/resolvers/zod"; // 1. Import resolver
import type { UseHookForm } from "@/types"; // 1. Import types

import { formSchema } from "../types/schema"; // 1. Import schema
import type { DataSchema, FormSchema } from "../types/schema"; // 1. Import types
import { useBindSelect } from "@/hooks/useBindSelect"; // 1. Import binding hook jika perlu
import { useSelectHook } from "./useSelectHook"; // 1. Import select hook jika perlu
import { createErrorHandler } from "@/utils"; // 1. Import error handler

export interface UseCustomForm extends UseHookForm<FormSchema, DataSchema> {
  // 2. Define interface
  form: UseFormReturn<FormSchema>;
}

export const useCustomForm = (defaultValue?: DataSchema | null): UseCustomForm => {
  // 3. Hook function
  const initValue = {
    // 3. Initialize default values
    field1: defaultValue?.field1 || "",
    field2: defaultValue?.field2 || "",
  };

  const form = useForm<FormSchema>({
    // 4. Setup React Hook Form
    resolver: zodResolver(formSchema),
    values: initValue,
    defaultValues: initValue,
  });

  const formSelect = useBindSelect({
    // 5. Bind select fields (opsional)
    form: form,
    selects: {
      selectField: useSelectHook(),
    },
  });

  const setValues = (data: DataSchema) => {
    // 6. Implement setValues
    form.setValue("field1", data.field1 || "");
    form.setValue("field2", data.field2 || "");
    formSelect.setValue("selectField", data.selectValue);
  };

  const setErrors = useCallback((errors: any) => createErrorHandler(form)(errors), [form]); // 6. Implement setErrors

  const reset = useCallback(() => {
    // 6. Implement reset
    form.reset(initValue);
  }, [form, initValue]);

  return {
    // 7. Return object
    selects: formSelect.data,
    form,
    setValues,
    setErrors,
    reset,
    registerSelect: formSelect.register,
  };
};
```

## Contoh Penggunaan

Berikut adalah contoh penggunaan custom hook form dalam komponen:

### Dalam Komponen Edit

```typescript
import { useCustomForm, type UseCustomForm } from "../hooks/useCustomForm";
import type { FormSchema } from "../types/schema";

const EditComponent = () => {
  const form: UseCustomForm = useCustomForm(); // Inisialisasi hook

  const onSubmitHandler = (data: FormSchema) => {
    // Logika submit
    updateData(data, {
      onSuccess: () => navigate("/list"),
      onError: (error) => {
        form.setErrors(error.validations); // Mengatur error
      },
    });
  };

  // Mengatur nilai saat data tersedia
  useEffect(() => {
    if (data) {
      form.setValues(data);
    }
  }, [data]);

  return (
    <FormComponent
      form={form}
      onSubmit={onSubmitHandler}
      isLoading={isLoading}
    />
  );
};
```

### Dalam Komponen Create

```typescript
const CreateComponent = () => {
  const form: UseCustomForm = useCustomForm(); // Inisialisasi hook

  const onSubmitHandler = (data: FormSchema) => {
    createData(data, {
      onSuccess: (response) => navigate("/edit/" + response.id),
    });
  };

  return (
    <FormComponent
      form={form}
      onSubmit={onSubmitHandler}
      isLoading={isLoading}
    />
  );
};
```

## Best Practice

- **Type Safety**: Selalu gunakan TypeScript untuk memastikan type safety pada form schema dan data.
- **Validasi**: Manfaatkan schema validasi (seperti Zod) untuk validasi yang kuat dan konsisten.
- **Reusability**: Buat hook form terpisah untuk setiap fitur agar mudah di-maintain dan di-reuse.
- **Error Handling**: Implementasikan error handling yang konsisten menggunakan `setErrors`.
- **Performance**: Gunakan `useCallback` untuk method yang dipassing sebagai props untuk menghindari re-render tidak perlu.
- **Default Values**: Berikan default values yang masuk akal untuk menghindari undefined errors.
- **Select Binding**: Jika ada select fields, pastikan binding dengan benar menggunakan custom hooks.
- **Testing**: Buat unit test untuk hook untuk memastikan fungsionalitas.

## Troubleshooting

- **Form tidak mereset**: Pastikan `reset` dipanggil dengan benar dan `initValue` terdefinisi.
- **Validasi error tidak muncul**: Periksa apakah `setErrors` dipanggil dengan format error yang benar dari server.
- **Select fields tidak update**: Pastikan `setValues` mengatur nilai select menggunakan `formSelect.setValue`.
- **Type errors**: Pastikan semua imports type dan schema sudah benar, dan gunakan TypeScript strict mode.
- **Performance issues**: Jika form besar, pertimbangkan untuk menggunakan `watch` secara selektif atau memoization.
- **Hook call error**: Pastikan hook dipanggil di dalam komponen React, bukan di dalam loop atau kondisi.

## Referensi

- **React Hook Form**: [https://react-hook-form.com/](https://react-hook-form.com/) - Library utama untuk state management form.
- **Zod**: [https://zod.dev/](https://zod.dev/) - Schema validation library.
- **@hookform/resolvers**: [https://github.com/react-hook-form/resolvers](https://github.com/react-hook-form/resolvers) - Resolver untuk integrasi Zod dengan React Hook Form.
- **Custom Hooks**: `useBindSelect` - Hook untuk binding select fields.
- **Utils**: `createErrorHandler` - Utility untuk handling error form.