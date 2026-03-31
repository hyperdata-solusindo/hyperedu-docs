---
title: "FormNavTab"
sidebar_label: "FormNavTab"
---

# FormNavTab

**Lokasi:** `components/custom/form-navtab.tsx`

## Deskripsi

Tab navigasi untuk form dengan beberapa bagian. FormNavTab digunakan untuk membagi form yang panjang menjadi beberapa bagian yang lebih terorganisir, memungkinkan pengguna untuk beralih antar bagian tanpa kehilangan data yang sudah diisi.

## Fitur

- Tab navigasi dengan indikator aktif
- Dukungan ikon pada setiap tab
- Styling konsisten dengan sistem desain
- Responsive layout
- Transisi halus saat beralih tab

## Komponen yang Tersedia

| Komponen | Deskripsi |
|----------|-----------|
| `FormNavTab` | Container utama tab navigasi |
| `NavTabItem` | Interface untuk item tab |

## Type Definitions

```typescript
interface NavTabItem {
  id: string;
  label: string;
  icon?: React.ReactNode;
}
```

## Props

| Prop | Tipe | Required | Default | Deskripsi |
|------|------|----------|---------|-----------|
| `items` | `NavTabItem[]` | Ya | - | Array tab items |
| `activeTab` | `string` | Ya | - | ID tab yang aktif |
| `onTabChange` | `(tabId: string) => void` | Ya | - | Handler perubahan tab |
| `className` | `string` | Tidak | - | Custom CSS classes |

## Tab Presets

### STUDENT_FORM_TABS
```typescript
export const STUDENT_FORM_TABS = [
  { id: "general", label: "General" },
  { id: "parent", label: "Parent Information" },
];
```

### TEACHER_FORM_TABS
```typescript
export const TEACHER_FORM_TABS = [
  { id: "general", label: "General" },
  { id: "parent", label: "Parent Information" },
];
```

## Contoh Penggunaan

### Basic Usage
```tsx
import { FormNavTab } from "@/components/custom/form-navtab";

function StudentForm() {
  const [activeTab, setActiveTab] = useState("general");

  return (
    <div>
      <FormNavTab
        items={[
          { id: "general", label: "General Information" },
          { id: "academic", label: "Academic Data" },
          { id: "parent", label: "Parent Information" },
        ]}
        activeTab={activeTab}
        onTabChange={setActiveTab}
      />
      
      <div className="mt-6">
        {activeTab === "general" && <GeneralInfoForm />}
        {activeTab === "academic" && <AcademicDataForm />}
        {activeTab === "parent" && <ParentInfoForm />}
      </div>
    </div>
  );
}
```

### Dengan Ikon
```tsx
import { UserIcon, BookOpenIcon, UsersIcon } from "lucide-react";

<FormNavTab
  items={[
    { id: "general", label: "General", icon: <UserIcon className="h-4 w-4" /> },
    { id: "academic", label: "Academic", icon: <BookOpenIcon className="h-4 w-4" /> },
    { id: "parent", label: "Parent", icon: <UsersIcon className="h-4 w-4" /> },
  ]}
  activeTab={activeTab}
  onTabChange={setActiveTab}
/>
```

### Menggunakan Preset
```tsx
import { FormNavTab, STUDENT_FORM_TABS } from "@/components/custom/form-navtab";

function StudentRegistrationForm() {
  const [activeTab, setActiveTab] = useState("general");

  return (
    <div className="space-y-6">
      <FormNavTab
        items={STUDENT_FORM_TABS}
        activeTab={activeTab}
        onTabChange={setActiveTab}
      />
      
      <div className="bg-white rounded-lg border p-6">
        {activeTab === "general" && <GeneralInfoForm />}
        {activeTab === "parent" && <ParentInfoForm />}
      </div>
    </div>
  );
}
```

### Dalam Form Card
```tsx
import { FormCard } from "@/components/custom/form-card";

function MultiStepForm() {
  const [activeTab, setActiveTab] = useState("personal");

  return (
    <FormCard title="Employee Registration">
      <FormNavTab
        items={[
          { id: "personal", label: "Personal Data" },
          { id: "employment", label: "Employment Details" },
          { id: "documents", label: "Documents" },
        ]}
        activeTab={activeTab}
        onTabChange={setActiveTab}
      />
      
      <div className="mt-6">
        {activeTab === "personal" && <PersonalDataForm />}
        {activeTab === "employment" && <EmploymentDetailsForm />}
        {activeTab === "documents" && <DocumentsUploadForm />}
      </div>
    </FormCard>
  );
}
```

### Dengan Validasi Per Tab
```tsx
function ValidatedMultiTabForm() {
  const [activeTab, setActiveTab] = useState("general");
  const [formData, setFormData] = useState({});
  const [errors, setErrors] = useState({});

  const handleTabChange = (tabId: string) => {
    // Validate current tab before switching
    const currentTabErrors = validateTab(activeTab);
    if (Object.keys(currentTabErrors).length > 0) {
      setErrors(currentTabErrors);
      toast.error("Please fill all required fields");
      return;
    }
    setActiveTab(tabId);
  };

  return (
    <div>
      <FormNavTab
        items={tabItems}
        activeTab={activeTab}
        onTabChange={handleTabChange}
      />
      
      <div className="mt-6">
        {activeTab === "general" && (
          <GeneralForm 
            data={formData} 
            onChange={setFormData}
            errors={errors}
          />
        )}
        {activeTab === "details" && (
          <DetailsForm 
            data={formData} 
            onChange={setFormData}
            errors={errors}
          />
        )}
      </div>
    </div>
  );
}
```

### Dengan Progress Indicator
```tsx
function ProgressMultiTabForm() {
  const [activeTab, setActiveTab] = useState(0);
  const tabs = ["Personal", "Contact", "Address", "Review"];
  const [completedTabs, setCompletedTabs] = useState<boolean[]>([false, false, false, false]);

  const handleTabChange = (index: number) => {
    // Mark current tab as completed if valid
    if (isTabValid(activeTab)) {
      setCompletedTabs(prev => {
        const newCompleted = [...prev];
        newCompleted[activeTab] = true;
        return newCompleted;
      });
    }
    setActiveTab(index);
  };

  const progress = (completedTabs.filter(Boolean).length / tabs.length) * 100;

  return (
    <div className="space-y-6">
      <div className="space-y-2">
        <div className="h-2 bg-muted rounded-full overflow-hidden">
          <div 
            className="h-full bg-primary transition-all duration-300"
            style={{ width: `${progress}%` }}
          />
        </div>
        <FormNavTab
          items={tabs.map((label, i) => ({
            id: i.toString(),
            label,
            icon: completedTabs[i] ? <CheckIcon className="h-4 w-4 text-green-500" /> : undefined,
          }))}
          activeTab={activeTab.toString()}
          onTabChange={(id) => handleTabChange(parseInt(id))}
        />
      </div>
      
      <div className="bg-white rounded-lg border p-6">
        {activeTab === 0 && <PersonalForm />}
        {activeTab === 1 && <ContactForm />}
        {activeTab === 2 && <AddressForm />}
        {activeTab === 3 && <ReviewForm />}
      </div>
    </div>
  );
}
```

### Dengan React Hook Form (Multi-step)
```tsx
import { useForm, FormProvider } from "react-hook-form";

function MultiStepFormWithRHF() {
  const [step, setStep] = useState(0);
  const methods = useForm();

  const steps = [
    { id: "personal", label: "Personal Info", component: PersonalInfoStep },
    { id: "contact", label: "Contact Details", component: ContactStep },
    { id: "confirm", label: "Confirmation", component: ConfirmStep },
  ];

  const nextStep = async () => {
    const isValid = await methods.trigger();
    if (isValid && step < steps.length - 1) {
      setStep(step + 1);
    }
  };

  const prevStep = () => {
    if (step > 0) setStep(step - 1);
  };

  const CurrentStepComponent = steps[step].component;

  return (
    <FormProvider {...methods}>
      <div className="space-y-6">
        <FormNavTab
          items={steps.map((s, i) => ({
            id: s.id,
            label: s.label,
            icon: i < step ? <CheckIcon className="h-4 w-4" /> : undefined,
          }))}
          activeTab={steps[step].id}
          onTabChange={(id) => {
            const index = steps.findIndex(s => s.id === id);
            if (index <= step) setStep(index);
          }}
        />
        
        <div className="bg-white rounded-lg border p-6">
          <CurrentStepComponent />
        </div>
        
        <div className="flex justify-between gap-2">
          <Button
            variant="outline"
            onClick={prevStep}
            disabled={step === 0}
          >
            Previous
          </Button>
          {step === steps.length - 1 ? (
            <Button onClick={methods.handleSubmit(onSubmit)}>
              Submit
            </Button>
          ) : (
            <Button onClick={nextStep}>Next</Button>
          )}
        </div>
      </div>
    </FormProvider>
  );
}
```

## Kustomisasi

### Custom Styling
```tsx
<FormNavTab
  className="bg-muted p-1 rounded-lg"
  items={items}
  activeTab={activeTab}
  onTabChange={setActiveTab}
/>
```

### Custom Tab Styling
Tab styling dapat dimodifikasi melalui CSS classes:
- Tab aktif: `text-indigo-600 border-b-2 border-indigo-600`
- Tab tidak aktif: `text-slate-500 hover:text-slate-700`

## Best Practices

### 1. Gunakan untuk Form Panjang
FormNavTab ideal untuk form dengan lebih dari 10 field:
```tsx
// ✅ Baik: Form panjang dibagi menjadi beberapa tab
<FormNavTab items={["Personal", "Contact", "Address", "Employment"]} />

// ❌ Hindari: Form pendek dengan banyak tab
<FormNavTab items={["Name", "Email"]} />
```

### 2. Validasi Sebelum Pindah Tab
Validasi data sebelum membiarkan user pindah ke tab berikutnya untuk mencegah data tidak lengkap.

### 3. Indikator Progress
Gunakan progress bar untuk memberikan gambaran sejauh mana pengguna telah mengisi form.

### 4. Persist Data Antar Tab
Pastikan data tetap tersimpan saat user beralih antar tab.

## Troubleshooting

### Tab Tidak Berganti
- Pastikan `onTabChange` handler mengupdate state `activeTab`
- Periksa `activeTab` prop sudah sesuai dengan state

### Ikon Tidak Muncul
- Pastikan `icon` prop adalah ReactNode yang valid
- Periksa ukuran ikon (gunakan `h-4 w-4` untuk konsistensi)

## Referensi

- [Shadcn Tabs Documentation](https://ui.shadcn.com/docs/components/tabs)
- [Radix UI Tabs](https://www.radix-ui.com/primitives/docs/components/tabs)
- [React Hook Form Multi-step](https://react-hook-form.com/advanced-usage#WizardFormFunnel)
