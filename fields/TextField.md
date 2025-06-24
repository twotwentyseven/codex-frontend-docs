# TextField Component

## Overview
The TextField component provides a versatile text input field with comprehensive form functionality including password visibility toggle, label management, hint system, helper text, and error handling. It supports various input types, layout configurations, and accessibility features while integrating seamlessly with the form validation system through provide/inject patterns.

## Basic Usage
```vue
<template>
  <div class="text-input-form">
    <codex-text-field
      v-model="firstName"
      :name="'first_name'"
      :type="'text'"
      :label="'First Name'"
      :placeholder="'Enter your first name'"
      :required="true"
    />
  </div>
</template>

<script setup>
const firstName = ref('')
</script>
```

## Key Features
- Multiple input types support (text, email, password, etc.)
- Password visibility toggle functionality
- Flexible layout configurations (auto, quarter, third, half, full)
- Label and hint system with tooltip support
- Helper text and error handling
- Accessibility features with ARIA labels
- Testing support with Dusk attributes
- Readonly state management

## Configuration Props

### Required Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `name` | `String` | `required` | Field name for form submission |
| `type` | `String` | `required` | Input type (text, email, password, etc.) |

### Content Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `label` | `String` | `''` | Field label text |
| `placeholder` | `String` | `''` | Input placeholder text |
| `hint` | `String` | `''` | Hint text for additional guidance |
| `helperText` | `String` | `''` | Helper text below the input |
| `ariaLabel` | `String` | `''` | ARIA label for accessibility |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'1'` | Layout size (auto, 4, 3, 2, 1) |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `required` | `Boolean` | `true` | Whether field is required |
| `readonly` | `Boolean` | `false` | Whether field is readonly |
| `hasError` | `Boolean` | `false` | Whether field has error state |

### Error Handling Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `errors` | `Array` | `[]` | Array of error messages |
| `error` | `Boolean\|String` | `false` | Single error state or message |

### Testing Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `dusk` | `String` | `''` | Browser testing identifier |
| `id` | `String` | `''` | HTML element ID |

### Tooltip Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `tooltipText` | `String` | `''` | Tooltip text content |
| `tooltipIcon` | `String` | `''` | Tooltip icon class |
| `link` | `String` | `undefined` | Link URL for hint |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `close` | `none` | Emitted when hint is closed |
| `customEvent` | `event` | Custom event from hint component |

## Model Value

The component uses `defineModel()` to create a two-way binding with the parent component:

```vue
<template>
  <codex-text-field v-model="userInput" />
</template>

<script setup>
const userInput = ref('')
// userInput automatically updates when field changes
</script>
```

## Layout Configurations

### Layout Sizes
- `auto`: Automatic sizing based on content
- `4`: Quarter width (25%)
- `3`: Third width (33.33%)
- `2`: Half width (50%) 
- `1`: Full width (100%) - default

### Layout Examples
```vue
<template>
  <div class="form-grid">
    <!-- Quarter width fields -->
    <codex-text-field
      v-model="title"
      :name="'title'"
      :type="'text'"
      :layout="'4'"
      :label="'Title'"
    />
    
    <!-- Half width fields -->
    <codex-text-field
      v-model="firstName"
      :name="'first_name'"
      :type="'text'"
      :layout="'2'"
      :label="'First Name'"
    />
    
    <codex-text-field
      v-model="lastName"
      :name="'last_name'"
      :type="'text'"
      :layout="'2'"
      :label="'Last Name'"
    />
    
    <!-- Full width field -->
    <codex-text-field
      v-model="address"
      :name="'address'"
      :type="'text'"
      :layout="'1'"
      :label="'Street Address'"
    />
  </div>
</template>
```

## Examples

### Basic Contact Form
```vue
<template>
  <form class="contact-form" @submit.prevent="submitForm">
    <div class="form-row">
      <codex-text-field
        v-model="form.firstName"
        :name="'first_name'"
        :type="'text'"
        :label="'First Name'"
        :placeholder="'Enter your first name'"
        :layout="'2'"
        :required="true"
        :errors="fieldErrors.firstName"
      />
      
      <codex-text-field
        v-model="form.lastName"
        :name="'last_name'"
        :type="'text'"
        :label="'Last Name'"
        :placeholder="'Enter your last name'"
        :layout="'2'"
        :required="true"
        :errors="fieldErrors.lastName"
      />
    </div>
    
    <codex-text-field
      v-model="form.email"
      :name="'email'"
      :type="'email'"
      :label="'Email Address'"
      :placeholder="'your.email@example.com'"
      :required="true"
      :errors="fieldErrors.email"
      :hint="'We\'ll never share your email'"
    />
    
    <codex-text-field
      v-model="form.phone"
      :name="'phone'"
      :type="'tel'"
      :label="'Phone Number'"
      :placeholder="'+1 (555) 123-4567'"
      :required="false"
      :helper-text="'Optional - for urgent contact only'"
    />
    
    <button type="submit" class="submit-btn">Send Message</button>
  </form>
</template>

<script setup>
const form = reactive({
  firstName: '',
  lastName: '',
  email: '',
  phone: ''
})

const fieldErrors = ref({
  firstName: [],
  lastName: [],
  email: []
})

const submitForm = async () => {
  try {
    await submitContactForm(form)
    console.log('Form submitted successfully')
  } catch (error) {
    if (error.response?.data?.errors) {
      fieldErrors.value = error.response.data.errors
    }
  }
}
</script>
```

### Password Field with Validation
```vue
<template>
  <div class="password-form">
    <codex-text-field
      v-model="password"
      :name="'password'"
      :type="'password'"
      :label="'Password'"
      :placeholder="'Create a strong password'"
      :required="true"
      :errors="passwordErrors"
      :helper-text="passwordHelperText"
      :tooltip-text="'Password must be at least 8 characters'"
      :tooltip-icon="'ri-information-line'"
    />
    
    <codex-text-field
      v-model="confirmPassword"
      :name="'confirm_password'"
      :type="'password'"
      :label="'Confirm Password'"
      :placeholder="'Re-enter your password'"
      :required="true"
      :errors="confirmPasswordErrors"
      :has-error="password !== confirmPassword && confirmPassword.length > 0"
    />
    
    <div class="password-strength">
      <div class="strength-meter">
        <div class="strength-bar" :class="passwordStrengthClass"></div>
      </div>
      <span class="strength-text">{{ passwordStrengthText }}</span>
    </div>
  </div>
</template>

<script setup>
const password = ref('')
const confirmPassword = ref('')
const passwordErrors = ref([])
const confirmPasswordErrors = ref([])

const passwordHelperText = computed(() => {
  if (password.value.length === 0) return 'Enter your password'
  if (password.value.length < 8) return 'Password must be at least 8 characters'
  return 'Good password strength'
})

const passwordStrengthClass = computed(() => {
  if (password.value.length < 6) return 'weak'
  if (password.value.length < 10) return 'medium'
  return 'strong'
})

const passwordStrengthText = computed(() => {
  if (password.value.length < 6) return 'Weak'
  if (password.value.length < 10) return 'Medium'
  return 'Strong'
})

watch(confirmPassword, (newValue) => {
  if (newValue && newValue !== password.value) {
    confirmPasswordErrors.value = ['Passwords do not match']
  } else {
    confirmPasswordErrors.value = []
  }
})
</script>
```

### Search Field with Dynamic Hints
```vue
<template>
  <div class="search-container">
    <codex-text-field
      v-model="searchQuery"
      :name="'search'"
      :type="'search'"
      :label="'Search Products'"
      :placeholder="'Type to search...'"
      :required="false"
      :hint="searchHint"
      :helper-text="searchResultsText"
      @customEvent="handleSearchEvent"
    />
    
    <div v-if="searchResults.length > 0" class="search-results">
      <div v-for="result in searchResults" :key="result.id" class="search-result">
        {{ result.name }}
      </div>
    </div>
  </div>
</template>

<script setup>
const searchQuery = ref('')
const searchResults = ref([])

const searchHint = computed(() => {
  if (searchQuery.value.length === 0) return 'Start typing to search'
  if (searchQuery.value.length < 3) return 'Type at least 3 characters'
  return 'Press Enter to search'
})

const searchResultsText = computed(() => {
  if (searchResults.value.length === 0) return ''
  return `Found ${searchResults.value.length} results`
})

const handleSearchEvent = (event) => {
  console.log('Search event:', event)
}

// Debounced search
watch(searchQuery, debounce(async (newQuery) => {
  if (newQuery.length >= 3) {
    searchResults.value = await performSearch(newQuery)
  } else {
    searchResults.value = []
  }
}, 300))
</script>
```

### Readonly Field Display
```vue
<template>
  <div class="profile-display">
    <h3>Profile Information</h3>
    
    <codex-text-field
      v-model="user.username"
      :name="'username'"
      :type="'text'"
      :label="'Username'"
      :readonly="true"
      :layout="'2'"
      :helper-text="'Username cannot be changed'"
    />
    
    <codex-text-field
      v-model="user.email"
      :name="'email'"
      :type="'email'"
      :label="'Email Address'"
      :readonly="!editMode"
      :layout="'2'"
      :errors="emailErrors"
    />
    
    <codex-text-field
      v-model="user.displayName"
      :name="'display_name'"
      :type="'text'"
      :label="'Display Name'"
      :readonly="!editMode"
      :placeholder="'How others see your name'"
    />
    
    <div class="form-actions">
      <button v-if="!editMode" @click="editMode = true" class="edit-btn">
        Edit Profile
      </button>
      <template v-else>
        <button @click="saveProfile" class="save-btn">Save Changes</button>
        <button @click="cancelEdit" class="cancel-btn">Cancel</button>
      </template>
    </div>
  </div>
</template>

<script setup>
const editMode = ref(false)
const emailErrors = ref([])

const user = reactive({
  username: 'johndoe',
  email: 'john@example.com',
  displayName: 'John Doe'
})

const saveProfile = async () => {
  try {
    await updateUserProfile(user)
    editMode.value = false
    emailErrors.value = []
  } catch (error) {
    if (error.response?.data?.errors?.email) {
      emailErrors.value = error.response.data.errors.email
    }
  }
}

const cancelEdit = () => {
  editMode.value = false
  emailErrors.value = []
  // Reset form to original values
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-input-container` | Main container for the field |
| `_c-form-field--auto` | Auto-sized layout |
| `_c-form-field--quarter` | Quarter width layout (25%) |
| `_c-form-field--third` | Third width layout (33%) |
| `_c-form-field--half` | Half width layout (50%) |
| `_c-form-field--full` | Full width layout (100%) |
| `_c-show-password` | Password visibility toggle button |
| `_c-label-container` | Container for label and hint |

## Best Practices

### Input Types
- Use appropriate input types for better mobile keyboards and validation
- Common types: `text`, `email`, `password`, `tel`, `url`, `search`
- Always specify the `type` prop for semantic HTML

### Layout Design
- Use consistent layout patterns across forms
- Group related fields with similar layouts
- Consider mobile responsiveness when choosing layouts

### Accessibility
- Always provide meaningful labels
- Use `ariaLabel` for additional context when needed
- Ensure proper focus management for form navigation
- Test with screen readers and keyboard navigation

### Error Handling
- Display errors clearly and specifically
- Provide helpful error messages that guide users
- Use `hasError` prop for custom error styling
- Clear errors when user starts correcting input

### Password Security
- Use password type for sensitive fields
- Implement password strength indicators
- Provide clear password requirements
- Consider password visibility toggle for better UX

### Performance
- Use `readonly` instead of `disabled` when showing non-editable data
- Implement debouncing for search/autocomplete fields
- Avoid unnecessary re-renders with proper reactive references

### Validation
- Validate on blur for better UX
- Provide real-time feedback for critical fields
- Use helper text to guide users proactively
- Handle both client and server-side validation

## Component Registration
```javascript
// Global registration
app.component('CodexTextField', TextField)

// Local registration  
import TextField from '@/components/fields/TextField.vue'

export default {
  components: {
    CodexTextField: TextField
  }
}
``` 