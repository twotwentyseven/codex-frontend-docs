# NotificationSummary Component

## Overview
The NotificationSummary component provides a simple badge-style interface for displaying notification counts with customizable content. It features unread notification count indicators, customizable text content, and slot-based flexibility for notification status display in navigation or header areas.

## Basic Usage
```vue
<codex-notification-summary
  content="Notifications"
/>
```

## Key Features
- Unread notification count badge
- Customizable text content
- Automatic count visibility (shows only when > 0)
- Slot-based content customization
- Lightweight and minimal design
- Real-time count updates
- Integrates with notification system

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| content | String | No | '' | Text content to display alongside count badge |

### Common Props
All common props from `@/config/common` are supported.

## Events
The component doesn't emit any custom events.

## Slots

### Notification Summary Slot
```vue
<template #notification-summary>
  <!-- Custom content with count badge -->
</template>
```

### Slot Props
The slot provides access to:
- Default content text
- Unread notification count
- Badge styling

## Notification Count
The component automatically displays the unread notification count:
- **Count Source**: Uses `unreadNotificationCount` from `useNotifications`
- **Visibility**: Badge only appears when count > 0
- **Real-time Updates**: Automatically updates when notification state changes
- **Styling**: Consistent badge styling with `_c-notification-summary` class

## Internationalization
The component accepts any text content via the `content` prop, making it fully internationalization-ready.

## Examples

### Basic Implementation
```vue
<codex-notification-summary
  content="Messages"
/>
```

### In Navigation Menu
```vue
<nav class="main-navigation">
  <a href="/dashboard">Dashboard</a>
  <a href="/profile">Profile</a>
  <a href="/notifications" class="notification-link">
    <codex-notification-summary content="Notifications" />
  </a>
</nav>
```

### In Header Bar
```vue
<header class="app-header">
  <div class="header-content">
    <h1>My App</h1>
    <div class="header-actions">
      <codex-notification-summary content="Inbox" />
      <button @click="openUserMenu">User Menu</button>
    </div>
  </div>
</header>
```

### Custom Slot Content
```vue
<codex-notification-summary>
  <template #notification-summary>
    <div class="custom-notification-summary">
      <i class="notification-icon"></i>
      <span>Messages</span>
      <span v-if="unreadNotificationCount > 0" class="custom-badge">
        {{ unreadNotificationCount }}
      </span>
    </div>
  </template>
</codex-notification-summary>
```

### With Icon and Text
```vue
<codex-notification-summary>
  <template #notification-summary>
    <div class="notification-button">
      <svg class="notification-icon" viewBox="0 0 24 24">
        <path d="M12 22c1.1 0 2-.9 2-2h-4c0 1.1.89 2 2 2zm6-6v-5c0-3.07-1.64-5.64-4.5-6.32V4c0-.83-.67-1.5-1.5-1.5s-1.5.67-1.5 1.5v.68C7.63 5.36 6 7.92 6 11v5l-2 2v1h16v-1l-2-2z"/>
      </svg>
      <span>Notifications</span>
      <span v-if="unreadNotificationCount > 0" class="_c-notification-summary">
        {{ unreadNotificationCount }}
      </span>
    </div>
  </template>
</codex-notification-summary>
```

### In Dropdown Menu
```vue
<div class="dropdown-menu">
  <a href="/messages">
    <codex-notification-summary content="Messages" />
  </a>
  <a href="/alerts">
    <codex-notification-summary content="Alerts" />
  </a>
  <a href="/updates">
    <codex-notification-summary content="Updates" />
  </a>
</div>
```

### Clickable Notification Button
```vue
<button @click="openNotificationCenter" class="notification-button">
  <codex-notification-summary content="Notifications" />
</button>
```

## CSS Classes
- `_c-notification-summary`: Badge styling for unread count

## Best Practices

### Recommended Usage Patterns
- Use meaningful content text that describes the notification type
- Place in navigation areas where users expect to see notification status
- Combine with clickable elements for better UX
- Use consistent styling across the application
- Update content text based on application context
- Provide clear visual hierarchy

### Common Pitfalls to Avoid
- Not providing meaningful content text
- Placing in areas where badges might be overlooked
- Using inconsistent styling across different instances
- Not making the element interactive when appropriate
- Missing accessibility considerations for count badges
- Not handling zero count states properly

### Accessibility Considerations
- Provide screen reader friendly content
- Use appropriate ARIA labels for count information
- Ensure adequate color contrast for badge visibility
- Include descriptive text for notification purpose
- Handle keyboard navigation appropriately
- Provide clear indication of interactive elements

### Error Handling
- Handle notification system connection failures
- Provide fallbacks when count data is unavailable
- Clear error states when system recovers
- Handle edge cases with large notification counts
- Manage system errors gracefully

### State Management
- Coordinate with global notification state
- Handle real-time count updates properly
- Manage component lifecycle appropriately
- Handle authentication state changes
- Update display when notification preferences change

### Performance Considerations
- Minimize re-renders when count doesn't change
- Use efficient update mechanisms
- Handle large notification counts appropriately
- Implement proper cleanup for reactive data
- Optimize for frequent count updates

### Visual Design
- Use consistent badge styling
- Ensure badge visibility without being intrusive
- Handle overflow scenarios for large counts
- Maintain visual hierarchy with surrounding elements
- Consider mobile display requirements
- Use appropriate sizing for different contexts

### Integration Patterns
- Combine with navigation components effectively
- Integrate with modal or dropdown systems
- Coordinate with full notification center interfaces
- Handle click events for better user experience
- Provide clear pathways to detailed notification views

## Component Registration
The component is registered as `codex-notification-summary` in the application. 