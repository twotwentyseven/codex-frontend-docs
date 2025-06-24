# Checkbox Component

## Overview
The Checkbox component provides an interactive checkbox input with visual icon integration, v-model support, and dependency injection capabilities. It features custom true/false values, error state handling, accessibility attributes, and Remix icon-based visual feedback for flexible checkbox implementation in forms and selection interfaces.

## Basic Usage
```vue
<codex-checkbox
  v-model="selectedValue"
  :id="'terms-checkbox'"
  :dusk="'terms-checkbox'"
/>
```

## Key Features
- Two-way data binding with v-model support
- Custom true/false value configuration
- Visual icon feedback with Remix icons
- Error state visualization and handling
- Dependency injection for configuration
- Disabled state support
- Accessibility attributes and ARIA support
- Custom label association through ID
- Checkbox/label coordination

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| modelValue | Boolean\|String\|Number | No | false | Checkbox checked state for two-way binding |
| dusk | String | No | '' | Testing identifier attribute |
| id | String | No | '' | Checkbox and label ID for association |
| name | String | No | '' | Checkbox name attribute |

### Injected Props
The component receives additional configuration through Vue's dependency injection:

| Injected Prop | Type | Default | Description |
|---------------|------|---------|-------------|
| trueValue | Any | true | Value when checkbox is checked |
| falseValue | Any | false | Value when checkbox is unchecked |
| disabled | Boolean | false | Whether checkbox is disabled |
| placeholder | String | '' | Placeholder text (not typically used for checkboxes) |
| ariaLabel | String | '' | ARIA label for accessibility |
| required | Boolean | false | Whether checkbox is required |
| hasError | Boolean | false | Whether checkbox has validation errors |

## Events
The component uses v-model which automatically handles:
- **Input Events**: Updates model value when checkbox state changes
- **Change Events**: Triggers when checkbox state toggles

## Visual Icons
The component uses Remix icons for visual feedback:
- **Checked State**: `ri-checkbox-fill` (filled checkbox icon)
- **Unchecked State**: `ri-checkbox-blank-line` (empty checkbox outline)
- **Dynamic Display**: Icons change based on model value vs trueValue

## True/False Value Handling
The component supports custom values for checked/unchecked states:
- **Default**: `true` for checked, `false` for unchecked
- **Custom Values**: Configurable through dependency injection
- **Type Flexibility**: Supports Boolean, String, Number types
- **Comparison Logic**: Uses strict equality to determine checked state

## Error State Handling
When the `hasError` injection is true:
- Applies `_c-error` CSS class to the label
- Provides visual feedback for validation errors
- Maintains accessibility for screen readers
- Integrates with form validation systems

## Label Integration
The component includes an integrated label element:
- **Label Association**: Uses `for` attribute linking to checkbox ID
- **Click Handling**: Label clicks toggle checkbox state
- **CSS Classes**: `_c-checkbox` for styling, `_c-error` for error states
- **Icon Container**: Label contains visual feedback icons

## Accessibility Features
- **Label Association**: Proper checkbox-label relationship via ID
- **ARIA Labels**: Supports aria-label through injection
- **Required Indication**: Handles required field marking
- **Disabled States**: Properly handles disabled checkbox interaction
- **Keyboard Navigation**: Standard checkbox keyboard support
- **Screen Reader**: Compatible with assistive technologies

## Dependency Injection Integration
The component uses Vue's provide/inject pattern:
- **Form Integration**: Receives configuration from parent form components
- **Validation Systems**: Connects with validation error states
- **Custom Values**: Supports custom true/false value definitions
- **Accessibility**: Inherits ARIA attributes from form context

## CSS Classes
- `_c-checkbox-input`: Applied to the checkbox input element
- `_c-checkbox`: Applied to the label element
- `_c-error`: Applied to label when hasError injection is true

## Internationalization
The Checkbox component does not include built-in internationalization. ARIA labels should be localized before injection.

## Examples

### Basic Checkbox
```vue
<codex-checkbox
  v-model="isChecked"
  :id="'basic-checkbox'"
  :dusk="'basic-checkbox'"
/>
```

### Terms and Conditions Checkbox
```vue
<template>
  <div class="terms-field">
    <codex-checkbox
      v-model="termsAccepted"
      :id="'terms-checkbox'"
      :dusk="'terms-checkbox'"
    />
    <span class="terms-label">
      I agree to the 
      <a href="/terms" target="_blank">Terms and Conditions</a>
    </span>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const termsAccepted = ref(false)

provide('required', true)
provide('ariaLabel', 'Accept terms and conditions')
</script>
```

### Checkbox with Custom Values
```vue
<template>
  <div class="subscription-checkbox">
    <codex-checkbox
      v-model="subscriptionStatus"
      :id="'subscription-checkbox'"
      :dusk="'subscription-checkbox'"
    />
    <label for="subscription-checkbox">
      Subscribe to newsletter
    </label>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const subscriptionStatus = ref('no')

provide('trueValue', 'yes')
provide('falseValue', 'no')
provide('ariaLabel', 'Newsletter subscription preference')
</script>
```

### Checkbox Group with Validation
```vue
<template>
  <form class="preferences-form">
    <fieldset>
      <legend>Email Preferences</legend>
      
      <div class="checkbox-group">
        <div class="checkbox-item">
          <codex-checkbox
            v-model="preferences.news"
            :id="'news-checkbox'"
            :dusk="'news-checkbox'"
          />
          <label for="news-checkbox">News Updates</label>
        </div>
        
        <div class="checkbox-item">
          <codex-checkbox
            v-model="preferences.promotions"
            :id="'promotions-checkbox'"
            :dusk="'promotions-checkbox'"
          />
          <label for="promotions-checkbox">Promotional Offers</label>
        </div>
        
        <div class="checkbox-item">
          <codex-checkbox
            v-model="preferences.updates"
            :id="'updates-checkbox'"
            :dusk="'updates-checkbox'"
          />
          <label for="updates-checkbox">Product Updates</label>
        </div>
      </div>
      
      <codex-error v-if="preferencesError" :error="preferencesError" />
    </fieldset>
  </form>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const preferences = ref({
  news: false,
  promotions: false,
  updates: false
})

const preferencesError = ref('')

const validatePreferences = () => {
  const hasSelection = Object.values(preferences.value).some(pref => pref)
  if (!hasSelection) {
    preferencesError.value = 'Please select at least one preference'
  } else {
    preferencesError.value = ''
  }
}

// Watch for changes to validate
watch(preferences, validatePreferences, { deep: true })
</script>
```

### Disabled Checkbox
```vue
<template>
  <div class="disabled-checkbox">
    <codex-checkbox
      v-model="disabledValue"
      :id="'disabled-checkbox'"
      :dusk="'disabled-checkbox'"
    />
    <label for="disabled-checkbox">
      This option is currently unavailable
    </label>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const disabledValue = ref(false)

provide('disabled', true)
provide('ariaLabel', 'Disabled option')
</script>
```

### Multi-selection Checkbox List
```vue
<template>
  <div class="multi-select-checkboxes">
    <h3>Select Skills</h3>
    
    <div class="skills-list">
      <div 
        v-for="skill in availableSkills" 
        :key="skill.id"
        class="skill-item"
      >
        <codex-checkbox
          v-model="selectedSkills"
          :id="`skill-${skill.id}`"
          :name="'skills'"
          :dusk="`skill-${skill.id}-checkbox`"
        />
        <label :for="`skill-${skill.id}`">
          {{ skill.name }}
        </label>
      </div>
    </div>
    
    <div class="selection-summary">
      Selected: {{ selectedSkills.length }} skills
    </div>
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const availableSkills = ref([
  { id: 1, name: 'JavaScript' },
  { id: 2, name: 'Vue.js' },
  { id: 3, name: 'CSS' },
  { id: 4, name: 'Node.js' }
])

const selectedSkills = ref([])

provide('trueValue', computed(() => availableSkills.value.map(skill => skill.id)))
provide('falseValue', [])
</script>
```

### Conditional Checkbox
```vue
<template>
  <div class="conditional-checkbox">
    <div class="primary-option">
      <codex-checkbox
        v-model="enableFeature"
        :id="'feature-checkbox'"
        :dusk="'feature-checkbox'"
      />
      <label for="feature-checkbox">
        Enable advanced features
      </label>
    </div>
    
    <div v-if="enableFeature" class="sub-options">
      <div class="sub-option">
        <codex-checkbox
          v-model="advancedOptions.analytics"
          :id="'analytics-checkbox'"
          :dusk="'analytics-checkbox'"
        />
        <label for="analytics-checkbox">
          Enable analytics tracking
        </label>
      </div>
      
      <div class="sub-option">
        <codex-checkbox
          v-model="advancedOptions.notifications"
          :id="'notifications-checkbox'"
          :dusk="'notifications-checkbox'"
        />
        <label for="notifications-checkbox">
          Enable push notifications
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
const enableFeature = ref(false)
const advancedOptions = ref({
  analytics: false,
  notifications: false
})

// Clear sub-options when main feature is disabled
watch(enableFeature, (enabled) => {
  if (!enabled) {
    advancedOptions.value = {
      analytics: false,
      notifications: false
    }
  }
})
</script>
```

### Checkbox with Error State
```vue
<template>
  <div class="error-checkbox">
    <codex-checkbox
      v-model="requiredConsent"
      :id="'consent-checkbox'"
      :dusk="'consent-checkbox'"
    />
    <label for="consent-checkbox">
      I consent to data processing (required)
    </label>
    
    <codex-error v-if="consentError" :error="consentError" />
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const requiredConsent = ref(false)
const consentError = ref('')

const hasConsentError = computed(() => !!consentError.value)

const validateConsent = () => {
  if (!requiredConsent.value) {
    consentError.value = 'Consent is required to proceed'
  } else {
    consentError.value = ''
  }
}

provide('required', true)
provide('hasError', hasConsentError)
provide('ariaLabel', 'Required consent checkbox')

watch(requiredConsent, validateConsent)
</script>
```

### Select All Checkbox
```vue
<template>
  <div class="select-all-checkboxes">
    <div class="select-all">
      <codex-checkbox
        v-model="selectAll"
        :id="'select-all-checkbox'"
        :dusk="'select-all-checkbox'"
        @update:model-value="handleSelectAll"
      />
      <label for="select-all-checkbox">
        Select All Items
      </label>
    </div>
    
    <div class="items-list">
      <div 
        v-for="item in items" 
        :key="item.id"
        class="item-checkbox"
      >
        <codex-checkbox
          v-model="selectedItems"
          :id="`item-${item.id}`"
          :dusk="`item-${item.id}-checkbox`"
          @update:model-value="updateSelectAll"
        />
        <label :for="`item-${item.id}`">
          {{ item.name }}
        </label>
      </div>
    </div>
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const items = ref([
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' },
  { id: 3, name: 'Item 3' },
  { id: 4, name: 'Item 4' }
])

const selectedItems = ref([])
const selectAll = ref(false)

const handleSelectAll = (checked) => {
  if (checked) {
    selectedItems.value = items.value.map(item => item.id)
  } else {
    selectedItems.value = []
  }
}

const updateSelectAll = () => {
  selectAll.value = selectedItems.value.length === items.value.length
}

provide('trueValue', computed(() => items.value.map(item => item.id)))
provide('falseValue', [])
</script>
```

### Checkbox in Table Row
```vue
<template>
  <table class="data-table">
    <thead>
      <tr>
        <th>Select</th>
        <th>Name</th>
        <th>Email</th>
        <th>Status</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user in users" :key="user.id">
        <td>
          <codex-checkbox
            v-model="selectedUsers"
            :id="`user-${user.id}`"
            :dusk="`user-${user.id}-checkbox`"
          />
        </td>
        <td>{{ user.name }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.status }}</td>
      </tr>
    </tbody>
  </table>
  
  <div class="selected-count">
    Selected: {{ selectedUsers.length }} users
  </div>
</template>

<script setup>
import { provide, ref } from 'vue'

const users = ref([
  { id: 1, name: 'John Doe', email: 'john@example.com', status: 'Active' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com', status: 'Pending' },
  { id: 3, name: 'Bob Johnson', email: 'bob@example.com', status: 'Active' }
])

const selectedUsers = ref([])

provide('trueValue', computed(() => users.value.map(user => user.id)))
provide('falseValue', [])
</script>
```

### Checkbox with Custom Styling
```vue
<template>
  <div class="custom-styled-checkbox">
    <codex-checkbox
      v-model="customValue"
      :id="'custom-checkbox'"
      :dusk="'custom-checkbox'"
      class="custom-checkbox-component"
    />
    <label for="custom-checkbox" class="custom-label">
      Custom styled checkbox
    </label>
  </div>
</template>

<script setup>
const customValue = ref(false)
</script>

<style scoped>
.custom-checkbox-component .ri-checkbox-fill {
  color: #10b981;
  font-size: 1.5rem;
}

.custom-checkbox-component .ri-checkbox-blank-line {
  color: #6b7280;
  font-size: 1.5rem;
}

.custom-label {
  font-weight: 600;
  color: #374151;
}
</style>
```

## CSS Classes
- `_c-checkbox-input`: Checkbox input element styling
- `_c-checkbox`: Label element styling
- `_c-error`: Error state styling for label

## Best Practices

### Recommended Usage Patterns
- Use clear and descriptive labels for checkboxes
- Provide unique IDs for proper label association
- Use appropriate true/false values for data needs
- Handle validation errors gracefully
- Group related checkboxes logically
- Use dependency injection for form integration
- Provide meaningful ARIA labels for accessibility
- Handle disabled states appropriately

### Common Pitfalls to Avoid
- Not providing unique IDs for checkbox-label association
- Missing labels or unclear label text
- Not handling validation errors visually
- Forgetting to coordinate with parent form components
- Using vague or unclear checkbox purposes
- Missing accessibility attributes
- Not handling disabled states properly

### Accessibility Considerations
- Always associate checkboxes with labels via ID
- Provide clear and descriptive label text
- Use ARIA labels for additional context
- Handle disabled states for keyboard navigation
- Ensure sufficient color contrast for icons
- Support screen reader announcements
- Use semantic HTML structure consistently
- Handle focus management appropriately

### Form Integration
- Use dependency injection for form context sharing
- Coordinate with validation systems
- Handle form field relationships properly
- Provide clear form structure with checkboxes
- Support form reset and clear functionality
- Handle form accessibility requirements
- Integrate with form submission handling

### Value Management
- Use appropriate true/false values for your data model
- Handle value type consistency (Boolean vs String vs Number)
- Coordinate checkbox values with backend expectations
- Handle null or undefined states appropriately
- Consider default values carefully
- Handle value changes reactively

### Group Management
- Use consistent naming for checkbox groups
- Handle select all / deselect all functionality
- Manage group validation appropriately
- Provide clear group labeling and structure
- Handle partial selections in groups
- Coordinate group state changes

### Error Handling
- Provide clear validation feedback
- Handle required checkbox validation
- Show errors near relevant checkboxes
- Clear errors when validation passes
- Handle edge cases in checkbox validation
- Support multiple validation rules

### State Management
- Use reactive properties for checkbox values
- Handle state changes with v-model
- Manage validation states consistently
- Track checkbox interaction states
- Handle component lifecycle properly
- Coordinate with parent component state

### Visual Design
- Use consistent checkbox styling across application
- Provide clear visual feedback for checked/unchecked states
- Handle error state visualization appropriately
- Consider responsive design for checkbox layouts
- Use appropriate spacing for checkbox groups
- Coordinate with design system patterns

### Performance Considerations
- Minimize re-renders for large checkbox lists
- Use computed properties for derived checkbox states
- Handle large datasets efficiently in checkbox groups
- Optimize checkbox group updates
- Implement proper cleanup for watchers

## Component Registration
The component is registered as `codex-checkbox` in the application. 