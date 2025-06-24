# PasswordUpdate Component

## Overview
The PasswordUpdate component provides a secure interface for customer password updates with validation, confirmation, and success feedback. It features password field validation, confirmation matching, processing states, success notifications, and customer authentication integration for secure password management.

**Note**: This component is deprecated and has been merged into the Profile component. It remains documented for reference purposes.

## Basic Usage
```vue
<codex-password-update
  :show-labels="true"
  :title="'Update Password'"
  :title-tag="'h2'"
/>
```

## Key Features
- Secure password update interface
- Password confirmation validation
- Customer authentication requirement
- Processing states during update
- Success feedback display
- Error handling and display
- Configurable label display
- Custom title support
- Form validation
- Legacy browser support

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| showLabels | Boolean | No | false | Whether to show form field labels |
| title | String\|Boolean | No | false | Custom title for the password update section |
| titleTag | String | No | 'h1' | HTML tag for the title element |

### Common Props
All common props from `@/config/common` are supported.

### Account Props
All account-related props from `@/config/account` are supported.

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| password-updated | `customer: Object` | Emitted when password is successfully updated |
| update-failed | `errors: Array` | Emitted when password update fails |

## Slots

### Title Slot
```vue
<template #title="{ title, titleTag }">
  <!-- Custom title content -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

### Login Slot
```vue
<template #login>
  <!-- Custom login required message -->
</template>
```

### Default Slot (Success State)
```vue
<template #default>
  <!-- Custom success message -->
</template>
```

## Component States
The component has multiple states:

1. **Unauthenticated State** - Shows login required message
2. **Form State** - Shows password update form
3. **Processing State** - Shows loading during update
4. **Success State** - Shows success confirmation
5. **Error State** - Shows validation errors

## Form Validation
The component includes built-in validation:
- **Required Fields**: Both password and confirmation required
- **Password Confirmation**: Must match new password
- **Submit Button**: Disabled until valid input provided
- **Error Display**: Shows server-side validation errors

## Security Features
- **Authentication Required**: Only authenticated customers can update
- **Secure Form**: Uses POST method with autocomplete disabled
- **Password Confirmation**: Requires confirmation for safety
- **Error Handling**: Secure error messaging

## Internationalization

The `PasswordUpdate` component uses translation keys for password change interface and status messages:

### Core Translation Keys

| Translation Key | Usage | Example Context |
|----------------|-------|-----------------|
| `account.update_password_title` | Default title for password update section | Form header |
| `password.password` | Password field label | New password input |
| `account.new_password` | New password placeholder text | Input placeholder |
| `password.confirm_password` | Confirm password field label | Password confirmation input |
| `account.confirm_new_password` | Confirm password placeholder text | Input placeholder |
| `button.update_password` | Submit button text | Password update action |
| `button.updating` | Processing state button text | During password update |
| `account.password_updated_successfully` | Success message after password update | Confirmation message |
| `account.must_be_logged_in_to_update_password` | Authentication required message | Login requirement |

### Implementation Examples

```vue
<!-- Form title -->
<component :is="titleTag" class="cdx_title">
    {{ title || $t('account.update_password_title') }}
</component>

<!-- Password fields -->
<div class="cdx_password">
    <codex-visually-hidden v-if="!showLabels" as="label">
        {{ $t('password.password') }}
    </codex-visually-hidden>
    <input 
        type="password" 
        v-model="password" 
        :placeholder="$t('account.new_password')+'*'" 
        required
    >
    <label v-if="showLabels" class="cdx_label">
        {{ $t('password.password') }}
    </label>
</div>

<div class="cdx_confirm-password">
    <codex-visually-hidden v-if="!showLabels" as="label">
        {{ $t('password.confirm_password') }}
    </codex-visually-hidden>
    <input 
        type="password" 
        v-model="password_confirmation" 
        :placeholder="$t('account.confirm_new_password')+'*'" 
        required
    >
    <label v-if="showLabels" class="cdx_label">
        {{ $t('password.confirm_password') }}
    </label>
</div>

<!-- Submit button with state management -->
<button type="submit" :disabled="!password || !password_confirmation">
    <template v-if="!updating">{{ $t('button.update_password') }}</template>
    <template v-else>{{ $t('button.updating') }}</template>
</button>

<!-- Success state -->
<div v-if="updated" class="cdx_panel-updated">
    <slot>{{ $t('account.password_updated_successfully') }}</slot>
</div>

<!-- Login required state -->
<div v-if="!customer" class="cdx_panel-login">
    <slot name="login">{{ $t('account.must_be_logged_in_to_update_password') }}</slot>
</div>
```

### Form Validation Integration

The component integrates with customer password update composable for error handling:

```javascript
import useCustomerPasswordUpdate from '@/composition/useCustomer/useCustomerPasswordUpdate.js';

const { 
    customer, 
    updatePassword, 
    updating, 
    updated, 
    error, 
    fieldErrors, 
    genericErrors 
} = useCustomerPasswordUpdate();
```

### Error Message Display

```vue
<!-- Error messages from composable -->
<slot v-if="error" name="error-messages" :genericErrors="genericErrors">
    <codex-error :errors="genericErrors" />
</slot>
```

### Accessibility Features

- Uses `codex-visually-hidden` for accessible labels when visual labels are disabled
- Proper form field associations with labels
- Required field indicators with asterisks

### Notes
- Component marked as deprecated in favor of integrated Profile component
- Error messages come from the password update composable
- Supports both visible and hidden label modes for different UI designs
- Form validation prevents submission until both password fields are filled
- Custom title can override default translation through props

## Examples

### Basic Implementation
```vue
<codex-password-update />
```

### With Labels and Custom Title
```vue
<codex-password-update
  :show-labels="true"
  :title="'Change Your Password'"
  :title-tag="'h3'"
/>
```

### Custom Title Slot
```vue
<codex-password-update>
  <template #title="{ title, titleTag }">
    <div class="password-update-header">
      <h2>Security Settings</h2>
      <p>Update your password to keep your account secure</p>
    </div>
  </template>
</codex-password-update>
```

### Custom Error Handling
```vue
<codex-password-update>
  <template #error-messages="{ genericErrors }">
    <div class="custom-error-display">
      <h4>Password Update Failed</h4>
      <ul class="error-list">
        <li v-for="error in genericErrors" :key="error">
          {{ error }}
        </li>
      </ul>
      <p class="error-help">
        Please ensure your password meets all security requirements.
      </p>
    </div>
  </template>
</codex-password-update>
```

### Custom Success Message
```vue
<codex-password-update>
  <template #default>
    <div class="custom-success">
      <div class="success-icon">
        <i class="checkmark-icon"></i>
      </div>
      <h3>Password Updated Successfully!</h3>
      <p>Your password has been updated. Please use your new password for future logins.</p>
      <button @click="redirectToDashboard">Continue to Dashboard</button>
    </div>
  </template>
</codex-password-update>
```

### Custom Login Required Message
```vue
<codex-password-update>
  <template #login>
    <div class="login-required">
      <h3>Login Required</h3>
      <p>Please log in to update your password.</p>
      <codex-button 
        @click="redirectToLogin"
        :default-text="'Go to Login'"
        variant="primary"
      />
    </div>
  </template>
</codex-password-update>
```

### With Event Handling
```vue
<codex-password-update
  @password-updated="handlePasswordUpdated"
  @update-failed="handleUpdateFailed"
/>

<script setup>
const handlePasswordUpdated = (customer) => {
  console.log('Password updated for:', customer.email)
  // Redirect or show additional success actions
  showSuccessNotification('Password updated successfully!')
}

const handleUpdateFailed = (errors) => {
  console.log('Update failed:', errors)
  // Handle specific error scenarios
  trackPasswordUpdateFailure(errors)
}
</script>
```

### In Account Settings Section
```vue
<div class="account-settings">
  <div class="settings-section">
    <h2>Account Security</h2>
    
    <div class="password-section">
      <codex-password-update 
        :show-labels="true"
        :title="'Update Password'"
        :title-tag="'h3'"
      />
    </div>
    
    <div class="security-options">
      <!-- Other security settings -->
    </div>
  </div>
</div>
```

## CSS Classes
- `codex`: Base component identifier
- `cdx_password-update`: Password update specific styling
- `cdx_panel`: Panel container styling
- `cdx_title`: Title styling
- `cdx_form`: Form container styling
- `cdx_password-update-form`: Form specific styling
- `cdx_password`: Password field container
- `cdx_inputs`: Input field styling
- `cdx_label`: Label styling
- `cdx_confirm-password`: Confirm password field container
- `cdx_submit-wrapper`: Submit button wrapper
- `cdx_btn`: Button styling
- `cdx_panel-updated`: Success state styling
- `cdx_panel-login`: Login required state styling

## Best Practices

### Recommended Usage Patterns
- Always validate password confirmation matches
- Provide clear feedback during processing
- Use secure form practices
- Handle authentication requirements properly
- Implement proper error handling
- Provide meaningful success feedback
- Use appropriate security messaging
- Consider password strength requirements

### Common Pitfalls to Avoid
- Not validating password confirmation
- Missing authentication checks
- Insufficient error handling
- Not providing processing feedback
- Missing security considerations
- Poor form validation
- Inadequate success messaging
- Not handling edge cases

### Accessibility Considerations
- Provide clear labels for form fields
- Use appropriate ARIA attributes
- Ensure keyboard navigation works properly
- Provide screen reader friendly feedback
- Include proper form validation messages
- Handle focus management appropriately
- Use semantic HTML for form structure
- Ensure adequate color contrast

### Security Considerations
- Validate password strength requirements
- Implement proper authentication checks
- Use secure form submission methods
- Handle password data securely
- Implement rate limiting protection
- Validate on both client and server
- Use HTTPS for form submission
- Clear sensitive data appropriately

### Error Handling
- Display clear validation errors
- Handle server-side errors gracefully
- Provide meaningful error messages
- Clear errors when input changes
- Handle network connectivity issues
- Implement proper retry mechanisms
- Validate input before submission

### State Management
- Track form validation state
- Handle authentication state changes
- Manage processing states properly
- Update UI state after operations
- Handle success state transitions
- Coordinate with global auth state
- Manage form reset appropriately

### Performance Considerations
- Minimize form validation overhead
- Optimize password strength checking
- Handle form submission efficiently
- Implement proper cleanup
- Avoid unnecessary re-renders
- Cache validation results appropriately
- Optimize error message display

### Form Validation
- Implement real-time validation
- Validate password strength
- Ensure confirmation matching
- Handle edge cases properly
- Provide immediate feedback
- Use appropriate validation rules
- Clear validation state appropriately

### Migration Considerations
- **Deprecated Status**: Component merged into Profile component
- **Legacy Support**: Still functional but not recommended for new development
- **Migration Path**: Use Profile component for new implementations
- **Compatibility**: Existing implementations continue to work
- **Future Plans**: May be removed in future versions

## Component Registration
The component is registered as `password-update` in the application.

**Deprecation Notice**: This component has been merged into the Profile component and is no longer recommended for new development. Use the Profile component's password update functionality instead. 