# Password Reset Component

## Overview
The Password Reset component provides a user interface for completing the password reset process. It handles token validation, password confirmation, error states, and success feedback while maintaining a consistent user experience through internationalization support and flexible layouts.

## Basic Usage
```vue
<codex-password-reset
  :token="resetToken"
  :email="userEmail"
/>
```

## Key Features
- Token validation
- Password confirmation matching
- Error handling and display
- Success state management
- Internationalization support
- Flexible slot-based layout
- Loading state handling
- Form validation
- Security measures
- Responsive design

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| token | String\|Boolean | No | false | Reset token from URL or prop |
| email | String\|Boolean | No | false | User email from URL or prop |
| redirectUrl | String | No | "/pages/login" | Redirect URL after reset |
| showLabels | Boolean | No | false | Show input labels |

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
<template #header="{ error, fieldErrors, genericErrors, canReset }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ error, fieldErrors, genericErrors, canReset, fields }">
  <!-- Custom form content -->
</template>
```

### Footer Slot
```vue
<template #footer="{ error, fieldErrors, genericErrors, canReset }">
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
| canReset | Boolean | Whether reset is possible |
| fields | Object | Form field values |

## States
1. Invalid Token/Email (error state)
2. Initial Form Display
3. Loading (request processing)
4. Validation Errors
5. Success (password reset)

## Internationalization
The component uses the following translation keys:
- `password_reset.title`: Form title
- `password_reset.introduction`: Introduction text
- `password_reset.password`: Password field label
- `password_reset.password_placeholder`: Password field placeholder
- `password_reset.confirm_password`: Confirmation field label
- `password_reset.sending`: Loading state text
- `password_reset.send_me_a_reset_email`: Submit button text
- `password_reset.email_sent`: Success state title
- `password_reset.success_body`: Success message
- `button.error`: Error state button text

## Examples

### Basic Implementation
```vue
<codex-password-reset
  :token="urlToken"
  :email="userEmail"
  :show-labels="true"
/>
```

### Custom Error Display
```vue
<codex-password-reset>
  <template #error-messages="{ genericErrors }">
    <div class="custom-error-display">
      <i class="error-icon" />
      <ul>
        <li v-for="error in genericErrors" :key="error">
          {{ error }}
        </li>
      </ul>
    </div>
  </template>
</codex-password-reset>
```

### Custom Success State
```vue
<codex-password-reset>
  <template #content="{ updated }">
    <div v-if="updated" class="custom-success">
      <h3>Password Updated Successfully</h3>
      <p>You can now log in with your new password.</p>
      <button @click="redirectToLogin">Go to Login</button>
    </div>
    <form v-else>
      <!-- Default form content -->
    </form>
  </template>
</codex-password-reset>
```

### Modal Integration
```vue
<codex-modal modal-name="password-reset">
  <codex-password-reset
    :token="resetToken"
    :email="userEmail"
    @close="closeModal"
  />
</codex-modal>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-password-reset`: Default component class
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-form`: Form container
- `_c-error-container`: Error state container
- `_c-error-icon`: Error icon container
- `_c-error-title`: Error message title
- `_c-error-desc`: Error message description
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon container
- `_c-success-title`: Success message title
- `_c-success-desc`: Success message description

## Best Practices

### Recommended Usage
- Validate token and email before showing form
- Implement strong password requirements
- Provide clear password requirements
- Handle all error states gracefully
- Implement proper security measures
- Use appropriate loading indicators

### Security Considerations
- Validate token expiration
- Implement password strength requirements
- Protect against brute force attempts
- Secure token transmission
- Clear sensitive data after reset
- Use HTTPS for all requests

### Accessibility Considerations
- Provide clear error messages
- Ensure keyboard navigation
- Use appropriate ARIA labels
- Maintain focus management
- Consider screen reader users
- Provide visual feedback

### Performance Considerations
- Validate input before submission
- Handle network timeouts
- Implement proper loading states
- Optimize error handling
- Consider rate limiting
- Handle token expiration

### Common Pitfalls to Avoid
- Not validating token/email
- Unclear password requirements
- Missing error states
- Poor security practices
- Insufficient validation
- Unclear success feedback

### State Management
- Handle form state properly
- Manage token validation
- Track submission attempts
- Handle session timeouts
- Maintain error states
- Clear sensitive data

## Component Registration
The component is registered as `codex-password-reset` in the application. 