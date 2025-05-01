# Password Recovery Component

## Overview
The Password Recovery component provides a user interface for initiating password reset requests. It features a form for email submission, error handling, success states, and internationalization support, with a flexible slot-based layout system.

## Basic Usage
```vue
<codex-password-recover />
```

## Key Features
- Email validation and submission
- Error handling and display
- Success state management
- Internationalization support
- Flexible slot-based layout
- Loading state handling
- Form validation
- Modal integration support
- Responsive design

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| showLabels | Boolean | No | false | Show input labels |
| title | String\|Boolean | No | false | Custom title text |

### Common Props
All common props from `commonProps` are supported, including:
- `titleTag`: HTML tag for titles
- Other common configuration options

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| close | - | Emitted when the component should be closed |

## Slots

### Header Slot
```vue
<template #header="{ error, fieldErrors, genericErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ error, fieldErrors, genericErrors }">
  <!-- Custom form content -->
</template>
```

### Footer Slot
```vue
<template #footer="{ error, fieldErrors, genericErrors }">
  <!-- Custom footer content -->
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
1. Initial (form display)
2. Loading (request processing)
3. Error (validation or submission errors)
4. Success (email sent)

## Internationalization
The component uses the following translation keys:
- `password.title`: Form title
- `password.introduction`: Introduction text
- `password.enter_your_email`: Email field label
- `password.enter_your_email_placeholder`: Email field placeholder
- `password.sending`: Loading state text
- `password.send_me_a_reset_email`: Submit button text
- `password.email_sent`: Success state title
- `password.success_body`: Success message
- `password.do_have_an_account`: Login prompt text
- `password.login_here`: Login link text
- `button.error`: Error state button text

## Examples

### Basic Implementation
```vue
<codex-password-recover
  title="Reset Your Password"
  :show-labels="true"
/>
```

### Custom Error Handling
```vue
<codex-password-recover>
  <template #error-messages="{ genericErrors }">
    <div class="custom-errors">
      <p v-for="error in genericErrors" :key="error">
        {{ error }}
      </p>
    </div>
  </template>
</codex-password-recover>
```

### Custom Success State
```vue
<codex-password-recover>
  <template #content="{ updated }">
    <div v-if="updated" class="custom-success">
      <h3>Check Your Email</h3>
      <p>Instructions have been sent to reset your password.</p>
    </div>
    <form v-else>
      <!-- Default form content -->
    </form>
  </template>
</codex-password-recover>
```

### Modal Integration
```vue
<codex-modal modal-name="password-recover">
  <codex-password-recover
    @close="closeModal"
    class="modal-content"
  />
</codex-modal>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-password-recover`: Default component class
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-form`: Form container
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon container
- `_c-success-title`: Success message title
- `_c-success-desc`: Success message description
- `_c-link`: Link styling

## Best Practices

### Recommended Usage
- Implement clear error messages
- Provide visual feedback during submission
- Use appropriate validation rules
- Handle all error states gracefully
- Consider mobile responsiveness
- Implement proper security measures

### Accessibility Considerations
- Ensure proper form labeling
- Provide clear error feedback
- Maintain keyboard navigation
- Use appropriate ARIA attributes
- Consider screen reader compatibility
- Ensure sufficient color contrast

### Performance Considerations
- Validate input before submission
- Handle network timeouts
- Implement proper loading states
- Consider rate limiting
- Optimize error handling

### Common Pitfalls to Avoid
- Missing error states
- Unclear success feedback
- Poor mobile responsiveness
- Insufficient validation
- Missing loading indicators
- Unclear user instructions

### State Management
- Handle form state properly
- Manage loading states
- Track submission attempts
- Handle session timeouts
- Maintain consistent error state
- Clear form on success

## Component Registration
The component is registered as `codex-password-recover` in the application. 