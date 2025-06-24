# Input Component

## Overview
The Input component provides a versatile atomic text input element with v-model support, debouncing capabilities, error state handling, and dependency injection integration. It features dynamic type support, CSS class application based on input type, accessibility attributes, and real-time input validation feedback for comprehensive form integration.

## Basic Usage
```vue
<codex-input
  v-model="textValue"
  :dusk="'username-input'"
/>
```

## Key Features
- Two-way data binding with v-model support
- Input debouncing for performance optimization
- Dynamic input type support (text, password, email, etc.)
- Error state visualization and handling
- Readonly and disabled state support
- Accessibility attributes and ARIA support
- Dependency injection for configuration
- Custom CSS class application based on input type
- Real-time input validation feedback

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| modelValue | String\|Number | No | '' | Input value for two-way binding |
| dusk | String | No | '' | Testing identifier attribute |
| debounceTime | Number\|Boolean | No | false | Debounce delay in milliseconds or false to disable |

### Injected Props
The component receives additional configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| placeholder | String | '' | Placeholder text for input |
| ariaLabel | String | '' | ARIA label for accessibility |
| name | String | '' | Input name attribute |
| id | String | '' | Input ID attribute |
| type | String | '' | Input type (text, password, email, etc.) |
| required | Boolean | false | Whether input is required |
| readonly | Boolean | false | Whether input is readonly |
| hasError | Boolean | false | Whether input has validation errors |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| update:modelValue | `value: String\|Number` | Emitted when input value changes |

## Input Type Support
The component supports all standard HTML input types:
- **text**: Default text input
- **password**: Password field with hidden content
- **email**: Email input with validation
- **url**: URL input with validation
- **tel**: Telephone number input
- **number**: Numeric input
- **search**: Search input with enhanced styling
- **date**: Date picker input
- **time**: Time picker input
- **datetime-local**: Date and time input

## CSS Class Application
The component applies specific CSS classes:
- **Base Classes**: `_c-input` applied to all inputs
- **Type-Specific**: `_c-input-{type}` based on injected type
- **Error State**: `_c-error` when hasError injection is true

## Error State Handling
When the `hasError` injection is true:
- Applies `_c-error` CSS class
- Provides visual feedback for validation errors
- Maintains accessibility for screen readers
- Integrates with form validation systems

## Debouncing Behavior
The component includes built-in input debouncing:
- **Default**: No debouncing (immediate updates)
- **Custom Delay**: Configurable millisecond delay via debounceTime prop
- **Performance**: Reduces excessive API calls or validations
- **Timeout Management**: Properly clears previous timeouts
- **Immediate Response**: Updates internal state immediately, emits with delay

## Dependency Injection Integration
The component uses Vue's provide/inject pattern:
- **Form Field Integration**: Receives configuration from parent form components
- **Validation Integration**: Connects with validation systems
- **Accessibility**: Inherits ARIA attributes from form context
- **Styling**: Applies contextual CSS classes based on type

## Accessibility Features
- **ARIA Labels**: Supports aria-label through injection
- **Required Indication**: Handles required field marking
- **Error Announcement**: Provides error state feedback
- **Keyboard Navigation**: Standard input keyboard support
- **Screen Reader**: Compatible with assistive technologies

## Internationalization
The Input component does not include built-in internationalization. Placeholder text and labels should be localized before injection.

## Examples

### Basic Text Input
```vue
<codex-input
  v-model="username"
  :dusk="'username-input'"
/>
```

### Input with Debouncing
```vue
<codex-input
  v-model="searchQuery"
  :debounce-time="300"
  :dusk="'search-input'"
/>
```

### Password Input with Form Integration
```vue
<template>
  <div class="form-field">
    <codex-label />
    <codex-input
      v-model="password"
      :dusk="'password-input'"
    />
    <codex-error v-if="passwordError" :error="passwordError" />
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const password = ref('')
const passwordError = ref('')

const hasPasswordError = computed(() => !!passwordError.value)

provide('id', 'password-input')
provide('name', 'password')
provide('type', 'password')
provide('placeholder', 'Enter your password')
provide('ariaLabel', 'Account password')
provide('required', true)
provide('hasError', hasPasswordError)
</script>
```

### Email Input with Validation
```vue
<template>
  <div class="form-field">
    <codex-input
      v-model="email"
      :debounce-time="500"
      :dusk="'email-input'"
      @update:model-value="validateEmail"
    />
    <codex-error :error="emailValidationError" />
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const email = ref('')
const emailValidationError = ref('')

const validateEmail = (value) => {
  if (!value) {
    emailValidationError.value = 'Email is required'
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
    emailValidationError.value = 'Please enter a valid email address'
  } else {
    emailValidationError.value = ''
  }
}

provide('type', 'email')
provide('placeholder', 'Enter your email address')
provide('ariaLabel', 'Email address')
provide('required', true)
provide('hasError', computed(() => !!emailValidationError.value))
</script>
```

### Search Input with Live Results
```vue
<template>
  <div class="search-container">
    <codex-input
      v-model="searchTerm"
      :debounce-time="250"
      :dusk="'search-input'"
      @update:model-value="performSearch"
    />
    
    <div v-if="searchResults.length > 0" class="search-results">
      <div v-for="result in searchResults" :key="result.id" class="search-result">
        {{ result.title }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const searchTerm = ref('')
const searchResults = ref([])

const performSearch = async (query) => {
  if (query.length < 2) {
    searchResults.value = []
    return
  }
  
  try {
    const results = await searchAPI(query)
    searchResults.value = results
  } catch (error) {
    console.error('Search failed:', error)
    searchResults.value = []
  }
}

provide('type', 'search')
provide('placeholder', 'Search for items...')
provide('ariaLabel', 'Search input')
</script>
```

### Multi-step Form Input
```vue
<template>
  <div class="form-step">
    <codex-input
      v-model="stepValue"
      :dusk="'step-input'"
      @update:model-value="handleStepChange"
    />
    
    <div class="step-progress">
      Step {{ currentStep }} of {{ totalSteps }}
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const stepValue = ref('')
const currentStep = ref(1)
const totalSteps = ref(3)

const stepConfig = computed(() => {
  const configs = {
    1: {
      type: 'text',
      placeholder: 'Enter your full name',
      ariaLabel: 'Full name'
    },
    2: {
      type: 'email',
      placeholder: 'Enter your email address',
      ariaLabel: 'Email address'
    },
    3: {
      type: 'tel',
      placeholder: 'Enter your phone number',
      ariaLabel: 'Phone number'
    }
  }
  return configs[currentStep.value]
})

const handleStepChange = (value) => {
  // Validate current step and potentially advance
  if (value && isValidForCurrentStep(value)) {
    // Advance to next step logic
  }
}

provide('type', computed(() => stepConfig.value.type))
provide('placeholder', computed(() => stepConfig.value.placeholder))
provide('ariaLabel', computed(() => stepConfig.value.ariaLabel))
provide('required', true)
</script>
```

### Numeric Input with Constraints
```vue
<template>
  <div class="numeric-input">
    <codex-input
      v-model="numericValue"
      :dusk="'numeric-input'"
      @update:model-value="handleNumericInput"
    />
    <div class="input-constraints">
      Range: {{ minValue }} - {{ maxValue }}
    </div>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const numericValue = ref('')
const minValue = 1
const maxValue = 100

const handleNumericInput = (value) => {
  // Only allow numeric input
  const numeric = value.replace(/[^0-9]/g, '')
  
  if (numeric !== value) {
    numericValue.value = numeric
  }
  
  // Validate range
  const num = parseInt(numeric)
  if (num < minValue || num > maxValue) {
    // Handle validation
  }
}

provide('type', 'number')
provide('placeholder', `Enter number (${minValue}-${maxValue})`)
provide('ariaLabel', `Numeric input between ${minValue} and ${maxValue}`)
</script>
```

### Real-time Validation Input
```vue
<template>
  <div class="validated-input">
    <codex-input
      v-model="validatedValue"
      :debounce-time="300"
      :dusk="'validated-input'"
    />
    
    <div class="validation-indicators">
      <div :class="['indicator', { valid: hasMinLength }]">
        ✓ At least 8 characters
      </div>
      <div :class="['indicator', { valid: hasUppercase }]">
        ✓ Contains uppercase letter
      </div>
      <div :class="['indicator', { valid: hasNumber }]">
        ✓ Contains number
      </div>
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const validatedValue = ref('')

const hasMinLength = computed(() => validatedValue.value.length >= 8)
const hasUppercase = computed(() => /[A-Z]/.test(validatedValue.value))
const hasNumber = computed(() => /\d/.test(validatedValue.value))

const isValid = computed(() => hasMinLength.value && hasUppercase.value && hasNumber.value)

provide('type', 'password')
provide('placeholder', 'Enter a secure password')
provide('ariaLabel', 'Password with validation requirements')
provide('hasError', computed(() => validatedValue.value.length > 0 && !isValid.value))
</script>
```

### Dynamic Input Type
```vue
<template>
  <div class="dynamic-input">
    <div class="input-type-selector">
      <button @click="setInputType('text')" :class="{ active: currentType === 'text' }">
        Text
      </button>
      <button @click="setInputType('email')" :class="{ active: currentType === 'email' }">
        Email
      </button>
      <button @click="setInputType('url')" :class="{ active: currentType === 'url' }">
        URL
      </button>
    </div>
    
    <codex-input
      v-model="dynamicValue"
      :dusk="'dynamic-input'"
    />
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const dynamicValue = ref('')
const currentType = ref('text')

const setInputType = (type) => {
  currentType.value = type
  dynamicValue.value = '' // Clear value when type changes
}

const placeholderText = computed(() => {
  const placeholders = {
    text: 'Enter any text',
    email: 'Enter email address',
    url: 'Enter website URL'
  }
  return placeholders[currentType.value]
})

provide('type', currentType)
provide('placeholder', placeholderText)
provide('ariaLabel', computed(() => `${currentType.value} input field`))
</script>
```

### File Path Input
```vue
<template>
  <div class="file-path-input">
    <codex-input
      v-model="filePath"
      :dusk="'file-path-input'"
      @update:model-value="validateFilePath"
    />
    
    <div v-if="pathValidation" class="path-validation">
      {{ pathValidation }}
    </div>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const filePath = ref('')
const pathValidation = ref('')

const validateFilePath = (path) => {
  if (!path) {
    pathValidation.value = ''
    return
  }
  
  if (path.includes('..')) {
    pathValidation.value = 'Path cannot contain ".."'
  } else if (!/^[a-zA-Z0-9\/\-_.]+$/.test(path)) {
    pathValidation.value = 'Path contains invalid characters'
  } else {
    pathValidation.value = 'Valid file path'
  }
}

provide('type', 'text')
provide('placeholder', '/path/to/file.txt')
provide('ariaLabel', 'File path input')
provide('hasError', computed(() => pathValidation.value.includes('cannot') || pathValidation.value.includes('invalid')))
</script>
```

### Auto-complete Input
```vue
<template>
  <div class="autocomplete-input">
    <codex-input
      v-model="autocompleteValue"
      :debounce-time="200"
      :dusk="'autocomplete-input'"
      @update:model-value="updateSuggestions"
    />
    
    <div v-if="suggestions.length > 0" class="suggestions">
      <div 
        v-for="suggestion in suggestions" 
        :key="suggestion"
        @click="selectSuggestion(suggestion)"
        class="suggestion"
      >
        {{ suggestion }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const autocompleteValue = ref('')
const suggestions = ref([])

const predefinedOptions = [
  'apple', 'banana', 'cherry', 'date', 'elderberry',
  'fig', 'grape', 'honeydew', 'kiwi', 'lemon'
]

const updateSuggestions = (value) => {
  if (!value || value.length < 2) {
    suggestions.value = []
    return
  }
  
  suggestions.value = predefinedOptions.filter(option =>
    option.toLowerCase().includes(value.toLowerCase())
  ).slice(0, 5)
}

const selectSuggestion = (suggestion) => {
  autocompleteValue.value = suggestion
  suggestions.value = []
}

provide('type', 'text')
provide('placeholder', 'Type to search...')
provide('ariaLabel', 'Search with autocomplete')
</script>
```

### Currency Input
```vue
<template>
  <div class="currency-input">
    <div class="currency-symbol">$</div>
    <codex-input
      v-model="currencyValue"
      :debounce-time="300"
      :dusk="'currency-input'"
      @update:model-value="formatCurrency"
    />
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const currencyValue = ref('')

const formatCurrency = (value) => {
  // Remove non-numeric characters except decimal point
  let cleaned = value.replace(/[^0-9.]/g, '')
  
  // Ensure only one decimal point
  const parts = cleaned.split('.')
  if (parts.length > 2) {
    cleaned = parts[0] + '.' + parts.slice(1).join('')
  }
  
  // Limit to 2 decimal places
  if (parts[1] && parts[1].length > 2) {
    cleaned = parts[0] + '.' + parts[1].substring(0, 2)
  }
  
  currencyValue.value = cleaned
}

provide('type', 'text')
provide('placeholder', '0.00')
provide('ariaLabel', 'Currency amount in dollars')
</script>
```

## CSS Classes
- `_c-input`: Base input styling
- `_c-input-{type}`: Type-specific styling based on injection
- `_c-error`: Error state styling

## Best Practices

### Recommended Usage Patterns
- Use appropriate input types for semantic meaning and validation
- Implement debouncing for expensive operations (API calls, complex validation)
- Provide meaningful placeholder text for user guidance
- Use dependency injection for form integration
- Handle error states visually and semantically
- Provide clear ARIA labels for accessibility
- Use v-model for two-way data binding
- Coordinate with parent form components

### Common Pitfalls to Avoid
- Setting debounce time too low (causing performance issues)
- Not providing placeholder text for user guidance
- Using wrong input types (affects mobile keyboards and validation)
- Missing error state handling and feedback
- Not coordinating with parent form components
- Forgetting accessibility attributes
- Missing validation for required fields

### Accessibility Considerations
- Use appropriate input types for semantic meaning
- Provide meaningful ARIA labels
- Handle error states with proper ARIA attributes
- Ensure keyboard navigation works correctly
- Support screen reader announcements
- Use semantic HTML input elements
- Provide clear placeholder text
- Handle focus management appropriately

### Input Type Selection
- **text**: General text input, default choice
- **email**: Email addresses, provides mobile keyboard optimization
- **password**: Sensitive data, masks input content
- **url**: Web addresses, provides validation and mobile optimization
- **tel**: Phone numbers, provides numeric keyboard on mobile
- **number**: Numeric input, provides spinner controls
- **search**: Search queries, may provide enhanced styling
- **date/time**: Date and time selection, provides native pickers

### Error Handling
- Handle validation errors gracefully
- Provide clear error state visualization
- Clear errors when content becomes valid
- Handle edge cases in validation
- Provide meaningful error feedback
- Support multiple validation rules
- Handle async validation appropriately

### State Management
- Use reactive properties for input values
- Handle state changes with v-model
- Manage validation states consistently
- Coordinate with parent component state
- Handle component lifecycle properly
- Track input focus and blur states

### Performance Considerations
- Use appropriate debouncing for expensive operations
- Minimize re-renders during input
- Cache validation results when appropriate
- Optimize input event handling
- Handle large forms efficiently
- Implement proper cleanup for timeouts

### Form Integration
- Use dependency injection for form context
- Coordinate with validation systems
- Handle form submission states
- Provide clear form field relationships
- Support form reset functionality
- Handle form accessibility requirements

### Validation Integration
- Connect with validation libraries
- Handle real-time validation feedback
- Support async validation patterns
- Provide clear validation messaging
- Handle validation state transitions
- Support multiple validation rules

### User Experience
- Provide immediate visual feedback
- Use appropriate input types for mobile optimization
- Handle input formatting (currency, phone numbers, etc.)
- Support auto-completion where appropriate
- Provide helpful placeholder text
- Handle copy/paste operations gracefully

## Component Registration
The component is registered as `codex-input` in the application. 