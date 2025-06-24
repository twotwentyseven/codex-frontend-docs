# PopupTrigger Component

## Overview
The PopupTrigger component provides an intelligent modal and popup management system with support for hover interactions, click triggers, and keyboard navigation. It integrates with the global modal system to handle z-index stacking, URL state management, and provides extensive customization options for trigger behaviors, positioning, and content display with built-in accessibility features.

## Basic Usage
```vue
<template>
  <div class="popup-trigger-example">
    <codex-popup-trigger 
      :modal-name="'example-popup'"
      :trigger-on="'hover'"
      :delay="300"
    >
      <template #trigger>
        <button class="trigger-button">
          Hover me for popup
        </button>
      </template>
      
      <template #content>
        <div class="popup-content">
          <h3>Popup Content</h3>
          <p>This content appears on hover.</p>
        </div>
      </template>
    </codex-popup-trigger>
  </div>
</template>
```

## Key Features
- Multiple trigger types (hover, click, focus, manual)
- Configurable delay and duration settings
- Z-index stacking management
- URL state synchronization
- Keyboard navigation support (Escape to close)
- Position-aware content display
- Event-based control system
- Auto-close functionality
- Accessibility compliance

## Configuration Props

### Trigger Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modalName` | `String` | `required` | Unique identifier for the popup |
| `triggerOn` | `String` | `'hover'` | Trigger type (hover, click, focus, manual) |
| `delay` | `Number` | `0` | Delay before showing popup (ms) |
| `duration` | `Number` | `0` | Auto-close duration (ms, 0 = no auto-close) |

### Behavior Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `closeOnClickOutside` | `Boolean` | `true` | Close popup when clicking outside |
| `closeOnEscape` | `Boolean` | `true` | Close popup on Escape key |
| `preventDefaultClick` | `Boolean` | `false` | Prevent default click behavior |

### State Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `open` | `Boolean` | `false` | Manual control of popup state |
| `disabled` | `Boolean` | `false` | Disable popup trigger |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `popup-open` | `modalName` | Emitted when popup opens |
| `popup-close` | `modalName` | Emitted when popup closes |
| `trigger-click` | `event` | Emitted on trigger click |
| `trigger-hover` | `event` | Emitted on trigger hover |

## Slots

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `trigger` | `{ open, close, toggle, isOpen }` | Trigger element content |
| `content` | `{ open, close, toggle, isOpen }` | Popup content |
| `default` | `{ open, close, toggle, isOpen }` | Fallback content (used as trigger if no trigger slot) |

## Trigger Types

### Hover Trigger
```vue
<codex-popup-trigger 
  modal-name="hover-popup"
  trigger-on="hover"
  :delay="200"
>
  <!-- Content appears on hover after 200ms delay -->
</codex-popup-trigger>
```

### Click Trigger
```vue
<codex-popup-trigger 
  modal-name="click-popup"
  trigger-on="click"
>
  <!-- Content appears on click -->
</codex-popup-trigger>
```

### Focus Trigger
```vue
<codex-popup-trigger 
  modal-name="focus-popup"
  trigger-on="focus"
>
  <!-- Content appears when trigger receives focus -->
</codex-popup-trigger>
```

### Manual Trigger
```vue
<codex-popup-trigger 
  modal-name="manual-popup"
  trigger-on="manual"
  :open="manualOpen"
>
  <!-- Content controlled manually via prop -->
</codex-popup-trigger>
```

## Control API

The popup can be controlled through custom events:

```javascript
// Open popup
document.dispatchEvent(new CustomEvent('codex.modal.open.popup-name'))

// Close popup
document.dispatchEvent(new CustomEvent('codex.modal.close.popup-name'))

// Toggle popup
document.dispatchEvent(new CustomEvent('codex.modal.toggle.popup-name'))
```

## Examples

### Interactive Tooltip Popup
```vue
<template>
  <div class="tooltip-demo">
    <h2>Interactive Tooltip Demo</h2>
    
    <div class="content-grid">
      <codex-popup-trigger 
        modal-name="info-tooltip"
        trigger-on="hover"
        :delay="500"
        :duration="3000"
      >
        <template #trigger="{ isOpen }">
          <div class="info-item" :class="{ active: isOpen }">
            <i class="ri-information-line"></i>
            <span>Hover for info</span>
          </div>
        </template>
        
        <template #content>
          <div class="tooltip-content">
            <h4>Information Tooltip</h4>
            <p>This tooltip provides additional context about the item.</p>
            <div class="tooltip-stats">
              <div class="stat">
                <strong>Views:</strong> 1,234
              </div>
              <div class="stat">
                <strong>Likes:</strong> 567
              </div>
            </div>
          </div>
        </template>
      </codex-popup-trigger>
      
      <codex-popup-trigger 
        modal-name="warning-tooltip"
        trigger-on="hover"
        :delay="200"
      >
        <template #trigger="{ isOpen }">
          <div class="warning-item" :class="{ active: isOpen }">
            <i class="ri-warning-line"></i>
            <span>Warning item</span>
          </div>
        </template>
        
        <template #content>
          <div class="tooltip-content warning">
            <h4>⚠️ Warning</h4>
            <p>This action requires admin privileges.</p>
            <button class="dismiss-btn">Understood</button>
          </div>
        </template>
      </codex-popup-trigger>
      
      <codex-popup-trigger 
        modal-name="success-tooltip"
        trigger-on="click"
      >
        <template #trigger="{ isOpen, toggle }">
          <button 
            @click="toggle"
            class="success-button" 
            :class="{ active: isOpen }"
          >
            <i class="ri-check-line"></i>
            Click for details
          </button>
        </template>
        
        <template #content="{ close }">
          <div class="tooltip-content success">
            <h4>✓ Success</h4>
            <p>Operation completed successfully!</p>
            <div class="action-buttons">
              <button @click="viewDetails" class="btn-primary">
                View Details
              </button>
              <button @click="close" class="btn-secondary">
                Close
              </button>
            </div>
          </div>
        </template>
      </codex-popup-trigger>
    </div>
  </div>
</template>

<script setup>
const viewDetails = () => {
  console.log('Viewing details...')
}
</script>

<style scoped>
.content-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 2rem;
  margin: 2rem 0;
}

.info-item,
.warning-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem;
  border: 2px solid #dee2e6;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.info-item:hover,
.info-item.active {
  border-color: #007bff;
  background: #f8f9ff;
}

.warning-item:hover,
.warning-item.active {
  border-color: #ffc107;
  background: #fffbf0;
}

.success-button {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 1rem;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.success-button:hover,
.success-button.active {
  background: #1e7e34;
}

.tooltip-content {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  max-width: 300px;
  min-width: 250px;
}

.tooltip-content.warning {
  border-left: 4px solid #ffc107;
}

.tooltip-content.success {
  border-left: 4px solid #28a745;
}

.tooltip-stats {
  display: flex;
  justify-content: space-between;
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #dee2e6;
}

.action-buttons {
  display: flex;
  gap: 0.5rem;
  margin-top: 1rem;
}

.btn-primary,
.btn-secondary,
.dismiss-btn {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
}

.btn-primary {
  background: #007bff;
  color: white;
}

.btn-secondary {
  background: #6c757d;
  color: white;
}

.dismiss-btn {
  background: #ffc107;
  color: #212529;
  width: 100%;
}
</style>
```

### Context Menu Popup
```vue
<template>
  <div class="context-menu-demo">
    <h2>Context Menu Demo</h2>
    
    <div class="menu-items">
      <codex-popup-trigger 
        v-for="item in menuItems"
        :key="item.id"
        :modal-name="`menu-${item.id}`"
        trigger-on="click"
        :close-on-click-outside="true"
      >
        <template #trigger="{ isOpen }">
          <div class="menu-item" :class="{ active: isOpen }">
            <div class="item-content">
              <i :class="item.icon"></i>
              <div class="item-info">
                <h4>{{ item.title }}</h4>
                <p>{{ item.description }}</p>
              </div>
            </div>
            <i class="ri-more-2-line menu-trigger"></i>
          </div>
        </template>
        
        <template #content="{ close }">
          <div class="context-menu">
            <div class="menu-header">
              <h4>{{ item.title }}</h4>
            </div>
            
            <div class="menu-actions">
              <button 
                v-for="action in item.actions"
                :key="action.id"
                @click="handleAction(action, item, close)"
                class="menu-action"
                :class="action.variant"
              >
                <i :class="action.icon"></i>
                <span>{{ action.label }}</span>
              </button>
            </div>
          </div>
        </template>
      </codex-popup-trigger>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const menuItems = ref([
  {
    id: 1,
    title: 'Project Alpha',
    description: 'React application with TypeScript',
    icon: 'ri-folder-line',
    actions: [
      { id: 'edit', label: 'Edit', icon: 'ri-edit-line', variant: 'primary' },
      { id: 'duplicate', label: 'Duplicate', icon: 'ri-file-copy-line', variant: 'secondary' },
      { id: 'share', label: 'Share', icon: 'ri-share-line', variant: 'secondary' },
      { id: 'delete', label: 'Delete', icon: 'ri-delete-bin-line', variant: 'danger' }
    ]
  },
  {
    id: 2,
    title: 'Marketing Site',
    description: 'Vue.js website with Nuxt',
    icon: 'ri-global-line',
    actions: [
      { id: 'view', label: 'View Live', icon: 'ri-external-link-line', variant: 'primary' },
      { id: 'edit', label: 'Edit', icon: 'ri-edit-line', variant: 'secondary' },
      { id: 'deploy', label: 'Deploy', icon: 'ri-rocket-line', variant: 'success' },
      { id: 'archive', label: 'Archive', icon: 'ri-archive-line', variant: 'warning' }
    ]
  },
  {
    id: 3,
    title: 'API Service',
    description: 'Node.js backend with Express',
    icon: 'ri-server-line',
    actions: [
      { id: 'logs', label: 'View Logs', icon: 'ri-file-text-line', variant: 'primary' },
      { id: 'restart', label: 'Restart', icon: 'ri-restart-line', variant: 'warning' },
      { id: 'scale', label: 'Scale', icon: 'ri-arrow-up-line', variant: 'secondary' },
      { id: 'stop', label: 'Stop', icon: 'ri-stop-line', variant: 'danger' }
    ]
  }
])

const handleAction = (action, item, closeMenu) => {
  console.log(`Action: ${action.label} on ${item.title}`)
  
  // Simulate action processing
  switch (action.id) {
    case 'edit':
      // Navigate to edit page
      break
    case 'delete':
    case 'stop':
      if (confirm(`Are you sure you want to ${action.label.toLowerCase()} ${item.title}?`)) {
        // Perform destructive action
      }
      break
    default:
      // Handle other actions
      break
  }
  
  closeMenu()
}
</script>

<style scoped>
.menu-items {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 600px;
}

.menu-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  background: white;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
}

.menu-item:hover,
.menu-item.active {
  background: #f8f9fa;
  border-color: #007bff;
}

.item-content {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.item-content i {
  font-size: 1.5rem;
  color: #6c757d;
}

.item-info h4 {
  margin: 0 0 0.25rem 0;
  color: #212529;
}

.item-info p {
  margin: 0;
  color: #6c757d;
  font-size: 0.875rem;
}

.menu-trigger {
  color: #6c757d;
  padding: 0.5rem;
}

.context-menu {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  overflow: hidden;
  min-width: 200px;
}

.menu-header {
  padding: 1rem;
  background: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
}

.menu-header h4 {
  margin: 0;
  color: #212529;
}

.menu-actions {
  padding: 0.5rem 0;
}

.menu-action {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  width: 100%;
  padding: 0.75rem 1rem;
  background: none;
  border: none;
  text-align: left;
  cursor: pointer;
  transition: background-color 0.2s;
}

.menu-action:hover {
  background: #f8f9fa;
}

.menu-action.primary { color: #007bff; }
.menu-action.secondary { color: #6c757d; }
.menu-action.success { color: #28a745; }
.menu-action.warning { color: #ffc107; }
.menu-action.danger { color: #dc3545; }

.menu-action.danger:hover {
  background: #f8d7da;
}
</style>
```

### Form Field Popup Help
```vue
<template>
  <div class="form-help-demo">
    <h2>Form with Popup Help</h2>
    
    <form @submit.prevent="submitForm" class="help-form">
      <div class="form-group">
        <label for="username">
          Username
          <codex-popup-trigger 
            modal-name="username-help"
            trigger-on="hover"
            :delay="300"
          >
            <template #trigger>
              <i class="ri-question-line help-icon"></i>
            </template>
            
            <template #content>
              <div class="help-content">
                <h4>Username Requirements</h4>
                <ul>
                  <li>3-20 characters long</li>
                  <li>Letters, numbers, and underscores only</li>
                  <li>Cannot start with a number</li>
                  <li>Must be unique</li>
                </ul>
              </div>
            </template>
          </codex-popup-trigger>
        </label>
        <input 
          v-model="form.username"
          type="text" 
          id="username"
          :class="{ error: errors.username }"
          @blur="validateUsername"
        >
        <span v-if="errors.username" class="error-message">{{ errors.username }}</span>
      </div>
      
      <div class="form-group">
        <label for="email">
          Email
          <codex-popup-trigger 
            modal-name="email-help"
            trigger-on="focus"
          >
            <template #trigger>
              <i class="ri-mail-line help-icon"></i>
            </template>
            
            <template #content>
              <div class="help-content">
                <h4>Email Guidelines</h4>
                <p>We'll use this email for:</p>
                <ul>
                  <li>Account verification</li>
                  <li>Password reset</li>
                  <li>Important notifications</li>
                </ul>
                <p><strong>We never share your email with third parties.</strong></p>
              </div>
            </template>
          </codex-popup-trigger>
        </label>
        <input 
          v-model="form.email"
          type="email" 
          id="email"
          :class="{ error: errors.email }"
          @blur="validateEmail"
        >
        <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
      </div>
      
      <div class="form-group">
        <label for="password">
          Password
          <codex-popup-trigger 
            modal-name="password-help"
            trigger-on="click"
            :close-on-click-outside="true"
          >
            <template #trigger="{ isOpen }">
              <i class="ri-lock-line help-icon" :class="{ active: isOpen }"></i>
            </template>
            
            <template #content="{ close }">
              <div class="help-content">
                <h4>Password Security</h4>
                <div class="security-tips">
                  <div class="tip-section">
                    <h5>Requirements:</h5>
                    <ul>
                      <li :class="{ valid: passwordChecks.length }">At least 8 characters</li>
                      <li :class="{ valid: passwordChecks.uppercase }">One uppercase letter</li>
                      <li :class="{ valid: passwordChecks.lowercase }">One lowercase letter</li>
                      <li :class="{ valid: passwordChecks.number }">One number</li>
                      <li :class="{ valid: passwordChecks.special }">One special character</li>
                    </ul>
                  </div>
                  
                  <div class="tip-section">
                    <h5>Tips:</h5>
                    <ul>
                      <li>Use a unique password</li>
                      <li>Consider using a password manager</li>
                      <li>Avoid personal information</li>
                    </ul>
                  </div>
                </div>
                
                <div class="help-actions">
                  <button @click="generatePassword" class="btn-generate">
                    Generate Strong Password
                  </button>
                  <button @click="close" class="btn-close">
                    Close
                  </button>
                </div>
              </div>
            </template>
          </codex-popup-trigger>
        </label>
        <input 
          v-model="form.password"
          type="password" 
          id="password"
          :class="{ error: errors.password }"
          @input="checkPassword"
          @blur="validatePassword"
        >
        <span v-if="errors.password" class="error-message">{{ errors.password }}</span>
      </div>
      
      <button type="submit" :disabled="!isFormValid" class="submit-btn">
        Create Account
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

const form = reactive({
  username: '',
  email: '',
  password: ''
})

const errors = reactive({})

const passwordChecks = computed(() => ({
  length: form.password.length >= 8,
  uppercase: /[A-Z]/.test(form.password),
  lowercase: /[a-z]/.test(form.password),
  number: /\d/.test(form.password),
  special: /[!@#$%^&*(),.?":{}|<>]/.test(form.password)
}))

const isFormValid = computed(() => {
  return form.username && 
         form.email && 
         form.password && 
         Object.keys(errors).length === 0
})

const validateUsername = () => {
  if (!form.username) {
    errors.username = 'Username is required'
  } else if (form.username.length < 3) {
    errors.username = 'Username must be at least 3 characters'
  } else if (!/^[a-zA-Z_][a-zA-Z0-9_]*$/.test(form.username)) {
    errors.username = 'Invalid username format'
  } else {
    delete errors.username
  }
}

const validateEmail = () => {
  if (!form.email) {
    errors.email = 'Email is required'
  } else if (!/\S+@\S+\.\S+/.test(form.email)) {
    errors.email = 'Please enter a valid email'
  } else {
    delete errors.email
  }
}

const validatePassword = () => {
  const checks = passwordChecks.value
  if (!form.password) {
    errors.password = 'Password is required'
  } else if (!Object.values(checks).every(Boolean)) {
    errors.password = 'Password does not meet requirements'
  } else {
    delete errors.password
  }
}

const checkPassword = () => {
  // Real-time validation feedback
  if (form.password && errors.password) {
    validatePassword()
  }
}

const generatePassword = () => {
  const chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*'
  let password = ''
  
  // Ensure at least one of each required type
  password += 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'[Math.floor(Math.random() * 26)]
  password += 'abcdefghijklmnopqrstuvwxyz'[Math.floor(Math.random() * 26)]
  password += '0123456789'[Math.floor(Math.random() * 10)]
  password += '!@#$%^&*'[Math.floor(Math.random() * 8)]
  
  // Fill the rest randomly
  for (let i = 4; i < 12; i++) {
    password += chars[Math.floor(Math.random() * chars.length)]
  }
  
  // Shuffle the password
  form.password = password.split('').sort(() => Math.random() - 0.5).join('')
  validatePassword()
}

const submitForm = () => {
  validateUsername()
  validateEmail()
  validatePassword()
  
  if (isFormValid.value) {
    console.log('Form submitted:', form)
  }
}
</script>

<style scoped>
.help-form {
  max-width: 500px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.5rem;
  font-weight: 600;
  color: #333;
}

.help-icon {
  color: #007bff;
  cursor: pointer;
  padding: 0.25rem;
  border-radius: 50%;
  transition: all 0.2s;
}

.help-icon:hover,
.help-icon.active {
  background: #e3f2fd;
  color: #0056b3;
}

.form-group input {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.2s;
}

.form-group input:focus {
  outline: none;
  border-color: #007bff;
}

.form-group input.error {
  border-color: #dc3545;
}

.error-message {
  color: #dc3545;
  font-size: 0.875rem;
  margin-top: 0.25rem;
  display: block;
}

.help-content {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  max-width: 350px;
}

.help-content h4 {
  margin: 0 0 1rem 0;
  color: #212529;
}

.help-content ul {
  margin: 0.5rem 0;
  padding-left: 1.5rem;
}

.help-content li {
  margin-bottom: 0.25rem;
}

.security-tips {
  margin: 1rem 0;
}

.tip-section {
  margin-bottom: 1rem;
}

.tip-section h5 {
  margin: 0 0 0.5rem 0;
  color: #495057;
}

.tip-section li.valid {
  color: #28a745;
}

.help-actions {
  display: flex;
  gap: 0.5rem;
  margin-top: 1rem;
}

.btn-generate,
.btn-close {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
}

.btn-generate {
  background: #007bff;
  color: white;
}

.btn-close {
  background: #6c757d;
  color: white;
}

.submit-btn {
  width: 100%;
  padding: 1rem;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: 600;
  transition: background-color 0.2s;
}

.submit-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.submit-btn:not(:disabled):hover {
  background: #218838;
}
</style>
```

## Event Handling

### Custom Event Listeners
```javascript
// Listen for popup events
document.addEventListener('popup-open', (event) => {
  console.log('Popup opened:', event.detail.modalName)
})

document.addEventListener('popup-close', (event) => {
  console.log('Popup closed:', event.detail.modalName)
})
```

### Keyboard Navigation
The component automatically handles:
- **Escape key**: Closes the popup
- **Tab navigation**: Maintains focus within popup content
- **Enter/Space**: Activates trigger when focused

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-popup-trigger` | Main trigger container |
| `_c-trigger-element` | Trigger element wrapper |
| `_c-popup-content` | Popup content container |
| `_c-popup-open` | Applied when popup is open |
| `_c-popup-disabled` | Applied when trigger is disabled |

## Accessibility Features

### ARIA Support
- `aria-expanded` on trigger element
- `aria-describedby` linking trigger to content
- `role="tooltip"` for tooltip-style popups
- `role="dialog"` for modal-style popups

### Keyboard Support
- Full keyboard navigation
- Escape key handling
- Focus management
- Screen reader compatibility

## Best Practices

### Trigger Design
- Use clear, descriptive trigger elements
- Provide visual feedback for interactive states
- Consider touch device interactions
- Test with various input methods
- Ensure adequate touch target sizes

### Content Design
- Keep popup content concise and focused
- Use appropriate contrast ratios
- Provide clear action buttons
- Consider mobile viewport constraints
- Test with screen readers

### Performance
- Use appropriate delays to avoid accidental triggers
- Implement proper cleanup for event listeners
- Avoid creating too many simultaneous popups
- Consider lazy loading for heavy content
- Monitor popup creation/destruction cycles

### User Experience
- Provide clear ways to close popups
- Use consistent trigger behaviors across the application
- Consider auto-close for informational content
- Handle edge cases like rapid mouse movement
- Test with various user interaction patterns

### Accessibility
- Ensure keyboard navigation works properly
- Provide proper ARIA labels and descriptions
- Test with screen readers
- Consider users with motor disabilities
- Implement proper focus management

## Component Registration
```javascript
// Global registration
app.component('CodexPopupTrigger', PopupTrigger)

// Local registration  
import PopupTrigger from '@/components/molecules/PopupTrigger.vue'

export default {
  components: {
    CodexPopupTrigger: PopupTrigger
  }
}
```

## Internationalization

The PopupTrigger component has minimal translation requirements as it primarily provides modal trigger functionality and behavior management.

### Translation Notes

#### Minimal Translation Requirements
The PopupTrigger component itself does not use direct translation keys as it:
- Acts as a wrapper for button and modal functionality
- Delegates content rendering to slot content
- Handles positioning and interaction behavior

#### Button Text Translation
Button text is handled through the underlying `codex-button` component props:

```vue
<codex-popup-trigger 
  modal-name="example-popup"
  :default-text="$t('button.open_popup')"
  :processing-text="$t('button.loading')"
  :error-text="$t('button.error')"
  :success-text="$t('button.success')"
>
  <template #content>
    <div>{{ $t('popup.content_message') }}</div>
  </template>
</codex-popup-trigger>
```

#### Accessibility Translation
The component supports accessibility through translated aria labels:

```vue
<codex-popup-trigger 
  modal-name="info-popup"
  :button-aria-label="$t('accessibility.open_info_popup')"
  :default-text="$t('button.more_info')"
>
  <template #content>
    <h3>{{ $t('popup.info_title') }}</h3>
    <p>{{ $t('popup.info_description') }}</p>
  </template>
</codex-popup-trigger>
```

#### Content Translation
Translation is handled by the content components placed within the popup slots:

```vue
<codex-popup-trigger modal-name="user-menu">
  <template #trigger>
    <button>{{ $t('user.menu_trigger') }}</button>
  </template>
  
  <template #content="{ close }">
    <div class="user-menu">
      <h4>{{ $t('user.menu_title') }}</h4>
      <ul>
        <li><a href="/profile">{{ $t('user.profile') }}</a></li>
        <li><a href="/settings">{{ $t('user.settings') }}</a></li>
        <li><button @click="logout">{{ $t('user.logout') }}</button></li>
      </ul>
    </div>
  </template>
</codex-popup-trigger>
```

#### Modal Integration
The component integrates with the `codex-modal` component which handles its own minimal translations for:
- Close button accessibility labels
- Modal overlay interactions
- Keyboard navigation hints

#### Dynamic Content Translation
For dynamic popup content, translation can be handled in the slot content:

```vue
<codex-popup-trigger modal-name="status-popup">
  <template #content="{ close }">
    <div class="status-popup">
      <h3>{{ $t(`status.${currentStatus}.title`) }}</h3>
      <p>{{ $t(`status.${currentStatus}.description`) }}</p>
      <button @click="close">{{ $t('button.close') }}</button>
    </div>
  </template>
</codex-popup-trigger>
``` 