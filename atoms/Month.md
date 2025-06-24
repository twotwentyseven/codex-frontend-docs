# Month Component

## Overview
The Month component provides a specialized numeric input field for collecting month values (1-12) in date forms. It features automatic sanitization, pattern validation, error handling, dependency injection for form integration, and comprehensive accessibility support. The component works within provider contexts for form-wide configuration and validation.

## Basic Usage
```vue
<!-- Must be used within a provider that injects month configuration -->
<template>
  <div>
    <!-- Provider component that injects month configuration -->
    <date-provider
      :aria-label="'Select month'"
      :name="'birth_month'"
      :id="'month-input'"
      :required="true"
      :has-error="false"
    >
      <codex-month 
        v-model="monthValue"
        :dusk="'month-test'"
      />
    </date-provider>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const monthValue = ref('')
</script>
```

## Key Features
- Numeric-only input with automatic sanitization
- Month range validation (1-12)
- Real-time input filtering and length limiting
- Error state management with visual feedback
- Dependency injection from parent providers
- Accessible design with ARIA support
- Internationalized placeholder text
- Pattern-based validation with regex
- Input mode optimization for mobile keyboards

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | `String` | `undefined` | Two-way binding for the month value |
| `dusk` | `String` | `''` | Testing identifier for automation |

## Common Props (Injected)

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `ariaLabel` | `String` | `''` | Accessibility label for screen readers |
| `name` | `String` | `''` | Form field name attribute |
| `id` | `String` | `''` | Unique identifier for the input |
| `required` | `Boolean/String` | `''` | Whether the field is required |
| `hasError` | `Boolean` | `false` | External error state from parent |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:modelValue` | `string` | Emitted when month value changes |
| `input` | `Event` | Native input event with sanitized value |

## Slots
This component does not provide any slots.

## States

### Default State
```vue
<codex-month v-model="month" />
```

### Error State
```vue
<codex-month 
  v-model="month" 
  class="_c-error" 
/>
```

### Required State
```vue
<date-provider :required="true">
  <codex-month v-model="month" />
</date-provider>
```

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `date.month` | Month | Input placeholder text |

## Examples

### Basic Month Input
```vue
<template>
  <date-provider 
    :aria-label="'Birth month'"
    :name="'birth_month'"
    :required="true"
  >
    <codex-month v-model="birthMonth" />
  </date-provider>
</template>

<script setup>
const birthMonth = ref('')
</script>
```

### Month with Validation
```vue
<template>
  <date-provider 
    :has-error="monthError"
    :aria-label="'Expiry month'"
    :name="'expiry_month'"
  >
    <codex-month 
      v-model="expiryMonth"
      @update:modelValue="validateMonth"
    />
  </date-provider>
</template>

<script setup>
const expiryMonth = ref('')
const monthError = ref(false)

const validateMonth = (value) => {
  const month = parseInt(value)
  monthError.value = month < 1 || month > 12
}
</script>
```

### Complete Date Input Group
```vue
<template>
  <div class="date-group">
    <date-provider :name="'day'" :aria-label="'Day'">
      <codex-day v-model="day" />
    </date-provider>
    
    <date-provider :name="'month'" :aria-label="'Month'">
      <codex-month v-model="month" />
    </date-provider>
    
    <date-provider :name="'year'" :aria-label="'Year'">
      <codex-year v-model="year" />
    </date-provider>
  </div>
</template>

<script setup>
const day = ref('')
const month = ref('')
const year = ref('')
</script>
```

### Credit Card Expiry
```vue
<template>
  <div class="expiry-inputs">
    <date-provider 
      :name="'expiry_month'"
      :aria-label="'Expiry month'"
      :has-error="expiryError"
    >
      <codex-month 
        v-model="expiryMonth"
        :dusk="'expiry-month'"
      />
    </date-provider>
    
    <span>/</span>
    
    <date-provider 
      :name="'expiry_year'"
      :aria-label="'Expiry year'"
      :has-error="expiryError"
    >
      <codex-year 
        v-model="expiryYear"
        :dusk="'expiry-year'"
      />
    </date-provider>
  </div>
</template>

<script setup>
const expiryMonth = ref('')
const expiryYear = ref('')
const expiryError = ref(false)

watch([expiryMonth, expiryYear], () => {
  validateExpiry()
})

const validateExpiry = () => {
  const month = parseInt(expiryMonth.value)
  const year = parseInt(expiryYear.value)
  const currentDate = new Date()
  const currentMonth = currentDate.getMonth() + 1
  const currentYear = currentDate.getFullYear()
  
  expiryError.value = year < currentYear || 
    (year === currentYear && month < currentMonth)
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input` | Base input styling |
| `_c-input-month` | Month-specific styling |
| `_c-error` | Error state styling |

## Best Practices

### Accessibility
- Always provide meaningful `aria-label` through provider injection
- Use semantic form labels and field grouping
- Ensure proper focus management in date input groups
- Test with screen readers for date input workflows

### Validation
- Implement real-time validation feedback
- Validate month range (1-12) on input and blur events
- Consider contextual validation (e.g., expiry dates, birth dates)
- Provide clear error messages for invalid inputs

### User Experience
- Group month with day and year inputs for complete dates
- Use consistent styling across date input components
- Provide visual separation or formatting for date groups
- Consider auto-advancing focus between date fields

### Form Integration
- Use provider pattern for consistent form configuration
- Implement proper form validation strategies
- Handle form submission with complete date validation
- Consider server-side validation for date completeness

### Error Handling
- Distinguish between format errors and logical errors
- Provide immediate feedback for invalid month values
- Clear errors when valid input is provided
- Handle edge cases like leading zeros gracefully

### Performance
- Use ref for internal state management
- Implement efficient input sanitization
- Minimize re-renders with proper reactivity
- Optimize pattern matching for validation

### Internationalization
- Use translation keys for placeholder text
- Consider regional date format preferences
- Handle different calendar systems if needed
- Provide culturally appropriate month representations

## Component Registration
```javascript
// Global registration
app.component('CodexMonth', Month)

// Local registration  
import Month from '@/components/atoms/Month.vue'

export default {
  components: {
    CodexMonth: Month
  }
}
``` 