# Login Component

## Overview
The Login component provides a comprehensive user authentication interface that handles email/password login, error states, success feedback, and integrates with customer management. It features flexible layouts through slots, internationalization support, and seamless integration with registration flows.

## Basic Usage
```vue
<codex-login
  :title="false"
  :description="false"
  :password-reset-redirect="'codex-password-recover'"
  @success="handleLoginSuccess"
  @error="handleLoginError"
/>
```

## Key Features
- Email and password authentication
- Field validation and error handling
- Remember me functionality
- Success state with visual feedback
- Password reset integration
- Registration modal integration
- Loading states and processing indicators
- Internationalization support
- Flexible slot-based layout
- Error message linking to specific inputs
- Consistent styling with other components

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| title | String\|Boolean | No | false | Custom title or false to use default from language file |
| description | String\|Boolean | No | false | Custom description or false to use default from language file |
| passwordResetRedirect | String | No | 'codex-password-recover' | Modal name or URL for password reset link |

### Common Props
All common props from `@/config/common` are supported, including:
| Prop Name | Usage |
|-----------|-------|
| titleTag | HTML tag for the form title |
| enableBorder | Adds border styling to the card |

### Common Functions
The component uses utilities from `useCommon`:
| Function | Usage |
|----------|-------|
| useCommon | Provides common component functionality and props handling |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| success | - | Emitted when login is successful |
| error | - | Emitted when login fails |
| close | - | Emitted when component should be closed (e.g., to open register modal) |
| customEvent | - | Emitted for custom event handling from password field |

## Slots

### Header Slot
```vue
<template #header="{ genericErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ genericErrors, fieldErrors, error, loginSubmit, email, password }">
  <!-- Custom form content -->
</template>
```

### Footer Slot
```vue
<template #footer="{ error, genericErrors }">
  <!-- Custom footer content -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| genericErrors | Array | Generic error messages |
| fieldErrors | Object | Field-specific errors |
| error | Object | Current error state |
| loginSubmit | Function | Form submission handler |
| email | Ref | Email field value |
| password | Ref | Password field value |

## States
The component has two main states in order of priority:
1. **Form State** - Default login form display
2. **Success State** - Successful login confirmation with visual feedback

## Internationalization
The component uses the following translation keys:
- `login.title`: Form title
- `login.introduction`: Introduction text
- `login.email_address`: Email field label
- `login.email_address_placeholder`: Email field placeholder
- `login.password`: Password field label
- `login.password_placeholder`: Password field placeholder
- `login.forgot_password`: Password reset link text
- `login.remember_me`: Remember me checkbox label
- `login.logging_in`: Processing button text
- `login.login_button`: Submit button text
- `login.youre_logged_in`: Success state title
- `login.success_msg`: Success message
- `login.do_not_have_an_account`: Registration prompt text
- `login.register_here`: Registration link text
- `login.login_failed`: Generic login failure message
- `login.email_invalid`: Email validation error

### Translation Usage Examples
```vue
<!-- Using title prop with fallback to translation -->
<codex-login :title="false" />  <!-- Uses $t('login.title') -->
<codex-login title="Custom Title" />  <!-- Uses custom title -->

<!-- Translation keys in template examples -->
<template #footer="{ error, genericErrors }">
  <p>{{ $t('login.do_not_have_an_account') }} 
    <button @click="openRegister">{{ $t('login.register_here') }}</button>
  </p>
</template>
```

## Examples

### Basic Implementation
```vue
<codex-login
  @success="redirectToDashboard"
  @error="showErrorNotification"
/>
```

### Custom Title and Description
```vue
<codex-login
  title="Welcome Back"
  description="Sign in to access your account"
  :password-reset-redirect="'/forgot-password'"
/>
```

### Modal Integration
```vue
<codex-modal modal-name="login">
  <codex-login
    @success="closeLoginModal"
    @close="openRegisterModal"
  />
</codex-modal>
```

### Custom Header Content
```vue
<codex-login>
  <template #header="{ genericErrors }">
    <div class="custom-header">
      <img src="/logo.png" alt="Company Logo" />
      <h2>Member Login</h2>
      <codex-error v-if="genericErrors.length" :error="genericErrors" />
    </div>
  </template>
</codex-login>
```

### Custom Footer with Additional Links
```vue
<codex-login>
  <template #footer="{ error, genericErrors }">
    <div class="custom-footer">
      <p>{{ $t('login.do_not_have_an_account') }} 
        <button @click="openRegister">{{ $t('login.register_here') }}</button>
      </p>
      <p><a href="/help">Need Help?</a></p>
    </div>
  </template>
</codex-login>
```

## CSS Classes
- `codex`: Base component identifier
- `_c-card`: Main container class
- `_c-login`: Login-specific styling
- `_c-border`: Optional border styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-form`: Form container
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon container
- `_c-success-title`: Success message title
- `_c-success-desc`: Success message description
- `_c-link`: Link styling for registration button

## Best Practices

### Recommended Usage Patterns
- Always handle both success and error events
- Provide clear feedback during login process
- Use appropriate loading states
- Implement proper validation before submission
- Handle network timeouts gracefully
- Clear form data on successful login

### Common Pitfalls to Avoid
- Not validating email format before submission
- Missing error handling for network failures
- Not providing feedback during processing
- Forgetting to handle modal state management
- Not clearing sensitive data after login
- Missing accessibility considerations

### Accessibility Considerations
- Ensure proper form labels and ARIA attributes
- Provide clear error messages
- Maintain keyboard navigation support
- Use appropriate color contrast
- Support screen readers
- Provide visual focus indicators

### Error Handling
- Validate email format client-side
- Display field-specific errors
- Show generic errors for server issues
- Provide recovery options
- Clear errors when user corrects input
- Handle session timeouts

### State Management
- Use reactive refs for form data
- Handle loading states properly
- Manage error states consistently
- Clear success states after timeout
- Coordinate with customer state management
- Handle modal state transitions

### Performance Considerations
- Debounce validation where appropriate
- Minimize re-renders during typing
- Handle large error message lists
- Optimize network requests
- Consider lazy loading for heavy operations
- Implement proper cleanup

## Component Registration
The component is registered as `codex-login` in the application. 