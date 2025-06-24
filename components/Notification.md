# Notification Component

## Overview
The Notification component provides a card-based interface for displaying individual notifications with priority indicators, read/unread states, and interactive functionality. It features clickable notification cards, truncation support, delete functionality, and survey integration for notification-driven surveys.

## Basic Usage
```vue
<codex-notification
  :notification="notificationData"
  :go-to-notification="handleNotificationClick"
  :truncate-length="100"
/>
```

## Key Features
- Card-based notification display
- Priority indicators with color coding
- Read/unread visual states
- Clickable notification navigation
- Text truncation for long content
- Delete notification functionality
- Date formatting display
- Survey modal integration
- Subject and body content display
- Responsive layout with mobile adaptations
- Slot-based customization

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| notification | Object | Yes | null | Notification data object |
| goToNotification | Function | Yes | - | Function to handle notification click navigation |
| truncateLength | Number | No | undefined | Maximum length for notification body truncation |

### Common Props
All common props from `@/config/common` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| notification-clicked | `notification: Object` | Emitted when notification card is clicked |
| notification-deleted | `notification: Object` | Emitted when notification is deleted |
| survey-triggered | `notification: Object` | Emitted when survey modal is opened |

## Slots

### Header Slot
```vue
<template #header>
  <!-- Custom header content -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content -->
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
The component supports three priority levels with visual indicators:
- **High Priority**: Red/danger styling (`_c-danger`)
- **Medium Priority**: Orange/warning styling (`_c-warning`) 
- **Low Priority**: Green/success styling (`_c-success`)

## Read/Unread States
- **Unread**: Cards display with `_c-unread` class for distinct styling
- **Read**: Normal card styling without additional state classes
- **Visual Indicators**: Clear differentiation between read and unread states

## Text Truncation
- Configurable truncation length via `truncateLength` prop
- Automatic ellipsis addition for truncated content
- Line break normalization for clean display
- Preserves HTML content in notification body

## Internationalization
The component uses the following translation keys:
- `notification.read`: Read button text
- `notification.cta`: Call-to-action button text
- `notification.delete`: Delete confirmation text

## Examples

### Basic Implementation
```vue
<codex-notification
  :notification="notification"
  :go-to-notification="openNotificationModal"
/>
```

### With Custom Truncation
```vue
<codex-notification
  :notification="notification"
  :go-to-notification="handleNotificationClick"
  :truncate-length="150"
/>
```

### With Event Handlers
```vue
<codex-notification
  :notification="notification"
  :go-to-notification="handleNotificationClick"
  @notification-clicked="trackNotificationClick"
  @notification-deleted="handleNotificationDelete"
/>
```

### Custom Header Content
```vue
<codex-notification
  :notification="notification"
  :go-to-notification="handleNotificationClick"
>
  <template #header>
    <div class="custom-notification-header">
      <div class="notification-category">{{ notification.category }}</div>
      <div class="notification-timestamp">{{ formatTimestamp(notification.created_at) }}</div>
    </div>
  </template>
</codex-notification>
```

### Custom Footer with Actions
```vue
<codex-notification
  :notification="notification"
  :go-to-notification="handleNotificationClick"
>
  <template #footer>
    <div class="notification-actions">
      <button @click="markAsRead(notification)">Mark Read</button>
      <button @click="shareNotification(notification)">Share</button>
      <button @click="archiveNotification(notification)">Archive</button>
    </div>
  </template>
</codex-notification>
```

### In Notification List
```vue
<div class="notifications-list">
  <codex-notification
    v-for="notification in notifications"
    :key="notification.id"
    :notification="notification"
    :go-to-notification="openNotificationDetail"
    :truncate-length="120"
  />
</div>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-notification-card`: Notification-specific card styling
- `_c-unread`: Unread notification state styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-priority`: Priority indicator container
- `_c-flag`: Priority flag styling
- `_c-danger`: High priority styling
- `_c-warning`: Medium priority styling
- `_c-success`: Low priority styling
- `_c-row`: Row layout utility
- `_c-justify-between`: Space between flex items
- `_c-gap-xl`: Extra large gap utility
- `_c-items-center`: Center align flex items
- `_c-flex`: Flexbox utility
- `_c-grow`: Flex grow utility
- `_c-column`: Column layout utility
- `_c-desc`: Description text styling
- `_c-date`: Date display styling
- `_c-text-nowrap`: No text wrapping utility
- `_c-text-icon`: Icon button styling

## Best Practices

### Recommended Usage Patterns
- Always provide a `goToNotification` function for proper navigation
- Use appropriate truncation lengths for list displays
- Handle notification deletion with proper confirmation
- Provide clear visual feedback for different priority levels
- Implement proper read/unread state management
- Use consistent notification object structure
- Handle survey integration appropriately

### Common Pitfalls to Avoid
- Not handling notification click events properly
- Missing truncation for long notification content
- Forgetting to style read/unread states differently
- Not providing delete confirmation
- Missing error handling for notification actions
- Not handling survey modal state management
- Insufficient handling of HTML content in body

### Accessibility Considerations
- Ensure notification cards are keyboard navigable
- Provide clear labels for interactive elements
- Use appropriate ARIA attributes for state changes
- Ensure priority indicators are accessible
- Provide screen reader friendly content
- Include proper focus management
- Use semantic HTML for notification structure
- Ensure adequate color contrast for priority indicators

### Error Handling
- Handle missing notification data gracefully
- Provide fallbacks for malformed notification objects
- Handle delete action failures appropriately
- Manage survey loading errors
- Clear error states when appropriate
- Handle network failures for notification actions
- Provide user feedback for failed operations

### State Management
- Track notification read state properly
- Handle notification updates consistently
- Manage delete state transitions
- Coordinate with parent component state
- Handle survey modal state appropriately
- Manage priority state display
- Update notification lists after actions

### Performance Considerations
- Optimize text truncation for large content
- Lazy load survey modals when needed
- Minimize re-renders during state changes
- Handle large notification lists efficiently
- Implement proper cleanup for event listeners
- Cache formatted dates appropriately
- Optimize HTML content rendering

### Content Management
- Sanitize HTML content appropriately
- Handle empty or missing content gracefully
- Format dates consistently
- Truncate content intelligently
- Preserve important formatting in truncation
- Handle special characters properly
- Manage content overflow appropriately

### Priority Management
- Use consistent priority values
- Provide clear visual hierarchy
- Handle missing priority gracefully
- Map priority levels to appropriate styles
- Ensure priority indicators are accessible
- Use meaningful priority labels
- Handle priority changes appropriately

### Survey Integration
- Handle survey availability properly
- Manage survey modal state
- Provide clear survey triggers
- Handle survey completion events
- Clean up survey modals appropriately
- Handle survey loading failures
- Coordinate survey state with notifications

## Component Registration
The component is registered as `codex-notification` in the application. 