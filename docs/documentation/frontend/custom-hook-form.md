---
title: Best Practice Custom Hook Form
sidebar_label: Custom Hook Form
sidebar_position: 2
---
# Custom Hook Form

## Gambaran Umum

Custom hook form adalah pola desain yang memisahkan logika form dari komponen UI, sehingga menciptakan reusable logic yang dapat digunakan di berbagai komponen. Berikut adalah best practice penggunaan custom hook form.

## Struktur Custom Hook Form

### Interface Definition

```typescript
export interface UseRoleForm extends UseHookForm<RoleFormSchema, RoleDataSchema> {
  role: UseFormReturn<RoleFormSchema>;
}
```

**Penjelasan:**

- `UseHookForm` adalah interface global yang mendefinisikan struktur standar
- `RoleFormSchema` adalah tipe data form
- `RoleDataSchema` adalah tipe data entity
- `UseFormReturn` adalah return type dari React Hook Form

### Hook Implementation

Berikut adalah langkah-langkah detail implementasi custom hook form:

#### **Langkah 1: Parameter dan Initial Setup**

```typescript
export const useRoleForm = (defaultValue?: RoleDataSchema | null): UseRoleForm => {
  // Parameter defaultValue digunakan untuk mode edit
  // Jika null/undefined, berarti mode create
```

**Penjelasan:**

- `defaultValue` adalah data yang akan di-load ke form (untuk edit mode)
- Jika `defaultValue` ada, form akan terisi dengan data tersebut
- Jika `defaultValue` null/undefined, form akan kosong (untuk create mode)

#### **Langkah 2: Define Initial Values**

```typescript
const initValue = {
  name: defaultValue?.name || "",
  description: defaultValue?.description || "",
};
```

**Penjelasan:**

- `initValue` adalah object yang mendefinisikan nilai awal semua field
- Menggunakan optional chaining (`?.`) untuk safety check
- Fallback ke string kosong (`""`) jika data tidak ada
- Struktur harus sesuai dengan `RoleFormSchema`

#### **Langkah 3: Configure React Hook Form**

```typescript
const role = useForm<RoleFormSchema>({
  resolver: zodResolver(roleFormSchema), // Validasi dengan Zod
  values: initValue, // Controlled values
  defaultValues: initValue, // Fallback values
});
```

**Penjelasan:**

- `resolver: zodResolver(roleFormSchema)`: Integrasi dengan Zod untuk validasi
- `values: initValue`: Membuat form controlled (recommended untuk edit mode)
- `defaultValues: initValue`: Fallback jika values tidak disediakan
- `mode` default adalah "onSubmit" (validasi saat submit)

#### **Langkah 4: Setup Select Binding (Opsional)**

```typescript
const formSelect = useBindSelect({
  form: role, // Instance form dari React Hook Form
  selects: {
    // Konfigurasi select fields
    // Kosong jika tidak ada select field
  },
});
```

**Penjelasan:**

- `useBindSelect` adalah custom hook untuk binding dropdown/select
- Mengintegrasikan select dengan form state management
- `form` parameter adalah instance React Hook Form
- `selects` object mendefinisikan field-field yang menggunakan select

#### **Langkah 5: Implementasi Utility Functions**

##### **setValues Function:**

```typescript
const setValues = (data: RoleDataSchema) => {
  role.setValue("name", data.name || "");
  role.setValue("description", data.description || "");
};
```

**Penjelasan:**

- Fungsi untuk mengisi form dengan data dari API/database
- Menggunakan `role.setValue()` dari React Hook Form
- Setiap field di-set satu per satu
- Fallback ke empty string jika data null/undefined

##### **setErrors Function:**

```typescript
const setErrors = useCallback((errors: any) => createErrorHandler(role)(errors), [role]);
```

**Penjelasan:**

- Fungsi untuk menampilkan error dari API response
- `createErrorHandler` adalah utility function yang mengkonversi API errors ke form errors
- Menggunakan `useCallback` untuk performance optimization
- Dependency array `[role]` memastikan callback selalu up-to-date

##### **reset Function:**

```typescript
const reset = useCallback(() => {
  role.reset(initValue);
}, [role, initValue]);
```

**Penjelasan:**

- Fungsi untuk reset form ke nilai awal
- `role.reset()` dari React Hook Form
- Menggunakan `useCallback` untuk mencegah re-render unnecessary
- Dependency `[role, initValue]` memastikan reset selalu menggunakan nilai terbaru

#### **Langkah 6: Return Interface**

```typescript
return {
  selects: formSelect.data, // Data untuk select fields
  role, // React Hook Form instance
  setValues, // Utility untuk set form values
  setErrors, // Utility untuk set form errors
  reset, // Utility untuk reset form
  registerSelect: formSelect.register, // Function untuk register select
};
```

**Penjelasan:**

- `selects`: Data yang dibutuhkan untuk select/dropdown components
- `role`: Instance lengkap React Hook Form (register, handleSubmit, formState, dll)
- `setValues`: Function untuk programmatically set form values
- `setErrors`: Function untuk set validation errors dari API
- `reset`: Function untuk reset form ke initial state
- `registerSelect`: Function untuk bind select components dengan form

### Hook Implementation (Lengkap)

Berikut adalah flow lengkap dari hook implementation:

```typescript
// 1. Import dependencies
import { useCallback } from "react";
import { useForm, type UseFormReturn } from "react-hook-form";
import { useBindSelect } from "@/hooks/useBindSelect";
import { createErrorHandler } from "@/utils";
import { zodResolver } from "@hookform/resolvers/zod";

// 2. Import types dan schemas
import type { UseHookForm } from "@/types";
import { roleFormSchema } from "../types/schema";
import type { RoleDataSchema, RoleFormSchema } from "../types/schema";

// 3. Define interface
export interface UseRoleForm extends UseHookForm<RoleFormSchema, RoleDataSchema> {
  role: UseFormReturn<RoleFormSchema>;
}

// 4. Implement hook
export const useRoleForm = (defaultValue?: RoleDataSchema | null): UseRoleForm => {
  // Step 1: Setup initial values
  const initValue = {
    name: defaultValue?.name || "",
    description: defaultValue?.description || "",
  };

  // Step 2: Initialize React Hook Form
  const role = useForm<RoleFormSchema>({
    resolver: zodResolver(roleFormSchema),
    values: initValue,
    defaultValues: initValue,
  });

  // Step 3: Setup select binding
  const formSelect = useBindSelect({
    form: role,
    selects: {},
  });

  // Step 4: Create utility functions
  const setValues = (data: RoleDataSchema) => {
    role.setValue("name", data.name || "");
    role.setValue("description", data.description || "");
  };

  const setErrors = useCallback((errors: any) => createErrorHandler(role)(errors), [role]);

  const reset = useCallback(() => {
    role.reset(initValue);
  }, [role, initValue]);

  // Step 5: Return complete interface
  return {
    selects: formSelect.data,
    role,
    setValues,
    setErrors,
    reset,
    registerSelect: formSelect.register,
  };
};
```

## Implementasi di Component

### Page Implementation

```typescript
const RoleCreate = () => {
  const navigate = useNavigate();

  // Initialize form hook
  const form: UseRoleForm = useRoleForm();

  // Mutation hook
  const { createRole, createRoleLoading, createRoleError } = useMutationRole();

  // Submit handler
  const onSubmitHandler = (data: RoleFormSchema) =>
    createRole(data, {
      onSuccess: (response) => navigate(roleBase + "/edit/" + response.data.id),
    });

  return (
    <RoleForm
      form={form}
      state="create"
      onSubmit={onSubmitHandler}
      isLoading={createRoleLoading}
      errorMessage={createRoleError?.message}
    />
  );
};
```



### Form Component Structure

```typescript
interface RoleFormProps {
  form: UseRoleForm;
  state?: "create" | "edit";
  previousUrl?: string;
  defaultValue?: RoleDataSchema | null;
  isLoading?: boolean;
  errorMessage?: string;
  onSubmit: SubmitHandler<RoleFormSchema>;
}

export const RoleForm = ({
  form,
  state = "create",
  isLoading = false,
  previousUrl,
  errorMessage,
  onSubmit,
}: RoleFormProps) => {
  const {
    role: {
      register,
      handleSubmit,
      formState: { errors },
    },
  } = form;

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Form fields */}
    </form>
  );
};
```

## Common Pitfalls & Solutions

### Stale Closure dalam useCallback

❌ **Problem:**

```typescript
const setErrors = useCallback((errors) => {
  // form reference bisa stale
  createErrorHandler(form)(errors);
}, []); // Dependency array kosong
```

✅ **Solution:**

```typescript
const setErrors = useCallback(
  (errors) => {
    createErrorHandler(form)(errors);
  },
  [form],
); // Include form dependency
```

### Race Condition dalam Async Operations

❌ **Problem:**

```typescript
const validateUsername = async (username) => {
  const result = await api.checkUsername(username);
  form.setError("username", { message: result.message });
  // User sudah ganti input sebelum response datang
};
```

✅ **Solution:**

```typescript
const validateUsername = useCallback(
  async (username) => {
    const result = await api.checkUsername(username);
    // Check if input masih sama
    if (form.getValues("username") === username) {
      form.setError("username", { message: result.message });
    }
  },
  [form],
);
```

### Memory Leaks

❌ **Problem:**

```typescript
useEffect(() => {
  const subscription = form.watch((value) => {
    // Side effect yang tidak dibersihkan
    api.logFormChange(value);
  });
  // Tidak ada cleanup
}, [form]);
```

✅ **Solution:**

```typescript
useEffect(() => {
  const subscription = form.watch((value) => {
    api.logFormChange(value);
  });

  return () => subscription.unsubscribe(); // Cleanup
}, [form]);
```

## Performance Optimization

### Memoization

```typescript
export const useOptimizedForm = (defaultValue?: DataSchema) => {
  const initValue = useMemo(
    () => ({
      name: defaultValue?.name || "",
      description: defaultValue?.description || "",
    }),
    [defaultValue],
  );

  const form = useForm({
    // ...
  });

  const setValues = useCallback(
    (data: DataSchema) => {
      form.setValue("name", data.name || "");
      form.setValue("description", data.description || "");
    },
    [form],
  );

  return useMemo(
    () => ({
      form,
      setValues,
    }),
    [form, setValues],
  );
};
```

### Debounced Validation

```typescript
export const useDebouncedForm = () => {
  const form = useForm({
    /* ... */
  });
  const [debouncedValue, setDebouncedValue] = useState("");

  const debouncedValidate = useDebounce((value) => {
    // Expensive validation
    validateComplexRule(value);
  }, 500);

  useEffect(() => {
    const subscription = form.watch((value) => {
      setDebouncedValue(value.field);
      debouncedValidate(value.field);
    });

    return () => subscription.unsubscribe();
  }, [form, debouncedValidate]);

  return { form, debouncedValue };
};
```

## Summary

Custom hook form memberikan:

1. **Reusability** - Logic dapat digunakan di multiple components
2. **Type Safety** - Full TypeScript support dengan Zod
3. **Separation of Concerns** - UI terpisah dari business logic
4. **Testability** - Mudah di-test secara isolated
5. **Maintainability** - Centralized form logic
6. **Performance** - Optimized dengan proper memoization

Dengan mengikuti best practices ini, Anda akan memiliki form yang robust, maintainable, dan scalable untuk aplikasi React Anda.