# TabSystem Component

## Overview
The TabSystem component provides a comprehensive tabbed interface with support for both horizontal and vertical layouts, router integration, and flexible content management. It handles tab navigation, state management, URL synchronization, and provides extensive customization options through slots and props. The component can work with or without Vue Router and supports customer-only visibility restrictions.

## Basic Usage
```vue
<template>
  <div class="tab-interface">
    <codex-tab-system 
      :layout="'vertical'"
      :show-customers-only="false"
      v-model="activeTab"
    >
      <template #tab1>
        <div>Content for Tab 1</div>
      </template>
      <template #tab2>
        <div>Content for Tab 2</div>
      </template>
    </codex-tab-system>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const activeTab = ref('tab1')
</script>
```

## Key Features
- Horizontal and vertical tab layouts
- Vue Router integration with URL synchronization
- Customer authentication integration
- Flexible tab content rendering
- Query parameter and path-based routing
- Custom tab components and labels
- Mobile-responsive collapsible navigation
- Slot-based content customization
- Tab state management and persistence

## Configuration Props

### Layout Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `layout` | `String` | `'vertical'` | Tab layout orientation (vertical, horizontal) |
| `fullScreen` | `Boolean` | `false` | Enable full-screen layout mode |

### State Management Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | `String` | `null` | v-model for active tab ID |
| `defaultTab` | `String` | `null` | Default tab to activate |
| `renderAllTabs` | `Boolean` | `false` | Render all tab content or only active |

### Router Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `paramName` | `String` | `'tab'` | Parameter name for router integration |
| `useQueryParam` | `Boolean` | `false` | Use query parameters instead of path |

### Authentication Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `showCustomersOnly` | `Boolean` | `false` | Only show content to authenticated users |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:modelValue` | `tabId` | Emitted when active tab changes |
| `tab-change` | `tabId, tab` | Emitted when tab selection changes |

## Slots

### Navigation Slots
| Slot | Props | Description |
|------|-------|-------------|
| `nav-container-start` | `{ activeTab, tabs }` | Content before navigation container |
| `nav-container-end` | `{ activeTab, tabs }` | Content after navigation container |
| `nav-start` | `{ activeTab, tabs }` | Content before navigation tabs |
| `nav-end` | `{ activeTab, tabs }` | Content after navigation tabs |
| `tab` | `{ tab, active }` | Custom tab button content |

### Content Slots
| Slot | Props | Description |
|------|-------|-------------|
| `header` | N/A | Header content area |
| `content` | N/A | Main content area (overrides tab content) |
| `footer` | N/A | Footer content area |
| `[tabId]` | `{ tab, key, onTabChange }` | Individual tab content |

## Tab Configuration

The component accepts tab configuration through injection from parent components:

```javascript
// In parent component
provide('tabSystem.tabs', computed(() => [
  {
    id: 'overview',
    label: 'Overview',
    icon: 'ri-dashboard-line',
    component: 'OverviewComponent',
    labelComponent: 'CustomLabel',
    labelProps: { count: 5 },
    replaceTab: false
  },
  {
    id: 'settings',
    label: 'Settings',
    icon: 'ri-settings-line'
  }
]))
```

## Router Integration

The component integrates with Vue Router through injection:

```javascript
// Router configuration
provide('tabSystem.useRouter', true)
provide('tabSystem.disableRouting', false)
provide('tabSystem.routerMode', 'history')
```

## Examples

### Basic Vertical Tabs
```vue
<template>
  <div class="vertical-tabs">
    <codex-tab-system 
      :layout="'vertical'"
      v-model="currentTab"
      :default-tab="'dashboard'"
    >
      <template #dashboard>
        <div class="dashboard-content">
          <h2>Dashboard</h2>
          <p>Welcome to your dashboard</p>
        </div>
      </template>
      
      <template #profile>
        <div class="profile-content">
          <h2>Profile</h2>
          <p>Manage your profile settings</p>
        </div>
      </template>
      
      <template #settings>
        <div class="settings-content">
          <h2>Settings</h2>
          <p>Configure your preferences</p>
        </div>
      </template>
    </codex-tab-system>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const currentTab = ref('dashboard')
</script>
```

### Horizontal Mobile-Friendly Tabs
```vue
<template>
  <div class="horizontal-tabs">
    <codex-tab-system 
      :layout="'horizontal'"
      v-model="activeTab"
      :full-screen="true"
    >
      <template #nav-start="{ activeTab, tabs }">
        <div class="tab-counter">
          {{ tabs.findIndex(t => t.id === activeTab) + 1 }} / {{ tabs.length }}
        </div>
      </template>
      
      <template #tab="{ tab, active }">
        <div class="custom-tab" :class="{ active }">
          <i v-if="tab.icon" :class="tab.icon"></i>
          <span>{{ tab.label }}</span>
          <div v-if="tab.badge" class="tab-badge">{{ tab.badge }}</div>
        </div>
      </template>
      
      <template #events>
        <div class="events-tab">
          <h2>Upcoming Events</h2>
          <div class="event-list">
            <div v-for="event in upcomingEvents" :key="event.id" class="event-item">
              <h3>{{ event.name }}</h3>
              <p>{{ event.date }}</p>
            </div>
          </div>
        </div>
      </template>
      
      <template #bookings>
        <div class="bookings-tab">
          <h2>Your Bookings</h2>
          <div class="booking-list">
            <div v-for="booking in userBookings" :key="booking.id" class="booking-item">
              <h3>{{ booking.event.name }}</h3>
              <p>{{ booking.date }}</p>
              <button @click="cancelBooking(booking.id)">Cancel</button>
            </div>
          </div>
        </div>
      </template>
      
      <template #account>
        <div class="account-tab">
          <h2>Account Settings</h2>
          <form @submit.prevent="updateAccount">
            <div class="form-group">
              <label>Name</label>
              <input v-model="accountForm.name" type="text">
            </div>
            <div class="form-group">
              <label>Email</label>
              <input v-model="accountForm.email" type="email">
            </div>
            <button type="submit">Update Account</button>
          </form>
        </div>
      </template>
    </codex-tab-system>
  </div>
</template>

<script setup>
import { ref, provide, computed } from 'vue'

const activeTab = ref('events')

const upcomingEvents = ref([
  { id: 1, name: 'Yoga Class', date: '2024-01-15' },
  { id: 2, name: 'Pilates Session', date: '2024-01-16' }
])

const userBookings = ref([
  { id: 1, event: { name: 'Hot Yoga' }, date: '2024-01-14' }
])

const accountForm = ref({
  name: 'John Doe',
  email: 'john@example.com'
})

// Provide tab configuration
provide('tabSystem.tabs', computed(() => [
  {
    id: 'events',
    label: 'Events',
    icon: 'ri-calendar-line',
    badge: upcomingEvents.value.length
  },
  {
    id: 'bookings',
    label: 'Bookings',
    icon: 'ri-bookmark-line',
    badge: userBookings.value.length
  },
  {
    id: 'account',
    label: 'Account',
    icon: 'ri-user-line'
  }
]))

const cancelBooking = (bookingId) => {
  userBookings.value = userBookings.value.filter(b => b.id !== bookingId)
}

const updateAccount = () => {
  console.log('Updating account:', accountForm.value)
}
</script>

<style scoped>
.custom-tab {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  position: relative;
}

.tab-badge {
  background: #dc3545;
  color: white;
  border-radius: 50%;
  padding: 0.25rem 0.5rem;
  font-size: 0.75rem;
  min-width: 1.5rem;
  text-align: center;
}

.tab-counter {
  font-size: 0.875rem;
  color: #6c757d;
  padding: 0.5rem;
}

.form-group {
  margin-bottom: 1rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.25rem;
  font-weight: bold;
}

.form-group input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.event-list, .booking-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.event-item, .booking-item {
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 6px;
}
</style>
```

### Router-Integrated Tabs
```vue
<template>
  <div class="router-tabs">
    <codex-tab-system 
      :layout="'vertical'"
      :use-query-param="true"
      :param-name="'section'"
    >
      <template #nav-container-start="{ activeTab }">
        <div class="nav-header">
          <h2>Navigation</h2>
          <p>Current: {{ activeTab }}</p>
        </div>
      </template>
      
      <template #dashboard>
        <router-view name="dashboard" />
      </template>
      
      <template #analytics>
        <router-view name="analytics" />
      </template>
      
      <template #reports>
        <router-view name="reports" />
      </template>
      
      <template #footer>
        <div class="tab-footer">
          <button @click="navigateToSettings">Go to Settings</button>
        </div>
      </template>
    </codex-tab-system>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const router = inject('router')

// Provide router configuration
provide('tabSystem.useRouter', true)
provide('tabSystem.routerMode', 'history')

const navigateToSettings = () => {
  router.push('/settings')
}
</script>
```

## CSS Classes

| Class | Description |
|-------|-------------|
| `_c-tab-system` | Main container styling |
| `_c-horizontal-nav` | Horizontal layout styling |
| `_c-vertical-nav` | Vertical layout styling |
| `_c-full-screen` | Full-screen mode styling |
| `_c-tab-nav-container` | Navigation container |
| `_c-tab-nav-container-inner` | Inner navigation container |
| `_c-tab-nav` | Navigation list styling |
| `_c-tab-nav-toggle` | Mobile navigation toggle |
| `_c-nav-open` | Open navigation state |
| `_c-tab-button` | Individual tab button |
| `_c-tab-active` | Active tab styling |
| `_c-tab-label` | Tab label text |
| `_c-arrow-icon` | Navigation arrow icon |
| `_c-tab-content` | Content container |
| `_c-content-header` | Content header area |
| `_c-content-body` | Content body area |
| `_c-content-footer` | Content footer area |
| `_c-tab-pane` | Individual tab pane |
| `_c-tab-pane-active` | Active tab pane |

## Internationalization

The TabSystem component has minimal translation requirements as it primarily provides structural layout and navigation functionality.

### Translation Notes

#### Minimal Translation Requirements
The TabSystem component itself does not use direct translation keys as it:
- Receives tab labels through configuration from parent components
- Handles layout and navigation behavior
- Manages state and routing functionality

#### Tab Label Translation
Tab labels are typically translated in the parent component that provides the tab configuration:

```vue
<!-- Parent component providing translated tabs -->
<script setup>
import { computed, provide } from 'vue'
import { useI18n } from 'vue-i18n'

const { t } = useI18n()

// Provide translated tab configuration
provide('tabSystem.tabs', computed(() => [
  {
    id: 'overview',
    label: t('tabs.overview'),
    icon: 'ri-dashboard-line'
  },
  {
    id: 'settings', 
    label: t('tabs.settings'),
    icon: 'ri-settings-line'
  },
  {
    id: 'profile',
    label: t('tabs.profile'),
    icon: 'ri-user-line'
  }
]))
</script>

<template>
  <codex-tab-system v-model="activeTab">
    <template #overview>
      <div>{{ t('content.overview_description') }}</div>
    </template>
    <!-- Other tab content -->
  </codex-tab-system>
</template>
```

#### Content Translation
Translation is handled by the content components placed within each tab slot:
- Each tab's content components handle their own internationalization
- Form components use field-level translation props
- Data display components use their respective translation keys

#### Authentication Integration
When `showCustomersOnly` is enabled, the component integrates with the login component which handles its own translations for:
- Login form labels and placeholders
- Authentication error messages
- Success and validation feedback

#### Label Component Integration
Tabs can include label components that handle their own translations:
```javascript
{
  id: 'notifications',
  label: t('tabs.notifications'),
  labelComponent: 'NotificationCounter',
  labelProps: { 
    countLabel: t('notifications.count_label')
  }
}
```

## Best Practices

### Tab Configuration
- Define clear, descriptive tab labels
- Use consistent icon styles across tabs
- Provide meaningful default tab selection
- Handle tab state persistence appropriately
- Consider mobile navigation patterns

### Router Integration
- Use query parameters for non-hierarchical tabs
- Use path parameters for hierarchical navigation
- Handle browser back/forward navigation
- Provide fallback for non-router environments
- Test URL synchronization thoroughly

### Content Management
- Use renderAllTabs sparingly for performance
- Implement lazy loading for heavy content
- Handle tab content lifecycle properly
- Provide loading states for async content
- Consider memory usage with multiple tabs

### Mobile Experience
- Use horizontal layout for mobile devices
- Implement collapsible navigation
- Ensure touch-friendly interaction
- Test on various screen sizes
- Consider swipe gestures for navigation

### Accessibility
- Provide proper ARIA labels and roles
- Support keyboard navigation
- Ensure proper focus management
- Use semantic HTML structure
- Test with screen readers

### Performance
- Avoid unnecessary re-renders
- Implement content virtualization for many tabs
- Cache tab content appropriately
- Optimize for rapid tab switching
- Monitor memory usage with complex content

## Component Registration
```javascript
// Global registration
app.component('CodexTabSystem', TabSystem)

// Local registration  
import TabSystem from '@/components/molecules/TabSystem.vue'

export default {
  components: {
    CodexTabSystem: TabSystem
  }
}
``` 