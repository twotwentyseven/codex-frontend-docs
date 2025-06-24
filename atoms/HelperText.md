# HelperText Component

## Overview
The HelperText component provides a simple atomic element for displaying helper text content through dependency injection. It features conditional rendering, dependency injection integration, and standardized helper text styling for form fields and other contexts where additional guidance is needed.

## Basic Usage
```vue
<codex-helper-text />
```

## Key Features
- Conditional rendering based on helper text availability
- Dependency injection for content configuration
- Simple div-based display structure
- CSS class-based styling
- Form field integration support
- Minimal and lightweight implementation

## Props
The HelperText component does not accept direct props - all configuration is handled through dependency injection.

### Injected Props
The component receives configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| helperText | String | '' | Helper text content to display |

## Events
The HelperText component does not emit any custom events.

## Conditional Rendering
The component only renders when helper text content is available:
- **With Helper Text**: Displays text content in styled container
- **Without Helper Text**: Does not render anything (v-if condition)
- **Empty String**: Treated as no content, component does not render

## Content Display
The component provides straightforward text display:
- **Plain Text**: Displays helper text as provided
- **No HTML**: Content is displayed as plain text (no v-html)
- **CSS Styling**: Applies `_c-helper-text` class for consistent styling

## Dependency Injection Integration
The component uses Vue's provide/inject pattern:
- **Form Field Integration**: Receives helper text from parent form components
- **Context Sharing**: Integrates with form field contexts
- **Configuration**: Accepts helper text through injection
- **Flexibility**: Allows dynamic helper text content

## Accessibility Features
- **Semantic HTML**: Uses div element for text display
- **CSS Classes**: Provides styling hooks for visual design
- **Text Content**: Plain text display for screen reader compatibility
- **Contextual**: Integrates with form field accessibility patterns

## Use Cases
The component is designed for:
- **Form Field Guidance**: Additional help text for form inputs
- **Instructional Content**: Brief instructions or explanations
- **Contextual Information**: Supporting information for UI elements
- **User Guidance**: Helpful tips and clarifications

## Internationalization
The HelperText component does not include built-in internationalization. Helper text content should be localized before injection.

## Examples

### Basic Helper Text
```vue
<template>
  <div class="form-field">
    <codex-label />
    <codex-input v-model="password" />
    <codex-helper-text />
  </div>
</template>

<script setup>
import { provide } from 'vue'

const password = ref('')

provide('label', 'Password')
provide('helperText', 'Password must be at least 8 characters long')
</script>
```

### Form Field with Multiple Text Elements
```vue
<template>
  <div class="form-field">
    <codex-label />
    <codex-input v-model="email" />
    <codex-helper-text />
    <codex-error v-if="emailError" :error="emailError" />
  </div>
</template>

<script setup>
import { provide } from 'vue'

const email = ref('')
const emailError = ref('')

provide('label', 'Email Address')
provide('helperText', 'We will send verification to this email address')
</script>
```

### Dynamic Helper Text
```vue
<template>
  <div class="form-field">
    <codex-input v-model="username" @input="updateHelperText" />
    <codex-helper-text />
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const username = ref('')
const helperText = ref('Choose a unique username')

const updateHelperText = () => {
  if (username.value.length === 0) {
    helperText.value = 'Choose a unique username'
  } else if (username.value.length < 3) {
    helperText.value = 'Username must be at least 3 characters'
  } else {
    helperText.value = 'Username looks good!'
  }
}

provide('helperText', helperText)
</script>
```

### Conditional Helper Text Display
```vue
<template>
  <div class="form-field">
    <codex-input v-model="apiKey" @focus="showApiHelp" @blur="hideApiHelp" />
    <codex-helper-text v-if="showHelp" />
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const apiKey = ref('')
const showHelp = ref(false)

const showApiHelp = () => {
  showHelp.value = true
}

const hideApiHelp = () => {
  showHelp.value = false
}

provide('helperText', 'You can find your API key in the developer dashboard')
</script>
```

### Multi-language Helper Text
```vue
<template>
  <div class="form-field">
    <codex-input v-model="phoneNumber" />
    <codex-helper-text />
  </div>
</template>

<script setup>
import { provide, computed } from 'vue'

const phoneNumber = ref('')
const currentLanguage = ref('en')

const localizedHelperText = computed(() => {
  const texts = {
    en: 'Include area code for US numbers',
    es: 'Incluya el código de área para números de EE.UU.',
    fr: 'Inclure l\'indicatif régional pour les numéros américains'
  }
  return texts[currentLanguage.value] || texts.en
})

provide('helperText', localizedHelperText)
</script>
```

### Form Step with Helper Text
```vue
<template>
  <div class="form-step">
    <h3>Contact Information</h3>
    
    <div class="form-field">
      <codex-label />
      <codex-input v-model="formData.primaryEmail" />
      <codex-helper-text />
    </div>
    
    <div class="form-field">
      <codex-label />
      <codex-input v-model="formData.backupEmail" />
      <codex-helper-text />
    </div>
  </div>
</template>

<script setup>
import { provide } from 'vue'

const formData = ref({
  primaryEmail: '',
  backupEmail: ''
})

// Provide different helper text for each field context
provide('helperText', 'This will be your main contact email')
</script>
```

### Helper Text with Validation Context
```vue
<template>
  <div class="form-field">
    <codex-input v-model="complexField" />
    <codex-helper-text />
    <codex-error v-if="validationError" :error="validationError" />
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const complexField = ref('')
const validationError = ref('')

const contextualHelperText = computed(() => {
  if (validationError.value) {
    return '' // Hide helper text when there's an error
  }
  return 'Format: ABC-123-XYZ (letters, numbers, hyphens only)'
})

provide('helperText', contextualHelperText)
</script>
```

### Progressive Form Helper Text
```vue
<template>
  <div class="progressive-form">
    <div class="form-field">
      <codex-input v-model="currentField" @input="updateProgress" />
      <codex-helper-text />
    </div>
    
    <div class="progress-indicator">
      Step {{ currentStep }} of {{ totalSteps }}
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const currentField = ref('')
const currentStep = ref(1)
const totalSteps = ref(5)

const stepHelperText = computed(() => {
  const helpTexts = {
    1: 'Enter your full legal name as it appears on ID',
    2: 'Provide your current residential address',
    3: 'Enter your primary phone number',
    4: 'Add your email address for notifications',
    5: 'Review and confirm all information'
  }
  return helpTexts[currentStep.value] || ''
})

const updateProgress = () => {
  // Logic to advance to next step based on field completion
}

provide('helperText', stepHelperText)
</script>
```

### Helper Text with Field State
```vue
<template>
  <div class="form-field">
    <codex-input 
      v-model="statefulField" 
      @focus="handleFocus"
      @blur="handleBlur"
      @input="handleInput"
    />
    <codex-helper-text />
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const statefulField = ref('')
const fieldState = ref('initial') // 'initial', 'focused', 'typing', 'complete'
const helperText = ref('')

const handleFocus = () => {
  fieldState.value = 'focused'
  helperText.value = 'Start typing your response'
}

const handleBlur = () => {
  if (statefulField.value) {
    fieldState.value = 'complete'
    helperText.value = 'Field completed successfully'
  } else {
    fieldState.value = 'initial'
    helperText.value = 'This field is required'
  }
}

const handleInput = () => {
  fieldState.value = 'typing'
  helperText.value = `${statefulField.value.length} characters entered`
}

provide('helperText', helperText)
</script>
```

### Helper Text for Complex Input Types
```vue
<template>
  <div class="complex-input-group">
    <div class="form-field">
      <codex-label />
      <div class="date-input-group">
        <input v-model="day" type="number" min="1" max="31" placeholder="DD" />
        <input v-model="month" type="number" min="1" max="12" placeholder="MM" />
        <input v-model="year" type="number" min="1900" max="2100" placeholder="YYYY" />
      </div>
      <codex-helper-text />
    </div>
  </div>
</template>

<script setup>
import { provide } from 'vue'

const day = ref('')
const month = ref('')
const year = ref('')

provide('label', 'Date of Birth')
provide('helperText', 'Enter date in DD/MM/YYYY format')
</script>
```

## CSS Classes
- `_c-helper-text`: Base helper text styling

## Best Practices

### Recommended Usage Patterns
- Use dependency injection for helper text configuration
- Provide clear and concise helper text content
- Position helper text appropriately relative to form fields
- Use helper text to supplement labels, not replace them
- Coordinate helper text with validation messages
- Keep helper text brief and actionable
- Use consistent helper text formatting across application

### Common Pitfalls to Avoid
- Not providing helper text content (component won't render)
- Using helper text as a replacement for proper labels
- Making helper text too verbose or complex
- Not coordinating with error message display
- Missing internationalization considerations
- Not considering responsive design for helper text

### Accessibility Considerations
- Use helper text to provide additional context for form fields
- Ensure helper text is readable and provides value
- Coordinate with ARIA attributes for form field associations
- Use appropriate text contrast for helper text styling
- Consider screen reader experience with helper text
- Position helper text logically in tab order

### Content Guidelines
- Keep helper text concise and helpful
- Use plain language that users can understand
- Provide specific guidance rather than generic statements
- Focus on what the user should do, not what they shouldn't
- Use consistent tone and style across helper texts
- Test helper text with real users for clarity

### Form Integration
- Use helper text to supplement form labels
- Coordinate helper text visibility with form state
- Handle helper text in validation workflows
- Position helper text consistently across form fields
- Consider helper text in form layout and spacing
- Handle helper text during form submission states

### State Management
- Use reactive properties for dynamic helper text
- Handle helper text changes based on user interactions
- Coordinate helper text with form validation states
- Manage helper text visibility appropriately
- Handle helper text updates efficiently

### Performance Considerations
- Use dependency injection efficiently for helper text
- Minimize re-renders when helper text doesn't change
- Cache helper text content when appropriate
- Handle dynamic helper text updates efficiently
- Optimize helper text display for large forms

### Internationalization
- Localize helper text content before injection
- Handle text length variations across languages
- Consider text direction for RTL languages
- Test helper text layouts with translated content
- Provide fallback helper text for missing translations

### Visual Design Integration
- Coordinate helper text styling with design system
- Use appropriate typography for helper text
- Handle helper text spacing and layout consistently
- Consider helper text in responsive design
- Ensure helper text doesn't interfere with form functionality

## Component Registration
The component is registered as `codex-helper-text` in the application. 