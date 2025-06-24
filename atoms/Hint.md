# Hint Component

## Overview
The Hint component provides an interactive hint button with link handling capabilities, modal integration, and custom event support. It features dynamic content display through dependency injection, URL validation, modal triggering, and conditional rendering based on hint availability.

## Basic Usage
```vue
<codex-hint />
```

## Key Features
- Conditional rendering based on hint content availability
- Interactive button with click prevention
- URL validation and external link handling
- Custom modal integration through events
- Custom event emission for flexible interaction
- HTML content support through v-html
- Dependency injection for configuration
- Type-specific button behavior

## Props
The Hint component does not accept direct props - all configuration is handled through dependency injection.

### Injected Props
The component receives configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| hint | String | '' | Hint content to display (HTML supported) |
| link | String | '' | Link URL or action identifier |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| close | None | Emitted when modal closing is triggered |
| customEvent | None | Emitted when link value is 'custom' |

## Link Handling Behavior
The component supports multiple link handling modes:

### URL Links
- **Valid URLs**: Opens in same window using `window.open(link, '_self')`
- **URL Validation**: Uses native URL constructor for validation
- **External Navigation**: Direct browser navigation to provided URL

### Custom Events  
- **Custom Trigger**: When link is `'custom'`, emits `customEvent`
- **Flexible Integration**: Allows parent components to handle custom interactions
- **Event-based Communication**: Uses Vue event system

### Modal Integration
- **Modal Prefix**: Links starting with `'codex-'` trigger modal system
- **Custom Events**: Dispatches `'codex.modal.toggle.' + link` events
- **Close Event**: Emits `'close'` event after modal dispatch
- **Document Events**: Uses document-level event dispatch for modal coordination

## Conditional Rendering
The component only renders when hint content is available:
- **With Hint Content**: Displays interactive button with hint content
- **Without Hint**: Does not render (v-if condition)
- **HTML Support**: Renders hint content as HTML through v-html

## URL Validation
The component includes URL validation logic:
- **Valid URL Check**: Uses try/catch with URL constructor
- **Error Handling**: Gracefully handles invalid URL formats
- **Boolean Return**: Returns true for valid URLs, false otherwise

## Dependency Injection Integration
The component uses Vue's provide/inject pattern:
- **Content Injection**: Receives hint content from parent context
- **Link Injection**: Receives link configuration from parent
- **Form Integration**: Connects with form field contexts
- **Modal System**: Integrates with application modal system

## Accessibility Features
- **Button Element**: Uses semantic button element
- **Type Attribute**: Specifies button type for form integration
- **Click Prevention**: Prevents default button behavior
- **HTML Content**: Supports rich hint content display

## Internationalization
The Hint component does not include built-in internationalization. Hint content should be localized before injection.

## Examples

### Basic Hint with Link
```vue
<template>
  <div class="form-field">
    <codex-label />
    <codex-input v-model="apiKey" />
    <codex-hint />
  </div>
</template>

<script setup>
import { provide } from 'vue'

const apiKey = ref('')

provide('hint', 'Click here to learn how to find your API key')
provide('link', 'https://docs.example.com/api-keys')
</script>
```

### Hint with Modal Integration
```vue
<template>
  <div class="form-field">
    <codex-input v-model="securityCode" />
    <codex-hint @close="handleHintClose" />
  </div>
</template>

<script setup>
import { provide } from 'vue'

const securityCode = ref('')

const handleHintClose = () => {
  console.log('Hint modal closed')
}

provide('hint', 'What is a security code?')
provide('link', 'codex-security-code-help')
</script>
```

### Hint with Custom Event
```vue
<template>
  <div class="form-field">
    <codex-input v-model="complexField" />
    <codex-hint @custom-event="handleCustomHint" />
  </div>
</template>

<script setup>
import { provide } from 'vue'

const complexField = ref('')

const handleCustomHint = () => {
  // Custom hint handling logic
  showCustomHelpDialog()
}

const showCustomHelpDialog = () => {
  // Show custom help interface
}

provide('hint', 'Need help with this field?')
provide('link', 'custom')
</script>
```

### Rich HTML Hint Content
```vue
<template>
  <div class="form-field">
    <codex-input v-model="email" />
    <codex-hint />
  </div>
</template>

<script setup>
import { provide } from 'vue'

const email = ref('')

provide('hint', '<strong>Email Format:</strong> Use your work email address ending in @company.com')
provide('link', 'https://help.company.com/email-guidelines')
</script>
```

### Conditional Hint Display
```vue
<template>
  <div class="form-field">
    <codex-input v-model="advancedSetting" />
    <!-- Hint only shows when user is in advanced mode -->
    <codex-hint v-if="showAdvancedHints" />
  </div>
</template>

<script setup>
import { provide, computed } from 'vue'

const advancedSetting = ref('')
const userLevel = ref('basic') // 'basic' or 'advanced'

const showAdvancedHints = computed(() => userLevel.value === 'advanced')

provide('hint', 'This setting affects system performance')
provide('link', 'https://docs.example.com/advanced-settings')
</script>
```

### Multi-language Hint
```vue
<template>
  <div class="form-field">
    <codex-input v-model="phoneNumber" />
    <codex-hint />
  </div>
</template>

<script setup>
import { provide, computed } from 'vue'

const phoneNumber = ref('')
const currentLanguage = ref('en')

const localizedHint = computed(() => {
  const hints = {
    en: 'Include country code for international numbers',
    es: 'Incluya el código de país para números internacionales',
    fr: 'Inclure l\'indicatif du pays pour les numéros internationaux'
  }
  return hints[currentLanguage.value] || hints.en
})

const localizedLink = computed(() => {
  const links = {
    en: 'https://help.example.com/phone-format',
    es: 'https://help.example.com/es/formato-telefono',
    fr: 'https://help.example.com/fr/format-telephone'
  }
  return links[currentLanguage.value] || links.en
})

provide('hint', localizedHint)
provide('link', localizedLink)
</script>
```

### Form Field with Multiple Help Options
```vue
<template>
  <div class="complex-form-field">
    <codex-label />
    <codex-input v-model="taxId" />
    <div class="help-options">
      <codex-hint />
      <button @click="showExamples" class="example-button">
        Show Examples
      </button>
    </div>
  </div>
</template>

<script setup>
import { provide } from 'vue'

const taxId = ref('')

const showExamples = () => {
  // Show examples modal or dropdown
}

provide('label', 'Tax ID Number')
provide('hint', 'Format varies by country')
provide('link', 'codex-tax-id-formats')
</script>
```

### Hint with Dynamic Content
```vue
<template>
  <div class="form-field">
    <codex-input v-model="dynamicField" />
    <codex-hint @custom-event="handleDynamicHint" />
  </div>
</template>

<script setup>
import { provide, computed } from 'vue'

const dynamicField = ref('')
const fieldType = ref('email')

const dynamicHint = computed(() => {
  const hints = {
    email: 'Enter a valid email address',
    phone: 'Enter phone number with area code',
    url: 'Enter a complete URL starting with http://',
    password: 'Password must be at least 8 characters'
  }
  return hints[fieldType.value] || 'Enter a value'
})

const dynamicLink = computed(() => {
  return `https://help.example.com/${fieldType.value}-format`
})

const handleDynamicHint = () => {
  // Handle field-specific help
  console.log(`Showing help for ${fieldType.value} field`)
}

provide('hint', dynamicHint)
provide('link', dynamicLink)
</script>
```

### Hint with Error State Integration
```vue
<template>
  <div class="form-field">
    <codex-input v-model="validatedField" />
    <codex-error v-if="fieldError" :error="fieldError" />
    <codex-hint />
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const validatedField = ref('')
const fieldError = ref('')

const hintContent = computed(() => {
  if (fieldError.value) {
    return 'Click for help resolving this error'
  }
  return 'Click for formatting guidelines'
})

const hintLink = computed(() => {
  if (fieldError.value) {
    return 'codex-error-help'
  }
  return 'https://help.example.com/formatting'
})

provide('hint', hintContent)
provide('link', hintLink)
</script>
```

### Progressive Hint Disclosure
```vue
<template>
  <div class="form-field">
    <codex-input v-model="complexInput" @focus="handleFieldFocus" />
    <codex-hint v-if="showHint" @custom-event="showDetailedHelp" />
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const complexInput = ref('')
const showHint = ref(false)

const handleFieldFocus = () => {
  // Show hint when user focuses on complex field
  showHint.value = true
}

const showDetailedHelp = () => {
  // Show comprehensive help interface
  openDetailedHelpModal()
}

provide('hint', 'This field requires special formatting')
provide('link', 'custom')
</script>
```

## CSS Classes
- `_c-hint`: Base hint button styling

## Best Practices

### Recommended Usage Patterns
- Use dependency injection for hint configuration
- Provide clear and concise hint content
- Choose appropriate link handling based on use case
- Handle custom events for complex interactions
- Use HTML content for rich hint display
- Coordinate with form validation systems
- Provide meaningful hint text for accessibility

### Common Pitfalls to Avoid
- Not providing hint content (component won't render)
- Using invalid URLs without proper validation
- Missing event handlers for custom events
- Not handling modal integration properly
- Forgetting to prevent default button behavior
- Missing coordination with parent form components

### Accessibility Considerations
- Use semantic button elements for hints
- Provide meaningful hint content for screen readers
- Ensure hint buttons are keyboard accessible
- Handle focus management appropriately
- Use appropriate ARIA attributes when needed
- Support assistive technology navigation
- Provide clear hint purpose and destination

### Link Handling
- Validate URLs before using external links
- Use appropriate navigation methods for different link types
- Handle link errors gracefully
- Provide fallback behavior for invalid links
- Consider security implications of external links
- Test link functionality across different browsers

### Custom Event Integration
- Use descriptive event names for custom interactions
- Handle custom events appropriately in parent components
- Provide fallback behavior when custom events aren't handled
- Document custom event behavior clearly
- Test custom event integration thoroughly

### Modal Integration
- Use consistent modal naming conventions
- Handle modal state coordination properly
- Provide appropriate close event handling
- Test modal integration across application
- Handle modal focus management
- Coordinate with application modal systems

### Content Management
- Keep hint content concise and actionable
- Use HTML content responsibly for formatting
- Localize hint content appropriately
- Handle dynamic content updates efficiently
- Provide context-appropriate hint information
- Test hint content across different screen sizes

### Form Integration
- Coordinate hints with form validation
- Position hints appropriately relative to form fields
- Handle hint visibility based on form state
- Provide field-specific hint content
- Support progressive disclosure of hints
- Handle hint interactions during form submission

### Performance Considerations
- Use dependency injection efficiently
- Minimize re-renders of hint content
- Cache hint configuration when possible
- Handle dynamic hint updates appropriately
- Optimize HTML content rendering
- Implement proper cleanup for event listeners

## Component Registration
The component is registered as `codex-hint` in the application. 