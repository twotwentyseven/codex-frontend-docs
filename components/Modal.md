# Modal Component

## Overview
The Modal component provides a flexible, teleportable modal dialog system with support for positioning, overlay control, transitions, and URL state management. It features z-index stacking management, auto-close functionality, and extensive customization options. The component can be triggered from anywhere in the DOM and includes built-in accessibility features and responsive behavior.

## Basic Usage
```vue
<template>
  <div class="modal-example">
    <button @click="openModal">Open Modal</button>
    
    <codex-modal 
      modal-name="example-modal"
      :show-close-button="true"
      :x-position="'center'"
      :y-position="'center'"
    >
      <template #default="{ close }">
        <div class="modal-content">
          <h2>Modal Title</h2>
          <p>This is the modal content.</p>
          <button @click="close">Close Modal</button>
        </div>
      </template>
    </codex-modal>
  </div>
</template>

<script setup>
const openModal = () => {
  document.dispatchEvent(new CustomEvent('codex.modal.open.example-modal'))
}
</script>
```

## Key Features
- Teleportable modal rendering with portal selector
- Flexible positioning system (9 position combinations)
- Z-index stacking management for multiple modals
- URL state synchronization
- Auto-close with countdown timer
- Overlay click-to-close functionality
- Smooth transitions and animations
- Global modal event system
- Responsive height calculation
- Built-in accessibility support

## Configuration Props

### Modal Control Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modalName` | `String` | `required` | Unique identifier for the modal |
| `defaultState` | `Boolean` | `false` | Initial open/closed state |
| `showCloseButton` | `Boolean` | `true` | Display built-in close button |

### Positioning Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `xPosition` | `String` | `'center'` | Horizontal position (left, center, right) |
| `yPosition` | `String` | `'center'` | Vertical position (top, center, bottom) |

### Behavior Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `useOverlay` | `Boolean` | `true` | Show background overlay |
| `delayClose` | `Number|Boolean` | `false` | Auto-close delay in milliseconds |

### Display Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `transition` | `String` | `'fade'` | CSS transition name |
| `portalSelector` | `String|Boolean` | `'body'` | Portal target selector |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:open` | `boolean` | Emitted when modal open state changes |
| `open` | N/A | Emitted when modal opens |
| `close` | N/A | Emitted when modal closes |

## Slots

### Default Slot
| Slot | Props | Description |
|------|-------|-------------|
| `default` | `{ delayClose, open, close }` | Main modal content with control functions |

## Modal Control API

The modal can be controlled through custom events:

```javascript
// Open modal
document.dispatchEvent(new CustomEvent('codex.modal.open.modal-name'))

// Close modal
document.dispatchEvent(new CustomEvent('codex.modal.close.modal-name'))

// Toggle modal
document.dispatchEvent(new CustomEvent('codex.modal.toggle.modal-name'))

// Close all modals
document.dispatchEvent(new CustomEvent('codex.modals.close'))
```

## URL Integration

The modal automatically manages URL parameters for state persistence:

- Opening a modal adds `?modal-name=true` to the URL
- Closing a modal removes the parameter
- Browser back/forward navigation is supported
- Both prefixed (`codex-modal`) and unprefixed names are supported

## Z-Index Management

The component uses a global stacking system for multiple modals:

```javascript
import { useModalStack } from '@/composition/useModalStack'

const { addToStack, removeFromStack, getZIndex } = useModalStack()
```

## Examples

### Basic Modal with Custom Content
```vue
<template>
  <div class="basic-modal-demo">
    <button @click="openBasicModal" class="trigger-btn">
      Open Basic Modal
    </button>
    
    <codex-modal 
      modal-name="basic-modal"
      :x-position="'center'"
      :y-position="'center'"
      :show-close-button="true"
    >
      <template #default="{ close }">
        <div class="basic-modal-content">
          <header class="modal-header">
            <h2>Welcome Modal</h2>
          </header>
          
          <div class="modal-body">
            <p>This is a basic modal with custom content.</p>
            <p>You can include any components or content here.</p>
            
            <div class="feature-list">
              <div class="feature-item">
                <i class="ri-check-line"></i>
                <span>Easy to use</span>
              </div>
              <div class="feature-item">
                <i class="ri-check-line"></i>
                <span>Highly customizable</span>
              </div>
              <div class="feature-item">
                <i class="ri-check-line"></i>
                <span>Accessible</span>
              </div>
            </div>
          </div>
          
          <footer class="modal-footer">
            <button @click="close" class="btn-secondary">
              Cancel
            </button>
            <button @click="handleProceed" class="btn-primary">
              Proceed
            </button>
          </footer>
        </div>
      </template>
    </codex-modal>
  </div>
</template>

<script setup>
const openBasicModal = () => {
  document.dispatchEvent(new CustomEvent('codex.modal.open.basic-modal'))
}

const handleProceed = () => {
  console.log('User proceeded')
  document.dispatchEvent(new CustomEvent('codex.modal.close.basic-modal'))
}
</script>

<style scoped>
.basic-modal-content {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  max-width: 500px;
  width: 90vw;
}

.modal-header {
  padding: 1.5rem 1.5rem 0;
  border-bottom: 1px solid #eee;
}

.modal-body {
  padding: 1.5rem;
}

.modal-footer {
  padding: 0 1.5rem 1.5rem;
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
}

.feature-list {
  margin: 1rem 0;
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.5rem;
}

.feature-item i {
  color: #28a745;
}

.btn-primary, .btn-secondary {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
}

.btn-primary {
  background: #007bff;
  color: white;
}

.btn-secondary {
  background: #6c757d;
  color: white;
}

.trigger-btn {
  padding: 1rem 2rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1rem;
}
</style>
```

### Positioned Modal with Auto-Close
```vue
<template>
  <div class="positioned-modal-demo">
    <div class="button-grid">
      <button 
        v-for="position in positions" 
        :key="position.name"
        @click="openPositionedModal(position)"
        class="position-btn"
      >
        {{ position.name }}
      </button>
    </div>
    
    <codex-modal 
      modal-name="positioned-modal"
      :x-position="currentPosition.x"
      :y-position="currentPosition.y"
      :show-close-button="false"
      :use-overlay="false"
      :delay-close="3000"
    >
      <template #default="{ delayClose }">
        <div class="positioned-modal-content">
          <div class="notification-icon">
            <i class="ri-notification-line"></i>
          </div>
          
          <div class="notification-content">
            <h3>{{ currentPosition.name }} Notification</h3>
            <p>This modal will auto-close in 3 seconds.</p>
            
            <div class="auto-close-actions">
              <button @click="delayClose(1000)" class="btn-sm">
                Close in 1s
              </button>
              <button @click="delayClose(5000)" class="btn-sm">
                Extend to 5s
              </button>
            </div>
          </div>
        </div>
      </template>
    </codex-modal>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const currentPosition = ref({ x: 'center', y: 'center', name: 'Center' })

const positions = [
  { x: 'left', y: 'top', name: 'Top Left' },
  { x: 'center', y: 'top', name: 'Top Center' },
  { x: 'right', y: 'top', name: 'Top Right' },
  { x: 'left', y: 'center', name: 'Center Left' },
  { x: 'center', y: 'center', name: 'Center' },
  { x: 'right', y: 'center', name: 'Center Right' },
  { x: 'left', y: 'bottom', name: 'Bottom Left' },
  { x: 'center', y: 'bottom', name: 'Bottom Center' },
  { x: 'right', y: 'bottom', name: 'Bottom Right' }
]

const openPositionedModal = (position) => {
  currentPosition.value = position
  document.dispatchEvent(new CustomEvent('codex.modal.open.positioned-modal'))
}
</script>

<style scoped>
.button-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  max-width: 400px;
  margin: 0 auto;
}

.position-btn {
  padding: 1rem;
  background: #f8f9fa;
  border: 2px solid #dee2e6;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s;
}

.position-btn:hover {
  background: #e9ecef;
  border-color: #007bff;
}

.positioned-modal-content {
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  max-width: 300px;
  display: flex;
  align-items: center;
  gap: 1rem;
}

.notification-icon {
  font-size: 1.5rem;
  color: #007bff;
}

.auto-close-actions {
  display: flex;
  gap: 0.5rem;
  margin-top: 1rem;
}

.btn-sm {
  padding: 0.25rem 0.75rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
}
</style>
```

### Modal with Form and Validation
```vue
<template>
  <div class="form-modal-demo">
    <button @click="openFormModal" class="open-form-btn">
      Open Contact Form
    </button>
    
    <codex-modal 
      modal-name="form-modal"
      :x-position="'center'"
      :y-position="'center'"
      :show-close-button="true"
    >
      <template #default="{ close }">
        <div class="form-modal-content">
          <header class="form-header">
            <h2>Contact Us</h2>
            <p>We'd love to hear from you!</p>
          </header>
          
          <form @submit.prevent="handleSubmit" class="contact-form">
            <div class="form-group">
              <label for="name">Name *</label>
              <input 
                v-model="form.name"
                type="text" 
                id="name"
                :class="{ error: errors.name }"
                required
              >
              <span v-if="errors.name" class="error-message">{{ errors.name }}</span>
            </div>
            
            <div class="form-group">
              <label for="email">Email *</label>
              <input 
                v-model="form.email"
                type="email" 
                id="email"
                :class="{ error: errors.email }"
                required
              >
              <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
            </div>
            
            <div class="form-group">
              <label for="subject">Subject</label>
              <select v-model="form.subject" id="subject">
                <option value="">Select a topic</option>
                <option value="general">General Inquiry</option>
                <option value="support">Technical Support</option>
                <option value="billing">Billing Question</option>
                <option value="feedback">Feedback</option>
              </select>
            </div>
            
            <div class="form-group">
              <label for="message">Message *</label>
              <textarea 
                v-model="form.message"
                id="message"
                rows="4"
                :class="{ error: errors.message }"
                required
              ></textarea>
              <span v-if="errors.message" class="error-message">{{ errors.message }}</span>
            </div>
            
            <div class="form-actions">
              <button type="button" @click="close" class="btn-cancel">
                Cancel
              </button>
              <button 
                type="submit" 
                :disabled="isSubmitting"
                class="btn-submit"
              >
                <span v-if="isSubmitting">
                  <i class="ri-loader-4-line spinning"></i>
                  Sending...
                </span>
                <span v-else>Send Message</span>
              </button>
            </div>
          </form>
          
          <div v-if="submitSuccess" class="success-message">
            <i class="ri-check-line"></i>
            <span>Message sent successfully!</span>
          </div>
        </div>
      </template>
    </codex-modal>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

const form = reactive({
  name: '',
  email: '',
  subject: '',
  message: ''
})

const errors = reactive({})
const isSubmitting = ref(false)
const submitSuccess = ref(false)

const openFormModal = () => {
  // Reset form state
  Object.assign(form, { name: '', email: '', subject: '', message: '' })
  Object.keys(errors).forEach(key => delete errors[key])
  submitSuccess.value = false
  isSubmitting.value = false
  
  document.dispatchEvent(new CustomEvent('codex.modal.open.form-modal'))
}

const validateForm = () => {
  Object.keys(errors).forEach(key => delete errors[key])
  
  if (!form.name.trim()) {
    errors.name = 'Name is required'
  }
  
  if (!form.email.trim()) {
    errors.email = 'Email is required'
  } else if (!/\S+@\S+\.\S+/.test(form.email)) {
    errors.email = 'Please enter a valid email'
  }
  
  if (!form.message.trim()) {
    errors.message = 'Message is required'
  } else if (form.message.length < 10) {
    errors.message = 'Message must be at least 10 characters'
  }
  
  return Object.keys(errors).length === 0
}

const handleSubmit = async () => {
  if (!validateForm()) return
  
  isSubmitting.value = true
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 2000))
    
    submitSuccess.value = true
    
    // Auto-close after success
    setTimeout(() => {
      document.dispatchEvent(new CustomEvent('codex.modal.close.form-modal'))
    }, 2000)
    
  } catch (error) {
    console.error('Form submission error:', error)
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
.form-modal-content {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  max-width: 500px;
  width: 90vw;
  max-height: 80vh;
  overflow-y: auto;
}

.form-header {
  padding: 2rem 2rem 1rem;
  text-align: center;
  border-bottom: 1px solid #eee;
}

.contact-form {
  padding: 2rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
  color: #333;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.2s;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #007bff;
}

.form-group input.error,
.form-group textarea.error {
  border-color: #dc3545;
}

.error-message {
  color: #dc3545;
  font-size: 0.875rem;
  margin-top: 0.25rem;
  display: block;
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  margin-top: 2rem;
}

.btn-cancel,
.btn-submit {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s;
}

.btn-cancel {
  background: #6c757d;
  color: white;
}

.btn-submit {
  background: #007bff;
  color: white;
}

.btn-submit:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.success-message {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: #d4edda;
  color: #155724;
  padding: 1rem;
  margin: 1rem 2rem 2rem;
  border-radius: 4px;
}

.open-form-btn {
  padding: 1rem 2rem;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1rem;
}

.spinning {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-modal` | Main modal container |
| `_c-modal-inner` | Inner modal content container |
| `_c-no-overlay` | Modal without background overlay |
| `_c-close` | Built-in close button |
| `_c-x-left` | Left horizontal positioning |
| `_c-x-center` | Center horizontal positioning |
| `_c-x-right` | Right horizontal positioning |
| `_c-y-top` | Top vertical positioning |
| `_c-y-center` | Center vertical positioning |
| `_c-y-bottom` | Bottom vertical positioning |
| `cdx_delayed-close-countdown` | Auto-close countdown animation |
| `cdx_auto-close` | Auto-close styling |

## Internationalization

The Modal component has minimal translation requirements as it primarily provides structural layout and behavior. Translation needs are typically handled by the content components placed within the modal.

### Core Modal Interface
| Key | Usage |
|-----|-------|
| `modal.close` | Close button aria-label (accessibility) |
| `modal.close_modal` | Close button tooltip text |

### Error and State Messages
| Key | Usage |
|-----|-------|
| `modal.loading` | Modal loading state message |
| `modal.error` | Modal error state message |

### Translation Usage Examples
```vue
<!-- Modal with translated accessibility attributes -->
<codex-modal 
  modal-name="example-modal"
  :show-close-button="true"
>
  <template #default="{ close }">
    <div class="modal-content">
      <!-- Modal content with its own translations -->
      <header class="modal-header">
        <h2>{{ $t('example.modal_title') }}</h2>
        <button 
          @click="close" 
          class="close-btn"
          :aria-label="$t('modal.close')"
          :title="$t('modal.close_modal')"
        >
          <i class="ri-close-line"></i>
        </button>
      </header>
      
      <div class="modal-body">
        <p>{{ $t('example.modal_content') }}</p>
      </div>
      
      <footer class="modal-footer">
        <button @click="close" class="btn-secondary">
          {{ $t('button.cancel') }}
        </button>
        <button @click="handleConfirm" class="btn-primary">
          {{ $t('button.confirm') }}
        </button>
      </footer>
    </div>
  </template>
</codex-modal>
```

### Translation Strategy

#### Content-Driven Translation
The Modal component follows a content-driven translation strategy:
- **Modal Structure**: The modal itself provides layout and behavior
- **Content Translation**: Components placed inside the modal handle their own translations
- **Accessibility**: Modal provides basic accessibility attributes with optional translation

#### Common Modal Translation Patterns
```vue
<!-- Login Modal Example -->
<codex-modal modal-name="codex-login">
  <codex-login /> <!-- Login component handles its own translations -->
</codex-modal>

<!-- Cart Modal Example -->
<codex-modal modal-name="codex-cart">
  <codex-cart-contents :cart="cart" />
</codex-modal>

<!-- Custom Content Modal -->
<codex-modal modal-name="confirmation-modal">
  <template #default="{ close }">
    <div class="confirmation-content">
      <h3>{{ $t('confirmation.title') }}</h3>
      <p>{{ $t('confirmation.message') }}</p>
      <div class="actions">
        <button @click="close">{{ $t('button.cancel') }}</button>
        <button @click="handleConfirm">{{ $t('button.confirm') }}</button>
      </div>
    </div>
  </template>
</codex-modal>
```

#### Accessibility Translation Support
- The built-in close button can have translated aria-label attributes
- Modal announcements for screen readers could be translated
- Focus management messages could use translated text

#### URL State Translation
- Modal names in URLs remain consistent (not translated)
- Modal state parameters use technical identifiers rather than user-facing text
- Query parameters like `?modal-name=true` are functional, not user-facing

## Best Practices

### Modal Management
- Use unique, descriptive modal names
- Implement proper cleanup on component unmount
- Handle multiple modal scenarios carefully
- Consider z-index stacking for complex applications
- Test modal behavior across different browsers

### User Experience
- Provide clear ways to close modals
- Use appropriate positioning for content type
- Consider mobile device constraints
- Implement smooth transitions
- Handle focus management properly

### Content Design
- Keep modal content focused and concise
- Use appropriate sizing for different screen sizes
- Implement scrolling for long content
- Consider loading states for dynamic content
- Provide clear action buttons

### Accessibility
- Ensure proper focus management
- Use semantic HTML structure
- Provide keyboard navigation support
- Include appropriate ARIA attributes
- Test with screen readers

### Performance
- Use teleportation efficiently
- Avoid unnecessary modal instances
- Implement lazy loading for heavy content
- Optimize for rapid open/close operations
- Monitor memory usage with complex modals

### Mobile Considerations
- Test on various screen sizes
- Consider touch interaction patterns
- Handle virtual keyboard appearance
- Optimize for smaller screens
- Test orientation changes

## Component Registration
```javascript
// Global registration
app.component('CodexModal', Modal)

// Local registration  
import Modal from '@/components/molecules/Modal.vue'

export default {
  components: {
    CodexModal: Modal
  }
}
``` 