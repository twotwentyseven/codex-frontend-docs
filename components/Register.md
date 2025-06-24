# Register Component

## Overview
The Register component provides a comprehensive multi-step user registration interface that handles dynamic form fields, validation, error states, and customer management. It features flexible multi-page forms, customizable field layouts, internationalization support, and seamless integration with login flows.

## Basic Usage
```vue
<codex-register
  :title="false"
  :description="false"
  :multi-page="false"
  :form-fields="false"
  @register-started="handleRegisterStart"
  @register-success="handleRegisterSuccess"
  @register-failure="handleRegisterFailure"
/>
```

## Key Features
- Multi-step form navigation
- Dynamic form field configuration
- Multiple field types support (text, email, password, date, select, checkbox, upload, textarea, repeater)
- Field validation and error handling
- Customer state management (logged in/out, registered/unregistered)
- Success state with visual feedback
- Login modal integration
- Loading states and processing indicators
- Internationalization support
- Flexible slot-based layout
- Responsive column layouts
- Page break functionality
- Error message linking to specific inputs

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| title | String\|Boolean | No | false | Custom title or false to use default from language file |
| description | String\|Boolean | No | false | Custom description or false to use default from language file |
| multiPage | Boolean | No | false | Enable multi-step form functionality |
| formFields | Object\|Array\|Boolean | No | false | Override default form fields configuration |
| buttonText | Object | No | `{defaultText, disabledText, processingText}` | Customize button text for different states |

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
| register-started | - | Emitted when registration process begins |
| register-success | - | Emitted when registration is successful |
| register-failure | - | Emitted when registration fails |
| close | - | Emitted when component should be closed (e.g., to open login modal) |
| customEvent | - | Emitted for custom event handling from form fields |

## Slots

### Header Slot
```vue
<template #header="{ genericErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ genericErrors, fieldErrors }">
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

## States
The component has multiple states in order of priority:
1. **Already Logged In (Not Registered)** - Shows logout option when user is already authenticated
2. **Form State** - Multi-step registration form display
3. **Success State** - Registration completion confirmation with visual feedback
4. **Already Registered** - Shows success message for already registered users

## Form Field Types
The component supports various field types:
- `text`: Standard text input
- `email`: Email input with validation
- `password`: Password input
- `date`: Date picker
- `select`: Dropdown selection
- `checkbox`: Checkbox input
- `upload`: File upload field
- `textarea`: Multi-line text input
- `text_repeater`: Repeatable text fields
- `telephone`: Phone number input
- `page_break`: Form page separator

## Internationalization
The component uses the following translation keys:

### Core Component Keys
- `register.title`: Form title
- `register.introduction`: Introduction text
- `register.youre_logged_in`: Already logged in title
- `register.youre_register`: Registration success title
- `register.success_msg`: Success message
- `register.do_have_an_account`: Login prompt text
- `register.login_here`: Login link text
- `register.register`: Register button text
- `register.registering`: Processing button text
- `register.field_is_required`: Required field validation error

### Navigation Keys
- `button.back`: Back button text
- `button.next`: Next button text
- `button.error`: Error button text

### Field Label Translation Keys
The component supports field-specific translation keys through `label_translation` and `placeholder_translation` properties:

- `customer.first_name`: First name label
- `customer.first_name_placeholder`: First name placeholder
- `customer.last_name`: Last name label
- `customer.last_name_placeholder`: Last name placeholder
- `customer.email`: Email label
- `customer.email_placeholder`: Email placeholder
- `customer.phone_number`: Phone number label
- `customer.phone_number_placeholder`: Phone number placeholder
- `customer.date_of_birth`: Date of birth label
- `customer.postcode`: Postcode label
- `customer.postcode_placeholder`: Postcode placeholder
- `customer.gender_placeholder`: Gender placeholder
- `customer.username`: Username label
- `customer.username_placeholder`: Username placeholder
- `customer.textarea`: Textarea label
- `customer.textarea_placeholder`: Textarea placeholder
- `customer.emergency_name`: Emergency contact name label
- `customer.emergency_name_placeholder`: Emergency contact name placeholder
- `customer.emergency_phone`: Emergency contact phone label
- `customer.emergency_phone_placeholder`: Emergency contact phone placeholder
- `customer.i_accept_email_marketing`: Email marketing consent label
- `customer.i_accept_sms_marketing`: SMS marketing consent label
- `account.password`: Password label
- `account.password_placeholder`: Password placeholder
- `account.confirm_password`: Confirm password label
- `account.confirm_password_placeholder`: Confirm password placeholder
- `register_fields.image`: Image upload field label
- `register_fields.image_placeholder`: Image upload field placeholder
- `register_fields.children`: Children field label
- `register_fields.children_placeholder`: Children field placeholder

### Translation Usage Examples
```vue
<!-- Using title prop with fallback to translation -->
<codex-register :title="false" />  <!-- Uses $t('register.title') -->
<codex-register title="Custom Title" />  <!-- Uses custom title -->

<!-- Custom field with translation keys -->
<codex-register :form-fields="{
  custom_field: {
    type: 'text',
    label_translation: 'custom.field_label',
    placeholder_translation: 'custom.field_placeholder',
    required: true
  }
}" />

<!-- Navigation buttons use translation keys -->
<template #content>
  <button>{{ $t('button.back') }}</button>
  <button>{{ $t('button.next') }}</button>
</template>
```

## Examples

### Basic Implementation
```vue
<codex-register
  @register-success="redirectToDashboard"
  @register-failure="showErrorNotification"
/>
```

### Multi-Page Form
```vue
<codex-register
  :multi-page="true"
  title="Create Your Account"
  description="Join us today and get started"
/>
```

### Custom Form Fields
```vue
<codex-register
  :form-fields="{
    first_name: {
      type: 'text',
      label: 'First Name',
      required: true
    },
    email: {
      type: 'email',
      label: 'Email Address',
      required: true
    },
    password: {
      type: 'password',
      label: 'Password',
      required: true
    }
  }"
/>
```

### Multi-Step Form with Custom Button Text
```vue
<codex-register
  :multi-page="true"
  :button-text="{
    defaultText: 'Create Account',
    disabledText: 'Please Wait...',
    processingText: 'Creating Account...'
  }"
/>
```

### Modal Integration
```vue
<codex-modal modal-name="register">
  <codex-register
    @register-success="closeRegisterModal"
    @close="openLoginModal"
  />
</codex-modal>
```

### Custom Header with Branding
```vue
<codex-register>
  <template #header="{ genericErrors }">
    <div class="custom-header">
      <img src="/logo.png" alt="Company Logo" />
      <h2>Join Our Community</h2>
      <p>Create your account to get started</p>
      <codex-error v-if="genericErrors.length" :error="genericErrors" />
    </div>
  </template>
</codex-register>
```

### Advanced Multi-Page Configuration
```vue
<codex-register
  :form-fields="{
    page_1: {
      first_name: { type: 'text', required: true },
      last_name: { type: 'text', required: true },
      email: { type: 'email', required: true }
    },
    page_break: { type: 'page_break' },
    page_2: {
      password: { type: 'password', required: true },
      password_confirmation: { type: 'password', required: true },
      dob: { type: 'date', required: true }
    }
  }"
  :multi-page="true"
/>
```

## CSS Classes
- `codex`: Base component identifier
- `_c-card`: Main container class
- `_c-register`: Register-specific styling
- `_c-border`: Optional border styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-form`: Form container
- `_c-form-page`: Individual form page container
- `_c-btn-container`: Button container
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon container
- `_c-success-title`: Success message title
- `_c-success-desc`: Success message description
- `_c-link`: Link styling for login button
- `cdx_btn`: Button base class
- `cdx_btn--back`: Back button specific styling

## Best Practices

### Recommended Usage Patterns
- Configure form fields based on business requirements
- Use multi-page forms for complex registration flows
- Always handle all registration events
- Provide clear progress indicators for multi-step forms
- Implement proper field validation
- Use appropriate loading states
- Handle customer state transitions properly

### Common Pitfalls to Avoid
- Not configuring required fields properly
- Missing validation for complex field types
- Not handling page navigation errors
- Forgetting to clear form data after successful registration
- Not providing feedback during multi-step processes
- Missing error handling for file uploads
- Not handling modal state management properly

### Accessibility Considerations
- Ensure proper form labels and ARIA attributes
- Provide clear error messages for each field
- Maintain keyboard navigation through multi-page forms
- Use appropriate color contrast
- Support screen readers for complex field types
- Provide visual focus indicators
- Include progress indicators for multi-step forms

### Error Handling
- Validate required fields before page navigation
- Display field-specific errors clearly
- Navigate to pages with errors automatically
- Show generic errors for server issues
- Provide recovery options for failed uploads
- Clear errors when user corrects input
- Handle network timeouts gracefully

### State Management
- Handle customer authentication states properly
- Manage form data across multiple pages
- Track registration progress
- Handle session timeouts
- Coordinate with login state management
- Clear sensitive data after registration
- Manage modal state transitions

### Performance Considerations
- Lazy load complex field components
- Optimize form field rendering
- Handle large file uploads efficiently
- Debounce validation where appropriate
- Minimize re-renders during form navigation
- Consider pagination for very long forms
- Implement proper cleanup for uploaded files

### Security Considerations
- Validate all input on both client and server
- Implement proper password requirements
- Sanitize file uploads
- Protect against injection attacks
- Use HTTPS for all form submissions
- Implement rate limiting for registration attempts
- Clear sensitive data from memory

### Multi-Page Form Management
- Validate required fields before allowing navigation
- Preserve form data between pages
- Provide clear progress indicators
- Handle errors by navigating to the appropriate page
- Allow backward navigation when appropriate
- Save draft data for long forms
- Implement proper page transitions

## Component Registration
The component is registered as `codex-register` in the application. 