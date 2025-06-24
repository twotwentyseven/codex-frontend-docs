# Label Component

## Overview
The Label component provides an atomic form label element with tooltip integration, dependency injection support, and accessibility features. It features required field indication, ARIA label support, flexible content configuration, and automatic association with form inputs through the `for` attribute.

## Basic Usage
```vue
<codex-label
  :id="'email-input'"
  :label="'Email Address'"
  :required="true"
  :dusk="'email-label'"
/>
```

## Key Features
- Form input association through `for` attribute
- Required field indication with asterisk (*)
- Integrated tooltip support
- Dependency injection for configuration
- ARIA label support for accessibility
- Testing support with dusk attribute
- Flexible label content configuration
- Automatic required field styling

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| dusk | String | No | '' | Testing identifier attribute |
| id | String | No | '' | Associated input element ID for `for` attribute |
| required | Boolean | No | - | Whether the associated field is required |
| label | String | No | - | Label text content |
| ariaLabel | String | No | - | ARIA label for accessibility |
| name | String | No | - | Associated input name attribute |

### Injected Props
The component receives additional configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| name | String | '' | Input name attribute from parent context |
| ariaLabel | String | '' | ARIA label from parent context |
| label | String | '' | Label text from parent context |
| required | Boolean | '' | Required status from parent context |

## Events
The Label component does not emit any custom events.

## Computed Properties
The component uses computed properties to merge props with injected values:
- **computedName**: Combines prop and injected name values
- **computedAriaLabel**: Combines prop and injected ARIA label values  
- **computedLabel**: Combines prop and injected label text
- **computedRequired**: Combines prop and injected required status

## Required Field Indication
When the `required` status is true:
- Displays an asterisk (*) after the label text
- Applies `_c-required` CSS class to the asterisk
- Provides visual indication of mandatory fields
- Follows standard form labeling conventions

## Tooltip Integration
The component includes an integrated `codex-tooltip` component:
- Automatically rendered within the label
- Inherits configuration from parent context
- Provides additional context or help information
- Follows tooltip dependency injection patterns

## Dependency Injection Integration
The component uses Vue's provide/inject pattern:
- **Form Field Integration**: Receives configuration from parent form components
- **Context Sharing**: Shares label context with child components
- **Accessibility**: Inherits ARIA attributes from form context
- **Validation**: Connects with validation systems

## Accessibility Features
- **Label Association**: Links to input via `for` attribute
- **ARIA Labels**: Supports aria-label through props or injection
- **Required Indication**: Visual and semantic required field marking
- **Screen Reader**: Compatible with assistive technologies
- **Semantic HTML**: Uses proper `<label>` element

## Testing Support
The component supports testing through:
- **Dusk Attribute**: Testing identifier for automated tests
- **ID Association**: Links to specific input elements
- **Name Attribute**: Form field identification
- **Consistent Structure**: Predictable DOM structure for testing

## Internationalization
The Label component does not include built-in internationalization. Label text and ARIA content should be localized before passing to the component.

## Examples

### Basic Form Label
```vue
<codex-label
  :id="'username-input'"
  :label="'Username'"
  :required="true"
  :dusk="'username-label'"
/>
```

### Label with Form Integration
```vue
<template>
  <div class="form-field">
    <codex-label
      :id="'email-field'"
      :label="'Email Address'"
      :required="true"
      :aria-label="'User email address'"
      :dusk="'email-label'"
    />
    <codex-input 
      v-model="email"
      :id="'email-field'"
      :dusk="'email-input'"
    />
    <codex-error :error="emailError" />
  </div>
</template>

<script setup>
const email = ref('')
const emailError = ref('')
</script>
```

### Label with Dependency Injection
```vue
<template>
  <form class="registration-form">
    <div class="form-group">
      <codex-label :dusk="'password-label'" />
      <codex-input v-model="password" />
      <codex-hint />
    </div>
  </form>
</template>

<script setup>
import { provide } from 'vue'

const password = ref('')

provide('id', 'password-input')
provide('name', 'password')
provide('label', 'Password')
provide('ariaLabel', 'Account password')
provide('required', true)
provide('hint', 'Must be at least 8 characters')
</script>
```

### Optional Field Label
```vue
<codex-label
  :id="'phone-input'"
  :label="'Phone Number'"
  :required="false"
  :aria-label="'Optional phone number'"
  :dusk="'phone-label'"
/>
```

### Dynamic Label Content
```vue
<template>
  <div class="dynamic-form">
    <codex-label
      :id="fieldId"
      :label="fieldLabel"
      :required="fieldRequired"
      :dusk="`${fieldType}-label`"
    />
  </div>
</template>

<script setup>
const fieldType = ref('email')

const fieldConfig = computed(() => {
  const configs = {
    email: {
      label: 'Email Address',
      required: true,
      id: 'email-input'
    },
    phone: {
      label: 'Phone Number',
      required: false,
      id: 'phone-input'
    },
    address: {
      label: 'Home Address',
      required: true,
      id: 'address-input'
    }
  }
  return configs[fieldType.value]
})

const fieldId = computed(() => fieldConfig.value.id)
const fieldLabel = computed(() => fieldConfig.value.label)
const fieldRequired = computed(() => fieldConfig.value.required)
</script>
```

### Label with Tooltip Context
```vue
<template>
  <div class="form-field-with-help">
    <codex-label
      :id="'api-key-input'"
      :label="'API Key'"
      :required="true"
      :dusk="'api-key-label'"
    />
    
    <!-- Tooltip is automatically included in label -->
  </div>
</template>

<script setup>
import { provide } from 'vue'

// Provide tooltip configuration
provide('tooltip', 'Your API key can be found in the developer dashboard')
provide('tooltipPosition', 'right')
</script>
```

### Multi-language Label
```vue
<template>
  <div class="multilingual-form">
    <codex-label
      :id="'name-input'"
      :label="localizedLabel"
      :required="true"
      :aria-label="localizedAriaLabel"
      :dusk="'name-label'"
    />
  </div>
</template>

<script setup>
import { computed } from 'vue'

const currentLanguage = ref('en')

const localizedLabel = computed(() => {
  const labels = {
    en: 'Full Name',
    es: 'Nombre Completo',
    fr: 'Nom Complet'
  }
  return labels[currentLanguage.value] || labels.en
})

const localizedAriaLabel = computed(() => {
  const ariaLabels = {
    en: 'Enter your full name',
    es: 'Ingrese su nombre completo',
    fr: 'Entrez votre nom complet'
  }
  return ariaLabels[currentLanguage.value] || ariaLabels.en
})
</script>
```

### Form Section Labels
```vue
<template>
  <form class="multi-section-form">
    <fieldset>
      <legend>Personal Information</legend>
      
      <div class="form-group">
        <codex-label
          :id="'first-name'"
          :label="'First Name'"
          :required="true"
          :dusk="'first-name-label'"
        />
        <codex-input v-model="firstName" :id="'first-name'" />
      </div>
      
      <div class="form-group">
        <codex-label
          :id="'last-name'"
          :label="'Last Name'"
          :required="true"
          :dusk="'last-name-label'"
        />
        <codex-input v-model="lastName" :id="'last-name'" />
      </div>
    </fieldset>
    
    <fieldset>
      <legend>Contact Information</legend>
      
      <div class="form-group">
        <codex-label
          :id="'email'"
          :label="'Email Address'"
          :required="true"
          :aria-label="'Primary email address'"
          :dusk="'email-label'"
        />
        <codex-input v-model="email" :id="'email'" />
      </div>
    </fieldset>
  </form>
</template>
```

### Label with Conditional Required State
```vue
<template>
  <div class="conditional-form">
    <div class="form-group">
      <codex-checkbox v-model="enableNotifications" />
      <label>Enable email notifications</label>
    </div>
    
    <div v-if="enableNotifications" class="form-group">
      <codex-label
        :id="'notification-email'"
        :label="'Notification Email'"
        :required="enableNotifications"
        :dusk="'notification-email-label'"
      />
      <codex-input v-model="notificationEmail" :id="'notification-email'" />
    </div>
  </div>
</template>

<script setup>
const enableNotifications = ref(false)
const notificationEmail = ref('')
</script>
```

### Label for Complex Input Groups
```vue
<template>
  <div class="input-group">
    <codex-label
      :id="'phone-number'"
      :label="'Phone Number'"
      :required="true"
      :aria-label="'Phone number with country code'"
      :dusk="'phone-label'"
    />
    
    <div class="phone-input-group">
      <select v-model="countryCode" class="country-code">
        <option value="+1">US (+1)</option>
        <option value="+44">UK (+44)</option>
      </select>
      
      <codex-input 
        v-model="phoneNumber" 
        :id="'phone-number'"
        :placeholder="'555-0123'"
      />
    </div>
  </div>
</template>

<script setup>
const countryCode = ref('+1')
const phoneNumber = ref('')
</script>
```

## CSS Classes
- `_c-label`: Base label styling
- `_c-required`: Required field asterisk styling

## Best Practices

### Recommended Usage Patterns
- Always associate labels with inputs using `id` and `for`
- Use meaningful and descriptive label text
- Implement proper required field indication
- Provide ARIA labels for complex form fields
- Use dependency injection for form integration
- Include testing identifiers with dusk attributes
- Coordinate label styling with form design
- Handle tooltip integration appropriately

### Common Pitfalls to Avoid
- Missing `for` attribute association with inputs
- Not indicating required fields visually
- Using vague or unclear label text
- Missing ARIA labels for accessibility
- Not coordinating with parent form components
- Forgetting to handle internationalization
- Missing testing support attributes

### Accessibility Considerations
- Always use proper `<label>` elements for form fields
- Provide clear and descriptive label text
- Use ARIA labels for additional context
- Associate labels with inputs via `for` attribute
- Indicate required fields clearly for screen readers
- Ensure sufficient color contrast for label text
- Use semantic HTML structure consistently
- Support keyboard navigation patterns

### Form Integration
- Use dependency injection for form context sharing
- Coordinate with validation systems
- Handle form field relationships properly
- Provide clear form structure with labels
- Support form reset and clear functionality
- Handle form accessibility requirements
- Integrate with tooltip systems appropriately

### Required Field Handling
- Clearly indicate required fields with asterisks
- Use consistent required field styling
- Provide semantic indication for screen readers
- Handle conditional required states appropriately
- Coordinate required indication with validation
- Use appropriate CSS classes for required styling

### Testing Integration
- Use consistent dusk attributes for testing
- Provide predictable label structure for tests
- Handle dynamic label content in tests
- Coordinate with input testing identifiers
- Support automated accessibility testing
- Handle form testing scenarios appropriately

### Internationalization Support
- Localize label text before passing to component
- Handle text direction for RTL languages
- Consider label length variations across languages
- Provide localized ARIA labels
- Handle dynamic language switching
- Test label layouts with longer translated text

### Performance Considerations
- Use computed properties for reactive label content
- Minimize re-renders when label content doesn't change
- Cache tooltip configuration when possible
- Handle large forms with many labels efficiently
- Implement proper cleanup for watchers and computed properties

### Content Guidelines
- Use clear and concise label text
- Avoid redundant information in labels
- Use consistent capitalization (sentence case recommended)
- Provide context without being verbose
- Use parallel structure for related labels
- Consider user mental model when labeling fields

## Component Registration
The component is registered as `codex-label` in the application. 