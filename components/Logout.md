# Logout Component

## Overview
The Logout component is a flexible button component that handles user logout functionality. It supports both simple button and enhanced button styles, custom text states, and loading indicators while maintaining internationalization support.

## Basic Usage
```vue
<codex-logout />
```

## Key Features
- Two button style modes (simple and enhanced)
- Loading state handling
- Customizable text for default and processing states
- Internationalization support
- Slot-based content customization
- Customer state awareness
- Custom class support

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| isSimple | Boolean | No | false | Use simple HTML button instead of codex-button |
| defaultText | String\|Boolean | No | false | Custom text for default state |
| processingText | String\|Boolean | No | false | Custom text for processing state |
| className | String | No | '' | Additional CSS classes |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| click | Event | Native click event (prevented by default) |

## Slots

### Default Slot
```vue
<template #default="slotProps">
  <!-- Custom content with access to slot props -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| logout | Function | Logout function |
| currentText | String | Current button text based on state |
| customer | Object | Current customer object |
| processingText | String | Processing state text |
| className | String | Current CSS class name |

## States
1. Default (logged in)
2. Processing (logging out)
3. Not Visible (no customer)

## Internationalization
The component uses the following translation keys:
- `cart.logging_you_out`: Text shown during logout process
- `cart.logout`: Default logout button text

## Examples

### Basic Logout Button
```vue
<codex-logout />
```

### Simple Button Style
```vue
<codex-logout
  :is-simple="true"
  class="custom-button"
/>
```

### Custom Text States
```vue
<codex-logout
  default-text="Sign Out"
  processing-text="Signing Out..."
/>
```

### Custom Content with Slot
```vue
<codex-logout class="custom-logout">
  <template #default="{ currentText, customer }">
    <span>{{ currentText }} ({{ customer.name }})</span>
  </template>
</codex-logout>
```

### Enhanced Button with Icons
```vue
<codex-logout>
  <template #default="{ currentText }">
    <i class="ri-logout-box-line"></i>
    {{ currentText }}
  </template>
</codex-logout>
```

## CSS Classes
- Any classes passed via `className` prop
- Button inherits classes from either native button or codex-button component

## Best Practices

### Recommended Usage
- Place logout in appropriate navigation areas
- Use consistent button styling across the application
- Consider user context when positioning the logout button
- Implement proper error handling for logout failures
- Use appropriate loading indicators

### Accessibility Considerations
- Ensure button is keyboard accessible
- Provide clear visual feedback during logout process
- Maintain proper focus management
- Use appropriate ARIA attributes
- Consider color contrast for button states

### Performance Considerations
- Handle logout state changes efficiently
- Manage customer session cleanup properly
- Consider caching implications
- Handle network failures gracefully

### Common Pitfalls to Avoid
- Not handling logout failures
- Missing loading states
- Inconsistent button styling
- Poor error feedback
- Not clearing sensitive data on logout

### State Management
- Handle customer session properly
- Manage loading states effectively
- Clear relevant cache/storage
- Handle navigation after logout
- Maintain consistent state across components

## Component Registration
The component is registered as `codex-logout` in the application. 