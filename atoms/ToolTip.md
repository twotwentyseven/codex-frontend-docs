# ToolTip Component

## Overview
The ToolTip component provides a lightweight tooltip display system using dependency injection for configuration and CSS-based positioning. It features conditional rendering based on tooltip text availability, icon-based visual representation, and data attribute tooltip content for simple and accessible tooltip implementation.

## Basic Usage
```vue
<template>
  <div>
    <!-- Provide tooltip configuration -->
    <script setup>
    provide('tooltipText', 'This is helpful information')
    provide('tooltipIcon', 'ri-information-line')
    </script>
    
    <codex-tooltip />
  </div>
</template>
```

## Key Features
- Dependency injection-based configuration
- Conditional rendering based on tooltip text availability
- Icon-based visual representation with Remix icons
- CSS data attribute tooltip content system
- Lightweight and minimal implementation
- Accessibility-friendly tooltip structure
- Customizable icon display
- Hover and focus interaction support (CSS-based)

## Injected Configuration
The component receives all configuration through Vue's dependency injection:

| Injected Property | Type | Default | Description |
|-------------------|------|---------|-------------|
| tooltipText | String | '' | Tooltip content text displayed on hover/focus |
| tooltipIcon | String | '' | Remix icon class for tooltip trigger |

## Visual Structure
The component renders as:
- **Icon Element**: `<i>` element with Remix icon class
- **CSS Class**: `_c-tooltip` for tooltip styling and behavior
- **Data Attribute**: `data-tooltip` containing the tooltip text
- **Conditional Rendering**: Only renders when tooltipText has content

## CSS-Based Tooltip System
The component relies on CSS for tooltip functionality:
- **Positioning**: CSS handles tooltip position relative to trigger
- **Display Logic**: CSS controls show/hide on hover and focus
- **Content**: `data-tooltip` attribute provides content to CSS
- **Styling**: `_c-tooltip` class applies all visual styling

## Icon Integration
The component uses Remix icons for visual triggers:
- **Icon Classes**: Accepts any Remix icon class (ri-*)
- **Visual Indicator**: Icon serves as tooltip trigger element
- **Hover Target**: Icon provides hover area for tooltip activation
- **Accessibility**: Icon can be focused for keyboard users

## Dependency Injection Pattern
The component follows dependency injection for configuration:
- **Provide/Inject**: Parent components provide tooltip configuration
- **Flexible Setup**: Allows configuration at any parent level
- **Context Sharing**: Multiple components can share tooltip context
- **Clean API**: No props required on component itself

## Accessibility Features
- **Keyboard Focus**: Icon element can receive keyboard focus
- **ARIA Support**: Can be enhanced with ARIA attributes via CSS
- **Screen Reader**: Icon element accessible to assistive technologies
- **Standard HTML**: Uses semantic HTML `<i>` element for icon

## CSS Requirements
The component expects CSS implementation for:
- Tooltip positioning and display logic
- Hover and focus state handling
- Content extraction from data-tooltip attribute
- Visual styling for tooltip appearance
- Responsive positioning based on viewport

## Internationalization
The ToolTip component does not include built-in internationalization. Tooltip text should be localized before providing via dependency injection.

## Examples

### Basic Information Tooltip
```vue
<template>
  <div class="form-field">
    <label>Username</label>
    <input type="text" />
    <codex-tooltip />
  </div>
</template>

<script setup>
import { provide } from 'vue'

provide('tooltipText', 'Username must be 3-20 characters long')
provide('tooltipIcon', 'ri-information-line')
</script>
```

### Help Tooltip with Question Icon
```vue
<template>
  <div class="complex-setting">
    <label>API Rate Limit</label>
    <input type="number" />
    <codex-tooltip />
  </div>
</template>

<script setup>
import { provide } from 'vue'

provide('tooltipText', 'Maximum number of API requests per minute. Higher values may impact performance.')
provide('tooltipIcon', 'ri-question-line')
</script>
```

### Warning Tooltip
```vue
<template>
  <div class="dangerous-action">
    <button class="delete-button">Delete Account</button>
    <codex-tooltip />
  </div>
</template>

<script setup>
import { provide } from 'vue'

provide('tooltipText', 'This action cannot be undone. All data will be permanently deleted.')
provide('tooltipIcon', 'ri-alert-line')
</script>
```

### Dynamic Tooltip Content
```vue
<template>
  <div class="dynamic-tooltip">
    <select v-model="selectedOption">
      <option value="basic">Basic Plan</option>
      <option value="premium">Premium Plan</option>
      <option value="enterprise">Enterprise Plan</option>
    </select>
    <codex-tooltip />
  </div>
</template>

<script setup>
import { provide, ref, computed } from 'vue'

const selectedOption = ref('basic')

const tooltipText = computed(() => {
  const descriptions = {
    basic: 'Basic plan includes 10GB storage and email support',
    premium: 'Premium plan includes 100GB storage, priority support, and advanced features',
    enterprise: 'Enterprise plan includes unlimited storage, dedicated support, and custom integrations'
  }
  return descriptions[selectedOption.value]
})

provide('tooltipText', tooltipText)
provide('tooltipIcon', 'ri-information-line')
</script>
```

### Multiple Tooltips in Form
```vue
<template>
  <form class="user-form">
    <div class="form-field">
      <label>Email Address</label>
      <input type="email" />
      <codex-tooltip key="email-tooltip" />
    </div>
    
    <div class="form-field">
      <label>Password</label>
      <input type="password" />
      <codex-tooltip key="password-tooltip" />
    </div>
    
    <div class="form-field">
      <label>Confirm Password</label>
      <input type="password" />
      <codex-tooltip key="confirm-tooltip" />
    </div>
  </form>
</template>

<script setup>
import { provide } from 'vue'

// Note: In real implementation, you'd need separate provide contexts
// This example shows the concept but would need component structure changes

provide('tooltipText', 'We will never share your email address')
provide('tooltipIcon', 'ri-shield-check-line')
</script>
```

### Contextual Tooltip Provider
```vue
<template>
  <div class="tooltip-context">
    <TooltipProvider
      :text="'Click to expand detailed view'"
      :icon="'ri-eye-line'"
    >
      <button @click="toggleDetails">View Details</button>
      <codex-tooltip />
    </TooltipProvider>
    
    <TooltipProvider
      :text="'Save changes to your profile'"
      :icon="'ri-save-line'"
    >
      <button @click="saveProfile">Save Profile</button>
      <codex-tooltip />
    </TooltipProvider>
  </div>
</template>

<script setup>
// Helper component to provide tooltip context
const TooltipProvider = defineComponent({
  props: ['text', 'icon'],
  setup(props, { slots }) {
    provide('tooltipText', props.text)
    provide('tooltipIcon', props.icon)
    
    return () => slots.default()
  }
})

const toggleDetails = () => {
  // Toggle details logic
}

const saveProfile = () => {
  // Save profile logic
}
</script>
```

### Feature Description Tooltips
```vue
<template>
  <div class="feature-list">
    <div 
      v-for="feature in features" 
      :key="feature.id"
      class="feature-item"
    >
      <TooltipProvider
        :text="feature.description"
        :icon="'ri-information-line'"
      >
        <h3>{{ feature.name }}</h3>
        <codex-tooltip />
      </TooltipProvider>
    </div>
  </div>
</template>

<script setup>
const features = ref([
  {
    id: 1,
    name: 'Real-time Sync',
    description: 'Automatically synchronizes data across all your devices in real-time'
  },
  {
    id: 2,
    name: 'Advanced Analytics',
    description: 'Comprehensive analytics with detailed reports and insights'
  },
  {
    id: 3,
    name: 'Team Collaboration',
    description: 'Work together with your team using shared workspaces and permissions'
  }
])

// TooltipProvider component as defined in previous example
</script>
```

### Status Indicator Tooltips
```vue
<template>
  <div class="status-indicators">
    <div class="status-item">
      <span class="status-label">Server Status</span>
      <span :class="['status-indicator', serverStatus]"></span>
      <TooltipProvider
        :text="serverStatusTooltip"
        :icon="serverStatusIcon"
      >
        <codex-tooltip />
      </TooltipProvider>
    </div>
    
    <div class="status-item">
      <span class="status-label">Database Status</span>
      <span :class="['status-indicator', dbStatus]"></span>
      <TooltipProvider
        :text="dbStatusTooltip"
        :icon="dbStatusIcon"
      >
        <codex-tooltip />
      </TooltipProvider>
    </div>
  </div>
</template>

<script setup>
const serverStatus = ref('healthy')
const dbStatus = ref('warning')

const serverStatusTooltip = computed(() => {
  const tooltips = {
    healthy: 'Server is running normally with no issues',
    warning: 'Server is experiencing minor issues but functioning',
    error: 'Server is experiencing critical issues and may be unavailable'
  }
  return tooltips[serverStatus.value]
})

const serverStatusIcon = computed(() => {
  const icons = {
    healthy: 'ri-checkbox-circle-line',
    warning: 'ri-alert-line',
    error: 'ri-error-warning-line'
  }
  return icons[serverStatus.value]
})

const dbStatusTooltip = computed(() => {
  const tooltips = {
    healthy: 'Database is responding normally',
    warning: 'Database response time is slower than usual',
    error: 'Database is unavailable or unresponsive'
  }
  return tooltips[dbStatus.value]
})

const dbStatusIcon = computed(() => {
  const icons = {
    healthy: 'ri-database-2-line',
    warning: 'ri-database-2-line',
    error: 'ri-database-2-line'
  }
  return icons[dbStatus.value]
})
</script>
```

### Interactive Tooltip with Actions
```vue
<template>
  <div class="action-tooltip">
    <button @click="performAction" class="action-button">
      Complex Action
    </button>
    <TooltipProvider
      :text="actionTooltip"
      :icon="'ri-settings-line'"
    >
      <codex-tooltip />
    </TooltipProvider>
  </div>
</template>

<script setup>
const actionEnabled = ref(true)
const lastActionTime = ref(null)

const actionTooltip = computed(() => {
  if (!actionEnabled.value) {
    return 'Action is currently disabled. Please check your permissions.'
  }
  
  if (lastActionTime.value) {
    return `Last performed: ${lastActionTime.value.toLocaleString()}`
  }
  
  return 'Perform a complex action that may take several minutes to complete'
})

const performAction = () => {
  if (actionEnabled.value) {
    lastActionTime.value = new Date()
    // Perform action logic
  }
}
</script>
```

### Conditional Tooltip Display
```vue
<template>
  <div class="conditional-tooltip">
    <input 
      v-model="userInput" 
      :class="{ 'has-error': hasValidationError }"
      placeholder="Enter text"
    />
    
    <TooltipProvider
      v-if="showTooltip"
      :text="currentTooltipText"
      :icon="currentTooltipIcon"
    >
      <codex-tooltip />
    </TooltipProvider>
  </div>
</template>

<script setup>
const userInput = ref('')
const hasValidationError = ref(false)

const showTooltip = computed(() => hasValidationError.value || userInput.value.length > 0)

const currentTooltipText = computed(() => {
  if (hasValidationError.value) {
    return 'Input contains invalid characters. Please use only letters and numbers.'
  }
  
  if (userInput.value.length > 0) {
    return `Character count: ${userInput.value.length}/100`
  }
  
  return ''
})

const currentTooltipIcon = computed(() => {
  if (hasValidationError.value) {
    return 'ri-error-warning-line'
  }
  
  return 'ri-information-line'
})

// Watch for validation changes
watch(userInput, (newValue) => {
  hasValidationError.value = !/^[a-zA-Z0-9\s]*$/.test(newValue)
})
</script>
```

### Localized Tooltips
```vue
<template>
  <div class="localized-tooltips">
    <button class="save-button">{{ $t('buttons.save') }}</button>
    <TooltipProvider
      :text="$t('tooltips.save_description')"
      :icon="'ri-save-line'"
    >
      <codex-tooltip />
    </TooltipProvider>
    
    <button class="cancel-button">{{ $t('buttons.cancel') }}</button>
    <TooltipProvider
      :text="$t('tooltips.cancel_description')"
      :icon="'ri-close-line'"
    >
      <codex-tooltip />
    </TooltipProvider>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'

const { t } = useI18n()

// Translation keys would be defined in language files:
// en.json:
// {
//   "tooltips": {
//     "save_description": "Save your changes permanently",
//     "cancel_description": "Discard changes and return to previous state"
//   }
// }
</script>
```

## CSS Classes
- `_c-tooltip`: Main tooltip styling and behavior class

## CSS Implementation Requirements
The component requires CSS implementation for full functionality:

```css
._c-tooltip {
  /* Tooltip trigger styling */
  cursor: help;
  position: relative;
}

._c-tooltip::after {
  /* Tooltip content from data-tooltip attribute */
  content: attr(data-tooltip);
  position: absolute;
  /* Positioning logic */
  /* Styling for tooltip appearance */
  /* Initially hidden */
  opacity: 0;
  visibility: hidden;
}

._c-tooltip:hover::after,
._c-tooltip:focus::after {
  /* Show tooltip on hover and focus */
  opacity: 1;
  visibility: visible;
}
```

## Best Practices

### Recommended Usage Patterns
- Use dependency injection to provide tooltip configuration from parent components
- Keep tooltip text concise and informative
- Use appropriate icons that match the tooltip purpose
- Provide tooltips for complex or unfamiliar interface elements
- Use consistent icon choices across similar tooltip types
- Test tooltip positioning across different screen sizes
- Ensure tooltip content is accessible via keyboard navigation

### Common Pitfalls to Avoid
- Not providing tooltipText (component won't render)
- Using overly long tooltip text that doesn't fit well
- Missing CSS implementation for tooltip display logic
- Not testing tooltip positioning at viewport edges
- Using tooltips for critical information that should be always visible
- Forgetting keyboard accessibility for tooltip triggers

### Accessibility Considerations
- Ensure tooltip triggers are focusable for keyboard users
- Provide sufficient contrast for tooltip text
- Keep tooltip content concise for screen readers
- Test with screen readers to ensure proper announcement
- Consider using ARIA attributes for enhanced accessibility
- Handle tooltip dismissal appropriately
- Avoid tooltips that interfere with other interactive elements

### Content Guidelines
- Keep tooltip text brief but informative
- Use plain language that's easy to understand
- Provide helpful context without being redundant
- Use consistent tone across all tooltips
- Avoid HTML content in tooltip text
- Consider text length in different languages

### Performance Considerations
- Minimize tooltip component instances when possible
- Use CSS for hover effects rather than JavaScript
- Cache tooltip content when using dynamic text
- Optimize icon loading for better performance
- Handle tooltip cleanup appropriately

### CSS Implementation
- Implement robust positioning logic for edge cases
- Handle tooltip overflow and viewport boundaries
- Provide smooth transitions for tooltip appearance
- Support both hover and focus states
- Handle responsive design considerations
- Test positioning across different browsers

### State Management
- Use reactive properties for dynamic tooltip content
- Handle tooltip context appropriately with dependency injection
- Coordinate tooltip display with component state
- Manage tooltip lifecycle properly
- Handle tooltip updates efficiently

### Mobile Considerations
- Consider touch interaction patterns for mobile devices
- Test tooltip behavior on touch screens
- Handle tooltip dismissal on mobile appropriately
- Consider alternative interaction patterns for mobile
- Test tooltip positioning on smaller screens

### User Experience
- Use tooltips to enhance understanding, not replace clear UI
- Position tooltips logically relative to their triggers
- Provide immediate visual feedback when hovering
- Handle tooltip conflicts when multiple elements are close
- Test tooltip behavior in real usage scenarios

## Component Registration
The component is registered as `codex-tooltip` in the application. 