# NotificationPopup Component

## Overview
The NotificationPopup component provides a modal-style interface for displaying detailed notification content with full-featured notification management. It features priority indicators, complete notification content display, read/unread management actions, delete functionality, and survey integration for comprehensive notification interaction.

## Basic Usage
```vue
<codex-notification-popup
  :notification="notificationData"
  @close="handleModalClose"
/>
```

## Key Features
- Modal-style notification display
- Priority indicators with color coding
- Full notification content (no truncation)
- Mark as unread functionality
- Delete notification functionality
- Close modal handling
- Survey modal integration
- Title and date display
- Action buttons for management
- Slot-based customization

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| notification | Object | Yes | null | Notification data object |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| close | - | Emitted when modal should be closed |
| notification-marked-unread | `notification: Object` | Emitted when notification is marked as unread |
| notification-deleted | `notification: Object` | Emitted when notification is deleted |
| survey-triggered | `notification: Object` | Emitted when survey modal is opened |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content (overrides default priority, title, and date) -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content (overrides default action buttons) -->
</template>
```

## Notification Object Structure
The notification prop expects an object with the following structure:
```javascript
{
  id: String|Number,          // Unique notification identifier
  subject: String,            // Notification subject/title
  body: String,               // Notification body content (HTML supported)
  priority: String,           // Priority level ('high', 'medium', 'low')
  read_at: Date|null,         // Read timestamp (null if unread)
  created_at: Date,           // Creation timestamp
  surveyUuid: String|null,    // Optional survey UUID for survey integration
  url: String|null            // Optional URL for navigation
}
```

## Priority System
The component supports three priority levels with visual flag indicators:
- **High Priority**: Red/danger styling (`_c-danger`)
- **Medium Priority**: Orange/warning styling (`_c-warning`) 
- **Low Priority**: Green/success styling (`_c-success`)

## Default Actions
The component provides two default management actions:
1. **Mark as Unread**: Marks the notification as unread and closes modal
2. **Delete**: Permanently deletes the notification and closes modal

## Survey Integration
- Displays survey modal when `surveyUuid` is present in notification
- Integrates with `codex-survey` component
- Modal management for survey display
- Proper survey state handling

## Internationalization
The component uses the following translation keys:
- `notification.unread`: Mark as unread button text
- `notification.delete`: Delete button text
- `notification.read`: Read status text
- `notification.cta`: Call-to-action button text

## Examples

### Basic Implementation
```vue
<codex-notification-popup
  :notification="selectedNotification"
  @close="closeNotificationModal"
/>
```

### In Modal Context
```vue
<codex-modal 
  :modal-name="'notification-detail'"
  x-position="center" 
  y-position="center"
>
  <codex-notification-popup
    :notification="activeNotification"
    @close="closeModal"
    @notification-marked-unread="handleUnreadMark"
    @notification-deleted="handleNotificationDelete"
  />
</codex-modal>
```

### Custom Header
```vue
<codex-notification-popup
  :notification="notification"
  @close="handleClose"
>
  <template #header>
    <div class="custom-notification-header">
      <div class="notification-meta">
        <span class="category">{{ notification.category }}</span>
        <span class="sender">From: {{ notification.sender }}</span>
      </div>
      <h2 class="notification-title">{{ notification.subject }}</h2>
      <div class="notification-actions">
        <button @click="shareNotification">Share</button>
        <button @click="bookmarkNotification">Bookmark</button>
      </div>
    </div>
  </template>
</codex-notification-popup>
```

### Custom Footer Actions
```vue
<codex-notification-popup
  :notification="notification"
  @close="handleClose"
>
  <template #footer>
    <div class="custom-actions">
      <codex-button 
        variant="secondary" 
        @click="replyToNotification"
        :default-text="'Reply'"
      />
      <codex-button 
        variant="secondary" 
        @click="forwardNotification"
        :default-text="'Forward'"
      />
      <codex-button 
        variant="primary" 
        @click="markImportant"
        :default-text="'Mark Important'"
      />
      <codex-button 
        variant="danger" 
        @click="deleteNotification"
        :default-text="'Delete'"
      />
    </div>
  </template>
</codex-notification-popup>
```

### With Event Handling
```vue
<codex-notification-popup
  :notification="notification"
  @close="handleModalClose"
  @notification-marked-unread="handleUnreadAction"
  @notification-deleted="handleDeleteAction"
  @survey-triggered="trackSurveyOpen"
/>
```

### In Notification Management System
```vue
<template v-for="notification in selectedNotifications" :key="notification.id">
  <codex-modal 
    :ref="el => { notificationModals[notification.id] = el }"
    :modal-name="'notification_popup_' + notification.id"
    x-position="center" 
    y-position="center"
  >
    <codex-notification-popup
      :notification="notification"
      @close="closeNotificationModal(notification.id)"
      @notification-marked-unread="updateNotificationList"
      @notification-deleted="removeFromList"
    />
  </codex-modal>
</template>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-notification-popup-card`: Notification popup specific styling
- `_c-header`: Header section
- `_c-content`: Content section  
- `_c-footer`: Footer section
- `_c-priority`: Priority indicator container
- `_c-flag`: Priority flag styling
- `_c-danger`: High priority styling
- `_c-warning`: Medium priority styling
- `_c-success`: Low priority styling
- `_c-justify-between`: Space between flex items
- `_c-items-end`: End align flex items
- `_c-title`: Title styling
- `_c-date`: Date display styling
- `_c-desc`: Description text styling
- `_c-btn-container`: Button container styling
- `_c-fill`: Fill container utility

## Best Practices

### Recommended Usage Patterns
- Always handle the close event to manage modal state
- Provide clear action feedback for mark unread and delete operations
- Use within proper modal containers for best UX
- Handle notification state updates after actions
- Implement proper survey integration when needed
- Provide error handling for failed actions
- Use consistent notification object structure

### Common Pitfalls to Avoid
- Not handling close events properly
- Missing notification state updates after actions
- Forgetting to clean up modal state
- Not providing action confirmation for destructive operations
- Missing error handling for failed API calls
- Not updating parent notification lists after changes
- Insufficient handling of survey modal state

### Accessibility Considerations
- Ensure modal is keyboard navigable
- Provide clear labels for action buttons
- Use appropriate ARIA attributes for modal content
- Ensure priority indicators are accessible
- Provide screen reader friendly content
- Include proper focus management for modal
- Use semantic HTML for notification structure
- Ensure adequate color contrast for all elements

### Error Handling
- Handle mark unread failures gracefully
- Provide retry mechanisms for failed delete operations
- Display clear error messages for API failures
- Handle survey loading errors appropriately
- Clear error states when operations succeed
- Provide user feedback for all operations
- Handle network connectivity issues

### State Management
- Update notification state after mark unread
- Remove notification from lists after delete
- Handle modal state transitions properly
- Coordinate with parent component state
- Manage survey modal state appropriately
- Track action completion status
- Handle concurrent notification operations

### Performance Considerations
- Lazy load survey modals when needed
- Optimize HTML content rendering
- Minimize re-renders during state changes
- Handle large notification content efficiently
- Implement proper cleanup for event listeners
- Cache notification data appropriately
- Optimize action button interactions

### Modal Integration
- Use appropriate modal containers
- Handle modal state properly
- Provide clear modal close mechanisms
- Implement proper modal accessibility
- Handle modal backdrop interactions
- Manage modal z-index appropriately
- Coordinate with other modals properly

### Action Management
- Provide clear confirmation for destructive actions
- Update UI state immediately for better UX
- Handle action failures with proper rollback
- Show loading states during actions
- Clear action states after completion
- Handle bulk action scenarios
- Provide undo functionality where appropriate

### Survey Integration
- Handle survey availability checks
- Manage survey modal lifecycle
- Provide clear survey entry points  
- Handle survey completion properly
- Clean up survey state appropriately
- Track survey interaction analytics
- Handle survey loading failures

## Component Registration
The component is registered as `codex-notification-popup` in the application. 