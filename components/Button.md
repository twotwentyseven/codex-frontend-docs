# Button Component

The Button component (`codex-button`) is a versatile, state-aware button that handles various interactions and states commonly needed in forms and user interfaces.

## Basic Usage

```vue
<codex-button
  :disabled="false"
  :processing="isSubmitting"
  :error="hasError"
  :success="isSuccess"
  :warning="hasWarning"
  @submitted="handleSubmit"
>
  Submit Form
</codex-button>
```

## Features

- Form submission handling
- State management (disabled, processing, error, success, warning)
- Confirmation flow
- Debounce protection
- Internationalization support
- Customizable text for different states
- Automatic success state timeout
- Slot support for content positioning

## Props

### State Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `disabled` | `Boolean` | Yes | `false` | Disables the button |
| `processing` | `Boolean` | Yes | `false` | Shows processing state |
| `error` | `Boolean` | Yes | `false` | Shows error state |
| `success` | `Boolean` | Yes | `false` | Shows success state |
| `warning` | `Boolean` | Yes | `false` | Shows warning state |

### Configuration Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `variant` | `String` | No | `'primary'` | Button style variant (`'primary'`, `'secondary'`, `'danger'`, `'success'`, `'warning'`, `'custom'`) |
| `debounce` | `Boolean` | No | `true` | Prevents multiple rapid clicks |
| `requireConfirmation` | `Boolean` | No | `false` | Requires second click to confirm action |
| `successTimeout` | `Number` | No | `5000` | Duration (ms) to show success state |

### Text Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `defaultText` | `String\|Boolean` | No | `false` | Default button text |
| `disabledText` | `String\|Boolean` | No | `false` | Text when disabled |
| `processingText` | `String\|Boolean` | No | `false` | Text while processing |
| `errorText` | `String\|Boolean` | No | `false` | Text when error occurs |
| `warningText` | `String\|Boolean` | No | `false` | Text for warning state |
| `successText` | `String\|Boolean` | No | `false` | Text for success state |
| `confirmationText` | `String\|Boolean` | No | `false` | Text for confirmation step |

## Events

| Event | Parameters | Description |
|-------|------------|-------------|
| `submitted` | `(event: Event)` | Emitted when button is clicked (after confirmation if enabled) |
| `confirm` | `(event: Event)` | Emitted on first click when confirmation is required |

## Slots

| Slot | Description |
|------|-------------|
| `before` | Content to place before the button text |
| `after` | Content to place after the button text |

## States and Transitions

The button has several states that follow a priority order:
1. Processing
2. Error
3. Success
4. Warning
5. Confirmation
6. Default (variant)

## Internationalization

The component uses Vue I18n for text localization. Default translations keys:

```javascript
{
  "button": {
    "submit": "Submit",
    "submitting": "Submitting...",
    "error": "Error",
    "success": "Success",
    "warning": "Warning",
    "confirm": "Confirm"
  }
}
```

## Examples

### Basic Submit Button
```vue
<codex-button
  :disabled="!isValid"
  :processing="isSubmitting"
  :success="isSuccess"
  @submitted="submitForm"
/>
```

### Confirmation Button
```vue
<codex-button
  variant="danger"
  :disabled="false"
  :processing="isDeleting"
  requireConfirmation
  defaultText="Delete Account"
  confirmationText="Click again to confirm"
  @submitted="deleteAccount"
/>
```

### Custom Styled Button with Slots
```vue
<codex-button
  variant="custom"
  :disabled="false"
  :processing="isLoading"
>
  <template #before>
    <Icon name="arrow-left" />
  </template>
  Go Back
  <template #after>
    <Badge count="2" />
  </template>
</codex-button>
```

### Form Submit Button with States
```vue
<codex-button
  :disabled="!formValid"
  :processing="isSubmitting"
  :error="hasError"
  :success="isSuccess"
  defaultText="Save Changes"
  processingText="Saving..."
  errorText="Failed to Save"
  successText="Changes Saved!"
  @submitted="saveForm"
/>
```

## CSS Classes

The component uses the following CSS classes:
- `_c-btn`: Base button styles
- `_c-[variant]-btn`: Variant-specific styles
- `_c-disabled-btn`: Disabled state styles

## Best Practices

1. **Form Submission**
   - Always use the `processing` prop during async operations
   - Handle errors appropriately and show feedback
   - Use success state to confirm completion

2. **Confirmation Flow**
   - Use `requireConfirmation` for destructive actions
   - Provide clear confirmation text
   - Consider using warning variant

3. **State Management**
   - Keep state management in parent component
   - Use computed properties for complex state logic
   - Reset states after operations complete

4. **Accessibility**
   - Provide meaningful text for screen readers
   - Use appropriate ARIA attributes
   - Ensure proper color contrast

5. **Error Handling**
   - Show clear error messages
   - Provide recovery options
   - Reset error state when appropriate

## Component Registration

This component is registered globally as `codex-button` in the application.