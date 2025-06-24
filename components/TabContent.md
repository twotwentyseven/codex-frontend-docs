# TabContent Component

## Overview
The TabContent component serves as a dynamic content renderer for tab systems, automatically resolving and rendering child components based on configuration presets or explicit component definitions. It integrates with customer authentication, translation patterns, and tab preset systems to provide context-aware content rendering with reactive updates and proper state management.

## Basic Usage
```vue
<template>
  <div class="tab-content-wrapper">
    <codex-tab-content 
      :preset="'accountOverview'"
      :on-tab-change="handleTabChange"
    />
  </div>
</template>

<script setup>
const handleTabChange = (tabId) => {
  console.log('Tab changed to:', tabId)
}
</script>
```

## Key Features
- Dynamic component resolution and rendering
- Integration with preset configuration system
- Customer context-aware content display
- Translation pattern processing
- Reactive component property binding
- Header, content, and footer slot support
- Tab change event handling
- Component lifecycle management

## Configuration Props

### Component Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `childComponents` | `Array` | `[]` | Array of component definitions to render |
| `preset` | `String` | `'accountOverview'` | Preset configuration identifier |

### Function Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `onTabChange` | `Function` | `undefined` | Callback function for tab change events |

## Computed Properties

### Component Resolution
- `resolvedComponents` - Processed array of components with resolved props
- `translationContext` - Reactive context for translation processing

## Component Definition Structure

Each component in the `childComponents` array should follow this structure:

```javascript
{
  component: 'ComponentName', // Component name or reference
  props: {                   // Component props (will be processed for translations)
    prop1: 'value1',
    prop2: '{{customer.first_name}}' // Translation patterns supported
  }
}
```

## Preset Integration

The component integrates with the preset system:

```javascript
import useTabPresets from '@/composition/useTabPresets'

const { getPreset } = useTabPresets()

// Presets define component configurations
const preset = getPreset('accountOverview')
```

## Translation Processing

Component props support translation patterns:

```javascript
// Props with translation patterns
{
  welcomeMessage: 'Hello {{customer.first_name}}!',
  accountType: '{{customer.account_type}}'
}

// Processed with customer context
{
  welcomeMessage: 'Hello John!',
  accountType: 'Premium'
}
```

## Slots

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `header` | `{ customer }` | Header content area |
| `content` | N/A | Main content area (overrides component rendering) |
| `footer` | `{ customer }` | Footer content area |

## Examples

### Basic Tab Content with Preset
```vue
<template>
  <div class="account-tabs">
    <codex-tab-content 
      :preset="'customerDashboard'"
      :on-tab-change="handleTabNavigation"
    >
      <template #header="{ customer }">
        <div class="dashboard-header">
          <h1>Welcome back, {{ customer?.first_name }}!</h1>
          <p>Last login: {{ formatDate(customer?.last_login_at) }}</p>
        </div>
      </template>
      
      <template #footer="{ customer }">
        <div class="dashboard-footer">
          <p>Account Status: {{ customer?.status }}</p>
          <button @click="contactSupport">Need Help?</button>
        </div>
      </template>
    </codex-tab-content>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n'

const { d } = useI18n()

const handleTabNavigation = (tabId) => {
  console.log('Navigating to tab:', tabId)
}

const formatDate = (date) => {
  return date ? d(new Date(date), 'short') : 'Never'
}

const contactSupport = () => {
  console.log('Opening support modal')
}
</script>

<style scoped>
.dashboard-header {
  text-align: center;
  padding: 2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 8px;
  margin-bottom: 2rem;
}

.dashboard-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 6px;
  margin-top: 2rem;
}
</style>
```

### Custom Child Components Configuration
```vue
<template>
  <div class="custom-tab-content">
    <codex-tab-content 
      :child-components="customComponents"
      :on-tab-change="onTabChange"
    >
      <template #header="{ customer }">
        <div class="custom-header">
          <div class="user-avatar">
            <img :src="customer?.avatar || '/default-avatar.png'" :alt="customer?.full_name">
          </div>
          <div class="user-info">
            <h2>{{ customer?.full_name }}</h2>
            <span class="user-role">{{ customer?.role }}</span>
          </div>
        </div>
      </template>
      
      <template #content>
        <!-- Custom content that overrides component rendering -->
        <div class="custom-content-override">
          <h3>Custom Content Area</h3>
          <p>This content replaces the dynamic component rendering.</p>
          
          <div class="manual-components">
            <component
              v-for="comp in manualComponents"
              :key="comp.id"
              :is="comp.component"
              v-bind="comp.props"
              @component-event="handleComponentEvent"
            />
          </div>
        </div>
      </template>
    </codex-tab-content>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import useCustomer from '@/composition/useCustomer'

const { customer } = useCustomer()

const customComponents = ref([
  {
    component: 'UserProfile',
    props: {
      title: 'Profile Settings',
      showAvatar: true,
      allowEditing: true,
      customMessage: 'Welcome {{customer.first_name}}!'
    }
  },
  {
    component: 'AccountSettings',
    props: {
      title: 'Account Configuration',
      showBilling: true,
      allowCancellation: '{{customer.account_type}}' === 'premium'
    }
  },
  {
    component: 'NotificationPreferences',
    props: {
      title: 'Notifications',
      emailEnabled: true,
      smsEnabled: false
    }
  }
])

const manualComponents = computed(() => [
  {
    id: 'stats',
    component: 'UserStatistics',
    props: {
      userId: customer.value?.id,
      showCharts: true
    }
  },
  {
    id: 'activity',
    component: 'RecentActivity', 
    props: {
      limit: 10,
      showTimestamps: true
    }
  }
])

const onTabChange = (tabId) => {
  console.log('Tab changed:', tabId)
}

const handleComponentEvent = (event) => {
  console.log('Component event:', event)
}
</script>

<style scoped>
.custom-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1.5rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

.user-avatar img {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  object-fit: cover;
}

.user-role {
  background: #007bff;
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.875rem;
}

.custom-content-override {
  padding: 2rem;
  background: #f8f9fa;
  border-radius: 8px;
}

.manual-components {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}
</style>
```

### Preset-Based Multi-Tab Content
```vue
<template>
  <div class="multi-tab-content">
    <div class="tab-navigation">
      <button 
        v-for="preset in presets"
        :key="preset.id"
        @click="switchPreset(preset)"
        :class="['tab-btn', { active: currentPreset === preset.id }]"
      >
        {{ preset.label }}
      </button>
    </div>
    
    <codex-tab-content 
      :preset="currentPreset"
      :on-tab-change="handlePresetTabChange"
      :key="currentPreset"
    >
      <template #header="{ customer }">
        <div class="preset-header">
          <h2>{{ getCurrentPresetLabel() }}</h2>
          <div class="customer-context" v-if="customer">
            <span>{{ customer.first_name }} {{ customer.last_name }}</span>
            <span class="customer-id">#{{ customer.id }}</span>
          </div>
        </div>
      </template>
      
      <template #footer="{ customer }">
        <div class="preset-footer">
          <div class="preset-info">
            <span>Preset: {{ currentPreset }}</span>
            <span>Components: {{ componentCount }}</span>
          </div>
          
          <div class="preset-actions">
            <button @click="refreshContent" class="refresh-btn">
              <i class="ri-refresh-line"></i>
              Refresh
            </button>
            <button @click="exportData" class="export-btn">
              <i class="ri-download-line"></i>
              Export
            </button>
          </div>
        </div>
      </template>
    </codex-tab-content>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import useTabPresets from '@/composition/useTabPresets'

const { getPreset } = useTabPresets()

const currentPreset = ref('overview')

const presets = [
  { id: 'overview', label: 'Overview' },
  { id: 'analytics', label: 'Analytics' },
  { id: 'settings', label: 'Settings' },
  { id: 'billing', label: 'Billing' }
]

const componentCount = computed(() => {
  const preset = getPreset(currentPreset.value)
  return preset?.length || 0
})

const switchPreset = (preset) => {
  currentPreset.value = preset.id
}

const getCurrentPresetLabel = () => {
  return presets.find(p => p.id === currentPreset.value)?.label || currentPreset.value
}

const handlePresetTabChange = (tabId) => {
  console.log('Preset tab change:', currentPreset.value, tabId)
}

const refreshContent = () => {
  console.log('Refreshing content for preset:', currentPreset.value)
  // Force re-render by changing key
  const currentId = currentPreset.value
  currentPreset.value = ''
  setTimeout(() => {
    currentPreset.value = currentId
  }, 50)
}

const exportData = () => {
  console.log('Exporting data for preset:', currentPreset.value)
}
</script>

<style scoped>
.tab-navigation {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
  border-bottom: 2px solid #eee;
}

.tab-btn {
  padding: 1rem 2rem;
  background: none;
  border: none;
  cursor: pointer;
  font-weight: 500;
  position: relative;
  transition: all 0.2s;
}

.tab-btn.active {
  color: #007bff;
}

.tab-btn.active::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  right: 0;
  height: 2px;
  background: #007bff;
}

.preset-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

.customer-context {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.25rem;
}

.customer-id {
  color: #6c757d;
  font-size: 0.875rem;
}

.preset-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 6px;
  margin-top: 2rem;
}

.preset-info {
  display: flex;
  gap: 1rem;
  font-size: 0.875rem;
  color: #6c757d;
}

.preset-actions {
  display: flex;
  gap: 0.5rem;
}

.refresh-btn,
.export-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.875rem;
  transition: all 0.2s;
}

.refresh-btn:hover,
.export-btn:hover {
  background: #f8f9fa;
  border-color: #007bff;
}
</style>
```

## Integration Patterns

### With Tab Systems
```vue
<template>
  <codex-tab-system>
    <template #dashboard>
      <codex-tab-content 
        :preset="'dashboardComponents'"
        :on-tab-change="onTabChange"
      />
    </template>
    
    <template #profile>
      <codex-tab-content 
        :preset="'profileComponents'"
        :on-tab-change="onTabChange"
      />
    </template>
  </codex-tab-system>
</template>
```

### With Customer Context
```vue
<template>
  <codex-tab-content 
    :child-components="customerSpecificComponents"
    :on-tab-change="onTabChange"
  />
</template>

<script setup>
import { computed } from 'vue'
import useCustomer from '@/composition/useCustomer'

const { customer } = useCustomer()

const customerSpecificComponents = computed(() => {
  if (!customer.value) return []
  
  const baseComponents = [
    { component: 'Welcome', props: { name: '{{customer.first_name}}' } }
  ]
  
  if (customer.value.is_premium) {
    baseComponents.push({
      component: 'PremiumFeatures',
      props: { level: '{{customer.subscription_level}}' }
    })
  }
  
  return baseComponents
})
</script>
```

## Best Practices

### Component Configuration
- Use descriptive preset names that reflect content purpose
- Keep component props simple and focused
- Leverage translation patterns for dynamic content
- Handle missing customer context gracefully
- Test component rendering with various data states

### Performance Optimization
- Use reactive refs for component arrays to avoid unnecessary updates
- Implement proper key attributes for component re-rendering
- Monitor component mounting/unmounting cycles
- Cache preset configurations when possible
- Avoid deep nesting of component configurations

### State Management
- Pass tab change handlers consistently
- Handle component lifecycle events properly
- Clean up component state on preset changes
- Maintain context consistency across components
- Implement proper error boundaries

### Translation Integration
- Use consistent translation pattern syntax
- Handle missing customer data in translation context
- Test translations with various customer states
- Provide fallback values for missing translations
- Document translation patterns used in presets

### Accessibility
- Ensure rendered components maintain accessibility standards
- Provide proper focus management
- Use semantic structure in slot content
- Test with assistive technologies
- Handle dynamic content changes appropriately

### Error Handling
- Implement fallback rendering for missing components
- Handle preset loading failures gracefully
- Provide meaningful error messages
- Log component resolution issues
- Test with invalid configurations

## Component Registration
```javascript
// Global registration
app.component('CodexTabContent', TabContent)

// Local registration  
import TabContent from '@/components/molecules/TabContent.vue'

export default {
  components: {
    CodexTabContent: TabContent
  }
}
```

## Internationalization

The `TabContent` component uses the translation pattern system for dynamic content translation and customer context integration.

### Translation Requirements

**No Direct Translation Keys**: The `TabContent` component itself does not contain hardcoded translation keys.

### Translation Pattern Integration

The component uses `useTranslationPattern` for processing dynamic translations in child components:

```javascript
import useTranslationPattern from '@/composition/useTranslationPattern';
const { translate, processTranslations } = useTranslationPattern();
```

### Dynamic Translation Processing

```javascript
// Creates reactive translation context
const translationContext = computed(() => ({
    customer: customer.value
}));

// Processes child component props with translations
const updateComponents = () => {
    resolvedComponents.value = components.map(child => ({
        component: child.component,
        props: {
            ...child.props ? processTranslations(child.props, translationContext.value) : {},
            onTabChange: props.onTabChange
        }
    }));
};
```

### Customer Context for Translations

Provides customer data for parameterized translations:

```vue
<slot name="header" :customer="customer"></slot>
<slot name="footer" :customer="customer"></slot>
```

### Child Component Translation Support

Child components receive processed translation props:

```vue
<template v-for="child in resolvedComponents" :key="child.component">
    <component
        :is="child.component"
        v-bind="child.props"
        :onTabChange="onTabChange">
    </component>
</template>
```

### Example Translation Configuration

```javascript
// Example child component with translation props
const childComponents = [
    {
        component: 'codex-available-credits',
        props: {
            title: 'credit.remaining_credits',
            description: 'credit.you_have_credits_remaining'
        }
    },
    {
        component: 'codex-bookings-completed',
        props: {
            title: 'booking.classes_completed',
            milestoneMessage: 'booking.next_milestone'
        }
    }
];
```

### Translation Pattern Features

- **Dynamic Context**: Customer data automatically injected into translation context
- **Reactive Updates**: Translations update when customer data changes
- **Preset Integration**: Supports predefined component configurations with translations
- **Parameterized Translations**: Customer-specific data in translation parameters

### Notes
- All visible text comes from child components using the processed translation props
- Translation context automatically includes customer data for personalization
- Supports both direct component configuration and preset-based setups
- Integration with `useTabPresets` for reusable translated component configurations 