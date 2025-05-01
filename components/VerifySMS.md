# SMS Verification Component

## Overview
The SMS Verification component provides a multi-step process for verifying user phone numbers through SMS. It handles phone number input, verification code sending and verification, with support for different states, error handling, and internationalization.

## Basic Usage
```vue
<codex-verify-sms />
```

## Key Features
- Multi-step verification process
- Phone number input and validation
- Verification code handling
- Auto-focus verification fields
- Success state management
- Error handling and display
- Customer state awareness
- Login integration
- Resend functionality
- Responsive design

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| title | String\|Boolean | No | false | Custom title text |
| titleTag | String | No | 'h2' | HTML tag for titles |

## Events
| Event Name | Parameters | Description |
|------------|------------|-------------|
| close | - | Emitted when the component should be closed |

## Slots

### Header Slot
```vue
<template #header="{ errorMessages }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content>
  <!-- Custom content -->
</template>
```

### Footer Slot
```vue
<template #footer>
  <!-- Custom footer content -->
</template>
```

## States
1. Not Logged In (login prompt)
2. Phone Number Input
3. Code Sending
4. Code Verification
5. Success
6. Error States

## Internationalization
The component uses the following translation keys:
- `sms.login_to_verify_account`: Login prompt title
- `sms.login_intro`: Login introduction text
- `sms.update_your_mobile_number`: Update number title
- `sms.verify_your_mobile_number_intro`: Verification intro text
- `sms.verify_code_sent`: Code sent title
- `sms.have_your_code_enter_here`: Code entry prompt
- `sms.your_mobile_number`: Phone number label
- `sms.send_code`: Send code button text
- `sms.sending`: Sending state text
- `sms.code_sent`: Success state text
- `sms.verification_code`: Code input label
- `sms.verify`: Verify button text
- `sms.verifying`: Verification in progress text
- `sms.verified`: Verification success text
- `sms.account_verified_successfully`: Success title
- `sms.success_msg`: Success message
- `button.loading`: Loading state text

## Examples

### Basic Implementation
```vue
<codex-verify-sms
  title="Verify Your Phone Number"
/>
```

### Custom Error Display
```vue
<codex-verify-sms>
  <template #header="{ errorMessages }">
    <div class="custom-errors">
      <codex-error :errors="errorMessages" />
    </div>
  </template>
</codex-verify-sms>
```

### Custom Success State
```vue
<codex-verify-sms>
  <template #content>
    <div v-if="verified" class="custom-success">
      <h3>Phone Number Verified!</h3>
      <p>You can now receive SMS notifications.</p>
    </div>
  </template>
</codex-verify-sms>
```

### Modal Integration
```vue
<codex-modal modal-name="verify-sms">
  <codex-verify-sms
    @close="closeModal"
  />
</codex-modal>
```

## CSS Classes
- `_c-card`: Main container class
- `_c-verifysms`: Component specific class
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-form`: Form container
- `_c-btn-container`: Button container
- `_c-primary-btn`: Primary button style
- `_c-link`: Link styling
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon container
- `_c-success-title`: Success message title
- `_c-success-desc`: Success message description
- `_c-form-footer`: Form footer section
- `_c-desc`: Description text styling

## Best Practices

### Recommended Usage
- Implement clear user instructions
- Show appropriate loading states
- Handle all verification states
- Provide resend functionality
- Implement proper error handling
- Consider mobile responsiveness
- Use appropriate timeouts

### Security Considerations
- Implement rate limiting
- Validate phone numbers
- Secure code transmission
- Handle session timeouts
- Protect against brute force
- Clear sensitive data
- Monitor failed attempts

### Accessibility Considerations
- Provide clear feedback
- Ensure keyboard navigation
- Use appropriate ARIA labels
- Maintain focus management
- Consider screen readers
- Provide visual indicators
- Handle timeout notifications

### Performance Considerations
- Optimize API calls
- Handle network issues
- Manage verification timeouts
- Implement proper caching
- Handle state transitions
- Optimize code input
- Consider rate limiting

### Common Pitfalls to Avoid
- Missing verification states
- Poor error handling
- Unclear instructions
- No resend option
- Missing loading states
- Poor mobile experience
- Insufficient validation

### State Management
- Track verification status
- Handle code expiration
- Manage resend attempts
- Track input validation
- Handle API errors
- Maintain user context
- Clear sensitive data

## Component Registration
The component is registered as `codex-verify-sms` in the application. 