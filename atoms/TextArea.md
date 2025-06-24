# TextArea Component

## Overview
The TextArea component provides a multi-line text input element with v-model support, debouncing capabilities, error state handling, and dependency injection integration. It features textarea-specific styling, validation feedback, accessibility support, and configurable input behavior for form integration.

## Basic Usage
```vue
<codex-textarea
  v-model="textValue"
  :dusk="'comment-textarea'"
  :debounce-time="300"
/>
```

## Key Features
- Two-way data binding with v-model support
- Input debouncing for performance optimization
- Multi-line text input with textarea element
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
| modelValue | String\|Number | No | '' | Textarea value for two-way binding |
| dusk | String | No | '' | Testing identifier attribute |
| debounceTime | Number | No | 300 | Debounce delay in milliseconds |

### Injected Props
The component receives additional configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| placeholder | String | '' | Placeholder text for textarea |
| ariaLabel | String | '' | ARIA label for accessibility |
| name | String | '' | Textarea name attribute |
| id | String | '' | Textarea ID attribute |
| type | String | '' | Input type for CSS class generation |
| required | Boolean | false | Whether textarea is required |
| readonly | Boolean | false | Whether textarea is readonly |
| hasError | Boolean | false | Whether textarea has validation errors |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| update:modelValue | `value: String\|Number` | Emitted when textarea value changes |

## Textarea Styling
The component applies specific CSS classes:
- **Base Classes**: `_c-input _c-textarea`
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
- **Default Delay**: 300ms (configurable via debounceTime prop)
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
- **Keyboard Navigation**: Standard textarea keyboard support
- **Screen Reader**: Compatible with assistive technologies

## Input Type Integration
While using a textarea element, the component:
- **Accepts Type Injection**: For CSS class generation
- **Applies Type Classes**: `_c-input-{type}` for consistent styling
- **Maintains Functionality**: Full textarea behavior regardless of type

## Internationalization
The TextArea component does not include built-in internationalization. Placeholder text and labels should be localized before injection.

## Examples

### Basic Textarea
```vue
<codex-textarea
  v-model="comment"
  :dusk="'user-comment'"
/>
```

### Textarea with Custom Debouncing
```vue
<codex-textarea
  v-model="description"
  :debounce-time="500"
  :dusk="'product-description'"
/>
```

### Textarea with Form Integration
```vue
<template>
  <div class="form-field">
    <codex-label />
    <codex-textarea
      v-model="formData.message"
      :dusk="'message-textarea'"
    />
    <codex-error v-if="errors.message" :error="errors.message" />
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const formData = ref({ message: '' })
const errors = ref({})

const messageHasError = computed(() => !!errors.value.message)

provide('id', 'message-textarea')
provide('name', 'message')
provide('placeholder', 'Enter your message here...')
provide('ariaLabel', 'Message content')
provide('required', true)
provide('hasError', messageHasError)
</script>
```

### Long-form Content Textarea
```vue
<template>
  <div class="content-editor">
    <codex-textarea
      v-model="articleContent"
      :debounce-time="1000"
      :dusk="'article-content'"
    />
    
    <div class="content-stats">
      Characters: {{ articleContent.length }}
      Words: {{ wordCount }}
    </div>
  </div>
</template>

<script setup>
import { provide, computed } from 'vue'

const articleContent = ref('')

const wordCount = computed(() => {
  return articleContent.value.trim().split(/\s+/).filter(word => word.length > 0).length
})

provide('placeholder', 'Write your article content here...')
provide('ariaLabel', 'Article content editor')
provide('type', 'text')
provide('required', true)
</script>
```

### Feedback Form Textarea
```vue
<template>
  <form class="feedback-form">
    <div class="form-group">
      <codex-label />
      <codex-textarea
        v-model="feedback"
        :debounce-time="400"
        :dusk="'feedback-textarea'"
      />
      <codex-helper-text />
    </div>
  </form>
</template>

<script setup>
import { provide } from 'vue'

const feedback = ref('')

provide('id', 'feedback-input')
provide('name', 'feedback')
provide('label', 'Your Feedback')
provide('placeholder', 'Please share your thoughts and suggestions...')
provide('ariaLabel', 'Provide your feedback')
provide('helperText', 'Your feedback helps us improve our service')
provide('required', false)
</script>
```

### Textarea with Character Limit
```vue
<template>
  <div class="limited-textarea">
    <codex-textarea
      v-model="limitedText"
      :dusk="'limited-textarea'"
      @update:model-value="handleTextChange"
    />
    
    <div class="character-counter" :class="{ 'near-limit': isNearLimit, 'over-limit': isOverLimit }">
      {{ limitedText.length }} / {{ maxLength }}
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const limitedText = ref('')
const maxLength = 500

const isNearLimit = computed(() => limitedText.value.length > maxLength * 0.8)
const isOverLimit = computed(() => limitedText.value.length > maxLength)

const handleTextChange = (value) => {
  if (value.length <= maxLength) {
    limitedText.value = value
  }
}

provide('placeholder', `Enter up to ${maxLength} characters...`)
provide('ariaLabel', `Text input with ${maxLength} character limit`)
provide('hasError', isOverLimit)
</script>
```

### Multi-step Form Textarea
```vue
<template>
  <div class="form-step">
    <codex-textarea
      v-model="stepData"
      :debounce-time="200"
      :dusk="'step-textarea'"
      @update:model-value="handleStepInput"
    />
    
    <div class="step-actions">
      <button @click="previousStep" :disabled="currentStep === 1">Previous</button>
      <button @click="nextStep" :disabled="!canProceed">Next</button>
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const stepData = ref('')
const currentStep = ref(2)

const canProceed = computed(() => stepData.value.trim().length > 0)

const handleStepInput = (value) => {
  // Trigger step validation
  validateStep(value)
}

provide('placeholder', 'Describe your requirements in detail...')
provide('required', true)
</script>
```

### Readonly Display Textarea
```vue
<template>
  <div class="display-textarea">
    <codex-textarea
      v-model="displayContent"
      :dusk="'display-textarea'"
    />
  </div>
</template>

<script setup>
import { provide } from 'vue'

const displayContent = ref('This is read-only content that cannot be edited.')

provide('readonly', true)
provide('ariaLabel', 'Read-only content display')
</script>
```

### Textarea with Real-time Validation
```vue
<template>
  <div class="validated-textarea">
    <codex-textarea
      v-model="validatedContent"
      :debounce-time="300"
      :dusk="'validated-textarea'"
    />
    
    <div v-if="validationMessage" class="validation-feedback">
      {{ validationMessage }}
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed, watch } from 'vue'

const validatedContent = ref('')
const validationMessage = ref('')

const isValid = computed(() => {
  return validatedContent.value.trim().length >= 10
})

provide('placeholder', 'Enter at least 10 characters...')
provide('hasError', computed(() => !isValid.value && validatedContent.value.length > 0))

watch(validatedContent, (newValue) => {
  if (newValue.length > 0 && !isValid.value) {
    validationMessage.value = 'Content must be at least 10 characters long'
  } else {
    validationMessage.value = ''
  }
})
</script>
```

### Support Ticket Textarea
```vue
<template>
  <div class="support-form">
    <div class="form-section">
      <codex-label />
      <codex-textarea
        v-model="ticketDescription"
        :debounce-time="500"
        :dusk="'ticket-description'"
      />
      <codex-hint />
    </div>
  </div>
</template>

<script setup>
import { provide } from 'vue'

const ticketDescription = ref('')

provide('id', 'ticket-description')
provide('name', 'description')
provide('label', 'Describe Your Issue')
provide('placeholder', 'Please provide as much detail as possible about the issue you are experiencing...')
provide('ariaLabel', 'Support ticket description')
provide('required', true)
provide('hint', 'Include steps to reproduce, error messages, and expected behavior')
</script>
```

### Auto-expanding Textarea Concept
```vue
<template>
  <div class="auto-expand-textarea">
    <codex-textarea
      v-model="expandingContent"
      :dusk="'expanding-textarea'"
      :style="{ minHeight: computedHeight }"
    />
  </div>
</template>

<script setup>
import { provide, computed } from 'vue'

const expandingContent = ref('')

const computedHeight = computed(() => {
  const lineCount = expandingContent.value.split('\n').length
  const minHeight = 100
  const lineHeight = 20
  return `${Math.max(minHeight, lineCount * lineHeight)}px`
})

provide('placeholder', 'This textarea will expand as you type...')
provide('ariaLabel', 'Auto-expanding text input')
</script>
```

## CSS Classes
- `_c-input`: Base input styling
- `_c-textarea`: Textarea-specific styling
- `_c-input-{type}`: Type-specific styling based on injection
- `_c-error`: Error state styling

## Best Practices

### Recommended Usage Patterns
- Use appropriate debounce timing for the use case
- Provide meaningful placeholder text
- Implement proper validation feedback
- Use dependency injection for form integration
- Handle error states visually and semantically
- Provide clear ARIA labels for accessibility
- Consider character limits for user guidance
- Use v-model for two-way data binding

### Common Pitfalls to Avoid
- Setting debounce time too low (causing performance issues)
- Not providing placeholder text for user guidance
- Missing error state handling and feedback
- Not coordinating with parent form components
- Forgetting accessibility attributes
- Missing validation for required fields
- Not handling readonly states appropriately

### Accessibility Considerations
- Provide meaningful ARIA labels
- Handle error states with proper ARIA attributes
- Ensure keyboard navigation works correctly
- Support screen reader announcements
- Use semantic HTML textarea elements
- Provide clear placeholder text
- Handle focus management appropriately
- Support assistive technology navigation

### Error Handling
- Handle validation errors gracefully
- Provide clear error state visualization
- Clear errors when content becomes valid
- Handle edge cases in validation
- Provide meaningful error feedback
- Support multiple validation rules
- Handle async validation appropriately

### State Management
- Use reactive properties for textarea values
- Handle state changes with v-model
- Manage validation states consistently
- Coordinate with parent component state
- Handle component lifecycle properly
- Track textarea focus and blur states

### Performance Considerations
- Use appropriate debouncing for expensive operations
- Minimize re-renders during input
- Cache validation results when appropriate
- Optimize textarea event handling
- Handle large content efficiently
- Implement proper cleanup for timeouts

### Form Integration
- Use dependency injection for form context
- Coordinate with validation systems
- Handle form submission states
- Provide clear form field relationships
- Support form reset functionality
- Handle form accessibility requirements

### Content Management
- Handle multi-line content appropriately
- Support copy/paste operations
- Consider content formatting needs
- Handle large amounts of text efficiently
- Provide content length feedback when appropriate
- Support undo/redo functionality where needed

### Validation Integration
- Connect with validation libraries
- Handle real-time validation feedback
- Support async validation patterns
- Provide clear validation messaging
- Handle validation state transitions
- Support multiple validation rules

### User Experience
- Provide immediate visual feedback
- Handle long content gracefully
- Consider auto-expanding behavior
- Support rich text editing patterns
- Provide helpful placeholder text
- Handle mobile input considerations

## Component Registration
The component is registered as `codex-textarea` in the application. 