# ErrorList Component

## Overview
The ErrorList component provides a comprehensive error display system for showing multiple error messages in a structured list format. It features conditional rendering, dependency injection integration, visual error indication with icons, and semantic HTML structure for accessibility and form validation feedback.

## Basic Usage
```vue
<codex-error-list />
```

## Key Features
- Conditional rendering based on error availability
- Multiple error message display in list format
- Visual error indication with Remix icon
- Dependency injection for error configuration
- Semantic HTML list structure
- CSS class-based styling system
- Error count-based visibility
- Icon-enhanced error presentation

## Props
The ErrorList component does not accept direct props - all configuration is handled through dependency injection.

### Injected Props
The component receives configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| errors | Array | [] | Array of error messages to display |

## Events
The ErrorList component does not emit any custom events.

## Conditional Rendering
The component only renders when errors are present:
- **With Errors**: Displays error list with icon and messages
- **Without Errors**: Does not render anything (v-if condition)
- **Empty Array**: Treated as no errors, component does not render

## Error Display Structure
The component provides structured error presentation:
- **Container**: `_c-error-list-container` wrapper with icon and list
- **Icon**: `ri-error-warning-fill` Remix icon for visual indication
- **List**: `_c-error-list _c-ul` semantic unordered list
- **Items**: Individual list items for each error message

## Icon Integration
The component includes visual error indication:
- **Remix Icon**: Uses `ri-error-warning-fill` for error indication
- **Visual Cue**: Provides immediate visual feedback for errors
- **Accessibility**: Enhances error recognition for users
- **Consistent Design**: Follows application icon patterns

## Dependency Injection Integration
The component uses Vue's provide/inject pattern:
- **Form Integration**: Receives errors from parent form components
- **Validation Systems**: Connects with validation error collections
- **Context Sharing**: Integrates with form validation contexts
- **Dynamic Updates**: Supports reactive error arrays

## Accessibility Features
- **Semantic HTML**: Uses proper ul/li list structure
- **Visual Icons**: Provides clear error indication
- **List Semantics**: Screen reader compatible list presentation
- **Error Grouping**: Logical grouping of multiple errors
- **ARIA Support**: Compatible with assistive technologies

## List Structure
The component renders errors in semantic list format:
- **Unordered List**: Uses `<ul>` element for semantic structure
- **List Items**: Each error rendered in `<li>` element
- **CSS Classes**: Applies `_c-error-list` and `_c-ul` classes
- **Key Binding**: Uses error content as key for Vue rendering

## Internationalization
The ErrorList component does not include built-in internationalization. Error messages should be localized before injection.

## Examples

### Basic Error List
```vue
<template>
  <form class="validation-form">
    <div class="form-fields">
      <!-- Form fields -->
    </div>
    
    <codex-error-list />
  </form>
</template>

<script setup>
import { provide, ref } from 'vue'

const errors = ref([
  'Email address is required',
  'Password must be at least 8 characters',
  'Terms and conditions must be accepted'
])

provide('errors', errors)
</script>
```

### Form Validation Error Display
```vue
<template>
  <form class="registration-form" @submit="handleSubmit">
    <div class="form-group">
      <codex-label />
      <codex-input v-model="formData.email" />
    </div>
    
    <div class="form-group">
      <codex-label />
      <codex-input v-model="formData.password" type="password" />
    </div>
    
    <codex-error-list />
    
    <button type="submit">Register</button>
  </form>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const formData = ref({
  email: '',
  password: ''
})

const validationErrors = ref([])

const handleSubmit = (event) => {
  event.preventDefault()
  validateForm()
}

const validateForm = () => {
  const errors = []
  
  if (!formData.value.email) {
    errors.push('Email address is required')
  } else if (!isValidEmail(formData.value.email)) {
    errors.push('Please enter a valid email address')
  }
  
  if (!formData.value.password) {
    errors.push('Password is required')
  } else if (formData.value.password.length < 8) {
    errors.push('Password must be at least 8 characters long')
  }
  
  validationErrors.value = errors
}

provide('errors', validationErrors)
</script>
```

### Dynamic Error List Updates
```vue
<template>
  <div class="dynamic-form">
    <div class="form-fields">
      <codex-input v-model="username" @input="validateUsername" />
      <codex-input v-model="email" @input="validateEmail" />
    </div>
    
    <codex-error-list />
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const username = ref('')
const email = ref('')
const currentErrors = ref([])

const validateUsername = () => {
  const usernameErrors = []
  
  if (username.value.length > 0 && username.value.length < 3) {
    usernameErrors.push('Username must be at least 3 characters')
  }
  
  if (username.value.includes(' ')) {
    usernameErrors.push('Username cannot contain spaces')
  }
  
  updateErrors('username', usernameErrors)
}

const validateEmail = () => {
  const emailErrors = []
  
  if (email.value.length > 0 && !isValidEmail(email.value)) {
    emailErrors.push('Please enter a valid email address')
  }
  
  updateErrors('email', emailErrors)
}

const errorMap = ref({})

const updateErrors = (field, errors) => {
  errorMap.value[field] = errors
  
  // Flatten all errors into single array
  currentErrors.value = Object.values(errorMap.value).flat()
}

provide('errors', currentErrors)
</script>
```

### Multi-step Form Error Summary
```vue
<template>
  <div class="multi-step-form">
    <div class="step-content">
      <!-- Current step content -->
    </div>
    
    <div class="error-summary">
      <h4 v-if="allErrors.length > 0">Please correct the following issues:</h4>
      <codex-error-list />
    </div>
    
    <div class="step-navigation">
      <button @click="previousStep">Previous</button>
      <button @click="nextStep" :disabled="allErrors.length > 0">Next</button>
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const currentStep = ref(1)
const stepErrors = ref({
  1: [],
  2: [],
  3: []
})

const allErrors = computed(() => {
  return Object.values(stepErrors.value).flat()
})

const validateCurrentStep = () => {
  const errors = []
  
  // Step-specific validation logic
  switch (currentStep.value) {
    case 1:
      // Validate step 1 fields
      break
    case 2:
      // Validate step 2 fields
      break
    case 3:
      // Validate step 3 fields
      break
  }
  
  stepErrors.value[currentStep.value] = errors
}

provide('errors', allErrors)
</script>
```

### Server Error Display
```vue
<template>
  <div class="form-with-server-errors">
    <form @submit="submitForm">
      <!-- Form fields -->
      
      <codex-error-list />
      
      <button type="submit" :disabled="isSubmitting">
        {{ isSubmitting ? 'Submitting...' : 'Submit' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const isSubmitting = ref(false)
const serverErrors = ref([])

const submitForm = async (event) => {
  event.preventDefault()
  isSubmitting.value = true
  serverErrors.value = []
  
  try {
    await submitToServer(formData.value)
    // Handle success
  } catch (error) {
    if (error.response && error.response.data.errors) {
      serverErrors.value = error.response.data.errors
    } else {
      serverErrors.value = ['An unexpected error occurred. Please try again.']
    }
  } finally {
    isSubmitting.value = false
  }
}

provide('errors', serverErrors)
</script>
```

### Conditional Error Display
```vue
<template>
  <div class="conditional-errors">
    <div class="form-section">
      <!-- Form fields -->
    </div>
    
    <!-- Only show errors during validation or after submission attempt -->
    <codex-error-list v-if="showErrors" />
    
    <button @click="attemptSubmit">Submit</button>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const showErrors = ref(false)
const validationErrors = ref([])

const attemptSubmit = () => {
  showErrors.value = true
  validateForm()
  
  if (validationErrors.value.length === 0) {
    // Proceed with submission
    submitForm()
  }
}

const validateForm = () => {
  const errors = []
  // Validation logic
  validationErrors.value = errors
}

provide('errors', validationErrors)
</script>
```

### Localized Error Messages
```vue
<template>
  <div class="localized-form">
    <form @submit="handleSubmit">
      <!-- Form fields -->
      
      <codex-error-list />
    </form>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const currentLanguage = ref('en')
const errorKeys = ref(['required_email', 'invalid_password'])

const localizedErrors = computed(() => {
  const translations = {
    en: {
      required_email: 'Email address is required',
      invalid_password: 'Password must be at least 8 characters'
    },
    es: {
      required_email: 'La dirección de correo electrónico es obligatoria',
      invalid_password: 'La contraseña debe tener al menos 8 caracteres'
    },
    fr: {
      required_email: 'L\'adresse e-mail est obligatoire',
      invalid_password: 'Le mot de passe doit comporter au moins 8 caractères'
    }
  }
  
  const lang = translations[currentLanguage.value] || translations.en
  return errorKeys.value.map(key => lang[key] || key)
})

provide('errors', localizedErrors)
</script>
```

### Error List with Categorization
```vue
<template>
  <div class="categorized-errors">
    <div class="form-content">
      <!-- Form fields -->
    </div>
    
    <div v-if="hasFieldErrors" class="field-errors">
      <h4>Field Errors:</h4>
      <codex-error-list />
    </div>
    
    <div v-if="hasServerErrors" class="server-errors">
      <h4>Server Errors:</h4>
      <codex-error-list />
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const fieldErrors = ref([])
const serverErrors = ref([])

const hasFieldErrors = computed(() => fieldErrors.value.length > 0)
const hasServerErrors = computed(() => serverErrors.value.length > 0)

// Provide different error contexts
provide('errors', fieldErrors) // For first error list
// Would need separate context for server errors
</script>
```

### Error List with Custom Styling
```vue
<template>
  <div class="styled-error-form">
    <form class="form-content">
      <!-- Form fields -->
    </form>
    
    <div class="error-section" :class="{ 'has-errors': formErrors.length > 0 }">
      <codex-error-list />
    </div>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const formErrors = ref([
  'Please review the highlighted fields',
  'All required information must be provided'
])

provide('errors', formErrors)
</script>

<style scoped>
.error-section {
  transition: all 0.3s ease;
  opacity: 0;
  height: 0;
  overflow: hidden;
}

.error-section.has-errors {
  opacity: 1;
  height: auto;
  padding: 1rem;
  background-color: #fee;
  border-left: 4px solid #dc2626;
}
</style>
```

### Progressive Error Disclosure
```vue
<template>
  <div class="progressive-errors">
    <form @submit="handleSubmit">
      <!-- Form fields -->
      
      <div v-if="showValidationErrors" class="validation-feedback">
        <codex-error-list />
      </div>
    </form>
  </div>
</template>

<script setup>
import { provide, ref, watch } from 'vue'

const formData = ref({ /* form fields */ })
const currentErrors = ref([])
const showValidationErrors = ref(false)

// Show errors only after user attempts to submit or after significant interaction
const userHasInteracted = ref(false)

const handleSubmit = (event) => {
  event.preventDefault()
  showValidationErrors.value = true
  validateForm()
}

// Watch for user interaction
watch(formData, () => {
  userHasInteracted.value = true
}, { deep: true })

// Show errors only when appropriate
watch([currentErrors, userHasInteracted], ([errors, interacted]) => {
  if (interacted && errors.length > 0) {
    showValidationErrors.value = true
  }
})

provide('errors', currentErrors)
</script>
```

## CSS Classes
- `_c-error-list-container`: Container wrapper with icon and list
- `_c-error-list`: Error list styling
- `_c-ul`: Semantic list styling

## Best Practices

### Recommended Usage Patterns
- Use dependency injection for error configuration
- Display errors in logical groupings
- Provide clear and actionable error messages
- Use semantic HTML list structure
- Coordinate with form validation systems
- Handle error state transitions smoothly
- Position error lists appropriately in forms

### Common Pitfalls to Avoid
- Not providing error array (component won't render)
- Using overly technical error messages
- Displaying too many errors simultaneously
- Missing error categorization for complex forms
- Not clearing errors when issues are resolved
- Forgetting internationalization for error messages

### Accessibility Considerations
- Use semantic list elements for error structure
- Provide clear and descriptive error messages
- Ensure error lists are announced by screen readers
- Use appropriate heading structure for error sections
- Handle focus management when errors appear
- Coordinate with ARIA live regions for dynamic updates

### Error Message Quality
- Write clear and specific error messages
- Provide actionable guidance for resolving errors
- Use consistent language and tone
- Avoid technical jargon in user-facing errors
- Group related errors logically
- Prioritize critical errors appropriately

### Form Integration
- Coordinate error display with form validation
- Clear errors when issues are resolved
- Handle error state during form submission
- Provide appropriate error timing and feedback
- Support both field-level and form-level errors
- Handle server error integration gracefully

### State Management
- Use reactive arrays for dynamic error updates
- Handle error state transitions appropriately
- Clear errors at appropriate times
- Coordinate with validation lifecycle
- Track error display states efficiently

### Performance Considerations
- Use dependency injection efficiently for errors
- Minimize re-renders when error content doesn't change
- Handle large error lists appropriately
- Optimize error display updates
- Implement proper cleanup for error watchers

### Internationalization
- Localize error messages before injection
- Handle error message length variations
- Support RTL languages for error display
- Test error layouts with translated content
- Provide fallback error messages

### Visual Design Integration
- Coordinate error styling with design system
- Use appropriate visual hierarchy for errors
- Handle error display spacing consistently
- Consider error list responsive behavior
- Ensure error visibility doesn't overwhelm interface

## Component Registration
The component is registered as `codex-error-list` in the application. 