# TabWrapper Component

## Overview
The TabWrapper component provides a simple container for wrapping tab content with optional before/after slots and support for dynamic child component rendering. It serves as a lightweight wrapper that can render a default component or fall back to slot content, making it ideal for flexible tab content layouts and component composition patterns.

## Basic Usage
```vue
<template>
  <div class="tab-wrapper-example">
    <codex-tab-wrapper 
      :class-name="'my-tab-content'"
      :default-slot="{ component: 'WelcomeComponent', props: { title: 'Hello' } }"
    >
      <template #before>
        <div class="tab-header">Header content</div>
      </template>
      
      <template #after>
        <div class="tab-footer">Footer content</div>
      </template>
    </codex-tab-wrapper>
  </div>
</template>
```

## Key Features
- Flexible slot-based content structure
- Dynamic component rendering support
- Child component array processing
- CSS class name customization
- Before and after content slots
- Component prop passing and merging
- Lightweight and performant

## Configuration Props

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `className` | `String` | `''` | CSS class name for the wrapper |

### Component Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `defaultSlot` | `Object` | `null` | Default component configuration |
| `childComponents` | `Array` | `[]` | Array of child components to render |

## Component Configuration Structure

### Default Slot Object
```javascript
{
  component: 'ComponentName', // Component name or reference
  props: {                   // Props to pass to the component
    prop1: 'value1',
    prop2: 'value2'
  }
}
```

### Child Components Array
```javascript
[
  {
    id: 'unique-id',         // Optional unique identifier
    component: 'ComponentA', // Component name or reference
    props: {                // Props to pass to the component
      prop1: 'value1'
    }
  },
  {
    component: 'ComponentB',
    props: {
      prop2: 'value2'
    }
  }
]
```

## Slots

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `before` | N/A | Content rendered before main content |
| `default` | N/A | Fallback content when no defaultSlot provided |
| `after` | N/A | Content rendered after main content |

## Prop Inheritance

The component inherits all attributes and passes them to child components:
- Attributes are merged with component-specific props
- Child component props take precedence over inherited attributes
- All inherited attributes are available to rendered components

## Examples

### Basic Tab Wrapper with Slots
```vue
<template>
  <div class="basic-wrapper">
    <codex-tab-wrapper class-name="content-wrapper">
      <template #before>
        <div class="content-header">
          <h2>Tab Content Header</h2>
          <div class="header-actions">
            <button @click="refreshContent">Refresh</button>
            <button @click="exportContent">Export</button>
          </div>
        </div>
      </template>
      
      <div class="main-content">
        <p>This is the main tab content rendered in the default slot.</p>
        <div class="content-grid">
          <div class="content-item">Item 1</div>
          <div class="content-item">Item 2</div>
          <div class="content-item">Item 3</div>
        </div>
      </div>
      
      <template #after>
        <div class="content-footer">
          <div class="footer-info">
            <span>Last updated: {{ lastUpdated }}</span>
          </div>
          <div class="footer-actions">
            <button @click="saveContent">Save Changes</button>
          </div>
        </div>
      </template>
    </codex-tab-wrapper>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const lastUpdated = ref(new Date().toLocaleString())

const refreshContent = () => {
  lastUpdated.value = new Date().toLocaleString()
  console.log('Content refreshed')
}

const exportContent = () => {
  console.log('Exporting content')
}

const saveContent = () => {
  console.log('Saving content changes')
}
</script>

<style scoped>
.content-wrapper {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
}

.content-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem;
  background: #f8f9fa;
  border-bottom: 1px solid #dee2e6;
}

.header-actions {
  display: flex;
  gap: 0.5rem;
}

.header-actions button {
  padding: 0.5rem 1rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.main-content {
  padding: 2rem;
}

.content-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
}

.content-item {
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 6px;
  text-align: center;
}

.content-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
  background: #f8f9fa;
  border-top: 1px solid #dee2e6;
}

.footer-info {
  font-size: 0.875rem;
  color: #6c757d;
}

.footer-actions button {
  padding: 0.5rem 1rem;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

### Dynamic Component Rendering
```vue
<template>
  <div class="dynamic-wrapper">
    <codex-tab-wrapper 
      :class-name="'dynamic-content'"
      :default-slot="primaryComponent"
      :child-components="additionalComponents"
      :custom-data="customData"
      :theme="selectedTheme"
    >
      <template #before>
        <div class="component-controls">
          <h3>Dynamic Component Demo</h3>
          
          <div class="controls-row">
            <label>
              Primary Component:
              <select v-model="selectedPrimary" @change="updatePrimaryComponent">
                <option value="dashboard">Dashboard</option>
                <option value="analytics">Analytics</option>
                <option value="settings">Settings</option>
                <option value="profile">Profile</option>
              </select>
            </label>
            
            <label>
              Theme:
              <select v-model="selectedTheme">
                <option value="light">Light</option>
                <option value="dark">Dark</option>
                <option value="auto">Auto</option>
              </select>
            </label>
          </div>
          
          <div class="component-toggles">
            <label>
              <input 
                v-model="showSidebar" 
                type="checkbox"
                @change="updateChildComponents"
              >
              Show Sidebar
            </label>
            
            <label>
              <input 
                v-model="showStats" 
                type="checkbox"
                @change="updateChildComponents"
              >
              Show Statistics
            </label>
            
            <label>
              <input 
                v-model="showNotifications" 
                type="checkbox"
                @change="updateChildComponents"
              >
              Show Notifications
            </label>
          </div>
        </div>
      </template>
      
      <template #after>
        <div class="component-info">
          <div class="info-section">
            <h4>Rendered Components</h4>
            <ul>
              <li>Primary: {{ primaryComponent?.component || 'None' }}</li>
              <li v-for="child in additionalComponents" :key="child.id">
                Child: {{ child.component }}
              </li>
            </ul>
          </div>
          
          <div class="info-section">
            <h4>Current Props</h4>
            <pre>{{ JSON.stringify(currentProps, null, 2) }}</pre>
          </div>
        </div>
      </template>
    </codex-tab-wrapper>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const selectedPrimary = ref('dashboard')
const selectedTheme = ref('light')
const showSidebar = ref(true)
const showStats = ref(false)
const showNotifications = ref(true)

const customData = ref({
  userId: 123,
  timestamp: Date.now(),
  environment: 'development'
})

const primaryComponent = computed(() => ({
  component: `${selectedPrimary.value}Component`,
  props: {
    theme: selectedTheme.value,
    customData: customData.value,
    title: `${selectedPrimary.value.charAt(0).toUpperCase() + selectedPrimary.value.slice(1)} View`
  }
}))

const additionalComponents = computed(() => {
  const components = []
  
  if (showSidebar.value) {
    components.push({
      id: 'sidebar',
      component: 'SidebarComponent',
      props: {
        collapsed: false,
        theme: selectedTheme.value
      }
    })
  }
  
  if (showStats.value) {
    components.push({
      id: 'stats',
      component: 'StatisticsComponent',
      props: {
        refreshInterval: 30000,
        showCharts: true
      }
    })
  }
  
  if (showNotifications.value) {
    components.push({
      id: 'notifications',
      component: 'NotificationCenter',
      props: {
        position: 'top-right',
        maxVisible: 5
      }
    })
  }
  
  return components
})

const currentProps = computed(() => ({
  primary: primaryComponent.value?.props,
  children: additionalComponents.value.map(c => ({ 
    component: c.component, 
    props: c.props 
  }))
}))

const updatePrimaryComponent = () => {
  console.log('Primary component changed to:', selectedPrimary.value)
}

const updateChildComponents = () => {
  console.log('Child components updated:', additionalComponents.value.map(c => c.component))
}
</script>

<style scoped>
.dynamic-content {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  margin: 1rem 0;
}

.component-controls {
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.controls-row {
  display: flex;
  gap: 2rem;
  margin: 1rem 0;
}

.controls-row label {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.controls-row select {
  padding: 0.5rem;
  border: none;
  border-radius: 4px;
}

.component-toggles {
  display: flex;
  gap: 1.5rem;
  margin-top: 1rem;
}

.component-toggles label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
}

.component-info {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  padding: 1.5rem;
  background: #f8f9fa;
  border-top: 1px solid #dee2e6;
}

.info-section h4 {
  margin: 0 0 1rem 0;
  color: #495057;
}

.info-section ul {
  list-style: none;
  padding: 0;
}

.info-section li {
  padding: 0.25rem 0;
  border-bottom: 1px solid #dee2e6;
}

.info-section pre {
  background: white;
  padding: 1rem;
  border-radius: 4px;
  font-size: 0.75rem;
  overflow-x: auto;
  max-height: 200px;
  overflow-y: auto;
}
</style>
```

### Conditional Component Wrapper
```vue
<template>
  <div class="conditional-wrapper">
    <div class="wrapper-controls">
      <h3>Conditional Component Rendering</h3>
      
      <div class="condition-controls">
        <label>
          User Role:
          <select v-model="userRole">
            <option value="guest">Guest</option>
            <option value="user">User</option>
            <option value="admin">Admin</option>
            <option value="super">Super Admin</option>
          </select>
        </label>
        
        <label>
          Feature Flags:
          <div class="checkbox-group">
            <label v-for="flag in availableFlags" :key="flag">
              <input 
                v-model="enabledFlags" 
                :value="flag"
                type="checkbox"
              >
              {{ flag }}
            </label>
          </div>
        </label>
      </div>
    </div>
    
    <codex-tab-wrapper 
      :class-name="wrapperClass"
      :default-slot="conditionalComponent"
      :child-components="conditionalChildren"
    >
      <template #before>
        <div class="access-header">
          <div class="user-badge" :class="userRole">
            <i :class="getUserIcon(userRole)"></i>
            <span>{{ userRole.toUpperCase() }}</span>
          </div>
          
          <div class="feature-status">
            <span v-for="flag in enabledFlags" :key="flag" class="flag-badge">
              {{ flag }}
            </span>
          </div>
        </div>
      </template>
      
      <div v-if="!conditionalComponent" class="access-denied">
        <div class="denied-icon">
          <i class="ri-lock-line"></i>
        </div>
        <h3>Access Restricted</h3>
        <p>You don't have permission to view this content.</p>
        <p>Required role: {{ getRequiredRole() }}</p>
      </div>
      
      <template #after>
        <div class="permissions-footer">
          <div class="permission-info">
            <strong>Current Permissions:</strong>
            <ul>
              <li v-for="permission in getUserPermissions()" :key="permission">
                {{ permission }}
              </li>
            </ul>
          </div>
          
          <div class="upgrade-prompt" v-if="canUpgrade()">
            <button @click="requestUpgrade" class="upgrade-btn">
              Request Higher Access
            </button>
          </div>
        </div>
      </template>
    </codex-tab-wrapper>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const userRole = ref('guest')
const availableFlags = ['beta_features', 'advanced_analytics', 'bulk_operations', 'api_access']
const enabledFlags = ref(['beta_features'])

const wrapperClass = computed(() => `role-${userRole.value} ${enabledFlags.value.join(' ')}`)

const conditionalComponent = computed(() => {
  const roleHierarchy = ['guest', 'user', 'admin', 'super']
  const requiredRole = 'user'
  const requiredRoleIndex = roleHierarchy.indexOf(requiredRole)
  const currentRoleIndex = roleHierarchy.indexOf(userRole.value)
  
  if (currentRoleIndex >= requiredRoleIndex) {
    return {
      component: getComponentForRole(userRole.value),
      props: {
        userRole: userRole.value,
        permissions: getUserPermissions(),
        featureFlags: enabledFlags.value
      }
    }
  }
  
  return null
})

const conditionalChildren = computed(() => {
  const children = []
  
  // Add analytics component for admin+
  if (['admin', 'super'].includes(userRole.value) && enabledFlags.value.includes('advanced_analytics')) {
    children.push({
      id: 'analytics',
      component: 'AdvancedAnalytics',
      props: {
        level: userRole.value === 'super' ? 'full' : 'limited'
      }
    })
  }
  
  // Add bulk operations for power users
  if (enabledFlags.value.includes('bulk_operations') && userRole.value !== 'guest') {
    children.push({
      id: 'bulk-ops',
      component: 'BulkOperations',
      props: {
        maxOperations: userRole.value === 'super' ? 1000 : 100
      }
    })
  }
  
  return children
})

const getComponentForRole = (role) => {
  const componentMap = {
    'guest': 'GuestDashboard',
    'user': 'UserDashboard', 
    'admin': 'AdminDashboard',
    'super': 'SuperAdminDashboard'
  }
  return componentMap[role] || 'GuestDashboard'
}

const getUserIcon = (role) => {
  const iconMap = {
    'guest': 'ri-user-line',
    'user': 'ri-user-fill',
    'admin': 'ri-admin-line',
    'super': 'ri-vip-crown-line'
  }
  return iconMap[role] || 'ri-user-line'
}

const getUserPermissions = () => {
  const permissionMap = {
    'guest': ['view_public'],
    'user': ['view_public', 'view_profile', 'edit_profile'],
    'admin': ['view_public', 'view_profile', 'edit_profile', 'manage_users', 'view_analytics'],
    'super': ['all_permissions']
  }
  return permissionMap[userRole.value] || []
}

const getRequiredRole = () => 'User'

const canUpgrade = () => userRole.value === 'guest'

const requestUpgrade = () => {
  console.log('Requesting access upgrade')
}
</script>

<style scoped>
.conditional-wrapper {
  max-width: 800px;
  margin: 0 auto;
}

.wrapper-controls {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.condition-controls {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 2rem;
  margin-top: 1rem;
}

.checkbox-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-top: 0.5rem;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin: 0;
}

.access-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: white;
  border-bottom: 1px solid #dee2e6;
}

.user-badge {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-weight: bold;
  font-size: 0.875rem;
}

.user-badge.guest { background: #6c757d; color: white; }
.user-badge.user { background: #007bff; color: white; }
.user-badge.admin { background: #28a745; color: white; }
.user-badge.super { background: #ffc107; color: #212529; }

.feature-status {
  display: flex;
  gap: 0.5rem;
}

.flag-badge {
  padding: 0.25rem 0.75rem;
  background: #e9ecef;
  border-radius: 12px;
  font-size: 0.75rem;
  color: #495057;
}

.access-denied {
  text-align: center;
  padding: 4rem 2rem;
  color: #6c757d;
}

.denied-icon {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.permissions-footer {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 1rem;
  background: #f8f9fa;
  border-top: 1px solid #dee2e6;
}

.permission-info ul {
  list-style: none;
  padding: 0;
  margin: 0.5rem 0 0 0;
}

.permission-info li {
  padding: 0.25rem 0;
  font-size: 0.875rem;
}

.upgrade-btn {
  padding: 0.75rem 1.5rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
}
</style>
```

## Use Cases

### Content Wrapping
- Adding consistent headers and footers to tab content
- Implementing common layout patterns
- Providing context-aware content containers

### Component Composition
- Combining multiple components in a single tab
- Creating flexible component layouts
- Managing component hierarchies

### Conditional Rendering
- Role-based content display
- Feature flag implementations
- Dynamic component loading

## Best Practices

### Component Organization
- Keep wrapper logic simple and focused
- Use meaningful class names for styling
- Separate presentation from business logic
- Document component prop requirements
- Test with various component combinations

### Performance Considerations
- Avoid deep component nesting
- Use component references instead of strings when possible
- Implement proper key attributes for dynamic components
- Monitor component mounting/unmounting
- Cache component configurations when appropriate

### Accessibility
- Ensure proper semantic structure
- Provide meaningful labels and descriptions
- Support keyboard navigation
- Test with screen readers
- Handle focus management appropriately

### Error Handling
- Implement fallbacks for missing components
- Handle invalid prop configurations
- Provide meaningful error messages
- Log component resolution issues
- Test with edge cases

### Styling
- Use CSS classes for consistent theming
- Implement responsive design patterns
- Consider component-specific styling needs
- Test with various content types
- Ensure visual consistency

## Component Registration
```javascript
// Global registration
app.component('CodexTabWrapper', TabWrapper)

// Local registration  
import TabWrapper from '@/components/molecules/TabWrapper.vue'

export default {
  components: {
    CodexTabWrapper: TabWrapper
  }
}
```

## Internationalization

The `TabWrapper` component has minimal direct translation requirements as it primarily serves as a layout container for dynamic component rendering.

### Translation Requirements

**No Direct Translation Keys**: The `TabWrapper` component itself does not use any `$t()` or translation functions.

### Translation Delegation

The component renders child components dynamically, which may have their own translation requirements:

```vue
<!-- Default slot content -->
<component
    v-if="defaultSlot?.component"
    :is="defaultSlot.component"
    v-bind="{ ...$attrs, ...defaultSlot.props }">
</component>

<!-- Child components -->
<component
    v-for="child in childComponents"
    :key="child.id || child.component"
    :is="child.component"
    v-bind="{ ...$attrs, ...child.props }">
</component>
```

### Props Structure

The component receives component configuration through props:

```javascript
defaultSlot: {
    type: Object,
    default: null
},
childComponents: {
    type: Array,
    default: () => []
}
```

### Notes
- All translation handling is delegated to the rendered child components
- Any translation props should be passed through the component props configuration
- The wrapper focuses on component orchestration rather than content display
- CSS class names for styling are configurable through the `className` prop
- Parent components using `TabWrapper` are responsible for providing translated content through component props
``` 