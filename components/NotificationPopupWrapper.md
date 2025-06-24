# NotificationPopupWrapper Component

## Overview
The NotificationPopupWrapper component provides an automatic notification popup system that displays unread notifications as modal overlays. It features automatic popup management, unread notification filtering, modal positioning, and integration with the notification center for seamless notification experience.

## Basic Usage
```vue
<codex-notification-popup-wrapper />
```

## Key Features
- Automatic popup display for unread notifications
- Modal management for multiple notifications
- Right-bottom positioning for non-intrusive display
- No overlay background for subtle presentation
- Customer authentication integration
- Unread notification filtering
- Automatic modal generation
- Integration with notification popup component
- Real-time notification updates
- Clean modal cleanup

## Props

### Common Props
All common props from `@/config/common` are supported.

## Events
The component doesn't emit direct events but coordinates with notification system events.

## Slots
The component doesn't provide custom slots as it manages automatic popup display.

## Automatic Popup Behavior
The component automatically handles notification popups:
- **Unread Filter**: Only displays notifications with `read_at == null`
- **Modal Creation**: Creates individual modals for each unread notification
- **Positioning**: Uses right-bottom positioning for subtle display
- **State Management**: Manages modal refs and states automatically
- **Customer Validation**: Only displays when customer is authenticated

## Modal Configuration
Each notification popup uses specific modal settings:
- **Position**: `x-position="right"` and `y-position="bottom"`
- **Overlay**: `use-overlay="false"` for non-intrusive display
- **Close Button**: `show-close-button="false"` for clean presentation
- **Default State**: `default-state="true"` for immediate display
- **Portal**: `portal-selector="false"` for direct DOM placement

## Integration Points
- **Customer System**: Uses `useCustomer` for authentication state
- **Notification System**: Uses `useNotifications` for data and counts
- **Modal System**: Integrates with `codex-modal` component
- **Popup Component**: Uses `codex-notification-popup` for content

## Internationalization
The component inherits internationalization from the notification popup component it contains.

## Examples

### Basic Implementation
```vue
<!-- Place in app layout for global notification popups -->
<codex-notification-popup-wrapper />
```

### In App Layout Component
```vue
<template>
  <div class="app-layout">
    <header class="app-header">
      <!-- Header content -->
    </header>
    
    <main class="app-content">
      <!-- Main content -->
    </main>
    
    <footer class="app-footer">
      <!-- Footer content -->
    </footer>
    
    <!-- Global notification popup system -->
    <codex-notification-popup-wrapper />
  </div>
</template>
```

### In Root Application Component
```vue
<template>
  <div id="app">
    <router-view />
    
    <!-- Global notification popups -->
    <codex-notification-popup-wrapper />
    
    <!-- Other global components -->
    <codex-loading-overlay />
    <codex-error-handler />
  </div>
</template>
```

### With Conditional Display
```vue
<template>
  <div class="dashboard">
    <!-- Dashboard content -->
    
    <!-- Only show notification popups on dashboard -->
    <codex-notification-popup-wrapper v-if="showNotificationPopups" />
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const showNotificationPopups = computed(() => 
  route.name === 'dashboard' || route.name === 'profile'
)
</script>
```

### In Customer Area Layout
```vue
<template>
  <div class="customer-area">
    <nav class="customer-navigation">
      <!-- Navigation -->
    </nav>
    
    <div class="customer-content">
      <router-view />
    </div>
    
    <!-- Customer-specific notification popups -->
    <codex-notification-popup-wrapper />
  </div>
</template>
```

## CSS Classes
- `codex`: Base component identifier
- `cdx_notification-popup`: Notification popup wrapper styling

## Best Practices

### Recommended Usage Patterns
- Place once in global application layout
- Ensure customer authentication is properly handled
- Position in app layout for consistent display
- Allow automatic modal management
- Don't interfere with modal state manually
- Use in combination with notification center
- Provide consistent notification experience

### Common Pitfalls to Avoid
- Placing multiple instances in the same application
- Manually managing the generated modals
- Interfering with automatic popup logic
- Not ensuring customer authentication context
- Blocking popup display with CSS z-index issues
- Not coordinating with notification center state
- Missing global placement in app structure

### Accessibility Considerations
- Ensure popups don't interfere with main content navigation
- Provide appropriate focus management for popups
- Consider screen reader interaction with automatic popups
- Ensure popups can be dismissed easily
- Handle keyboard navigation appropriately
- Consider reduced motion preferences
- Provide clear notification content structure

### Error Handling
- Handle notification system failures gracefully
- Manage modal creation errors appropriately
- Handle customer authentication errors
- Provide fallbacks when notification data is malformed
- Clean up modal state on errors
- Handle network connectivity issues
- Manage concurrent notification updates

### State Management
- Coordinate with global notification state
- Handle customer authentication changes
- Manage modal lifecycle properly
- Track notification read states
- Handle modal cleanup appropriately
- Coordinate with notification center updates
- Manage popup display timing

### Performance Considerations
- Minimize modal creation overhead
- Handle large numbers of unread notifications efficiently
- Implement proper cleanup for unused modals
- Optimize notification filtering performance
- Handle frequent notification updates appropriately
- Minimize DOM manipulation for modal management
- Consider popup display throttling for UX

### Modal Management
- Allow automatic modal lifecycle management
- Don't manually interfere with modal states
- Handle modal positioning conflicts
- Ensure proper modal cleanup
- Coordinate with other modal systems
- Handle modal z-index appropriately
- Manage modal accessibility properly

### User Experience
- Position popups non-intrusively
- Provide clear notification content
- Handle popup timing appropriately
- Allow easy dismissal of popups
- Coordinate with notification center workflow
- Provide consistent popup behavior
- Handle popup overflow scenarios

### Integration Considerations
- Coordinate with notification center component
- Handle authentication state changes
- Integrate with overall notification workflow
- Consider mobile display requirements
- Handle different notification types appropriately
- Coordinate with other overlay systems
- Manage notification priority properly

### Notification Workflow
- Coordinate automatic popups with manual notification viewing
- Handle notification state synchronization
- Manage popup display timing
- Coordinate with notification center navigation
- Handle bulk notification scenarios
- Provide consistent notification experience
- Track notification interaction analytics

## Component Registration
The component is registered as `codex-notification-popup-wrapper` in the application. 