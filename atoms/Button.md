# Button Component

## Overview
The Button component provides a comprehensive interactive button interface with multiple variants, state management, internationalization support, confirmation workflows, and automatic timeout handling. It features debouncing functionality, processing states, success feedback, error handling, and extensive customization options for various user interface contexts.

## Basic Usage
```vue
<codex-button
  :default-text="'Submit Form'"
  :variant="'primary'"
  @submitted="handleSubmit"
/>
```

## Key Features
- Multiple button variants (primary, secondary, tertiary, danger, success, warning, custom)
- State-based styling and text with automatic transitions
- Debouncing functionality to prevent rapid successive clicks
- Confirmation workflow with two-click requirement
- Processing state management with visual feedback
- Success state with automatic timeout and reset
- Internationalization support for button text
- Slot support for custom content (before/after text)
- Disabled state handling with appropriate styling

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| variant | String | No | 'primary' | Button style variant |
| className | String | No | '' | Additional CSS classes to apply |
| debounce | Boolean | No | true | Enable click debouncing |
| requireConfirmation | Boolean | No | false | Require two clicks for action |

### Text Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| defaultText | String\|Boolean | No | false | Default button text |
| disabledText | String\|Boolean | No | false | Text shown when disabled |
| processingText | String\|Boolean | No | false | Text shown during processing |
| errorText | String\|Boolean | No | false | Text shown in error state |
| warningText | String\|Boolean | No | false | Text shown in warning state |
| successText | String\|Boolean | No | false | Text shown in success state |
| confirmationText | String\|Boolean | No | false | Text shown during confirmation |

### State Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| disabled | Boolean | No | false | Whether button is disabled |
| processing | Boolean | No | false | Whether button is in processing state |
| error | Boolean | No | false | Whether button is in error state |
| success | Boolean | No | false | Whether button is in success state |
| warning | Boolean | No | false | Whether button is in warning state |
| successTimeout | Number | No | 5000 | Success state timeout in milliseconds |

## Button Variants
The component supports multiple visual variants:
- **primary**: Main call-to-action styling
- **secondary**: Secondary action styling
- **tertiary**: Minimal/text button styling
- **danger**: Destructive action styling
- **success**: Success/positive action styling
- **warning**: Warning/caution action styling
- **custom**: Custom styling via additional CSS classes

## State Management
The button automatically manages its appearance based on state priority:
1. **Processing**: Takes precedence over all other states
2. **Error**: Danger styling with error text
3. **Success**: Success styling with success text
4. **Warning**: Warning styling with warning text
5. **Confirmation**: Confirmation styling when requiring confirmation
6. **Default**: Standard variant styling

## Text Display Logic
Button text is determined by current state with fallback system:
- **Custom Text**: Uses provided text props when specified
- **Internationalization**: Falls back to i18n keys for default text
- **State Priority**: Text changes based on current button state
- **Fallback Chain**: Graceful fallback to default i18n keys

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| submitted | event: Event | Emitted when button action is triggered |
| confirm | event: Event | Emitted when confirmation is required |

## Slots
| Slot Name | Description |
|-----------|-------------|
| before | Content displayed before button text |
| after | Content displayed after button text |

## Confirmation Workflow
When `requireConfirmation` is true:
1. **First Click**: Sets confirmation state and emits 'confirm' event
2. **Confirmation State**: Shows confirmation text and styling
3. **Second Click**: Executes action and emits 'submitted' event
4. **Reset**: Returns to normal state after action

## Internationalization
The component uses these translation keys with fallbacks:
```javascript
{
  "button": {
    "submit": "Submit",
    "submitting": "Submitting...",
    "success": "Success!",
    "error": "Error",
    "warning": "Warning",
    "confirm": "Confirm Action"
  }
}
```

## Examples

### Basic Submit Button
```vue
<template>
  <form @submit.prevent="handleSubmit">
    <codex-button
      :default-text="'Save Changes'"
      :processing="isSubmitting"
      :processing-text="'Saving...'"
      :success="saveSuccess"
      :success-text="'Saved!'"
      @submitted="handleSubmit"
    />
  </form>
</template>

<script setup>
const isSubmitting = ref(false)
const saveSuccess = ref(false)

const handleSubmit = async () => {
  isSubmitting.value = true
  
  try {
    await saveData()
    saveSuccess.value = true
  } catch (error) {
    console.error('Save failed:', error)
  } finally {
    isSubmitting.value = false
  }
}
</script>
```

### Confirmation Delete Button
```vue
<template>
  <div class="delete-actions">
    <codex-button
      :variant="'danger'"
      :default-text="'Delete Item'"
      :confirmation-text="'Click Again to Confirm'"
      :require-confirmation="true"
      :processing="isDeleting"
      :processing-text="'Deleting...'"
      @confirm="handleDeleteConfirm"
      @submitted="handleDelete"
    />
  </div>
</template>

<script setup>
const isDeleting = ref(false)

const handleDeleteConfirm = () => {
  console.log('User initiated delete - showing confirmation')
}

const handleDelete = async () => {
  isDeleting.value = true
  
  try {
    await deleteItem()
    // Redirect or update UI after successful deletion
  } catch (error) {
    console.error('Delete failed:', error)
  } finally {
    isDeleting.value = false
  }
}
</script>
```

### Multi-State Button with Error Handling
```vue
<template>
  <div class="form-submission">
    <codex-button
      :variant="'primary'"
      :default-text="'Upload File'"
      :processing="uploadState.isProcessing"
      :processing-text="'Uploading...'"
      :success="uploadState.isSuccess"
      :success-text="'Upload Complete!'"
      :error="uploadState.hasError"
      :error-text="'Upload Failed'"
      :disabled="!selectedFile"
      :disabled-text="'Select a file first'"
      @submitted="handleUpload"
    />
    
    <div v-if="uploadState.hasError" class="error-details">
      {{ uploadState.errorMessage }}
    </div>
  </div>
</template>

<script setup>
const selectedFile = ref(null)
const uploadState = ref({
  isProcessing: false,
  isSuccess: false,
  hasError: false,
  errorMessage: ''
})

const handleUpload = async () => {
  uploadState.value = {
    isProcessing: true,
    isSuccess: false,
    hasError: false,
    errorMessage: ''
  }
  
  try {
    await uploadFile(selectedFile.value)
    uploadState.value.isSuccess = true
  } catch (error) {
    uploadState.value.hasError = true
    uploadState.value.errorMessage = error.message
  } finally {
    uploadState.value.isProcessing = false
  }
}
</script>
```

### Button with Custom Slots
```vue
<template>
  <div class="action-buttons">
    <codex-button
      :variant="'primary'"
      :default-text="'Download Report'"
      @submitted="handleDownload"
    >
      <template #before>
        <i class="ri-download-line"></i>
      </template>
      <template #after>
        <span class="badge">{{ reportCount }}</span>
      </template>
    </codex-button>
    
    <codex-button
      :variant="'secondary'"
      :default-text="'Share'"
      @submitted="handleShare"
    >
      <template #before>
        <i class="ri-share-line"></i>
      </template>
    </codex-button>
  </div>
</template>

<script setup>
const reportCount = ref(42)

const handleDownload = () => {
  console.log('Downloading report...')
}

const handleShare = () => {
  console.log('Sharing content...')
}
</script>
```

### Button Variants Showcase
```vue
<template>
  <div class="button-showcase">
    <h3>Button Variants</h3>
    
    <div class="button-grid">
      <codex-button
        :variant="'primary'"
        :default-text="'Primary Action'"
        @submitted="handleAction('primary')"
      />
      
      <codex-button
        :variant="'secondary'"
        :default-text="'Secondary Action'"
        @submitted="handleAction('secondary')"
      />
      
      <codex-button
        :variant="'tertiary'"
        :default-text="'Tertiary Action'"
        @submitted="handleAction('tertiary')"
      />
      
      <codex-button
        :variant="'danger'"
        :default-text="'Destructive Action'"
        @submitted="handleAction('danger')"
      />
      
      <codex-button
        :variant="'success'"
        :default-text="'Success Action'"
        @submitted="handleAction('success')"
      />
      
      <codex-button
        :variant="'warning'"
        :default-text="'Warning Action'"
        @submitted="handleAction('warning')"
      />
    </div>
  </div>
</template>

<script setup>
const handleAction = (variant) => {
  console.log(`${variant} button clicked`)
}
</script>

<style scoped>
.button-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 16px;
}
</style>
```

### Debounced Form Submission
```vue
<template>
  <form @submit.prevent="handleSubmit" class="debounced-form">
    <div class="form-fields">
      <input v-model="formData.name" placeholder="Name" />
      <input v-model="formData.email" placeholder="Email" />
    </div>
    
    <codex-button
      :default-text="'Submit Form'"
      :debounce="true"
      :processing="isSubmitting"
      :processing-text="'Processing...'"
      :disabled="!isFormValid"
      @submitted="handleSubmit"
    />
    
    <p class="note">
      Button uses debouncing to prevent rapid successive submissions
    </p>
  </form>
</template>

<script setup>
const formData = ref({
  name: '',
  email: ''
})

const isSubmitting = ref(false)

const isFormValid = computed(() => 
  formData.value.name.trim() && formData.value.email.trim()
)

const handleSubmit = async () => {
  if (isSubmitting.value) return // Debounce protection
  
  isSubmitting.value = true
  
  try {
    await submitForm(formData.value)
    console.log('Form submitted successfully')
  } catch (error) {
    console.error('Submission failed:', error)
  } finally {
    isSubmitting.value = false
  }
}
</script>
```

### Warning Button with Custom Timeout
```vue
<template>
  <div class="warning-actions">
    <codex-button
      :variant="'warning'"
      :default-text="'Clear All Data'"
      :warning="showWarning"
      :warning-text="'This action cannot be undone'"
      :success="clearSuccess"
      :success-text="'Data Cleared'"
      :success-timeout="3000"
      :require-confirmation="true"
      :confirmation-text="'Click again to clear'"
      @confirm="handleClearConfirm"
      @submitted="handleClearData"
    />
  </div>
</template>

<script setup>
const showWarning = ref(false)
const clearSuccess = ref(false)

const handleClearConfirm = () => {
  showWarning.value = true
  setTimeout(() => {
    showWarning.value = false
  }, 2000)
}

const handleClearData = async () => {
  try {
    await clearAllData()
    clearSuccess.value = true
  } catch (error) {
    console.error('Clear failed:', error)
  }
}
</script>
```

### Loading State Management
```vue
<template>
  <div class="async-operations">
    <h3>Async Operations</h3>
    
    <div class="operation-buttons">
      <codex-button
        v-for="operation in operations"
        :key="operation.id"
        :default-text="operation.label"
        :processing="operation.isLoading"
        :processing-text="operation.loadingText"
        :success="operation.isSuccess"
        :success-text="operation.successText"
        :error="operation.hasError"
        :error-text="operation.errorText"
        @submitted="executeOperation(operation)"
      />
    </div>
  </div>
</template>

<script setup>
const operations = ref([
  {
    id: 'save',
    label: 'Save Data',
    loadingText: 'Saving...',
    successText: 'Saved!',
    errorText: 'Save Failed',
    isLoading: false,
    isSuccess: false,
    hasError: false,
    action: saveData
  },
  {
    id: 'sync',
    label: 'Sync Data',
    loadingText: 'Syncing...',
    successText: 'Synced!',
    errorText: 'Sync Failed',
    isLoading: false,
    isSuccess: false,
    hasError: false,
    action: syncData
  }
])

const executeOperation = async (operation) => {
  // Reset states
  operation.isLoading = true
  operation.isSuccess = false
  operation.hasError = false
  
  try {
    await operation.action()
    operation.isSuccess = true
  } catch (error) {
    operation.hasError = true
    console.error(`${operation.id} failed:`, error)
  } finally {
    operation.isLoading = false
  }
}

const saveData = async () => {
  // Simulate API call
  await new Promise(resolve => setTimeout(resolve, 2000))
}

const syncData = async () => {
  // Simulate API call
  await new Promise(resolve => setTimeout(resolve, 1500))
}
</script>
```

### Custom Styled Button
```vue
<template>
  <div class="custom-buttons">
    <codex-button
      :variant="'custom'"
      :class-name="'gradient-button'"
      :default-text="'Custom Gradient'"
      @submitted="handleCustomAction"
    />
    
    <codex-button
      :variant="'custom'"
      :class-name="'outlined-button'"
      :default-text="'Custom Outline'"
      @submitted="handleCustomAction"
    />
  </div>
</template>

<script setup>
const handleCustomAction = () => {
  console.log('Custom button clicked')
}
</script>

<style scoped>
.custom-buttons :deep(.gradient-button) {
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
  border: none;
  color: white;
  font-weight: bold;
}

.custom-buttons :deep(.outlined-button) {
  background: transparent;
  border: 2px solid #007bff;
  color: #007bff;
}

.custom-buttons :deep(.outlined-button:hover) {
  background: #007bff;
  color: white;
}
</style>
```

### Internationalized Button
```vue
<template>
  <div class="i18n-buttons">
    <h3>{{ $t('forms.actions.title') }}</h3>
    
    <codex-button
      :default-text="$t('button.save')"
      :processing-text="$t('button.saving')"
      :success-text="$t('button.saved')"
      :processing="isSaving"
      :success="saveSuccess"
      @submitted="handleSave"
    />
    
    <codex-button
      :variant="'danger'"
      :default-text="$t('button.delete')"
      :confirmation-text="$t('button.confirmDelete')"
      :require-confirmation="true"
      @submitted="handleDelete"
    />
  </div>
</template>

<script setup>
const isSaving = ref(false)
const saveSuccess = ref(false)

const handleSave = async () => {
  isSaving.value = true
  try {
    await saveData()
    saveSuccess.value = true
  } finally {
    isSaving.value = false
  }
}

const handleDelete = async () => {
  await deleteData()
}
</script>
```

## CSS Classes
- `_c-btn`: Base button styling
- `_c-primary-btn`: Primary variant styling
- `_c-secondary-btn`: Secondary variant styling
- `_c-tertiary-btn`: Tertiary variant styling
- `_c-danger-btn`: Danger/destructive variant styling
- `_c-success-btn`: Success variant styling
- `_c-warning-btn`: Warning variant styling
- `_c-processing-btn`: Processing state styling
- `_c-confirmation-btn`: Confirmation state styling
- `_c-disabled-btn`: Disabled state styling
- Custom classes via className prop

## Best Practices

### Recommended Usage Patterns
- Use appropriate variants for different action types and importance levels
- Provide meaningful text for all button states
- Implement proper loading states for async operations
- Use confirmation flow for destructive actions
- Handle error states with clear feedback
- Test button behavior across different states
- Ensure consistent button styling across the application

### Common Pitfalls to Avoid
- Not handling all button states appropriately
- Using wrong variants for action types (e.g., primary for destructive actions)
- Missing debouncing for operations that could be triggered rapidly
- Not providing feedback during long-running operations
- Forgetting to handle error states
- Not using confirmation for destructive actions
- Missing internationalization for button text

### Accessibility Considerations
- Provide clear, descriptive text for all button states
- Ensure sufficient color contrast for all variants
- Support keyboard navigation and activation
- Use semantic button elements for proper screen reader support
- Provide appropriate ARIA attributes for different states
- Handle focus management during state changes
- Test with screen readers for proper state announcements

### State Management
- Coordinate button states with application logic
- Handle state transitions smoothly
- Provide appropriate feedback for user actions
- Reset states appropriately after operations
- Handle concurrent operations properly
- Use computed properties for complex state logic

### Performance Considerations
- Use debouncing to prevent rapid successive operations
- Optimize state updates to minimize re-renders
- Handle timeout cleanup properly
- Avoid memory leaks in long-running operations
- Cache internationalization strings when appropriate
- Use efficient event handling

### User Experience
- Provide immediate visual feedback for user interactions
- Use consistent timing for state transitions
- Handle success states with appropriate timeout duration
- Provide clear error messages and recovery options
- Use progressive enhancement for advanced features
- Test button behavior across different devices and contexts

### Form Integration
- Coordinate button states with form validation
- Handle form submission properly
- Provide appropriate disabled states for invalid forms
- Support form reset functionality
- Handle form submission errors gracefully
- Test form integration across different scenarios

### Error Handling
- Provide clear error states and messages
- Handle network errors appropriately
- Implement retry mechanisms where appropriate
- Log errors for debugging and monitoring
- Provide fallback behavior for critical operations
- Test error scenarios thoroughly

### Internationalization Best Practices
- Use descriptive i18n keys for all text content
- Provide fallback text for missing translations
- Test button text in different languages
- Handle text length variations across languages
- Support right-to-left languages appropriately
- Test internationalization with real content

## Component Registration
The component is registered as `codex-button` in the application. 