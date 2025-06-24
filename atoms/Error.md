# Error Component

## Overview
The Error component provides a flexible atomic element for displaying error messages with slot-based customization. It features conditional rendering based on error presence, scoped slot access to error data, and standardized error message styling for form validation and general error display throughout the application.

## Basic Usage
```vue
<codex-error
  :error="'This field is required'"
/>
```

## Key Features
- Conditional rendering based on error content
- Scoped slot with access to error data
- Default error message display
- CSS class-based error styling
- Flexible content customization through slots
- String-based error message support
- Minimal and lightweight implementation

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| error | String | No | '' | Error message content to display |

## Events
The Error component does not emit any custom events.

## Slots
| Slot Name | Scope | Description |
|-----------|-------|-------------|
| default | `{ error }` | Custom error display with access to error message |

## Conditional Rendering
The component uses conditional rendering for error display:
- **With Error**: Displays error message or slot content when error prop has value
- **Without Error**: Does not render when error prop is empty or falsy
- **Scoped Access**: Slot receives error value for custom implementations

## Slot Customization
The component provides flexible error display through slots:
- **Default Behavior**: Simple div with error message and `_c-error-msg` class
- **Custom Implementation**: Slot can override default display completely
- **Error Access**: Slot receives error prop value as scoped parameter
- **Fallback Content**: Default slot provides standard error styling

## Error Message Display
The default implementation provides:
- **CSS Class**: `_c-error-msg` for consistent error styling
- **Conditional Display**: Only renders when error prop has content
- **Text Content**: Direct display of error string
- **Standard Structure**: Simple div-based error presentation

## Use Cases
The component is suitable for:
- **Form Validation**: Individual field error messages
- **General Errors**: Application-wide error display
- **Custom Error UI**: Flexible slot-based error presentations
- **Validation Feedback**: Real-time form validation messages

## Internationalization
The Error component does not include built-in internationalization. Error messages should be localized before passing to the component.

## Examples

### Basic Error Message
```vue
<codex-error
  :error="'Email address is required'"
/>
```

### Form Field Error
```vue
<template>
  <div class="form-field">
    <codex-label />
    <codex-input v-model="email" />
    <codex-error :error="emailError" />
  </div>
</template>

<script setup>
const email = ref('')
const emailError = ref('')

const validateEmail = () => {
  if (!email.value) {
    emailError.value = 'Email address is required'
  } else if (!isValidEmail(email.value)) {
    emailError.value = 'Please enter a valid email address'
  } else {
    emailError.value = ''
  }
}
</script>
```

### Custom Error Display with Slot
```vue
<template>
  <codex-error :error="customError">
    <template #default="{ error }">
      <div class="custom-error-display">
        <i class="ri-error-warning-fill"></i>
        <span class="error-text">{{ error }}</span>
        <button @click="dismissError" class="dismiss-btn">×</button>
      </div>
    </template>
  </codex-error>
</template>

<script setup>
const customError = ref('Invalid input detected')

const dismissError = () => {
  customError.value = ''
}
</script>
```

### Conditional Error Display
```vue
<template>
  <div class="conditional-error">
    <codex-input v-model="userInput" @blur="validateInput" />
    <codex-error v-if="showError" :error="validationError" />
  </div>
</template>

<script setup>
const userInput = ref('')
const validationError = ref('')
const showError = ref(false)

const validateInput = () => {
  if (userInput.value.length < 3) {
    validationError.value = 'Input must be at least 3 characters'
    showError.value = true
  } else {
    validationError.value = ''
    showError.value = false
  }
}
</script>
```

### Multi-field Error Handling
```vue
<template>
  <form class="multi-field-form">
    <div class="form-group">
      <codex-input v-model="formData.name" />
      <codex-error :error="errors.name" />
    </div>
    
    <div class="form-group">
      <codex-input v-model="formData.email" />
      <codex-error :error="errors.email" />
    </div>
    
    <div class="form-group">
      <codex-input v-model="formData.phone" />
      <codex-error :error="errors.phone" />
    </div>
  </form>
</template>

<script setup>
const formData = ref({
  name: '',
  email: '',
  phone: ''
})

const errors = ref({
  name: '',
  email: '',
  phone: ''
})

const validateForm = () => {
  // Clear previous errors
  errors.value = { name: '', email: '', phone: '' }
  
  if (!formData.value.name) {
    errors.value.name = 'Name is required'
  }
  
  if (!formData.value.email) {
    errors.value.email = 'Email is required'
  } else if (!isValidEmail(formData.value.email)) {
    errors.value.email = 'Invalid email format'
  }
  
  if (!formData.value.phone) {
    errors.value.phone = 'Phone number is required'
  }
}
</script>
```

### Dynamic Error Messages
```vue
<template>
  <div class="dynamic-errors">
    <codex-input v-model="dynamicValue" @input="handleInput" />
    <codex-error :error="currentError" />
  </div>
</template>

<script setup>
const dynamicValue = ref('')
const currentError = ref('')

const errorMessages = {
  empty: 'This field cannot be empty',
  tooShort: 'Value must be at least 5 characters',
  invalid: 'Value contains invalid characters',
  valid: ''
}

const handleInput = () => {
  if (!dynamicValue.value) {
    currentError.value = errorMessages.empty
  } else if (dynamicValue.value.length < 5) {
    currentError.value = errorMessages.tooShort
  } else if (!/^[a-zA-Z0-9]+$/.test(dynamicValue.value)) {
    currentError.value = errorMessages.invalid
  } else {
    currentError.value = errorMessages.valid
  }
}
</script>
```

### Server Error Display
```vue
<template>
  <div class="server-error-display">
    <form @submit="submitForm">
      <!-- Form fields -->
      
      <codex-error :error="serverError">
        <template #default="{ error }">
          <div class="server-error-message">
            <i class="ri-server-fill"></i>
            <div class="error-content">
              <strong>Server Error:</strong>
              <p>{{ error }}</p>
              <button @click="retryRequest" class="retry-btn">Retry</button>
            </div>
          </div>
        </template>
      </codex-error>
    </form>
  </div>
</template>

<script setup>
const serverError = ref('')

const submitForm = async () => {
  try {
    await submitToServer()
    serverError.value = ''
  } catch (error) {
    serverError.value = error.message || 'An unexpected error occurred'
  }
}

const retryRequest = () => {
  serverError.value = ''
  submitForm()
}
</script>
```

### Error with Animation
```vue
<template>
  <div class="animated-error">
    <codex-input v-model="inputValue" />
    <transition name="error-fade">
      <codex-error :error="animatedError">
        <template #default="{ error }">
          <div class="fade-error-message">
            {{ error }}
          </div>
        </template>
      </codex-error>
    </transition>
  </div>
</template>

<script setup>
const inputValue = ref('')
const animatedError = ref('')

// Example validation that triggers animated error
watch(inputValue, (newValue) => {
  if (newValue && newValue.length < 3) {
    animatedError.value = 'Minimum 3 characters required'
  } else {
    animatedError.value = ''
  }
})
</script>

<style scoped>
.error-fade-enter-active, .error-fade-leave-active {
  transition: opacity 0.3s ease;
}
.error-fade-enter-from, .error-fade-leave-to {
  opacity: 0;
}
</style>
```

### Localized Error Messages
```vue
<template>
  <div class="localized-errors">
    <codex-input v-model="value" />
    <codex-error :error="localizedError" />
  </div>
</template>

<script setup>
const value = ref('')
const currentLanguage = ref('en')
const errorKey = ref('required')

const localizedError = computed(() => {
  const translations = {
    en: {
      required: 'This field is required',
      invalid: 'Invalid input format',
      tooShort: 'Input too short'
    },
    es: {
      required: 'Este campo es obligatorio',
      invalid: 'Formato de entrada inválido',
      tooShort: 'Entrada demasiado corta'
    },
    fr: {
      required: 'Ce champ est obligatoire',
      invalid: 'Format d\'entrée invalide',
      tooShort: 'Entrée trop courte'
    }
  }
  
  const lang = translations[currentLanguage.value] || translations.en
  return errorKey.value ? lang[errorKey.value] || '' : ''
})
</script>
```

### Error with Help Action
```vue
<template>
  <div class="error-with-help">
    <codex-input v-model="complexInput" />
    <codex-error :error="helpfulError">
      <template #default="{ error }">
        <div class="helpful-error">
          <span class="error-message">{{ error }}</span>
          <button @click="showHelp" class="help-button">
            Get Help
          </button>
        </div>
      </template>
    </codex-error>
  </div>
</template>

<script setup>
const complexInput = ref('')
const helpfulError = ref('Format not recognized')

const showHelp = () => {
  // Open help modal or tooltip
  console.log('Showing help for format requirements')
}
</script>
```

### Error State Integration
```vue
<template>
  <div class="error-state-integration">
    <codex-input v-model="stateValue" :class="{ 'has-error': hasError }" />
    <codex-error :error="stateError" />
  </div>
</template>

<script setup>
const stateValue = ref('')
const stateError = ref('')

const hasError = computed(() => !!stateError.value)

const validateState = () => {
  if (!stateValue.value) {
    stateError.value = 'Value is required'
  } else {
    stateError.value = ''
  }
}
</script>
```

## CSS Classes
- `_c-error-msg`: Default error message styling

## Best Practices

### Recommended Usage Patterns
- Use the error prop for simple string-based error messages
- Leverage scoped slots for custom error presentations
- Handle conditional rendering appropriately
- Provide clear and actionable error messages
- Coordinate with form validation systems
- Position errors logically relative to associated elements
- Use consistent error message formatting

### Common Pitfalls to Avoid
- Not providing error content (component won't render)
- Using overly technical error messages for users
- Missing error clearing when validation passes
- Not handling error state transitions smoothly
- Forgetting to coordinate with parent component validation
- Using slots unnecessarily for simple error display

### Accessibility Considerations
- Provide clear and descriptive error messages
- Use appropriate ARIA attributes for error associations
- Ensure error messages are announced by screen readers
- Coordinate with form field ARIA relationships
- Use semantic HTML in slot implementations
- Handle focus management when errors appear
- Provide sufficient color contrast for error text

### Error Message Quality
- Write clear and specific error messages
- Provide actionable guidance for resolving errors
- Use consistent language and tone across errors
- Avoid technical jargon in user-facing messages
- Be concise while remaining helpful
- Use parallel structure for similar error types

### State Management
- Clear errors when validation passes
- Handle error timing appropriately
- Coordinate error display with form submission
- Use reactive properties for dynamic errors
- Handle error state transitions smoothly
- Track error display lifecycle properly

### Form Integration
- Position errors near associated form fields
- Coordinate with field validation states
- Handle real-time validation feedback
- Support both synchronous and asynchronous validation
- Clear errors at appropriate times
- Integrate with form submission handling

### Custom Implementation
- Use slots for complex error presentations
- Access error data through scoped slot parameters
- Maintain consistent styling in custom implementations
- Handle error actions (dismiss, retry, help) appropriately
- Test custom error displays thoroughly
- Provide fallback behavior for slot failures

### Performance Considerations
- Minimize re-renders when error content doesn't change
- Use computed properties for dynamic error messages
- Handle error state updates efficiently
- Cache error message translations when appropriate
- Optimize conditional rendering logic

### Internationalization
- Localize error messages before passing to component
- Handle error message length variations across languages
- Support RTL languages in custom slot implementations
- Test error layouts with translated content
- Provide fallback error messages for missing translations

## Component Registration
The component is registered as `codex-error` in the application. 