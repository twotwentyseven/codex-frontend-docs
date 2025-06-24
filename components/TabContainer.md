# TabContainer Component

## Overview
The TabContainer component serves as a high-level wrapper around the TabSystem, providing route-based tab configuration and automatic component resolution. It bridges the gap between Vue Router configurations and tab interfaces, automatically converting route definitions into tab structures while handling customer authentication and component lifecycle management.

## Basic Usage
```vue
<template>
  <div class="tab-container-wrapper">
    <codex-tab-container 
      :layout="'vertical'"
      :use-router="true"
      :show-customers-only="false"
    />
  </div>
</template>
```

## Key Features
- Automatic route-to-tab conversion
- Vue Router integration with path-based navigation
- Component resolution from multiple sources
- Customer authentication integration
- Flexible configuration through data attributes or props
- Support for nested routes and child components
- Tab preset integration
- Dynamic component loading

## Configuration Props

### Authentication Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showCustomersOnly` | `Boolean` | `false` | Restrict content to authenticated users |

### Tab Definition Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `tabs` | `Array` | `[]` | Direct tab configuration array |
| `defaultTab` | `String` | `null` | Default tab identifier |

### Router Configuration Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `useRouter` | `Boolean` | `undefined` | Enable Vue Router integration |
| `disableRouting` | `Boolean` | `undefined` | Disable router navigation |
| `routerMode` | `String` | `undefined` | Router mode ('hash' or 'history') |

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'vertical'` | Tab layout orientation |
| `fullScreen` | `Boolean` | `false` | Enable full-screen mode |

## Configuration Sources

The component merges configuration from multiple sources with priority order:

1. **Props** (highest priority)
2. **Data attributes** (`data-codex-router-*`)
3. **Window configuration** (`window.codex.config.tabs`)
4. **Injected settings** (lowest priority)

### Data Attribute Configuration
```html
<div 
  data-codex-router="true"
  data-codex-router-mode="history"
  data-codex-router-routes='[{"path": "/dashboard", "component": "Dashboard"}]'
  data-codex-disable-routing="false"
>
  <codex-tab-container />
</div>
```

### Window Configuration
```javascript
window.codex = {
  config: {
    tabs: {
      useRouter: true,
      routerMode: 'history',
      routes: [...],
      disableRouting: false
    }
  }
}
```

## Route Processing

Routes are automatically converted to tab configurations:

```javascript
// Route definition
{
  path: '/dashboard',
  component: 'DashboardComponent',
  meta: {
    label: 'Dashboard',
    icon: 'ri-dashboard-line',
    labelComponent: 'CustomLabel',
    labelProps: { count: 5 }
  }
}

// Converted tab
{
  id: 'dashboard',
  label: 'Dashboard',
  icon: 'ri-dashboard-line',
  component: DashboardComponent,
  labelComponent: CustomLabel,
  labelProps: { count: 5 }
}
```

## Component Resolution

The container resolves components from the global registry:

```javascript
// Component registration
window.codex.componentRegistry.set('codex-dashboard', DashboardComponent)

// Resolution attempts multiple naming conventions
const possibleNames = [
  'DashboardComponent',
  'codex-dashboard-component',
  'dashboard_component',
  'dashboard'
]
```

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `tab-change` | `tabId` | Emitted when active tab changes |

## Slots

The component forwards all slots to the underlying TabSystem:

### Navigation Slots
| Slot | Props | Description |
|------|-------|-------------|
| `nav-container-start` | `{ activeTab, tabs }` | Before navigation container |
| `nav-container-end` | `{ activeTab, tabs }` | After navigation container |
| `nav-start` | `{ activeTab, tabs }` | Before navigation tabs |
| `nav-end` | `{ activeTab, tabs }` | After navigation tabs |
| `tab` | `{ tab, active }` | Custom tab button content |

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `header` | N/A | Header content area |
| `content` | N/A | Main content area |
| `footer` | N/A | Footer content area |

## Internationalization

The `TabContainer` component has minimal direct translation requirements as it primarily serves as a routing and orchestration layer for tab systems.

### Translation Requirements

**No Direct Translation Keys**: The `TabContainer` component itself does not use any `$t()` or translation functions directly.

### Translation Delegation

The component manages tab content through dynamic component rendering:

```vue
<component 
    :is="tab.component" 
    v-if="tab.component"
    v-bind="{ 
        ...tab.props, 
        ...tab.meta,
        childComponents: processChildComponents(tab.meta?.childComponents || []),
        onTabChange: handleTabChange
    }">
</component>
```

### Tab Configuration for Translations

When configuring tabs, translation props should be included in the tab configuration:

```javascript
// Example tab configuration with translations
const tabs = [
    {
        id: 'overview',
        component: 'codex-tab-content',
        props: {
            title: 'account.overview_title',
            description: 'account.overview_description'
        },
        meta: {
            childComponents: [
                {
                    component: 'codex-bookings-completed',
                    props: {
                        title: 'booking.completed_title'
                    }
                }
            ]
        }
    }
];
```

### Integration with Translation System

The component integrates with Vue i18n through the `useI18n` composable:

```javascript
import { useI18n } from 'vue-i18n';
const { t } = useI18n();
```

### Customer Context for Translations

Provides customer context for parameterized translations in child components:

```javascript
import useCustomer from '@/composition/useCustomer';
const { customer, loadCustomer } = useCustomer();
```

### Notes
- All translation handling is delegated to rendered child components
- Tab labels and content translations managed through tab configuration
- Router integration supports localized routes through configuration
- Customer data provided as context for dynamic translations
- Uses `useTabPresets` for predefined component configurations with translation support

## Examples

### Basic Route-Based Tabs
```vue
<template>
  <div class="route-tabs">
    <codex-tab-container 
      :use-router="true"
      :layout="'horizontal'"
      :router-mode="'history'"
    />
  </div>
</template>

<script setup>
import { provide } from 'vue'

// Provide route configuration
provide('tabSystem.tabs', computed(() => [
  {
    id: 'home',
    label: 'Home',
    path: '/home',
    component: 'HomePage',
    icon: 'ri-home-line'
  },
  {
    id: 'about',
    label: 'About',
    path: '/about',
    component: 'AboutPage',
    icon: 'ri-information-line'
  },
  {
    id: 'contact',
    label: 'Contact',
    path: '/contact',
    component: 'ContactPage',
    icon: 'ri-mail-line'
  }
]))
</script>
```

### Customer-Only Tabs with Custom Layout
```vue
<template>
  <div class="customer-tabs">
    <codex-tab-container 
      :show-customers-only="true"
      :layout="'vertical'"
      :full-screen="true"
      :default-tab="'dashboard'"
    >
      <template #nav-container-start="{ activeTab, tabs }">
        <div class="customer-welcome">
          <h2>Welcome, {{ customer?.first_name }}!</h2>
          <p>Current section: {{ getTabLabel(activeTab, tabs) }}</p>
        </div>
      </template>
      
      <template #nav-end="{ activeTab, tabs }">
        <button @click="logout" class="logout-btn">
          <i class="ri-logout-box-line"></i>
          Logout
        </button>
      </template>
      
      <template #header>
        <div class="content-header">
          <div class="breadcrumb">
            <span>Account</span>
            <i class="ri-arrow-right-s-line"></i>
            <span>{{ currentSectionName }}</span>
          </div>
          
          <div class="header-actions">
            <button @click="refreshData" class="refresh-btn">
              <i class="ri-refresh-line"></i>
            </button>
            <button @click="openSettings" class="settings-btn">
              <i class="ri-settings-line"></i>
            </button>
          </div>
        </div>
      </template>
      
      <template #footer>
        <div class="content-footer">
          <div class="footer-info">
            <span>Last updated: {{ lastUpdated }}</span>
          </div>
          
          <div class="footer-actions">
            <button @click="exportData" class="export-btn">
              Export Data
            </button>
            <button @click="openHelp" class="help-btn">
              Get Help
            </button>
          </div>
        </div>
      </template>
    </codex-tab-container>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import useCustomer from '@/composition/useCustomer'

const { customer } = useCustomer()

const lastUpdated = ref(new Date().toLocaleString())
const currentSectionName = ref('Dashboard')

const getTabLabel = (activeTabId, tabs) => {
  return tabs.find(tab => tab.id === activeTabId)?.label || 'Unknown'
}

const logout = () => {
  console.log('Logging out user')
}

const refreshData = () => {
  lastUpdated.value = new Date().toLocaleString()
  console.log('Refreshing data')
}

const openSettings = () => {
  console.log('Opening settings modal')
}

const exportData = () => {
  console.log('Exporting user data')
}

const openHelp = () => {
  console.log('Opening help center')
}
</script>

<style scoped>
.customer-welcome {
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.logout-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1rem;
  background: #dc3545;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin-top: 1rem;
}

.content-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: white;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}

.breadcrumb {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #6c757d;
}

.header-actions {
  display: flex;
  gap: 0.5rem;
}

.refresh-btn,
.settings-btn {
  padding: 0.5rem;
  background: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  cursor: pointer;
}

.content-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 6px;
  margin-top: 2rem;
}

.footer-info {
  color: #6c757d;
  font-size: 0.875rem;
}

.footer-actions {
  display: flex;
  gap: 1rem;
}

.export-btn,
.help-btn {
  padding: 0.5rem 1rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

### Dynamic Route Configuration
```vue
<template>
  <div class="dynamic-tabs">
    <div class="configuration-panel">
      <h3>Route Configuration</h3>
      
      <div class="config-options">
        <label>
          <input 
            v-model="routerConfig.useRouter" 
            type="checkbox"
          >
          Enable Router
        </label>
        
        <label>
          <input 
            v-model="routerConfig.disableRouting" 
            type="checkbox"
          >
          Disable Routing
        </label>
        
        <label>
          Router Mode:
          <select v-model="routerConfig.routerMode">
            <option value="history">History</option>
            <option value="hash">Hash</option>
          </select>
        </label>
        
        <label>
          Layout:
          <select v-model="layoutConfig.layout">
            <option value="vertical">Vertical</option>
            <option value="horizontal">Horizontal</option>
          </select>
        </label>
        
        <label>
          <input 
            v-model="layoutConfig.fullScreen" 
            type="checkbox"
          >
          Full Screen
        </label>
      </div>
      
      <button @click="addCustomRoute" class="add-route-btn">
        Add Custom Route
      </button>
    </div>
    
    <codex-tab-container 
      v-bind="mergedConfig"
      :tabs="dynamicTabs"
      @tab-change="handleTabChange"
    >
      <template #tab="{ tab, active }">
        <div class="custom-tab" :class="{ active }">
          <i v-if="tab.icon" :class="tab.icon"></i>
          <span>{{ tab.label }}</span>
          <button 
            v-if="tab.removable"
            @click.stop="removeTab(tab.id)"
            class="remove-tab"
          >
            <i class="ri-close-line"></i>
          </button>
        </div>
      </template>
    </codex-tab-container>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const routerConfig = ref({
  useRouter: true,
  disableRouting: false,
  routerMode: 'history'
})

const layoutConfig = ref({
  layout: 'vertical',
  fullScreen: false
})

const dynamicTabs = ref([
  {
    id: 'overview',
    label: 'Overview',
    icon: 'ri-dashboard-line',
    component: 'OverviewComponent',
    removable: false
  },
  {
    id: 'analytics',
    label: 'Analytics',
    icon: 'ri-bar-chart-line',
    component: 'AnalyticsComponent',
    removable: false
  }
])

const mergedConfig = computed(() => ({
  ...routerConfig.value,
  ...layoutConfig.value
}))

const handleTabChange = (tabId) => {
  console.log('Active tab changed to:', tabId)
}

const addCustomRoute = () => {
  const newId = `custom-${Date.now()}`
  dynamicTabs.value.push({
    id: newId,
    label: `Custom Tab ${dynamicTabs.value.length}`,
    icon: 'ri-add-line',
    component: 'CustomComponent',
    removable: true
  })
}

const removeTab = (tabId) => {
  const index = dynamicTabs.value.findIndex(tab => tab.id === tabId)
  if (index > -1) {
    dynamicTabs.value.splice(index, 1)
  }
}
</script>

<style scoped>
.configuration-panel {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  margin-bottom: 2rem;
}

.config-options {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin: 1rem 0;
}

.config-options label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.add-route-btn {
  padding: 0.75rem 1.5rem;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.custom-tab {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  position: relative;
}

.remove-tab {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.25rem;
  color: #dc3545;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.remove-tab:hover {
  background: #f8f9fa;
}
</style>
```

## Router Integration Patterns

### History Mode Navigation
```vue
<template>
  <codex-tab-container 
    :use-router="true"
    :router-mode="'history'"
    :disable-routing="false"
  />
</template>
```

### Hash Mode Navigation
```vue
<template>
  <codex-tab-container 
    :use-router="true"
    :router-mode="'hash'"
  />
</template>
```

### Disabled Routing (State Only)
```vue
<template>
  <codex-tab-container 
    :use-router="false"
    :disable-routing="true"
  />
</template>
```

## Component Lifecycle

The TabContainer manages component lifecycle through:

1. **Route Processing** - Converts routes to tab configurations
2. **Component Resolution** - Resolves component references
3. **Provider Setup** - Provides configuration to child components
4. **Navigation Handling** - Manages tab changes and routing
5. **State Synchronization** - Keeps URL and tab state in sync

## Best Practices

### Configuration Management
- Use props for static configuration
- Use data attributes for server-side rendering
- Use window configuration for global settings
- Test configuration precedence thoroughly
- Document configuration sources clearly

### Route Organization
- Structure routes logically for tab conversion
- Use meaningful route metadata
- Implement proper component lazy loading
- Handle route guards appropriately
- Test navigation edge cases

### Component Resolution
- Register components consistently
- Use descriptive component names
- Handle missing components gracefully
- Implement proper error boundaries
- Test component loading scenarios

### Performance Optimization
- Implement lazy loading for route components
- Cache resolved component references
- Avoid unnecessary route processing
- Monitor component mounting/unmounting
- Optimize for rapid navigation

### Accessibility
- Ensure proper navigation semantics
- Support keyboard navigation
- Provide meaningful labels
- Handle focus management
- Test with assistive technologies

### Error Handling
- Implement fallbacks for missing routes
- Handle component resolution failures
- Provide meaningful error messages
- Log configuration issues
- Test with invalid configurations

## Component Registration
```javascript
// Global registration
app.component('CodexTabContainer', TabContainer)

// Local registration  
import TabContainer from '@/components/molecules/TabContainer.vue'

export default {
  components: {
    CodexTabContainer: TabContainer
  }
}
```

## Internationalization

The `TabContainer` component has minimal direct translation requirements as it primarily serves as a routing and orchestration layer for tab systems.

### Translation Requirements

**No Direct Translation Keys**: The `TabContainer` component itself does not use any `$t()` or translation functions directly.

### Translation Delegation

The component manages tab content through dynamic component rendering:

```vue
<component 
    :is="tab.component" 
    v-if="tab.component"
    v-bind="{ 
        ...tab.props, 
        ...tab.meta,
        childComponents: processChildComponents(tab.meta?.childComponents || []),
        onTabChange: handleTabChange
    }">
</component>
```

### Tab Configuration for Translations

When configuring tabs, translation props should be included in the tab configuration:

```javascript
// Example tab configuration with translations
const tabs = [
    {
        id: 'overview',
        component: 'codex-tab-content',
        props: {
            title: 'account.overview_title',
            description: 'account.overview_description'
        },
        meta: {
            childComponents: [
                {
                    component: 'codex-bookings-completed',
                    props: {
                        title: 'booking.completed_title'
                    }
                }
            ]
        }
    }
];
```

### Integration with Translation System

The component integrates with Vue i18n through the `useI18n` composable:

```javascript
import { useI18n } from 'vue-i18n';
const { t } = useI18n();
```

### Customer Context for Translations

Provides customer context for parameterized translations in child components:

```javascript
import useCustomer from '@/composition/useCustomer';
const { customer, loadCustomer } = useCustomer();
```

### Notes
- All translation handling is delegated to rendered child components
- Tab labels and content translations managed through tab configuration
- Router integration supports localized routes through configuration
- Customer data provided as context for dynamic translations
- Uses `useTabPresets` for predefined component configurations with translation support 