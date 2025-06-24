# Year Component

## Overview
The Year component provides a specialized numeric input field for collecting four-digit year values in date forms. It features automatic sanitization, future date validation, pattern matching, error handling, dependency injection for form integration, and comprehensive accessibility support. The component prevents future year entry and validates year ranges.

## Basic Usage
```vue
<!-- Must be used within a provider that injects year configuration -->
<template>
  <div>
    <!-- Provider component that injects year configuration -->
    <date-provider
      :aria-label="'Select year'"
      :name="'birth_year'"
      :id="'year-input'"
      :required="true"
      :has-error="false"
    >
      <codex-year 
        v-model="yearValue"
        :dusk="'year-test'"
      />
    </date-provider>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const yearValue = ref('')
</script>
```

## Key Features
- Four-digit year input with automatic sanitization
- Future date validation (prevents years after current year)
- Real-time input filtering and length limiting
- Pattern-based validation with regex
- Error state management with visual feedback
- Dependency injection from parent providers
- Accessible design with ARIA support
- Internationalized placeholder text
- Input mode optimization for mobile keyboards

## Configuration Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | `String` | `undefined` | Two-way binding for the year value |
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
| `update:modelValue` | `string` | Emitted when year value changes |
| `input` | `Event` | Native input event with sanitized value |

## Slots
This component does not provide any slots.

## States

### Default State
```vue
<codex-year v-model="year" />
```

### Error State
```vue
<codex-year 
  v-model="year" 
  class="_c-error" 
/>
```

### Required State
```vue
<date-provider :required="true">
  <codex-year v-model="year" />
</date-provider>
```

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `date.year` | Year | Input placeholder text |

## Examples

### Basic Year Input
```vue
<template>
  <date-provider 
    :aria-label="'Birth year'"
    :name="'birth_year'"
    :required="true"
  >
    <codex-year v-model="birthYear" />
  </date-provider>
</template>

<script setup>
const birthYear = ref('')
</script>
```

### Year with Custom Validation
```vue
<template>
  <date-provider 
    :has-error="yearError"
    :aria-label="'Graduation year'"
    :name="'graduation_year'"
  >
    <codex-year 
      v-model="graduationYear"
      @update:modelValue="validateYear"
    />
  </date-provider>
</template>

<script setup>
const graduationYear = ref('')
const yearError = ref(false)

const validateYear = (value) => {
  const year = parseInt(value)
  const currentYear = new Date().getFullYear()
  
  // Allow future years for graduation
  yearError.value = year < 1900 || year > currentYear + 10
}
</script>
```

### Complete Birth Date
```vue
<template>
  <div class="birth-date">
    <h3>Date of Birth</h3>
    <div class="date-inputs">
      <date-provider :name="'birth_day'" :aria-label="'Birth day'">
        <codex-day v-model="birthDay" />
      </date-provider>
      
      <date-provider :name="'birth_month'" :aria-label="'Birth month'">
        <codex-month v-model="birthMonth" />
      </date-provider>
      
      <date-provider :name="'birth_year'" :aria-label="'Birth year'">
        <codex-year v-model="birthYear" />
      </date-provider>
    </div>
  </div>
</template>

<script setup>
const birthDay = ref('')
const birthMonth = ref('')
const birthYear = ref('')

const isValidDate = computed(() => {
  return birthDay.value && birthMonth.value && birthYear.value
})
</script>
```

### Academic Year Range
```vue
<template>
  <div class="academic-years">
    <div class="year-range">
      <label>Start Year</label>
      <date-provider 
        :name="'start_year'"
        :aria-label="'Academic start year'"
        :has-error="startYearError"
      >
        <codex-year 
          v-model="startYear"
          @update:modelValue="validateYearRange"
        />
      </date-provider>
    </div>
    
    <div class="year-range">
      <label>End Year</label>
      <date-provider 
        :name="'end_year'"
        :aria-label="'Academic end year'"
        :has-error="endYearError"
      >
        <codex-year 
          v-model="endYear"
          @update:modelValue="validateYearRange"
        />
      </date-provider>
    </div>
  </div>
</template>

<script setup>
const startYear = ref('')
const endYear = ref('')
const startYearError = ref(false)
const endYearError = ref(false)

const validateYearRange = () => {
  const start = parseInt(startYear.value)
  const end = parseInt(endYear.value)
  
  if (start && end) {
    startYearError.value = start >= end
    endYearError.value = end <= start
  }
}
</script>
```

### Historical Year Input
```vue
<template>
  <div class="historical-date">
    <date-provider 
      :name="'historical_year'"
      :aria-label="'Historical year'"
      :has-error="historicalError"
    >
      <codex-year 
        v-model="historicalYear"
        @update:modelValue="validateHistoricalYear"
      />
    </date-provider>
    <p v-if="historicalError" class="error-message">
      Please enter a year between 1000 and {{ currentYear }}
    </p>
  </div>
</template>

<script setup>
const historicalYear = ref('')
const historicalError = ref(false)
const currentYear = new Date().getFullYear()

const validateHistoricalYear = (value) => {
  const year = parseInt(value)
  historicalError.value = year < 1000 || year > currentYear
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input` | Base input styling |
| `_c-input-year` | Year-specific styling |
| `_c-error` | Error state styling |

## Best Practices

### Accessibility
- Always provide meaningful `aria-label` through provider injection
- Use semantic form labels and field grouping
- Ensure proper focus management in date input groups
- Test with screen readers for date input workflows
- Group year with related date components

### Validation
- Implement appropriate year range validation for your use case
- Consider future vs. historical date requirements
- Validate year format (4 digits) before business logic
- Provide clear feedback for validation errors
- Handle edge cases like very old or very new years

### User Experience
- Group year with day and month inputs for complete dates
- Use consistent styling across date input components
- Provide visual separation or formatting for date groups
- Consider auto-advancing focus between date fields
- Show year format expectations clearly

### Form Integration
- Use provider pattern for consistent form configuration
- Implement proper form validation strategies
- Handle form submission with complete date validation
- Consider server-side validation for date completeness
- Validate date logic across all components

### Error Handling
- Distinguish between format errors and business logic errors
- Provide immediate feedback for invalid year values
- Clear errors when valid input is provided
- Handle future date restrictions appropriately
- Consider user intent when validating years

### Performance
- Use ref for internal state management
- Implement efficient input sanitization
- Minimize re-renders with proper reactivity
- Optimize pattern matching for validation
- Cache current year value to avoid repeated calculations

### Internationalization
- Use translation keys for placeholder text
- Consider regional calendar systems
- Handle different year formats if needed
- Provide culturally appropriate year representations
- Consider locale-specific date validation rules

## Component Registration
```javascript
// Global registration
app.component('CodexYear', Year)

// Local registration  
import Year from '@/components/atoms/Year.vue'

export default {
  components: {
    CodexYear: Year
  }
}
``` 