# Day Component

## Overview
The Day component provides a specialized numeric input field for collecting day values (1-31) in date forms. It features automatic sanitization, day range validation, pattern matching, error handling, dependency injection for form integration, and comprehensive accessibility support. The component validates proper day ranges and integrates seamlessly with month and year components.

## Basic Usage
```vue
<!-- Must be used within a provider that injects day configuration -->
<template>
  <div>
    <!-- Provider component that injects day configuration -->
    <date-provider
      :aria-label="'Select day'"
      :name="'birth_day'"
      :id="'day-input'"
      :required="true"
      :has-error="false"
    >
      <codex-day 
        v-model="dayValue"
        :dusk="'day-test'"
      />
    </date-provider>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const dayValue = ref('')
</script>
```

## Key Features
- Numeric-only input with automatic sanitization
- Day range validation (1-31)
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
| `modelValue` | `String` | `undefined` | Two-way binding for the day value |
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
| `update:modelValue` | `string` | Emitted when day value changes |
| `input` | `Event` | Native input event with sanitized value |

## Slots
This component does not provide any slots.

## States

### Default State
```vue
<codex-day v-model="day" />
```

### Error State
```vue
<codex-day 
  v-model="day" 
  class="_c-error" 
/>
```

### Required State
```vue
<date-provider :required="true">
  <codex-day v-model="day" />
</date-provider>
```

## Internationalization Keys

| Key | Default | Description |
|-----|---------|-------------|
| `date.day` | Day | Input placeholder text |

## Examples

### Basic Day Input
```vue
<template>
  <date-provider 
    :aria-label="'Birth day'"
    :name="'birth_day'"
    :required="true"
  >
    <codex-day v-model="birthDay" />
  </date-provider>
</template>

<script setup>
const birthDay = ref('')
</script>
```

### Day with Custom Validation
```vue
<template>
  <date-provider 
    :has-error="dayError"
    :aria-label="'Event day'"
    :name="'event_day'"
  >
    <codex-day 
      v-model="eventDay"
      @update:modelValue="validateDay"
    />
  </date-provider>
</template>

<script setup>
const eventDay = ref('')
const dayError = ref(false)
const selectedMonth = ref('2') // February

const validateDay = (value) => {
  const day = parseInt(value)
  const month = parseInt(selectedMonth.value)
  
  // Custom validation for February
  if (month === 2 && day > 29) {
    dayError.value = true
  } else {
    dayError.value = false
  }
}
</script>
```

### Complete Date Input System
```vue
<template>
  <div class="date-input-system">
    <h3>Select Date</h3>
    <div class="date-fields">
      <div class="field">
        <label for="day">Day</label>
        <date-provider 
          :name="'day'" 
          :id="'day'"
          :aria-label="'Day'"
          :has-error="dateError"
        >
          <codex-day 
            v-model="selectedDay"
            @update:modelValue="validateDate"
          />
        </date-provider>
      </div>
      
      <div class="field">
        <label for="month">Month</label>
        <date-provider 
          :name="'month'" 
          :id="'month'"
          :aria-label="'Month'"
          :has-error="dateError"
        >
          <codex-month 
            v-model="selectedMonth"
            @update:modelValue="validateDate"
          />
        </date-provider>
      </div>
      
      <div class="field">
        <label for="year">Year</label>
        <date-provider 
          :name="'year'" 
          :id="'year'"
          :aria-label="'Year'"
          :has-error="dateError"
        >
          <codex-year 
            v-model="selectedYear"
            @update:modelValue="validateDate"
          />
        </date-provider>
      </div>
    </div>
    
    <p v-if="dateError" class="error">Please enter a valid date</p>
    <p v-if="isValidDate" class="success">Date: {{ formattedDate }}</p>
  </div>
</template>

<script setup>
const selectedDay = ref('')
const selectedMonth = ref('')
const selectedYear = ref('')
const dateError = ref(false)

const isValidDate = computed(() => {
  return selectedDay.value && selectedMonth.value && selectedYear.value && !dateError.value
})

const formattedDate = computed(() => {
  if (!isValidDate.value) return ''
  return `${selectedDay.value}/${selectedMonth.value}/${selectedYear.value}`
})

const validateDate = () => {
  if (!selectedDay.value || !selectedMonth.value || !selectedYear.value) {
    dateError.value = false
    return
  }
  
  const day = parseInt(selectedDay.value)
  const month = parseInt(selectedMonth.value)
  const year = parseInt(selectedYear.value)
  
  // Check if date is valid
  const date = new Date(year, month - 1, day)
  const isValid = date.getDate() === day && 
                  date.getMonth() === month - 1 && 
                  date.getFullYear() === year
  
  dateError.value = !isValid
}
</script>
```

### Appointment Booking Day
```vue
<template>
  <div class="appointment-day">
    <date-provider 
      :name="'appointment_day'"
      :aria-label="'Appointment day'"
      :has-error="appointmentError"
    >
      <codex-day 
        v-model="appointmentDay"
        :dusk="'appointment-day'"
        @update:modelValue="validateAppointmentDay"
      />
    </date-provider>
    
    <div v-if="availableDays.length" class="available-days">
      <p>Available days this month:</p>
      <div class="day-buttons">
        <button 
          v-for="day in availableDays" 
          :key="day"
          @click="selectDay(day)"
          :class="{ active: appointmentDay === day.toString() }"
        >
          {{ day }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const appointmentDay = ref('')
const appointmentError = ref(false)
const availableDays = ref([1, 3, 5, 8, 10, 15, 17, 22, 24, 29])

const validateAppointmentDay = (value) => {
  const day = parseInt(value)
  appointmentError.value = day && !availableDays.value.includes(day)
}

const selectDay = (day) => {
  appointmentDay.value = day.toString()
  appointmentError.value = false
}
</script>
```

### Birthday Input with Leap Year Support
```vue
<template>
  <div class="birthday-input">
    <div class="birthday-fields">
      <date-provider 
        :name="'birth_day'"
        :aria-label="'Birth day'"
        :has-error="birthdayError"
      >
        <codex-day 
          v-model="birthDay"
          @update:modelValue="validateBirthday"
        />
      </date-provider>
      
      <select v-model="birthMonth" @change="validateBirthday">
        <option value="">Month</option>
        <option v-for="month in months" :key="month.value" :value="month.value">
          {{ month.label }}
        </option>
      </select>
      
      <date-provider 
        :name="'birth_year'"
        :aria-label="'Birth year'"
        :has-error="birthdayError"
      >
        <codex-year 
          v-model="birthYear"
          @update:modelValue="validateBirthday"
        />
      </date-provider>
    </div>
    
    <p v-if="birthdayError" class="error">{{ birthdayErrorMessage }}</p>
  </div>
</template>

<script setup>
const birthDay = ref('')
const birthMonth = ref('')
const birthYear = ref('')
const birthdayError = ref(false)
const birthdayErrorMessage = ref('')

const months = ref([
  { value: '1', label: 'January' },
  { value: '2', label: 'February' },
  { value: '3', label: 'March' },
  // ... other months
])

const isLeapYear = (year) => {
  return (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)
}

const getDaysInMonth = (month, year) => {
  const daysInMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
  if (month === 2 && isLeapYear(year)) {
    return 29
  }
  return daysInMonth[month - 1]
}

const validateBirthday = () => {
  if (!birthDay.value || !birthMonth.value || !birthYear.value) {
    birthdayError.value = false
    return
  }
  
  const day = parseInt(birthDay.value)
  const month = parseInt(birthMonth.value)
  const year = parseInt(birthYear.value)
  
  const maxDays = getDaysInMonth(month, year)
  
  if (day > maxDays) {
    birthdayError.value = true
    birthdayErrorMessage.value = `${months.value.find(m => m.value === month.toString())?.label} ${year} only has ${maxDays} days`
  } else {
    birthdayError.value = false
    birthdayErrorMessage.value = ''
  }
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input` | Base input styling |
| `_c-input-day` | Day-specific styling |
| `_c-error` | Error state styling |

## Best Practices

### Accessibility
- Always provide meaningful `aria-label` through provider injection
- Use semantic form labels and field grouping
- Ensure proper focus management in date input groups
- Test with screen readers for date input workflows
- Associate labels with day input fields clearly

### Validation
- Implement real-time validation feedback
- Validate day range (1-31) on input and blur events
- Consider month-specific day validation (e.g., February has 28/29 days)
- Validate leap years when combined with month and year
- Provide clear error messages for invalid inputs

### User Experience
- Group day with month and year inputs for complete dates
- Use consistent styling across date input components
- Provide visual separation or formatting for date groups
- Consider auto-advancing focus between date fields
- Show expected format clearly

### Form Integration
- Use provider pattern for consistent form configuration
- Implement proper form validation strategies
- Handle form submission with complete date validation
- Consider server-side validation for date completeness
- Validate date logic across all components

### Error Handling
- Distinguish between format errors and logical date errors
- Provide immediate feedback for invalid day values
- Clear errors when valid input is provided
- Handle month-specific day limits appropriately
- Consider leap year calculations in validation

### Performance
- Use ref for internal state management
- Implement efficient input sanitization
- Minimize re-renders with proper reactivity
- Optimize pattern matching for validation
- Cache month/year calculations when validating days

### Internationalization
- Use translation keys for placeholder text
- Consider regional date format preferences
- Handle different calendar systems if needed
- Provide culturally appropriate day representations
- Consider locale-specific date validation rules

## Component Registration
```javascript
// Global registration
app.component('CodexDay', Day)

// Local registration  
import Day from '@/components/atoms/Day.vue'

export default {
  components: {
    CodexDay: Day
  }
}
``` 