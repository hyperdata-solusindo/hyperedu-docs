---
title: Best Practice Custom Hook Form
sidebar_label: Custom Hook Form
sidebar_position: 2
---
# Custom Hook Form

## Gambaran Umum

Custom hook form adalah pola desain yang memisahkan logika form dari komponen UI, sehingga menciptakan reusable logic yang dapat digunakan di berbagai komponen. Berikut adalah best practice penggunaan custom hook form.

## Struktur Custom Hook Form

### 1. Interface Definition

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

### 2. Hook Implementation

```typescript
export const useRoleForm = (defaultValue?: RoleDataSchema | null): UseRoleForm => {
  // 1. Initial values
  const initValue = {
    name: defaultValue?.name || "",
    description: defaultValue?.description || "",
  };

  // 2. Form configuration
  const role = useForm<RoleFormSchema>({
    resolver: zodResolver(roleFormSchema),
    values: initValue,
    defaultValues: initValue,
  });

  // 3. Select binding (jika ada)
  const formSelect = useBindSelect({
    form: role,
    selects: {},
  });

  // 4. Utility functions
  const setValues = (data: RoleDataSchema) => {
    role.setValue("name", data.name || "");
    role.setValue("description", data.description || "");
  };

  const setErrors = useCallback((errors: any) => createErrorHandler(role)(errors), [role]);

  const reset = useCallback(() => {
    role.reset(initValue);
  }, [role, initValue]);

  // 5. Return interface
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

## Best Practices

### 1. **Separation of Concerns**

✅ **Baik:**

```typescript
// Hook menangani logika form
const form = useRoleForm(initialData);

// Component fokus pada UI
const {
  register,
  handleSubmit,
  formState: { errors },
} = form.role;
```

❌ **Hindari:**

```typescript
// Campur logika dan UI dalam satu tempat
const MyComponent = () => {
  const [name, setName] = useState("");
  const [description, setDescription] = useState("");
  // ... banyak state management
};
```

### 2. **Type Safety dengan Zod**

✅ **Baik:**

```typescript
// Schema validation di types/schema.ts
export const roleFormSchema = z.object({
  name: z.string().min(1, "Name is required"),
  description: z.string().optional(),
});

// Hook menggunakan schema
const role = useForm<RoleFormSchema>({
  resolver: zodResolver(roleFormSchema),
  // ...
});
```

❌ **Hindari:**

```typescript
// Validation inline tanpa schema
const role = useForm({
  // Tidak ada validation yang ketat
});
```

### 3. **Initial Values Handling**

✅ **Baik:**

```typescript
const initValue = {
  name: defaultValue?.name || "",
  description: defaultValue?.description || "",
};

const role = useForm({
  resolver: zodResolver(roleFormSchema),
  values: initValue, // Untuk controlled form
  defaultValues: initValue, // Fallback
});
```

❌ **Hindari:**

```typescript
// Tidak konsisten antara values dan defaultValues
const role = useForm({
  defaultValues: { name: "", description: "" },
  // values tidak diset untuk edit mode
});
```

### 4. **Error Handling**

✅ **Baik:**

```typescript
const setErrors = useCallback((errors: any) => createErrorHandler(role)(errors), [role]);

// Penggunaan di component
const onSubmit = async (data) => {
  try {
    await createRole(data);
  } catch (error) {
    form.setErrors(error.response?.data?.errors);
  }
};
```

❌ **Hindari:**

```typescript
// Manual error handling
const onSubmit = async (data) => {
  try {
    await createRole(data);
  } catch (error) {
    // Manual set error per field
    if (error.response?.data?.errors?.name) {
      form.role.setError("name", { message: error.response.data.errors.name });
    }
    // ... repeat untuk setiap field
  }
};
```

### 5. **Select/Dropdown Integration**

✅ **Baik:**

```typescript
const formSelect = useBindSelect({
  form: role,
  selects: {
    // Define select fields yang perlu binding
  },
});

return {
  selects: formSelect.data,
  registerSelect: formSelect.register,
  // ...
};
```

❌ **Hindari:**

```typescript
// Tidak menggunakan binding
const [selectedValue, setSelectedValue] = useState("");
// State terpisah dari form
```

### 6. **Utility Functions**

✅ **Baik:**

```typescript
const setValues = (data: RoleDataSchema) => {
  role.setValue("name", data.name || "");
  role.setValue("description", data.description || "");
};

const reset = useCallback(() => {
  role.reset(initValue);
}, [role, initValue]);
```

❌ **Hindari:**

```typescript
// Tidak ada utility functions
// Setiap kali perlu set values, harus manual
form.role.setValue("name", data.name);
form.role.setValue("description", data.description);
// ... repeat
```

## Implementasi di Component

### 1. **Form Component Structure**

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

### 2. **Page Implementation**

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

## Advanced Patterns

### 1. **Conditional Fields**

```typescript
export const useAdvancedForm = (defaultValue?: DataSchema) => {
  const [showAdvanced, setShowAdvanced] = useState(false);

  const form = useForm({
    // ...
  });

  // Watch field untuk conditional logic
  const fieldValue = form.watch("type");

  useEffect(() => {
    if (fieldValue === "advanced") {
      setShowAdvanced(true);
    } else {
      setShowAdvanced(false);
      // Reset advanced fields
      form.setValue("advancedField", "");
    }
  }, [fieldValue, form]);

  return {
    form,
    showAdvanced,
  };
};
```

### 2. **Form Steps/Wizard**

```typescript
export const useWizardForm = () => {
  const [currentStep, setCurrentStep] = useState(1);

  const form = useForm({
    // Multi-step form logic
  });

  const nextStep = () => {
    // Validate current step before proceeding
    setCurrentStep((prev) => prev + 1);
  };

  const prevStep = () => {
    setCurrentStep((prev) => prev - 1);
  };

  return {
    form,
    currentStep,
    nextStep,
    prevStep,
  };
};
```

### 3. **Async Validation**

```typescript
export const useAsyncValidatedForm = () => {
  const form = useForm({
    resolver: zodResolver(schema),
    mode: "onChange", // Enable real-time validation
  });

  // Custom async validation
  const validateField = useCallback(async (fieldName: string, value: any) => {
    try {
      const result = await api.validateField(fieldName, value);
      return result.isValid;
    } catch (error) {
      return false;
    }
  }, []);

  return {
    form,
    validateField,
  };
};
```

## Testing Custom Hook Form

### 1. **Unit Test**

```typescript
import { renderHook, act } from "@testing-library/react";
import { useRoleForm } from "../hooks/useRoleForm";

describe("useRoleForm", () => {
  it("should initialize with default values", () => {
    const { result } = renderHook(() => useRoleForm());

    expect(result.current.role.getValues()).toEqual({
      name: "",
      description: "",
    });
  });

  it("should set values correctly", () => {
    const { result } = renderHook(() => useRoleForm());

    act(() => {
      result.current.setValues({
        id: "1",
        name: "Admin",
        description: "Administrator role",
      });
    });

    expect(result.current.role.getValues("name")).toBe("Admin");
  });
});
```

### 2. **Integration Test**

```typescript
import { render, screen, fireEvent, waitFor } from "@testing-library/react";
import { RoleCreate } from "../pages/role-create";

describe("RoleCreate Integration", () => {
  it("should create role successfully", async () => {
    render(<RoleCreate />);

    // Fill form
    fireEvent.change(screen.getByLabelText("Role Name"), {
      target: { value: "Test Role" },
    });

    fireEvent.change(screen.getByLabelText("Role Description"), {
      target: { value: "Test Description" },
    });

    // Submit
    fireEvent.click(screen.getByRole("button", { name: "Submit" }));

    // Assert success
    await waitFor(() => {
      expect(mockNavigate).toHaveBeenCalledWith("/masters/role/edit/1");
    });
  });
});
```

## Common Pitfalls & Solutions

### 1. **Stale Closure dalam useCallback**

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

### 2. **Race Condition dalam Async Operations**

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

### 3. **Memory Leaks**

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

### 1. **Memoization**

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

### 2. **Debounced Validation**

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

## Migration Guide

### From Class Components

```typescript
// ❌ Old way
class RoleForm extends Component {
  state = { name: "", description: "" };

  handleSubmit = (e) => {
    e.preventDefault();
    // Submit logic
  };

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          value={this.state.name}
          onChange={(e) => this.setState({ name: e.target.value })}
        />
      </form>
    );
  }
}

// ✅ New way
const RoleForm = () => {
  const form = useRoleForm();

  const onSubmit = (data) => {
    // Submit logic
  };

  return (
    <form onSubmit={form.role.handleSubmit(onSubmit)}>
      <input {...form.role.register("name")} />
    </form>
  );
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