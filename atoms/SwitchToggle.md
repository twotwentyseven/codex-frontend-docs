# SwitchToggle Component

## Overview
The SwitchToggle component provides a modern toggle switch interface with visual feedback, label integration, and v-model support. It features checkbox-based functionality with enhanced styling, active state visualization, handle animation, and flexible value types for intuitive on/off selection in forms and settings interfaces.

## Basic Usage
```vue
<codex-switch-toggle
  v-model="toggleValue"
  :id="'feature-toggle'"
  :label="'Enable notifications'"
/>
```

## Key Features
- Two-way data binding with v-model support
- Visual switch handle with active state animation
- Integrated label with click-to-toggle functionality
- Flexible value types (Boolean, String, Number)
- CSS-based styling with active state classes
- Accessibility-compliant checkbox implementation
- Required ID and label props for proper association
- Modern toggle switch appearance

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| id | String | Yes | - | Unique identifier for the switch and label association |
| label | String | Yes | - | Label text displayed next to the switch |

### Model Value
The component uses `defineModel` for two-way binding:
| Model Property | Type | Default | Description |
|----------------|------|---------|-------------|
| modelValue | Boolean\|String\|Number | false | Toggle state value |

## Visual Structure
The component creates a toggle switch with:
- **Container**: `_c-switch-container` wrapping the entire component
- **Checkbox Input**: Hidden checkbox with `_c-switch-input` class
- **Switch Label**: Visual switch with `_c-switch` class and active state
- **Switch Handle**: Visual handle element with `_c-switch-handle` class
- **Label Text**: Displayed label text for user interaction

## Active State Management
The switch provides visual feedback through CSS classes:
- **Active State**: `active` class applied when model value is truthy
- **Handle Position**: CSS handles the visual position of the switch handle
- **Transition Effects**: Smooth transitions between active/inactive states
- **Visual Feedback**: Clear indication of current toggle state

## Label Integration
The component includes integrated label functionality:
- **Label Association**: Uses `for` attribute linking to switch ID
- **Click Handling**: Label clicks toggle the switch state
- **Data Attribute**: `data-content` attribute contains label text
- **Dual Display**: Label text appears both as attribute and element content

## Accessibility Features
- **Semantic HTML**: Uses actual checkbox input for screen reader compatibility
- **Label Association**: Proper label-input relationship via ID
- **Keyboard Support**: Standard checkbox keyboard navigation (Space key)
- **Focus Management**: Browser-native focus handling
- **Screen Reader**: Announces state changes and label content
- **ARIA Compliance**: Inherits checkbox ARIA behaviors

## Value Type Flexibility
The component supports multiple value types:
- **Boolean**: Standard true/false toggle (default)
- **String**: Custom string values for on/off states
- **Number**: Numeric values for toggle states
- **Type Safety**: Vue 3 type checking with defineModel

## CSS Classes
- `_c-switch-container`: Container wrapper styling
- `_c-switch-input`: Hidden checkbox input styling
- `_c-switch`: Main switch visual styling
- `_c-switch.active`: Active state styling
- `_c-switch-handle`: Switch handle styling

## Internationalization
The SwitchToggle component does not include built-in internationalization. Label text should be localized before passing to the component.

## Examples

### Basic Toggle Switch
```vue
<codex-switch-toggle
  v-model="notificationsEnabled"
  :id="'notifications-toggle'"
  :label="'Enable push notifications'"
/>
```

### Settings Panel Toggle
```vue
<template>
  <div class="settings-panel">
    <h3>Privacy Settings</h3>
    
    <div class="setting-item">
      <codex-switch-toggle
        v-model="settings.dataCollection"
        :id="'data-collection-toggle'"
        :label="'Allow data collection'"
      />
    </div>
    
    <div class="setting-item">
      <codex-switch-toggle
        v-model="settings.analytics"
        :id="'analytics-toggle'"
        :label="'Enable analytics tracking'"
      />
    </div>
    
    <div class="setting-item">
      <codex-switch-toggle
        v-model="settings.marketing"
        :id="'marketing-toggle'"
        :label="'Receive marketing emails'"
      />
    </div>
  </div>
</template>

<script setup>
const settings = ref({
  dataCollection: false,
  analytics: true,
  marketing: false
})
</script>
```

### Form Integration with Validation
```vue
<template>
  <form class="preference-form">
    <div class="form-section">
      <h4>Account Preferences</h4>
      
      <div class="toggle-group">
        <codex-switch-toggle
          v-model="preferences.emailNotifications"
          :id="'email-notifications'"
          :label="'Email notifications'"
        />
        
        <codex-switch-toggle
          v-model="preferences.smsNotifications"
          :id="'sms-notifications'"
          :label="'SMS notifications'"
        />
        
        <codex-switch-toggle
          v-model="preferences.publicProfile"
          :id="'public-profile'"
          :label="'Make profile public'"
        />
      </div>
      
      <div v-if="!hasAnyNotification" class="validation-message">
        Please enable at least one notification method
      </div>
    </div>
  </form>
</template>

<script setup>
const preferences = ref({
  emailNotifications: true,
  smsNotifications: false,
  publicProfile: false
})

const hasAnyNotification = computed(() => 
  preferences.value.emailNotifications || preferences.value.smsNotifications
)
</script>
```

### String Value Toggle
```vue
<template>
  <div class="theme-selector">
    <codex-switch-toggle
      v-model="themeMode"
      :id="'theme-toggle'"
      :label="themeModeLabel"
    />
    
    <div class="theme-preview">
      Current theme: {{ themeMode }}
    </div>
  </div>
</template>

<script setup>
const themeMode = ref('light')

const themeModeLabel = computed(() => 
  themeMode.value === 'light' ? 'Switch to dark mode' : 'Switch to light mode'
)

// Toggle between 'light' and 'dark'
watch(themeMode, (newValue) => {
  if (newValue === true || newValue === 'dark') {
    themeMode.value = 'dark'
  } else {
    themeMode.value = 'light'
  }
})
</script>
```

### Numeric Value Toggle
```vue
<template>
  <div class="priority-setting">
    <codex-switch-toggle
      v-model="priorityLevel"
      :id="'priority-toggle'"
      :label="priorityLabel"
    />
    
    <div class="priority-info">
      Priority level: {{ priorityLevel }}
    </div>
  </div>
</template>

<script setup>
const priorityLevel = ref(0)

const priorityLabel = computed(() => 
  priorityLevel.value > 0 ? 'High priority mode (ON)' : 'Normal priority mode (OFF)'
)

// Toggle between 0 and 10
watch(priorityLevel, (newValue) => {
  if (newValue === true || newValue > 0) {
    priorityLevel.value = 10
  } else {
    priorityLevel.value = 0
  }
})
</script>
```

### Feature Flag Toggle
```vue
<template>
  <div class="feature-flags">
    <h3>Experimental Features</h3>
    
    <div class="feature-list">
      <div 
        v-for="feature in features" 
        :key="feature.id"
        class="feature-item"
      >
        <div class="feature-info">
          <strong>{{ feature.name }}</strong>
          <p>{{ feature.description }}</p>
        </div>
        
        <codex-switch-toggle
          v-model="featureStates[feature.id]"
          :id="`feature-${feature.id}`"
          :label="`Toggle ${feature.name}`"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
const features = ref([
  {
    id: 'beta-ui',
    name: 'Beta UI',
    description: 'Try our new interface design'
  },
  {
    id: 'advanced-search',
    name: 'Advanced Search',
    description: 'Enhanced search capabilities'
  },
  {
    id: 'real-time-sync',
    name: 'Real-time Sync',
    description: 'Live data synchronization'
  }
])

const featureStates = ref({
  'beta-ui': false,
  'advanced-search': true,
  'real-time-sync': false
})
</script>
```

### Conditional Toggle Groups
```vue
<template>
  <div class="conditional-toggles">
    <div class="master-toggle">
      <codex-switch-toggle
        v-model="masterEnabled"
        :id="'master-toggle'"
        :label="'Enable advanced features'"
      />
    </div>
    
    <div v-if="masterEnabled" class="sub-toggles">
      <div class="sub-toggle">
        <codex-switch-toggle
          v-model="subFeatures.automation"
          :id="'automation-toggle'"
          :label="'Automatic processing'"
        />
      </div>
      
      <div class="sub-toggle">
        <codex-switch-toggle
          v-model="subFeatures.reporting"
          :id="'reporting-toggle'"
          :label="'Advanced reporting'"
        />
      </div>
      
      <div class="sub-toggle">
        <codex-switch-toggle
          v-model="subFeatures.integrations"
          :id="'integrations-toggle'"
          :label="'Third-party integrations'"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
const masterEnabled = ref(false)
const subFeatures = ref({
  automation: false,
  reporting: false,
  integrations: false
})

// Clear sub-features when master is disabled
watch(masterEnabled, (enabled) => {
  if (!enabled) {
    subFeatures.value = {
      automation: false,
      reporting: false,
      integrations: false
    }
  }
})
</script>
```

### Toggle with Side Effects
```vue
<template>
  <div class="toggle-with-effects">
    <codex-switch-toggle
      v-model="maintenanceMode"
      :id="'maintenance-toggle'"
      :label="'Enable maintenance mode'"
    />
    
    <div v-if="maintenanceMode" class="maintenance-warning">
      ⚠️ Maintenance mode is active. Users will see a maintenance page.
    </div>
    
    <div v-if="showConfirmation" class="confirmation-dialog">
      <p>Are you sure you want to {{ maintenanceMode ? 'enable' : 'disable' }} maintenance mode?</p>
      <button @click="confirmToggle">Confirm</button>
      <button @click="cancelToggle">Cancel</button>
    </div>
  </div>
</template>

<script setup>
const maintenanceMode = ref(false)
const showConfirmation = ref(false)
const pendingState = ref(false)

const confirmToggle = () => {
  maintenanceMode.value = pendingState.value
  showConfirmation.value = false
  
  // Trigger side effects
  if (maintenanceMode.value) {
    enableMaintenanceMode()
  } else {
    disableMaintenanceMode()
  }
}

const cancelToggle = () => {
  showConfirmation.value = false
}

const enableMaintenanceMode = () => {
  console.log('Maintenance mode enabled')
  // API call to enable maintenance
}

const disableMaintenanceMode = () => {
  console.log('Maintenance mode disabled')
  // API call to disable maintenance
}

// Watch for toggle changes to show confirmation
watch(maintenanceMode, (newValue, oldValue) => {
  if (newValue !== oldValue) {
    pendingState.value = newValue
    maintenanceMode.value = oldValue // Revert temporarily
    showConfirmation.value = true
  }
})
</script>
```

### Animated Toggle States
```vue
<template>
  <div class="animated-toggles">
    <div class="toggle-item">
      <codex-switch-toggle
        v-model="animations.slideIn"
        :id="'slide-in-toggle'"
        :label="'Slide-in animations'"
      />
    </div>
    
    <div class="toggle-item">
      <codex-switch-toggle
        v-model="animations.fadeTransitions"
        :id="'fade-toggle'"
        :label="'Fade transitions'"
      />
    </div>
    
    <div class="demo-area" :class="animationClasses">
      <div class="demo-element">
        Animation demo area
      </div>
    </div>
  </div>
</template>

<script setup>
const animations = ref({
  slideIn: true,
  fadeTransitions: false
})

const animationClasses = computed(() => ({
  'slide-enabled': animations.value.slideIn,
  'fade-enabled': animations.value.fadeTransitions
}))
</script>

<style scoped>
.demo-area {
  padding: 2rem;
  border: 2px dashed #ccc;
  margin-top: 1rem;
  transition: all 0.3s ease;
}

.demo-area.slide-enabled .demo-element {
  transform: translateX(0);
  transition: transform 0.5s ease;
}

.demo-area:not(.slide-enabled) .demo-element {
  transform: translateX(-20px);
}

.demo-area.fade-enabled {
  opacity: 1;
  transition: opacity 0.3s ease;
}

.demo-area:not(.fade-enabled) {
  opacity: 0.7;
}
</style>
```

### Toggle with Local Storage
```vue
<template>
  <div class="persistent-toggles">
    <h3>Preferences (Auto-saved)</h3>
    
    <div class="toggle-list">
      <codex-switch-toggle
        v-model="preferences.darkMode"
        :id="'dark-mode-toggle'"
        :label="'Dark mode'"
      />
      
      <codex-switch-toggle
        v-model="preferences.compactView"
        :id="'compact-view-toggle'"
        :label="'Compact view'"
      />
      
      <codex-switch-toggle
        v-model="preferences.autoSave"
        :id="'auto-save-toggle'"
        :label="'Auto-save documents'"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'

const preferences = ref({
  darkMode: false,
  compactView: false,
  autoSave: true
})

// Load preferences from localStorage on mount
onMounted(() => {
  const saved = localStorage.getItem('user-preferences')
  if (saved) {
    try {
      preferences.value = { ...preferences.value, ...JSON.parse(saved) }
    } catch (error) {
      console.error('Error loading preferences:', error)
    }
  }
})

// Save preferences to localStorage when changed
watch(preferences, (newPreferences) => {
  try {
    localStorage.setItem('user-preferences', JSON.stringify(newPreferences))
  } catch (error) {
    console.error('Error saving preferences:', error)
  }
}, { deep: true })
</script>
```

### Toggle with Custom Styling
```vue
<template>
  <div class="custom-styled-toggles">
    <div class="toggle-group success">
      <codex-switch-toggle
        v-model="successToggle"
        :id="'success-toggle'"
        :label="'Success mode'"
        class="success-toggle"
      />
    </div>
    
    <div class="toggle-group warning">
      <codex-switch-toggle
        v-model="warningToggle"
        :id="'warning-toggle'"
        :label="'Warning mode'"
        class="warning-toggle"
      />
    </div>
    
    <div class="toggle-group danger">
      <codex-switch-toggle
        v-model="dangerToggle"
        :id="'danger-toggle'"
        :label="'Danger mode'"
        class="danger-toggle"
      />
    </div>
  </div>
</template>

<script setup>
const successToggle = ref(false)
const warningToggle = ref(false)
const dangerToggle = ref(false)
</script>

<style scoped>
.success-toggle ._c-switch.active {
  background-color: #10b981;
}

.warning-toggle ._c-switch.active {
  background-color: #f59e0b;
}

.danger-toggle ._c-switch.active {
  background-color: #ef4444;
}

.toggle-group {
  margin: 1rem 0;
  padding: 1rem;
  border-radius: 0.5rem;
}

.toggle-group.success {
  background-color: #f0fdf4;
}

.toggle-group.warning {
  background-color: #fffbeb;
}

.toggle-group.danger {
  background-color: #fef2f2;
}
</style>
```

## CSS Classes
- `_c-switch-container`: Container wrapper styling
- `_c-switch-input`: Hidden checkbox input styling
- `_c-switch`: Main switch visual styling
- `_c-switch.active`: Active state styling
- `_c-switch-handle`: Switch handle element styling

## Best Practices

### Recommended Usage Patterns
- Always provide unique and descriptive IDs for each toggle
- Use clear and concise label text that describes the toggle function
- Provide meaningful default values for toggle states
- Use Boolean values for simple on/off toggles
- Handle toggle state changes with appropriate side effects
- Group related toggles logically in the interface
- Provide visual feedback for toggle state changes
- Test toggle functionality with keyboard navigation

### Common Pitfalls to Avoid
- Not providing required ID and label props
- Using unclear or ambiguous label text
- Missing keyboard accessibility support
- Not handling toggle state changes appropriately
- Using complex logic inside toggle change handlers
- Forgetting to bind v-model for two-way data binding
- Not testing with screen readers for accessibility

### Accessibility Considerations
- Always provide descriptive label text for screen readers
- Ensure sufficient color contrast for toggle states
- Support keyboard navigation (Space key to toggle)
- Test with screen readers to verify proper announcements
- Use semantic HTML structure with actual checkbox inputs
- Provide clear visual indication of current state
- Handle focus management appropriately

### State Management
- Use reactive properties for toggle values
- Handle state changes with appropriate side effects
- Coordinate toggle states with application logic
- Persist important toggle states (localStorage, database)
- Handle state synchronization across components
- Provide default values for all toggle states

### Visual Design
- Use consistent toggle styling across the application
- Provide clear visual feedback for active/inactive states
- Handle smooth transitions between states with CSS
- Consider responsive design for different screen sizes
- Use appropriate colors for different toggle types
- Coordinate with overall design system

### Form Integration
- Group related toggles in logical sections
- Provide form validation for required toggles
- Handle form submission with toggle states
- Clear toggle states appropriately during form reset
- Coordinate with other form elements
- Handle form accessibility requirements

### Performance Considerations
- Minimize re-renders when toggle states don't change
- Use computed properties for derived toggle states
- Handle large numbers of toggles efficiently
- Optimize toggle change handler performance
- Implement proper cleanup for watchers

### User Experience
- Provide immediate visual feedback for toggle changes
- Use consistent toggle behavior across the application
- Handle toggle confirmations for critical actions
- Provide undo functionality for important toggles
- Group toggles logically for user comprehension
- Use helpful label text that explains toggle effects

### Value Type Management
- Use Boolean values for simple true/false states
- Use String values for named state options
- Use Number values for numeric toggle states
- Handle type conversion appropriately
- Validate toggle values when necessary
- Provide type-safe default values

## Component Registration
The component is registered as `codex-switch-toggle` in the application. 