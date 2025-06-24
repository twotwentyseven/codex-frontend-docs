# Profile Component

## Overview
The Profile component provides a comprehensive user profile management interface that handles customer information updates, form validation, success feedback, and dynamic field configuration. It features flexible form layouts, authentication state management, internationalization support, and customizable field sets for updating customer details.

## Basic Usage
```vue
<codex-profile
  :hide-on-submit="false"
  :title="'Update Profile'"
  :intro="'Keep your information up to date'"
  :form-fields="false"
  @update-started="handleUpdateStart"
  @update-success="handleUpdateSuccess"
  @update-failure="handleUpdateFailure"
/>
```

## Key Features
- Dynamic form field configuration
- Customer authentication state handling
- Field validation and error handling
- Success modal with auto-close
- Flexible form layouts with column spanning
- Multiple field types support (text, email, password, date, select, checkbox, textarea)
- Section organization (personal, address, emergency contact, opt-ins)
- Form hiding on successful submission
- Internationalization support
- Flexible slot-based layout
- Login integration for unauthenticated users
- Responsive design with customizable layouts

## Props

### Configuration Props
| Prop Name | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| hideOnSubmit | Boolean | No | false | Hide form after successful submission |
| title | String | No | undefined | Custom form title |
| intro | String | No | undefined | Introduction text above the form |
| formFields | Object\|Array\|Boolean | No | false | Override default form fields configuration |

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
| update-started | - | Emitted when profile update process begins |
| update-success | - | Emitted when profile update is successful |
| update-failure | - | Emitted when profile update fails |

## Slots

### Logged Out Slot
```vue
<template #logged-out>
  <!-- Custom content when user is not authenticated -->
</template>
```

### Header Slot
```vue
<template #header="{ genericErrors, hasError, fieldErrors }">
  <!-- Custom header content -->
</template>
```

### Content Slot
```vue
<template #content="{ genericErrors, hasError, fieldErrors }">
  <!-- Custom form content -->
</template>
```

### Footer Slot
```vue
<template #footer="{ genericErrors, hasError, fieldErrors }">
  <!-- Custom footer content -->
</template>
```

### Error Messages Slot
```vue
<template #error-messages="{ genericErrors }">
  <!-- Custom error display -->
</template>
```

### Success Messages Slot
```vue
<template #success-messages>
  <!-- Custom success state display -->
</template>
```

### Slot Props
| Prop Name | Type | Description |
|-----------|------|-------------|
| genericErrors | Array | Generic error messages |
| fieldErrors | Object | Field-specific errors |
| hasError | Function | Function to check if a field has errors |

## States
The component has multiple states in order of priority:
1. **Unauthenticated State** - Shows login component when `allowLogin` is true and no customer
2. **Form State** - Profile update form display with dynamic fields
3. **Success State** - Success modal with confirmation message
4. **Hidden Form State** - Form hidden after successful submission (if `hideOnSubmit` is true)

## Form Field Types
The component supports various field types:
- `text`: Standard text input
- `email`: Email input with validation
- `password`: Password input
- `date`: Date picker
- `select`: Dropdown selection
- `checkbox`: Checkbox input
- `textarea`: Multi-line text input
- `telephone`: Phone number input
- `title`: Section headers
- `paragraph`: Descriptive text

## Form Sections
Default form includes organized sections:
- **Personal Information**: First name, last name, DOB, gender, email, phone, password
- **Address Details**: Postcode and address information
- **Emergency Contact**: Emergency contact name and phone
- **Opt-in Preferences**: Email and SMS marketing preferences

## Internationalization
The component uses the following translation keys:
- `account.you_need_to_login_to_update_profile`: Login prompt title
- `account.profile_updated_successfully`: Success confirmation
- `account.update_my_profile`: Submit button text
- `account.address_details`: Address section title
- `account.emergency_contact`: Emergency contact section title
- `account.opt_in_preferences`: Opt-in section title
- `button.updating`: Processing button text
- `customer.*`: Customer field labels and placeholders
- `account.*`: Account field labels and placeholders

## Examples

### Basic Implementation
```vue
<codex-profile
  @update-success="showSuccessNotification"
  @update-failure="showErrorNotification"
/>
```

### Custom Title and Introduction
```vue
<codex-profile
  title="Account Settings"
  intro="Update your personal information and preferences"
  :hide-on-submit="true"
/>
```

### Custom Form Fields
```vue
<codex-profile
  :form-fields="{
    first_name: {
      type: 'text',
      label: 'First Name',
      required: true,
      layout: 'half'
    },
    last_name: {
      type: 'text',
      label: 'Last Name',
      required: true,
      layout: 'half'
    },
    email: {
      type: 'email',
      label: 'Email Address',
      required: true,
      layout: 'full'
    }
  }"
/>
```

### Custom Success Message
```vue
<codex-profile>
  <template #success-messages>
    <div class="custom-success">
      <h3>Profile Updated!</h3>
      <p>Your changes have been saved successfully.</p>
    </div>
  </template>
</codex-profile>
```

### Custom Authentication State
```vue
<codex-profile>
  <template #logged-out>
    <div class="custom-login-prompt">
      <h2>Please Sign In</h2>
      <p>Access your profile by logging in to your account.</p>
      <codex-login />
    </div>
  </template>
</codex-profile>
```

### Sectioned Form Layout
```vue
<codex-profile
  :form-fields="{
    personal_title: {
      type: 'title',
      content: 'Personal Information',
      tag: 'h2',
      layout: 'full'
    },
    first_name: {
      type: 'text',
      required: true,
      layout: 'half'
    },
    last_name: {
      type: 'text',
      required: true,
      layout: 'half'
    },
    preferences_title: {
      type: 'title',
      content: 'Preferences',
      tag: 'h2',
      layout: 'full'
    },
    opt_in_email: {
      type: 'checkbox',
      label: 'Email notifications',
      layout: 'full'
    }
  }"
/>
```

## CSS Classes
- `codex`: Base component identifier
- `_c-profile-card`: Profile-specific card styling
- `_c-card`: Main container class
- `_c-profile`: Default profile styling
- `_c-header`: Header section
- `_c-content`: Content section
- `_c-footer`: Footer section
- `_c-form`: Form container
- `_c-btn-container`: Button container
- `_c-success-container`: Success state container
- `_c-success-icon`: Success icon container
- `_c-success-title`: Success message title
- `_c-title`: Section title styling

## Best Practices

### Recommended Usage Patterns
- Configure form fields based on customer data requirements
- Always handle update events for proper feedback
- Use appropriate field layouts for responsive design
- Implement proper validation before submission
- Provide clear success feedback
- Handle authentication states gracefully
- Use section titles to organize complex forms

### Common Pitfalls to Avoid
- Not handling unauthenticated states
- Missing validation for required fields
- Forgetting to clear form data after successful update
- Not providing feedback during processing
- Missing error handling for network failures
- Not handling password confirmation validation
- Insufficient handling of opt-in preferences

### Accessibility Considerations
- Ensure proper form labels and ARIA attributes
- Provide clear error messages for each field
- Maintain keyboard navigation support
- Use appropriate color contrast
- Support screen readers for section navigation
- Provide visual focus indicators
- Include proper form validation feedback

### Error Handling
- Validate required fields before submission
- Display field-specific errors clearly
- Show generic errors for server issues
- Provide recovery options for failed updates
- Clear errors when user corrects input
- Handle network timeouts gracefully
- Validate password confirmation matching

### State Management
- Handle customer authentication states properly
- Populate form fields with existing customer data
- Track update progress and states
- Handle session timeouts
- Manage form state across field groups
- Clear sensitive data after updates
- Coordinate with customer state management

### Performance Considerations
- Lazy load complex field components
- Optimize form field rendering
- Debounce validation where appropriate
- Minimize re-renders during form interaction
- Handle large customer datasets efficiently
- Consider progressive enhancement for complex forms
- Implement proper cleanup for form state

### Security Considerations
- Validate all input on both client and server
- Implement proper password requirements
- Protect against injection attacks
- Use HTTPS for all form submissions
- Sanitize user input properly
- Implement rate limiting for updates
- Clear sensitive data from memory

### Form Field Configuration
- Use appropriate field types for data
- Implement proper validation rules
- Organize fields into logical sections
- Use responsive layouts with column spanning
- Provide helpful placeholder text
- Include appropriate field hints
- Handle optional vs required fields clearly

## Component Registration
The component is registered as `codex-profile` in the application. 