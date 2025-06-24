# Notification Centre Component

## Overview
The Notification Centre component provides a comprehensive interface for managing customer notifications with inbox-style functionality. It features notification display, read/unread tracking, notification popup modals, pagination support, and flexible slot-based customization for notification management.

## Basic Usage
```vue
<codex-notification-centre
  :group="'customer-notifications'"
/>
```

## Key Features
- Inbox-style notification display
- Message and unread count indicators
- Individual notification viewing with modal popup
- Read/unread state management
- Automatic read marking on notification view
- Loading states with skeleton placeholders
- No results handling
- Error message display
- Pagination support with filter context
- Customer authentication integration
- Notification truncation for list view
- Modal integration for detailed view

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| group | String | No | undefined | Filter context group for pagination and filtering |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| notification-read | `notification: Object` | Emitted when a notification is marked as read |
| notification-selected | `notification: Object` | Emitted when a notification is selected for viewing |
| notifications-loaded | `notifications: Array` | Emitted when notifications are loaded |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ error, fieldErrors, genericErrors }">
  <!-- Custom content and notification display -->
</template>
```

### Footer Slot
```vue
<template #footer="{ error, fieldErrors, genericErrors }">
  <!-- Custom footer content (pagination by default) -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| error | Object | Current error state |
| fieldErrors | Object | Field-specific errors |
| genericErrors | Array | Generic error messages |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Component hidden when no customer
2. **Loading State** - Shows skeleton notification cards during data fetch
3. **Error State** - Displays error messages
4. **No Results State** - Shows no notifications message
5. **Notifications Display State** - Normal notification list display
6. **Notification Modal State** - Detailed notification view in modal

## Notification Management
The component provides comprehensive notification management:

### Read State Management
- **Automatic Reading**: Notifications are marked as read when viewed in modal
- **Unread Tracking**: Tracks and displays unread notification count
- **Visual Indicators**: Different styling for read vs unread notifications

### Notification Viewing
- **List View**: Truncated notifications in the main list (100 characters default)
- **Modal View**: Full notification content in popup modal
- **Modal Integration**: Seamless transition from list to detailed view

### Counter Display
- **Total Messages**: Shows total notification count with icon
- **Unread Count**: Shows unread notification count with icon
- **Dynamic Updates**: Counters update as notifications are read

## Internationalization
The component uses the following translation keys:
- `notification.inbox`: Header title for notification centre
- `notification.messages`: Message count label (with pluralization)
- `notification.unread`: Unread count label (with pluralization)
- `notification.no_notifications`: No notifications available message

## Examples

### Basic Implementation
```vue
<codex-notification-centre />
```

### With Custom Header
```vue
<codex-notification-centre>
  <template #header>
    <div class="notification-header">
      <h2>Your Messages</h2>
      <div class="notification-actions">
        <button @click="markAllRead">Mark All Read</button>
        <button @click="clearAll">Clear All</button>
      </div>
    </div>
  </template>
</codex-notification-centre>
```

### Custom Content with Additional Features
```vue
<codex-notification-centre>
  <template #content="{ error, fieldErrors, genericErrors }">
    <div class="notifications-container">
      <div class="notification-filters">
        <button @click="filterUnread">Unread Only</button>
        <button @click="filterRead">Read Only</button>
        <button @click="showAll">Show All</button>
      </div>
      
      <div class="_c-notifications-list">
        <codex-notification 
          v-for="notification in filteredNotifications"
          :key="notification.id"
          :notification="notification"
          :truncate-length="150"
          @click="handleNotificationClick"
        />
      </div>
    </div>
  </template>
</codex-notification-centre>
```

### Custom No Results State
```vue
<codex-notification-centre>
  <template #content="{ error, genericErrors }">
    <div v-if="!notifications.length && !loading" class="custom-no-notifications">
      <h3>No Messages</h3>
      <p>You don't have any notifications yet. We'll notify you of important updates here.</p>
    </div>
    <div v-else class="_c-notifications-list">
      <!-- Default notification list -->
    </div>
  </template>
</codex-notification-centre>
```

### With Event Handlers
```vue
<codex-notification-centre
  @notification-read="trackNotificationRead"
  @notification-selected="handleNotificationSelection"
  @notifications-loaded="updateNotificationBadge"
/>
```

### Custom Footer with Actions
```vue
<codex-notification-centre>
  <template #footer="{ error, fieldErrors, genericErrors }">
    <div class="notification-footer">
      <div class="bulk-actions">
        <button @click="markAllRead">Mark All Read</button>
        <button @click="deleteRead">Delete Read</button>
      </div>
      <codex-pagination :group="group" />
    </div>
  </template>
</codex-notification-centre>
```

### With Filter Context
```vue
<codex-notification-centre
  :group="'filtered-notifications'"
/>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-notification-centre`: Notification centre specific styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-notification-header`: Header container with flex layout
- `_c-flex`: Flexbox utility
- `_c-justify-between`: Space between flex items
- `_c-gap-lg`: Large gap utility
- `_c-gap-sm`: Small gap utility
- `_c-items-center`: Center align flex items
- `_c-message-count`: Message count container
- `_c-unread-count`: Unread count container
- `_c-text-icon`: Icon styling for counts
- `_c-notifications-list`: Notification list container
- `_c-no-results`: No results state styling

## Best Practices

### Recommended Usage Patterns
- Always handle customer authentication state
- Implement proper loading states during notification fetch
- Provide clear read/unread visual indicators
- Use appropriate truncation lengths for list view
- Handle notification state changes with proper feedback
- Implement error retry mechanisms
- Use pagination for large notification datasets
- Provide bulk actions for notification management

### Common Pitfalls to Avoid
- Not handling unauthenticated customer states
- Missing loading states during data fetch
- Forgetting to mark notifications as read
- Not providing clear read/unread indicators
- Missing error handling for failed operations
- Not updating notification counts properly
- Insufficient handling of modal state management
- Not providing clear navigation between list and detail views

### Accessibility Considerations
- Ensure notifications are keyboard navigable
- Provide clear labels for read/unread states
- Use appropriate ARIA attributes for dynamic content
- Ensure modal dialogs are accessible
- Provide screen reader friendly notification information
- Include proper focus management for modals
- Use semantic HTML for notification data
- Provide clear indication of notification importance

### Error Handling
- Display clear error messages for failed loads
- Provide retry mechanisms for network failures
- Handle notification marking errors gracefully
- Show validation errors when appropriate
- Clear error states when operations succeed
- Handle authentication errors appropriately
- Provide fallback states for partial failures

### State Management
- Track notification read state properly
- Handle customer authentication changes
- Manage loading states consistently
- Update notification counts after state changes
- Handle modal state transitions
- Manage notification selection state
- Coordinate with pagination and filter state

### Performance Considerations
- Implement virtual scrolling for large notification lists
- Lazy load notification components
- Optimize list rendering performance
- Handle large notification datasets efficiently
- Consider pagination for better performance
- Implement proper cleanup for event listeners
- Cache notification data appropriately

### Notification Management
- Mark notifications as read appropriately
- Provide clear visual feedback for state changes
- Handle bulk operations efficiently
- Update counts in real-time
- Manage notification priority and ordering
- Handle notification expiration appropriately
- Implement proper notification archiving

### Modal Integration
- Ensure smooth transition from list to modal
- Handle modal state properly
- Provide clear navigation within modal
- Implement proper modal cleanup
- Handle modal accessibility requirements
- Manage modal focus appropriately
- Provide clear modal close mechanisms

### Counter Management
- Update counters in real-time
- Handle counter edge cases (zero states)
- Provide meaningful counter labels
- Handle pluralization properly
- Update counters after batch operations
- Sync counters with server state
- Handle counter overflow scenarios

### Filter Context Integration
- Use appropriate group names for filtering
- Handle filter state changes
- Coordinate pagination with filters
- Update notifications when filters change
- Handle filter reset scenarios
- Provide clear filter feedback
- Maintain filter state across navigation

## Component Registration
The component is registered as `codex-notification-centre` in the application. 